---
title: Common NuGet configurations
description: NuGet.Config files control NuGet's behavior both globally and on a per-project basis, and are modified with nuget config command.
author: JonDouglas
ms.author: jodou
ms.date: 01/10/2022
ms.topic: conceptual
---

# Common NuGet configurations

NuGet's behavior is driven by the accumulated settings in one or more `NuGet.Config` (XML) files that can exist at project-, user-, and computer-wide levels. A global `NuGetDefaults.Config` file also specifically configures package sources. Settings apply to all commands issued in the CLI, the Package Manager Console, and the Package Manager UI.

## Config file locations and uses

| Scope | `NuGet.Config` file location | Description |
| --- | --- | --- |
| Solution | Current folder (aka Solution folder) or any folder up to the drive root.| In a solution folder, settings apply to all projects in subfolders. Note that if a config file is placed in a project folder, it has no effect on that project. |
| User | **Windows:** `%appdata%\NuGet\NuGet.Config`<br/>**Mac/Linux:** `~/.config/NuGet/NuGet.Config` or `~/.nuget/NuGet/NuGet.Config` (varies by tooling) <br/>Additional configs are supported on all platforms. These configs cannot be edited by the tooling. </br> **Windows:** `%appdata%\NuGet\config\*.Config` <br/>**Mac/Linux:** `~/.config/NuGet/config/*.config` or `~/.nuget/config/*.config` | Settings apply to all operations, but are overridden by any project-level settings. |
| Computer | **Windows:** `%ProgramFiles(x86)%\NuGet\Config`<br/>**Mac/Linux:** `$XDG_DATA_HOME`. If `$XDG_DATA_HOME` is null or empty, `~/.local/share` or `/usr/local/share` will be used (varies by OS distribution)  | Settings apply to all operations on the computer, but are overridden by any user- or project-level settings. |

> [!Note]
> On Mac/Linux, the user config file location varies by tooling. .NET CLI uses `~/.nuget/NuGet` folder, while Mono uses `~/.config/NuGet` folder. 

### On Mac/Linux, the user-level config file location varies by tooling
On Mac/Linux, the user config file location varies by tooling. 
Majority of users use tools that look for the user config file under the `~/.nuget/NuGet` folder. 
These other tools look for the user config file under the `~/.config/NuGet` folder:
* Mono
* NuGet.exe
* Visual Studio 2019 for Mac (and earlier versions)
* Visual Studio 2022 for Mac (and later versions), only when working on classic Mono projects.

If the tooling you use involves both locations, consider consolidating them by following these steps to allow you to work with only one user-level config file:  
1. Check the contents of the two user-level config files and keep the one you want under `~/.nuget/NuGet` folder. 
2. Set symbolic link from `~/.nuget/NuGet` to `~/.config/Nuget`. E.g. Run bash command: `ln -s ~/.nuget/NuGet/NuGet.Config ~/.config/NuGet/NuGet.Config`.


Notes for earlier versions of NuGet:
- NuGet 3.3 and earlier used a `.nuget` folder for solution-wide settings. This folder is not used in NuGet 3.4+.
- For NuGet 2.6 to 3.x, the computer-level config file on Windows was located in `%ProgramData%\NuGet\Config[\{IDE}[\{Version}[\{SKU}]]]\NuGet.Config`, where `{IDE}` can be `VisualStudio`, `{Version}` was the Visual Studio version such as `14.0`, and `{SKU}` is either `Community`, `Pro`, or `Enterprise`. To migrate settings to NuGet 4.0+, simply copy the config file to `%ProgramFiles(x86)%\NuGet\Config`. On Linux, this previous location was `/etc/opt`, and on Mac, `/Library/Application Support`.

## Changing config settings

A `NuGet.Config` file is a simple XML text file containing key/value pairs as described in the [NuGet Configuration Settings](../reference/nuget-config-file.md) topic.

Settings are managed using the NuGet CLI [config command](../reference/cli-reference/cli-ref-config.md):
- By default, changes are made to the user-level config file. (On Mac/Linux, the location of user-level config file varies by tooling)
- To change settings in a different file, use the `-configFile` switch. In this case files can use any filename.
- Keys are always case sensitive.
- Elevation is required to change settings in the computer-level settings file.

> [!Warning]
> Although you can modify the file in any text editor, NuGet (v3.4.3 and later) silently ignores the entire configuration file if it contains malformed XML (mismatched tags, invalid quotation marks, etc.). This is why it's preferable to manage setting using `nuget config`.

### Setting a value

Windows:

```cli
# Set repositoryPath in the user-level config file
nuget config -set repositoryPath=c:\packages 

# Set repositoryPath in project-level files
nuget config -set repositoryPath=c:\packages -configfile c:\my.Config
nuget config -set repositoryPath=c:\packages -configfile .\myApp\NuGet.Config

# Set repositoryPath in the computer-level file (requires elevation)
nuget config -set repositoryPath=c:\packages -configfile %ProgramFiles(x86)%\NuGet\Config\NuGet.Config
```

