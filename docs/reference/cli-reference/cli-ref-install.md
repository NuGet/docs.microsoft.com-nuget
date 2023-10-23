---
title: NuGet CLI install command
description: Reference for the nuget.exe install command
author: JonDouglas
ms.author: jodou
ms.date: 01/18/2018
ms.topic: reference
---

# install command (NuGet CLI)

**Applies to:** package consumption &bullet; **Supported versions:** all

Downloads and installs a package into a project, defaulting to the current folder, using specified package sources.

> [!Tip]
> To download a package directly outside the context of a project, visit the package's page on [nuget.org](https://www.nuget.org) and select the **Download** link.

If no sources are specified, those listed in the global configuration file, `%appdata%\NuGet\NuGet.Config` (Windows) or `~/.nuget/NuGet/NuGet.Config` (Mac/Linux), are used. See [Common NuGet configurations](../../consume-packages/configuring-nuget-behavior.md) for additional details.

If no specific packages are specified, `install` installs all packages listed in the project's `packages.config` file, making it similar to [`restore`](cli-ref-restore.md).

The `install` command does not modify a project file or `packages.config`; in this way it's similar to `restore` in that it only adds packages to disk but does not change a project's dependencies.

To add a dependency, either add a package through the Package Manager UI or Console in Visual Studio, or modify `packages.config` and then run either `install` or `restore`.

## Usage

```cli
nuget install <packageID | configFilePath> [options]
```

where `<packageID>` names the package to install (using the latest version), or `<configFilePath>` identifies the `packages.config` file that lists the packages to install. You can indicate a specific version with the `-Version` option.

## Options

- **`-ConfigFile`**

  The NuGet configuration file to apply. If not specified, `%AppData%\NuGet\NuGet.Config` (Windows), or `~/.nuget/NuGet/NuGet.Config` or `~/.config/NuGet/NuGet.Config` (Mac/Linux) is used.

- **`-DependencyVersion`**

  *(4.4+)* The version of the dependency packages to use, which can be one of the following:<br/><ul><li>*Lowest* (default): the lowest version</li><li>*HighestPatch*: the version with the lowest major, lowest minor, highest patch</li><li>*HighestMinor*: the version with the lowest major, highest minor, highest patch</li><li>*Highest*: the highest version</li><li>*Ignore*: No dependency packages will be used</li></ul>

- **`-DirectDownload`**

  Download directly without populating any caches with metadata or binaries.

- **`-DisableParallelProcessing`**

  Disables installing multiple packages in parallel.

- **`-x|-ExcludeVersion`**

  Installs the package to a folder named with only the package name and not the version number.

- **`-FallbackSource`**

  *(3.2+)* A list of package sources to use as fallbacks in case the package isn't found in the primary or default source.

- **`-ForceEnglishOutput`**

  *(3.5+)* Forces nuget.exe to run using an invariant, English-based culture.

- **`-Framework`**

  *(4.4+)* Target framework used for selecting dependencies. Defaults to 'Any' if not specified.

- **`-?|-help`**

  Displays help information for the command.

- **`-NoHttpCache`**

  Prevents NuGet from using http cached packages. See [Managing the global packages and cache folders](../../consume-packages/managing-the-global-packages-and-cache-folders.md).

- **`-NonInteractive`**

  Suppresses prompts for user input or confirmations.

- **`-OutputDirectory`**

  Specifies the folder in which packages are installed. If no folder is specified, the current folder is used.

- **`-PackageSaveMode`**

  Specifies the types of files to save after package installation: one of `nuspec`, `nupkg`, or `nuspec;nupkg`.

- **`-PreRelease`**

  Allows prerelease packages to be installed. This flag is not required when restoring packages with `packages.config`.

- **`-RequireConsent`**

  Verifies that restoring packages is enabled before downloading and installing the packages. For details, see [Package Restore](../../consume-packages/package-restore.md).

- **`-SolutionDirectory`**

  Specifies root folder of the solution for which to restore packages.

- **`-Source`**

   Specifies the list of package sources (as URLs) to use. If omitted, the command uses the sources provided in configuration files, see [Common NuGet configurations](../../consume-packages/configuring-nuget-behavior.md).

- **`-Verbosity [normal|quiet|detailed]`**

  Specifies the amount of detail displayed in the output: `normal` (the default), `quiet`, or `detailed`.

- **`-Version`**

  Specifies the version of the package to install.

Also see [Environment variables](cli-ref-environment-variables.md)

## Examples

```cli
nuget install elmah

nuget install packages.config

nuget install ninject -OutputDirectory c:\proj
```
