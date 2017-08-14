---
# required metadata

title: Configuring the behavior of NuGet | Microsoft Docs
author: kraigb
ms.author: kraigb
manager: ghogen
ms.date: 7/25/2017
ms.topic: article
ms.prod: nuget
#ms.service:
ms.technology: null
ms.assetid: c1e34826-d07d-4609-a0fd-123459ae89c5

# optional metadata

description: NuGet.Config files control NuGet's behavior both globally and on a per-project basis, and are modified with nuget config command.
keywords: NuGet config files, NuGet configuration, NuGet behavior settings, NuGet settings, Nuget.Config, NuGetDefaults.Config, defaults
#ROBOTS:
#audience:
#ms.devlang:
ms.reviewer:
- karann
- unnir
#ms.suite:
#ms.tgt_pltfrm:
#ms.custom:

---

# Configuring NuGet behavior

NuGet's `NuGet.Config` files (in XML) store configuration settings and allow for changing default configuration values. NuGet uses several global configuration files and any number of configuration files within and near to a project to determine its exact behavior. In addition, in NuGet 2.7 and later you can use the `NuGetDefaults.Config` file to also specifically control package sources.

In this topic:

- [File locations and uses](#config-file-locations-and-uses)
- [Changing settings](#changing-config-settings)
- [How settings are applied](#how-settings-are-applied)
- [NuGet defaults file](#nuget-defaults-file)

## Config file locations and uses

The behavior of every NuGet command, whether issued from the command line, the Package Manager UI, or the Package Manager Console, is driven by the accumulated settings from any number of `NuGet.Config` files:

- Project-specific `NuGet.Config` files located in any folder from the solution folder up to the drive root. These allow control over settings as they apply to a project or a group of projects.
- A solution-specific `NuGet.Config` file located within a `.nuget` folder in the solution. Settings in this file apply only to solution-wide packages and is supported only in NuGet 3.3 and earlier. It is ignored for NuGet 3.4 and later.
- The global config file located in `%APPDATA%\NuGet\NuGet.Config`, which is always used unless you specify a different config file using the `-configFile` switch on any NuGet command. For example, `nuget restore -configfile c:\my.config` uses settings from `c:\my.config` for the command and ignores any settings in the global config file.
- Additional machine-wide config files:
    - NuGet 2.6 to NuGet 3.5: located in `%ProgramData%\NuGet\Config[\{IDE}[\{Version}[\{SKU}\]]]NuGet.Config`, where `{IDE}` can be `VisualStudio`, `{Version}` can be the Visual Studio version such as `14.0`, and `{SKU}` is either `Community`, `Pro`, or `Enterprise`. These variants allow you to create configurations that are specific to different versions and editions of Visual Studio if needs be.
    - NuGet 4.0+: located under `%ProgramFiles(x86)%\NuGet\Config` rather than `%ProgramData%`.
- The "default" file:
    - NuGet 2.7 to NuGet 3.5: located at `%PROGRAMDATA%\NuGet\NuGetDefaults.Config`, which is described later under [NuGet defaults file](#nuget-defaults-file) as a way to specifically enable and disable package sources. No other settings are supported in this file.
    - NuGet 4.0+: located under `%ProgramFiles(x86)%\NuGet\Config` rather than `%ProgramData%`.

> [!Note]
> The `%ProgramData%\NuGet` and `%ProgramFiles(x86)%\NuGet` folders typically requires Administrator permissions to modify. Administrators are expected to set the correct permissions on these folders based on the user and/or machine information.

## Changing config settings

A configuration file is a simple XML text file containing settings as described in the [NuGet Configuration Settings](../Schema/nuget-config-file.md) topic.

The preferred method for changing the configuration is using the NuGet [config command](../tools/nuget-exe-cli-reference.md#config) to set a key and value.

> [!Note]
> Keys are always case sensitive.

To **set a value** in any existing configuration file, use the `-configFile` switch as shown in the examples below.

```
nuget config -set repositoryPath=c:\packages -configfile c:\my.config
nuget config -set repositoryPath=c:\packages -configfile .\myApp\NuGet.Config
nuget config -set repositoryPath=c:\packages -configfile %ProgramData%\NuGet\Config\VisualStudio\14.0\NuGet.Config
nuget config -set repositoryPath=c:\packages -configfile %ProgramData%\NuGet\NuGetDefaults.Config
```

Without the `-configFile` switch, NuGet makes the change in the global config file.

> [!Note]
> In NuGet 3.4 and later you can use environment variables in any value, as in `repositoryPath=%PACKAGEHOME%`.

To **remove a value**, use the same commands but with an empty value, such as:

```
nuget config -set repositoryPath= -configfile c:\my.config
```

To create a new configuration file, copy the template below into that file and then use the `nuget config --configFile <filename>` command to set values:

```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
</configuration>
```
<br/>

> [!Warning]
> Although you can modify the file in any text editor, NuGet (v3.4.3 and later) silently ignores the entire configuration file if it contains malformed XML (mismatched tags, invalid quotation marks, etc.).

## How settings are applied

As described above in [Config file locations and uses](#config-file-locations-and-uses), any number of `NuGet.Config` files can exist on a machine, both in global locations and within a project's folder structure. This allows you to control settings in different places as they apply to a project, a group of projects, or all projects.

> [!Note]
> This process is sometimes referred to as "chaining" of config file, and the ability to insert a specific setting anywhere along the chain may be referred to as a "NuGet extensibility point."

When `nuget.exe` is run from the command line or run implicitly within Visual Studio, it loads settings from config files in the following order:

1. Files in `%ProgramData%\NuGet\Config` (NuGet 3.5 and earlier) or `%ProgramFiles(x86)%\NuGet\Config` (NuGet 4.0+) starting at this top folder and iterating down through the `{IDE}\{Version}\{SKU}\` subfolders if they exist.
1. The global config file, except in NuGet 3.4 and later if `-configFile` is used on the command line.
1. The file specified with `-configFile`.
1. Files found in the path to the current folder (where nuget.exe is invoked or the folder containing the Visual Studio solution), starting from the root and ending in the current folder.

As settings are found in these files, they are processed as follows:

1. For single-item elements, the last value found for any given key is used. This means that those settings that are "closest" to where NuGet was invoked is applied.

1. For collection elements (such as `<packageSources>`), values from all configuration files are added to the collection.

1. When `<clear />` is present for a given node, previously defined configuration values for that node are ignored.

See [the example](#settings-walkthrough) below for a walkthrough of this process.

Note also that the [NuGetDefaults.Config file](#nuget-defaults-file) can affect a few specific settings related to package sources, but does not have an effect on any other NuGet behavior.

### Priority ordering

It can also help to think about the "priority order" in which settings are applied, which is essentially the reverse of the processing order. For example, if a project is located in `c:\A\B\C`, then NuGet applies settings in the following priority order, meaning settings found higher up in the order win:

    (For solution-level packages only in NuGet 2.x; ignored in NuGet 3.x)
    c:\A\B\C\.nuget\NuGet.Config

    (For all version of NuGet)
    c:\A\B\C\NuGet.Config
    c:\A\B\NuGet.Config
    c:\A\NuGet.Config
    c:\NuGet.Config

    configFile, if specified

    (Ignored in NuGet 3.4 and later if -configFile is used)
    %AppData%\NuGet\NuGet.Config

    (NuGet 2.6 to 3.5)
    %ProgramData%\NuGet\Config\{IDE}\{Version}\{SKU}\NuGet.Config
    %ProgramData%\NuGet\Config\{IDE}\{Version}\NuGet.Config
    %ProgramData%\NuGet\Config\{IDE}\NuGet.Config
    %ProgramData%\NuGet\Config\NuGet.Config

    (NuGet 4.0+)
    %ProgramFiles(x86)%\NuGet\Config\{IDE}\{Version}\{SKU}\NuGet.Config
    %ProgramFiles(x86)%\NuGet\Config\{IDE}\{Version}\NuGet.Config
    %ProgramFiles(x86)%\NuGet\Config\{IDE}\NuGet.Config
    %ProgramFiles(x86)%\NuGet\Config\NuGet.Config

### Settings walkthrough

Let's say you have the following folder structure:

    c:\
       Users
    d:\
       Project1
         Source
       Project2
         Source
       tmp

You then have four `NuGet.Config` files in the following locations with the given content:

File A. Global configuration file, `%APPDATA%\NuGet\Nuget.Config`:

```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
    <activePackageSource>
    <add key="NuGet official package source" value="https://nuget.org/api/v2/" />
    </activePackageSource>
</configuration>
```

File B. `d:\NuGet.Config`:

```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
<config>
    <add key="repositoryPath" value="d:\tmp" />
    </config>
    <packageRestore>
    <add key="enabled" value="True" />
    </packageRestore>
</configuration>
```

File C. `d:\Project1\NuGet.Config`:

```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
    <config>
    <add key="repositoryPath" value="External\Packages" />
    <add key="DefaultPushSource" value="https://MyPrivateRepo/ES/api/v2/package" />
    </config>
    <packageSources>
    <clear /> <!-- ensure only the sources defined below are used -->
    <add key="MyPrivateRepo - ES" value="https://MyPrivateRepo/ES/nuget" />
    </packageSources>
</configuration>
```

File D. `d:\Project2\NuGet.Config`:

```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
    <packageSources>
    <!-- Add this repository to the list of available repositories -->
    <add key="MyPrivateRepo - DQ" value="https://MyPrivateRepo/DQ/nuget" />
    </packageSources>
</configuration>
```

Here's how NuGet loads and apply the settings, depending on where it's invoked:

- **Invoked from c:\users**: Only the default repository listed in the global configuration file (A) is used, because that's the only file found on the `c:` drive.

- **Invoked from d:\ or d:\tmp**: File (A) is loaded first, then NuGet goes to the root of `d:` and finds file (B). NuGet also looks for a configuration file in `d:\tmp` but does not find one. As a result, the default repository on nuget.org is used, package restore is enabled, and packages get expanded in `d:\tmp`.

- **Invoked from d:\Project1 or d:\Project1\Source**: File (A) is loaded first, then NuGet loads file (B) from the `d:` root, followed by file (C). Settings in (C) override those in (B) and (A), so the `repositoryPath` where packages get installed is `d:\Project1\External\Packages` instead of `d:\tmp`. Also, because (C) clears `<packageSources>`, nuget.org is no longer available a source leaving only `https://MyPrivateRepo/ES/nuget`.

- **Invoked from d:\Project2 or d:\Project2\Source**: File (A) is loaded first followed by file (B) and file (D). Because `packageSources` is not cleared, both `nuget.org` and `https://MyPrivateRepo/DQ/nuget` are available as sources. Packages get expanded in `d:\tmp` as specified in (B).

## NuGet defaults file

NuGet 3.3 and earlier is hardcoded to use nuget.org as the default package source, which cannot be changed. However, when using NuGet with internal package sources, it's often desirable to make sure that developers and build servers use packages from those sources instead of nuget.org. With NuGet 2.7 to 3.5, you can use the `%ProgramData%\NuGet\NuGetDefaults.Config` file (or `%ProgramFiles(x86)%\NuGet\NuGetDefaults.Config` with NuGet 4.0+) to control certain machine-wide defaults described below. This provides administrators a convenient way (using Group Policy, for example) to deploy consistent `NuGetDefaults.Config` files to developer and build machines, thus ensuring correct use of package sources.

Note that NuGet 3.4 and later does not use a hardcoded default source. When nuget.exe is run for the first time, `NuGet.Config` is generated and nuget.org is added as the default package source in the config file. In the absence of the `NuGet.Config` file, the default package source is undefined.

The defaults file works with the following settings:

- `packageSources`: this collection has the same meaning as `packageSources` in regular config files and specifies the default sources in their preferred order. If this setting exists in `NuGetDefaults.Config`, then NuGet doesn't use nuget.org as a default package source. An administrator can thus make sure that everyone using this file is working with the same sources and avoids using nuget.org if desired.
 
- `disabledPackageSources`: this collection also has the same meaning as in `NuGet.Config` files, where each affected source is listed by its name and a true/false value indicating whether it's disabled. This allows the source name and URL to remain in `packageSources` without it being turned on by default. Individual developers can then re-enable the source by setting the source's value to false in other `NuGet.Config` files without having to find the correct URL again. This is also useful to supply developers with a full list of internal source URLs for an organization while enabling only an individual team's source by default.

- `defaultPushSource`: when publishing a package, nuget.org is used as the destination or "push source" unless another is specified, which risks developers accidentally pushing internal packages to a public site. By changing the default push source, administrators can ensure that packages stay internal unless published to nuget.org explicitly.

> [!Note]
> The `NuGetDefaults.Config` file never causes a package source to be removed from a developer's NuGet configuration. That means if the developer has already used NuGet and therefore has the nuget.org package source registered, it won't be removed after the creation of a `NuGetDefaults.Config` file.
>
> Furthermore, neither `NuGetDefaults.Config` or any other mechanism in NuGet can prevent access to package sources like nuget.org. If an organization wishes to block such access, it much use other means such as firewalls to do so.

## Example NuGetDefaults.Config and application

The following is an example `NuGetDefaults.Config` file containing each of its allowable sections:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<configuration>
    <!-- DefaultPushSource key is similar to the 'DefaultPushSource' key of NuGet.Config schema-->
    <!-- This can be used by administrators to prevent accidental publishing of packages to nuget.org -->
    <config>
        <add key="DefaultPushSource" value="https://contoso.com/packages/" />
    </config>

    <!-- Default Package Sources -->
    <!-- They cannot be deleted or modified but can be disabled/enabled by user -->
    <!-- The following section is similar to 'packageSources' section of NuGet.Config schema -->
    <packageSources>
        <add key="Contoso Package Source" value="https://contoso.com/packages/" />
        <add key="nuget.org" value="https://www.nuget.org/api/v2/" />
    </packageSources>

    <!-- Default Package Sources that are disabled by default -->
    <!-- They cannot be modified or deleted either but can be enabled/disabled by user -->
    <!-- The following section is similar to 'disabledPackageSources' section of NuGet.Config schema -->
    <!-- The value part of the entry in 'disabledPackageSources' section has no effect -->
    <disabledPackageSources>
        <add key="nuget.org" value="true" />
    </disabledPackageSources>
</configuration>
```