Mac/Linux:

```cli
# Set repositoryPath in the user-level config file
nuget config -set repositoryPath=/home/packages 

# Set repositoryPath in project-level files
nuget config -set repositoryPath=/home/projects/packages -configfile /home/my.Config
nuget config -set repositoryPath=/home/packages -configfile home/myApp/NuGet.Config

# Set repositoryPath in the computer-level file (requires elevation)
nuget config -set repositoryPath=/home/packages -configfile $XDG_DATA_HOME/NuGet.Config
```

> [!Note]
> In NuGet 3.4 and later you can use environment variables in any value, as in `repositoryPath=%PACKAGEHOME%` (Windows) and `repositoryPath=$PACKAGEHOME` (Mac/Linux).

### Removing a value

To remove a value, specify a key with an empty value.

```cli
# Windows
nuget config -set repositoryPath= -configfile c:\my.Config

# Mac/Linux
nuget config -set repositoryPath= -configfile /home/my.Config
```

### Creating a new config file

Copy the template below into the new file and then use `nuget config -configFile <filename>` to set values:

```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
</configuration>
```

## How settings are applied

Multiple `NuGet.Config` files allow you to store settings in different locations so that they apply to a single project, a group of projects, or all projects. These settings collectively apply to any NuGet operation invoked from the command line or from Visual Studio, with settings that exist "closest" to a project or the current folder taking precedence.

Specifically, NuGet loads settings from the different config files in the following order:

