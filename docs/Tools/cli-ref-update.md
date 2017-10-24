---
# required metadata

title: NuGet CLI update command | Microsoft Docs
author: kraigb
ms.author: kraigb
manager: ghogen
ms.date: 10/24/2017
ms.topic: reference
ms.prod: nuget
ms.technology: null
ms.assetid: 61fde945-6983-46a5-8636-da0fada4e641

# optional metadata

description: Reference for the nuget.exe update command
keywords: nuget update reference, update package command
ms.reviewer:
- karann-msft
- unniravindranathan

---

# update command (NuGet CLI)

**Applicable roles**: Package consumption
**Supported versions**: All

Updates all packages in a project the latest available versions. It is recommended to run ['restore'](#restore) before running the `update`.

Note: `update` does not work with the CLI running under Mono (Mac OSX or Linux). The command also does not work with projects using `project.json`.

The `update` command also updates assembly references in the project file, provided those references already exist. If an updated package has an added assembly, a new reference is *not* added. New package dependencies also don't have their assembly references added. To include these operations as part of an update, update the package in Visual Studio using the Package Manager UI or the Package Manager Console.

This command can also be used to update nuget.exe itself using the *-self* flag.

## Usage

```
nuget update <configPath> [options]
```

where `<configPath>` identifies either a `packages.config` or solution file that lists the project's dependencies.

## Options

| Option | Description |
| --- | --- |
| ConfigFile | *(2.5+)* The NuGet configuration file to apply. If not specified, *%AppData%\NuGet\NuGet.Config* is used. |
| FileConflictAction | *(2.5+)* Specifies the action to take when asked to overwrite or ignore existing files referenced by the project. Values are *overwrite, ignore, none*. |
| ForceEnglishOutput | *(3.5+)* Forces nuget.exe to run using an invariant, English-based culture. |
| Help | Displays help information for the command. |
| Id | Specifies a list of package IDs to update. |
| MSBuildPath | *(4.0+)* Specifies the path of MSBuild to use with the command, taking precedence over `-MSBuildVersion`. |
| MSBuildVersion | *(3.2+)* Specifies the version of MSBuild to be used with this command. Supported values are 4, 12, 14, 15. By default the MSBuild in your path is picked, otherwise it defaults to the highest installed version of MSBuild. |
| NonInteractive | Suppresses prompts for user input or confirmations. |
| PreRelease | Allows updating to prerelease versions. This flag is not required when updating prerelease packages that are already installed. |
| RepositoryPath | Specifies the local folder where packages are installed. |
| Safe | Specifies that only updates with the highest version available within the same major and minor version as the installed package will be installed. |
| Self | *(1.4+)* Updates nuget.exe to the latest version; all other arguments are ignored. |
| Source | Specifies a list of package sources to use for the updates. |
| Verbosity | Specifies the amount of detail displayed in the output: *normal*, *quiet*, *detailed (2.5+)*. |
| Version | When used with one package ID, specifies the version of the package to update. |

Also see [Environment variables](cli-ref-environment-variables.md)

## Examples

```
nuget update

# update packages installed in solution.sln, using MSBuild version 14.0 to load the solution and its project(s).
nuget update solution.sln -MSBuildVersion 14

nuget update -safe

nuget update -self
```
