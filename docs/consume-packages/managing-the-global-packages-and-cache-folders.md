---
title: How to manage the global packages, http cache, temp directories in NuGet
description: How to manage the global package installation directory, the http cache, and the temp directory that exist on a computer, which are used when installing, restoring, and updating packages.
author: JonDouglas
ms.author: jodou
ms.date: 01/30/2026
ms.topic: how-to
---

# Managing the global packages, cache, and temp directories

Whenever you install, update, or restore a package, NuGet manages packages and package information in several directories outside of your project structure:

| Name | Location |
| --- | --- |
| [global-packages](#global-packages) | <ul><li>Windows: `%userprofile%\.nuget\packages`</li><li>Mac/Linux: `~/.nuget/packages`</li><li>Override using the NUGET_PACKAGES environment variable, the `globalPackagesFolder` or `repositoryPath` [configuration settings](../reference/nuget-config-file.md#config-section) (when using PackageReference and `packages.config`, respectively), or the `RestorePackagesPath` MSBuild property (MSBuild only). The environment variable takes precedence over the configuration setting.</li></ul> |
| [http-cache](#http-cache) | <ul><li>Windows: `%localappdata%\NuGet\v3-cache`</li><li>Mac/Linux: `~/.local/share/NuGet/v3-cache`</li><li>Override using the NUGET_HTTP_CACHE_PATH environment variable.</li></ul> |
| [temp](#temp) | <li>Windows: `%temp%\NuGetScratch`</li><li>Mac: `/tmp/NuGetScratch`</li><li>Linux: `/tmp/NuGetScratch<username>`</li><li>Override using the NUGET_SCRATCH environment variable.</li></ul> |
| [plugins-cache](#plugin-cache) **4.8+** | <ul><li>Windows: `%localappdata%\NuGet\plugins-cache`</li><li>Mac/Linux: `~/.local/share/NuGet/plugins-cache`</li><li>Override using the NUGET_PLUGINS_CACHE_PATH environment variable.</li></ul> |

> [!Note]
> NuGet 3.5 and earlier uses *packages-cache* instead of the *http-cache*, which is located in `%localappdata%\NuGet\Cache`.

By using the *global-packages* directory, NuGet generally avoids downloading packages that already exist on the computer, improving the performance of install, update, and restore operations.
When using PackageReference, the *global-packages* directory also avoids keeping downloaded packages inside project directories, where they might be inadvertently added to source control, and reduces NuGet's overall impact on computer storage.

When asked to retrieve a package, NuGet first looks in the *global-packages* directory.
If the exact version of package is not there, then NuGet checks all non-HTTP package sources.
If the package is still not found, NuGet looks for the package in the *http-cache* unless you specify `--no-http-cache` with `dotnet.exe` commands or `-NoHttpCache` with `nuget.exe` commands.
If the package is not in the cache, or the cache isn't used, NuGet then retrieves the package over HTTP .

For more information, see [What happens when a package is installed?](../concepts/package-installation-process.md).

## global-packages

The *global-packages* directory is where NuGet installs any downloaded package.
Each package is fully expanded into a subdirectory that matches the package identifier and version number.
Projects using the [PackageReference](package-references-in-project-files.md) format always use packages directly from this directory.
When using the [packages.config](../reference/packages-config.md), packages are also installed to the *global-packages* directory, then copied into the project's `packages` directory.

### Cleaning the global-packages directory

The global-packages directory needs to be manually cleaned to remove packages that are no longer used.
You can do this with the `dotnet nuget locals global-packages --clear` command, or the "clear NuGet local resources" button in Visual Studio's options (equivalent to `dotnet nuget locals all --clear`).
After clearing the global-packages directory, you will need to restore your projects again to redownload all required packages.
In Visual Studio, you may need to reload your solution to clear NuGet's "up to date restores" cache, or alternatively do a command line restore (for example, within Visual Studio's terminal window) with `msbuild -t:restore your.sln`.

To clean only unused packages, it's a two step process.
First, there is a [nuget.config setting `updatePackageLastAccessTime`](../reference/nuget-config-file.md) that should be enabled.
This setting will cause NuGet to update each package's `.nupkg.metadata` file when it is used in a restore.
When restore runs, but a project is considered already up to date, the package timestamps are *not* updated, so if a project's restore inputs don't change for several weeks, the `.nupkg.metadata` files will not have their timestamps updated.
The `.nupkg.metadata` file is the last file that NuGet will create when downloading and extracting packages during a restore or install, and is the file that restore uses to check if a package has been extracted successfully.

Second, run a tool to perform the cleanup.
After the `updatePackageLastAccessTime` setting is enabled, we recommend waiting a few days to make sure that all the packages you use regularly have had their timestamps updated.

At this time, NuGet does not provide a tool or command to do this.
You can [add a 👍 reaction to this GitHub issue](https://github.com/NuGet/Home/issues/4980) to signal your interest.
Some community members have created their own open source NuGet cleaner tools that you can search for.

If you are going to write your own cleanup tool, it is important that the `.nupkg.metadata` file is deleted if any of the other package files are deleted, so we recommend that this file is deleted first.
Otherwise projects referencing the package may have unexpected behavior.
If writing a cleanup tool in .NET, consider using `ConcurrencyUtilities.ExecuteWithFileLocked[Async](..)` from the [NuGet.Common package](https://www.nuget.org/packages/NuGet.Common), passing the full nupkg path of the package directory you're going to delete as the key, to avoid deleting a package that restore is trying to extract at the same time.
The global packages directory can be programmatically found with the [NuGet.Configuration package](https://www.nuget.org/packages/NuGet.Configuration).
Use `Settings.LoadDefaultSettings(path)` to get an `ISettings` instance (you can pass `null` as the path, or pass a directory if you want to handle solutions with a nuget.config that redirects the global-packages directory), and then use `SettingsUtility.GetGlobalPackagesFolder(settings)`.
Alternatively, you can run `dotnet nuget locals global-packages --list` as a child process and parse the output.

## http-cache

NuGet will cache copies of most NuGet feed communications (excluding search), organized into subdirectories for each package source.
Packages are not expanded, and files with a last modified date older than 30 minutes are typically considered expired.

## temp

A directory where NuGet may store temporary files during its various operations.

If multiple NuGet operations are performed in parallel, for example a single machine running multiple CI agents, it's important for all of the processes to share the same temp (`NuGetScratch`) directory.
NuGet uses the temp directory to coordinate inter-process access to the http-cache and global-packages directories, using filesystem locks.
If different processes use different temp directories, but the same global-packages or http-cache directory, various errors might occur when tying to restore or install packages.
NuGet does not use filesystem locking to coordinate restoring projects, so two different processes trying to restore the same project at the same time can encounter problems even when using the same `NuGetScratch` directory.

## plugin-cache

A directory where NuGet stores the results from the operation claims request.
See the [cross platform plugins reference](../reference/extensibility/NuGet-Cross-Platform-Plugins.md) for more information.

## Viewing directory locations

You can view directory locations using the [dotnet nuget locals command](/dotnet/core/tools/dotnet-nuget-locals):

```dotnetcli
dotnet nuget locals all --list
```

Typical output (Windows; "user1" is the current username):

```output
http-cache: C:\Users\user1\AppData\Local\NuGet\v3-cache
global-packages: C:\Users\user1\.nuget\packages\
temp: C:\Users\user1\AppData\Local\Temp\NuGetScratch
plugins-cache: C:\Users\user1\AppData\Local\NuGet\plugins-cache
```

Typical output (Mac; "user1" is the current username):

```output
info : http-cache: /home/user1/.local/share/NuGet/v3-cache
info : global-packages: /home/user1/.nuget/packages/
info : temp: /tmp/NuGetScratch
info : plugins-cache: /home/user1/.local/share/NuGet/plugins-cache
```

Typical output (Linux; "user1" is the current username):

```output
info : http-cache: /home/user1/.local/share/NuGet/v3-cache
info : global-packages: /home/user1/.nuget/packages/
info : temp: /tmp/NuGetScratchuser1
info : plugins-cache: /home/user1/.local/share/NuGet/plugins-cache
```

To display the location of a single folder, use `http-cache`, `global-packages`, `temp`, or `plugins-cache` instead of `all`.

You can also view locations with NuGet.exe with [the locals command](../reference/cli-reference/cli-ref-locals.md):

```cli
# Display locals for all directories: global-packages, http cache, temp and plugins cache
nuget locals all -list
```

## Clearing local directories

### Command-line

If you encounter package installation problems or otherwise want to ensure that you're installing packages from a remote gallery, use the `locals --clear` option (dotnet.exe) or `locals -clear` (nuget.exe), specifying the location to clear, or `all` to clear all directories:

```cli
# Clear the 3.x+ cache (use either command)
dotnet nuget locals http-cache --clear
nuget locals http-cache -clear

# Clear the 2.x cache (NuGet CLI 3.5 and earlier only)
nuget locals packages-cache -clear

# Clear the global packages directory (use either command)
dotnet nuget locals global-packages --clear
nuget locals global-packages -clear

# Clear the temporary cache (use either command)
dotnet nuget locals temp --clear
nuget locals temp -clear

# Clear the plugins cache (use either command)
dotnet nuget locals plugins-cache --clear
nuget locals plugins-cache -clear

# Clear all caches (use either command)
dotnet nuget locals all --clear
nuget locals all -clear
```

### Visual Studio

Visual Studio supports clearing all local directories in the "NuGet Package Manager" options found under the **Tools > NuGet Package Manager > Package Manager Settings** menu command.

On the General page, select **Clear NuGet local resources**.
Once started, this action cannot be cancelled.
A progress bar will be shown and will contain the final status of the command.

The [Output Window](/visualstudio/ide/output-window) when selecting Show output from "Package Manager" will show additional details about the clear command, including any error messages.

![Clear NuGet local resources button highlighted in the General page of NuGet options](media/vsoptions/general.png)

For more information, see [NuGet Options in Visual Studio](nuget-visual-studio-options.md#clear-nuget-local-resources).

## Troubleshooting errors

The following errors can occur when using `nuget locals` or `dotnet nuget locals`:

- *Error: The process cannot access the file \<package\> because it is being used by another process* or *Clearing local resources failed: Unable to delete one or more files*

    One or more files in the folder are in use by another process; for example, a Visual Studio project is open that refers to packages in the *global-packages* folder. Close those processes and try again.

- *Error: Access to the path \<path\> is denied* or *The directory is not empty*

    You don't have permission to delete files in the cache. Change the folder permissions, if possible, and try again. Otherwise, contact your system administrator.

- *Error: The specified path, file name, or both are too long. The fully qualified file name must be less than 260 characters, and the directory name must be less than 248 characters.*

    Shorten the directory names and try again.
