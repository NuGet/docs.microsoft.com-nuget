---
title: NuGet CLI install command | Microsoft Docs
author: kraigb
ms.author: kraigb
manager: ghogen
ms.date: 12/07/2017
ms.topic: reference
ms.prod: nuget
ms.technology: null
ms.assetid: 59ac622f-837c-4545-bc93-a56330e02d71
description: Reference for the nuget.exe install command
keywords: nuget install reference, install package command
ms.reviewer:
- karann-msft
- unniravindranathan
---

# install command (NuGet CLI)

**Applies to:** package consumption &bullet; **Supported versions:** all

Downloads and installs a package into a project, defaulting to the current folder, using specified package sources.

> [!Tip]
> To download a package directly outside the context of a project, visit the package's page on [nuget.org](https://www.nuget.org) and select the **Download** link. 

If no sources are specified, those listed in the global configuration file, `%APPDATA%\NuGet\NuGet.Config`, are used. See [Configuring NuGet Behavior](../consume-packages/configuring-nuget-behavior.md) for additional details.

If no specific packages are specified, `install` installs all packages listed in the project's `packages.config` file, making it similar to [`restore`](#restore). (The `install` command does not work with `project.json`.)

The `install` command does not modify a project file or `packages.config`; in this way it's similar to `restore` in that it only adds packages to disk but does not change a project's dependencies.

To add a dependency, either add a project through the Package Manager UI or Console in Visual Studio, or modify `packages.config` and then run either `install` or `restore`. For projects using `project.json`, you can modify that file and then run `restore`.

## Usage

```
nuget install <packageID | configFilePath> [options]
```

where `<packageID>` names the package to install (using the latest version), or `<configFilePath>` identifies the `packages.config` file that lists the packages to install. You can indicate a specific version with the `-Version` option.

## Options

| Option | Description |
| --- | --- |
| ConfigFile | *(2.5+)* The NuGet configuration file to apply. If not specified, *%AppData%\NuGet\NuGet.Config* is used. |
| DisableParallelProcessing | Disables installing multiple packages in parallel. |
| ExcludeVersion | Installs the package to a folder named with only the package name and not the version number. |
| FallbackSource | *(3.2+)* A list of package sources to use as fallbacks in case the package isn't found in the primary or default source. |
| ForceEnglishOutput | *(3.5+)* Forces nuget.exe to run using an invariant, English-based culture. |
| Framework | *(4.4+)* Target framework used for selecting dependencies. Defaults to 'Any' if not specified. |
| Help | Displays help information for the command. |
| NoCache | Prevents NuGet from using packages from local machine caches. |
| NonInteractive | Suppresses prompts for user input or confirmations. |
| OutputDirectory | Specifies the folder in which packages are installed. If no folder is specified, the current folder is used. |
| PackageSaveMode | Specifies the types of files to save after package installation: one of `nuspec`, `nupkg`, or `nuspec;nupkg`. |
| PreRelease | Allows prerelease packages to be installed. This flag is not required when restoring packages with `packages.config`. |
| RequireConsent | Verifies that restoring packages is enabled before downloading and installing the packages. For details, see [Package Restore](../consume-packages/package-restore.md). |
| SolutionDirectory | Specifies root folder of the solution for which to restore packages. |
| Source | Specifies list of package sources (as URLS) to use. If omitted, the command uses the sources provided in configuration files, see [Configuring NuGet behvior](../Consume-Packages/Configuring-NuGet-Behavior.md). |
| Verbosity | Specifies the amount of detail displayed in the output: *normal*, *quiet*, *detailed (2.5+)*. |
| Version | Specifies the version of the package to install. |

Also see [Environment variables](cli-ref-environment-variables.md)

## Examples

```
nuget install elmah

nuget install packages.config

nuget install ninject -OutputDirectory c:\proj
```