1. The [`NuGetDefaults.Config` file](#nuget-defaults-file), which contains settings related only to package sources.
1. The computer-level file.
1. The user-level file.
1. The file specified with `-configFile`.
1. Files found in every folder in the path from the drive root to the current folder (where `nuget.exe` is invoked or the folder containing the Visual Studio project). For example, if a command is invoked in `c:\A\B\C`, NuGet looks for and loads config files in `c:\`, then `c:\A`, then `c:\A\B`, and finally `c:\A\B\C`.

As NuGet finds settings in these files, they are applied as follows:

1. For single-item elements, NuGet replaced any previously-found value for the same key. This means that settings that are "closest" to the current folder or project override any others found earlier. For example, the `defaultPushSource` setting in `NuGetDefaults.Config` is overridden if it exists in any other config file.
1. For collection elements (such as `<packageSources>`), NuGet combines the values from all configuration files into a single collection.
1. When `<clear />` is present for a given node, NuGet ignores previously defined configuration values for that node.

> [!Tip]
> Add a `nuget.config` file in the root of your project repository. This is considered a best practice as it promotes repeatability and ensures that different users have the same NuGet configuration.

### Settings walkthrough

Let's say you have the following folder structure on two separate drives:

```
disk_drive_1
    User
disk_drive_2
    Project1
        Source
    Project2
        Source
    tmp
```

You then have four `NuGet.Config` files in the following locations with the given content. (The computer-level file is not included in this example, but would behave similarly to the user-level file.)

File A. User-level file, (`%appdata%\NuGet\NuGet.Config` on Windows, `~/.config/NuGet/NuGet.Config` on Mac/Linux):

```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
    <activePackageSource>
        <add key="NuGet official package source" value="https://api.nuget.org/v3/index.json" />
    </activePackageSource>
</configuration>
```

File B. `disk_drive_2/NuGet.Config`:

```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
    <config>
        <add key="repositoryPath" value="disk_drive_2/tmp" />
    </config>
    <packageRestore>
        <add key="enabled" value="True" />
    </packageRestore>
</configuration>
```

File C. `disk_drive_2/Project1/NuGet.Config`:

```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
    <config>
        <add key="repositoryPath" value="External/Packages" />
        <add key="defaultPushSource" value="https://MyPrivateRepo/ES/api/v2/package" />
    </config>
    <packageSources>
        <clear /> <!-- ensure only the sources defined below are used -->
        <add key="MyPrivateRepo - ES" value="https://MyPrivateRepo/ES/nuget" />
    </packageSources>
</configuration>
```

File D. `disk_drive_2/Project2/NuGet.Config`:

```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
    <packageSources>
        <!-- Add this repository to the list of available repositories -->
        <add key="MyPrivateRepo - DQ" value="https://MyPrivateRepo/DQ/nuget" />
    </packageSources>
</configuration>
```

NuGet then loads and applies settings as follows, depending on where it's invoked:

- **Invoked from `disk_drive_1/users`**: Only the default repository listed in the user-level configuration file (A) is used, because that's the only file found on `disk_drive_1`.

- **Invoked from `disk_drive_2/` or `disk_drive_/tmp`**: The user-level file (A) is loaded first, then NuGet goes to the root of `disk_drive_2` and finds file (B). NuGet also looks for a configuration file in `/tmp` but does not find one. As a result, the default repository on `nuget.org` is used, package restore is enabled, and packages get expanded in `disk_drive_2/tmp`.

- **Invoked from `disk_drive_2/Project1` or `disk_drive_2/Project1/Source`**: The user-level file (A) is loaded first, then NuGet loads file (B) from the root of `disk_drive_2`, followed by file (C). Settings in (C) override those in (B) and (A), so the `repositoryPath` where packages get installed is `disk_drive_2/Project1/External/Packages` instead of `disk_drive_2/tmp`. Also, because (C) clears `<packageSources>`, nuget.org is no longer available as a source leaving only `https://MyPrivateRepo/ES/nuget`.

- **Invoked from `disk_drive_2/Project2` or `disk_drive_2/Project2/Source`**: The user-level file (A) is loaded first followed by file (B) and file (D). Because `packageSources` is not cleared, both `nuget.org` and `https://MyPrivateRepo/DQ/nuget` are available as sources. Packages get expanded in `disk_drive_2/tmp` as specified in (B).

## Additional user wide configuration

Starting with 5.7, NuGet has added support for additional user wide configuration files. This allows third-party vendors to add additional user configuration files without elevation.
These configuration files are found in the standard user wide configuration folder within a `config` subfolder.
All files ending with `.config` or `.Config` will be considered.
These files cannot be edited by the standard tooling.

| OS Platform  | Additional Configurations |
| --- | --- |
| Windows      | `%appdata%\NuGet\config\*.Config` |
| Mac/Linux    | `~/.config/NuGet/config/*.config` or `~/.nuget/config/*.config` |

## NuGet defaults file

The `NuGetDefaults.Config` file exists to specify package sources from which packages are installed and updated, and to control the default target for publishing packages with `nuget push`. Because administrators can conveniently (using Group Policy, for example) deploy consistent `NuGetDefaults.Config` files to developer and build machines, they can ensure that everyone in the organization is using the correct package sources rather than nuget.org.

> [!Important]
> The `NuGetDefaults.Config` file never causes a package source to be removed from a developer's NuGet configuration. That means if the developer has already used NuGet and therefore has the nuget.org package source registered, it won't be removed after the creation of a `NuGetDefaults.Config` file.
>
> Furthermore, neither `NuGetDefaults.Config` nor any other mechanism in NuGet can prevent access to package sources like nuget.org. If an organization wishes to block such access, it must use other means such as firewalls to do so.

### `NuGetDefaults.Config` location

The following table describes where the `NuGetDefaults.Config` file should be stored, depending on the target OS:

| OS Platform  | `NuGetDefaults.Config` Location |
| --- | --- |
| Windows      | **Visual Studio 2017 or NuGet 4.x+:** `%ProgramFiles(x86)%\NuGet` <br />**Visual Studio 2015 and earlier or NuGet 3.x and earlier:** `%PROGRAMDATA%\NuGet` |
| Mac/Linux    | `$XDG_DATA_HOME` (typically `~/.local/share` or `/usr/local/share`, depending on OS distribution)|

### NuGetDefaults.Config settings

- `packageSources`: this collection has the same meaning as `packageSources` in regular config files and specifies the default sources. NuGet uses the sources in order when installing or updating packages in projects using the `packages.config` management format. For projects using the PackageReference format, NuGet uses local sources first, then sources on network shares, then HTTP sources, regardless of the order in the configuration files. NuGet always ignores the order of sources with restore operations.

- `disabledPackageSources`: this collection also has the same meaning as in `NuGet.Config` files, where each affected source is listed by its name and a `true`/`false` value indicating whether it's disabled. This allows the source name and URL to remain in `packageSources` without having it turned on by default. Individual developers can then re-enable the source by setting the source's value to `false` in other `NuGet.Config` files without having to find the correct URL again. This is also useful to supply developers with a full list of internal source URLs for an organization while enabling only an individual team's source by default.

- `defaultPushSource`: specifies the default target for `nuget push` operations, overriding the built-in default of `nuget.org`. Administrators can deploy this setting to avoid publishing internal packages to the public `nuget.org` by accident, as developers specifically need to use `nuget push -Source` to publish to `nuget.org`.

### Example NuGetDefaults.Config and application

```xml
<?xml version="1.0" encoding="UTF-8"?>
<configuration>
    <!-- defaultPushSource key works like the 'defaultPushSource' key of NuGet.Config files. -->
    <!-- This can be used by administrators to prevent accidental publishing of packages to nuget.org. -->
    <config>
        <add key="defaultPushSource" value="https://contoso.com/packages/" />
    </config>

    <!-- Default Package Sources; works like the 'packageSources' section of NuGet.Config files. -->
    <!-- This collection cannot be deleted or modified but can be disabled/enabled by users. -->
    <packageSources>
        <add key="Contoso Package Source" value="https://contoso.com/packages/" />
        <add key="nuget.org" value="https://api.nuget.org/v3/index.json" />
    </packageSources>

    <!-- Default Package Sources that are disabled by default. -->
    <!-- Works like the 'disabledPackageSources' section of NuGet.Config files. -->
    <!-- Sources cannot be modified or deleted either but can be enabled/disabled by users. -->
    <disabledPackageSources>
        <add key="nuget.org" value="true" />
    </disabledPackageSources>
</configuration>
```
