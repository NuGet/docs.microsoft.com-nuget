---
# required metadata

title: NuGet CLI restore command | Microsoft Docs
author: kraigb
ms.author: kraigb
manager: ghogen
ms.date: 10/24/2017
ms.topic: reference
ms.prod: nuget
ms.technology: null
ms.assetid: 6ee41020-e548-4e61-b8cd-c82b77ac6af7

# optional metadata

description: Reference for the nuget.exe restore command
keywords: nuget restore reference, restore packages command
ms.reviewer:
- karann-msft
- unniravindranathan

---

# restore command (NuGet CLI)

**Applies to:** package consumption &bullet; **Supported versions:** 2.7+

NuGet 2.7+: Downloads and installs any packages missing from the `packages` folder.

NuGet 3.3+ with projects using `project.json`: Generates a `project.lock.json` file and a `<project>.nuget.props` file, if needed. (Both files can be omitted from source control.)

NuGet 4.0+ with project in which package references are included in the project file directly: Generates a `<project>.nuget.props` file, if needed, in the `obj` folder. (The file can be omitted from source control.)

On Mac OSX and Linux with the CLI on Mono, restoring packages is not supported with the PackageReference format.

## Usage

```
nuget restore <projectPath> [options]
```

where `<projectPath>` specifies the location of a solution, a `packages.config` file, or a `project.json` file. See [Remarks](#remarks) below for behavioral details.

## Options

| Option | Description |
| --- | --- |
| ConfigFile | The NuGet configuration file to apply. If not specified, *%AppData%\NuGet\NuGet.Config* is used. |
| DirectDownload | *(4.0+)* Downloads packages directly without populating caches with any binaries or metadata. |
| DisableParallelProcessing | Disables restoring multiple packages in parallel. |
| FallbackSource | *(3.2+)* A list of package sources to use as fallbacks in case the package isn't found in the primary or default source. |
| ForceEnglishOutput | *(3.5+)* Forces nuget.exe to run using an invariant, English-based culture. |
| Help | Displays help information for the command. |
| MSBuildPath | *(4.0+)* Specifies the path of MSBuild to use with the command, taking precedence over `-MSBuildVersion`. |
| MSBuildVersion | *(3.2+)* Specifies the version of MSBuild to be used with this command. Supported values are 4, 12, 14, 15. By default the MSBuild in your path is picked, otherwise it defaults to the highest installed version of MSBuild. |
| NoCache | Prevents NuGet from using packages from local machine caches. |
| NonInteractive | Suppresses prompts for user input or confirmations. |
| OutputDirectory | Specifies the folder in which packages are installed. If no folder is specified, the current folder is used. |
| PackageSaveMode | Specifies the types of files to save after package installation: one of `nuspec`, `nupkg`, or `nuspec;nupkg`. |
| PackagesDirectory | Same as `OutputDirectory`. |
| Project2ProjectTimeOut | Timeout in seconds for resolving project-to-project references. |
| Recursive | *(4.0+)* Restores all references projects for UWP and .NET Core projects. Does not apply to projects using `packages.config`. |
| RequireConsent | Verifies that restoring packages is enabled before downloading and installing the packages. For details, see [Package Restore](../consume-packages/package-restore.md). |
| SolutionDirectory | Specifies the solution folder. Not valid when restoring packages for a solution. |
| Source | Specifies list of package sources (as URLS) to use for the restore. If omitted, the command uses the sources provided in configuration files, see [Configuring NuGet behvior](../Consume-Packages/Configuring-NuGet-Behavior.md). |
| Verbosity |>Specifies the amount of detail displayed in the output: *normal*, *quiet*, *detailed (2.5+)*. |

Also see [Environment variables](cli-ref-environment-variables.md)

## Remarks

The restore command performs the following steps:

1. Determine the operation mode of the restore command.
    projectPath file type | Behavior
    | --- | --- |
    Solution (folder) | NuGet looks for a `.sln` file and uses that if found; otherwise gives an error. `(SolutionDir)\.nuget` is used as the starting folder.
    `.sln` file | Restore packages identified by the solution; gives an error if `-SolutionDirectory` is used. `$(SolutionDir)\.nuget` is used as the starting folder.
    `packages.config`, `project.json`, or project file | Restore packages listed in the file, resolving and installing dependencies.
    Other file type | File is assumed to be a `.sln` file as above; if it's not a solution, NuGet gives an error.
    (projectPath not specified) | - NuGet looks for solution files in the current folder. If a single file is found, that one is used to restore packages; if multiple solutions are found, NuGet gives an error.
    |- If there are no solution files, NuGet looks for a `packages.config` or `project.json` and uses that to restore packages.
    |- If no solution file, `packages.config`, or `project.json` is found, NuGet gives an error.

1. Determine the packages folder using the following priority order (NuGet gives an error if none of these folders are found):

    - The `%userprofile%\.nuget\packages` value in `project.json`.
    - The folder specified with `-PackagesDirectory`.
    - The `repositoryPath` vale in `Nuget.Config`
    - The folder specified with `-SolutionDirectory`
    - `$(SolutionDir)\packages`

1. When restoring packages for a solution, NuGet does the following:
    - Loads the solution file.
    - Restores solution level packages listed in `$(SolutionDir)\.nuget\packages.config` into the `packages` folder.
    - Restore packages listed in `$(ProjectDir)\packages.config` into the `packages` folder. For each package specified, restore the package in parallel unless `-DisableParallelProcessing` is specified.

## Examples

```
# Restore packages for a solution file
nuget restore a.sln

# Restore packages for a solution file, using MSBuild version 14.0 to load the solution and its project(s)
nuget restore a.sln -MSBuildVersion 14

# Restore packages for a project's packages.config file, with the packages folder at the parent
nuget restore proj1\packages.config -PackagesDirectory ..\packages

# Restore packages for the solution in the current folder, specifying package sources
nuget restore -source "https://api.nuget.org/v3/index.json;https://www.myget.org/F/nuget"
```
