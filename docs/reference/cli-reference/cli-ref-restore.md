---
title: NuGet CLI restore command
description: Reference for the nuget.exe restore command
author: JonDouglas
ms.author: jodou
ms.date: 01/18/2018
ms.topic: reference
---

# restore command (NuGet CLI)

**Applies to:** package consumption &bullet; **Supported versions:** 2.7+

Downloads and installs any packages missing from the `packages` folder. When used with NuGet 4.0+ and the PackageReference format, generates a `<project>.nuget.props` file, if needed, in the `obj` folder. (The file can be omitted from source control.)

On Mac OSX and Linux with the CLI on Mono, restoring packages is not supported with PackageReference.

## Usage

```cli
nuget restore <projectPath> [options]
```

where `<projectPath>` specifies the location of a solution or a `packages.config` file. See [Remarks](#remarks) below for behavioral details.

## Options

- **`-ConfigFile`**

  The NuGet configuration file to apply. If not specified, `%AppData%\NuGet\NuGet.Config` (Windows), or `~/.nuget/NuGet/NuGet.Config` or `~/.config/NuGet/NuGet.Config` (Mac/Linux) is used.

- **`-DirectDownload`**

  *(4.0+)* Downloads packages directly without populating caches with any binaries or metadata.

- **`-DisableParallelProcessing`**

   Disables restoring multiple packages in parallel.

- **`-FallbackSource`**

  *(3.2+)* A list of package sources to use as fallbacks in case the package isn't found in the primary or default source. Use a semicolon to separate list entries.

- **`-Force`**

  In PackageReference based projects, forces all dependencies to be resolved even if the last restore was successful. Specifying this flag is similar to deleting the `project.assets.json` file. This does not bypass the http-cache.

- **`-ForceEnglishOutput`**

  *(3.5+)* Forces nuget.exe to run using an invariant, English-based culture.

- **`-ForceEvaluate`**

  Forces restore to reevaluate all dependencies even if a lock file already exists.

- **`-?|-help`**

  Displays help information for the command.

- **`-LockFilePath`**

  Output location where project lock file is written. By default, this is `PROJECT_ROOT\packages.lock.json`.

- **`-LockedMode`**

  Don't allow updating project lock file.

- **`-MSBuildPath`**

   *(4.0+)* Specifies the path of MSBuild to use with the command, taking precedence over `-MSBuildVersion`.

- **`-MSBuildVersion`**

  *(3.2+)* Specifies the version of MSBuild to be used with this command. Supported values are 4, 12, 14, 15.1, 15.3, 15.4, 15.5, 15.6, 15.7, 15.8, 15.9. By default the MSBuild in your path is picked, otherwise it defaults to the highest installed version of MSBuild.

- **`-NoHttpCache`**

  Prevents NuGet from using http cached packages. See [Managing the global packages and cache folders](../../consume-packages/managing-the-global-packages-and-cache-folders.md).

- **`-NonInteractive`**

  Suppresses prompts for user input or confirmations.

- **`-OutputDirectory`**

  Specifies the folder in which packages are installed. If no folder is specified, the current folder is used. Required when restoring with a `packages.config` file unless `PackagesDirectory` or `SolutionDirectory` is used.

- **`-PackageSaveMode`**

  Specifies the types of files to save after package installation: one of `nuspec`, `nupkg`, or `nuspec;nupkg`.

- **`-PackagesDirectory`**

  Same as `OutputDirectory`. Required when restoring with a `packages.config` file unless `OutputDirectory` or `SolutionDirectory` is used.

- **`-Project2ProjectTimeOut`**

  Timeout in seconds for resolving project-to-project references.

- **`-Recursive`**

  *(4.0+)* Restores all references projects for UWP and .NET Core projects. Does not apply to projects using `packages.config`.

- **`-RequireConsent`**

  Verifies that restoring packages is enabled before downloading and installing the packages. For details, see [Package Restore](../../consume-packages/package-restore.md).

- **`-SolutionDirectory`**

  Specifies the solution folder. Not valid when restoring packages for a solution. Required when restoring with a `packages.config` file unless `PackagesDirectory` or `OutputDirectory` is used.

- **`-Source`**

  Specifies the list of package sources (as URLs) to use for the restore. If omitted, the command uses the sources provided in configuration files, see [Configuring NuGet behavior](../../consume-packages/configuring-nuget-behavior.md). Use a semicolon to separate list entries.

- **`-UseLockFile`**

  Enables project lock file to be generated and used with restore.

- **`-Verbosity [normal|quiet|detailed]`**

  Specifies the amount of detail displayed in the output: `normal` (the default), `quiet`, or `detailed`.

Also see [Environment variables](cli-ref-environment-variables.md)

## Remarks

The restore command performs the following steps:

1. Determine the operation mode of the restore command.

   | projectPath file type | Behavior |
   | --- | --- |
   | Solution (folder) | NuGet looks for a `.sln` file and uses that if found; otherwise gives an error. `(SolutionDir)\.nuget` is used as the starting folder. |
   | `.sln` file | Restore packages identified by the solution; gives an error if `-SolutionDirectory` is used. `$(SolutionDir)\.nuget` is used as the starting folder. |
   | `packages.config` or project file | Restore packages listed in the file, resolving and installing dependencies. |
   | Other file type | File is assumed to be a `.sln` file as above; if it's not a solution, NuGet gives an error. |
   | (projectPath not specified) | <ul><li>NuGet looks for solution files in the current folder. If a single file is found, that one is used to restore packages; if multiple solutions are found, NuGet gives an error.</li><li>If there are no solution files, NuGet looks for a `packages.config` and uses that to restore packages.</li><li>If no solution or `packages.config` file is found, NuGet gives an error.</ul> |

2. Determine the packages folder using the following priority order (NuGet gives an error if none of these folders are found):

    - The folder specified with `-PackagesDirectory`.
    - The `repositoryPath` value in `Nuget.Config`
    - The folder specified with `-SolutionDirectory`
    - `$(SolutionDir)\packages`

3. When restoring packages for a solution, NuGet does the following:
    - Loads the solution file.
    - Restores solution level packages listed in `$(SolutionDir)\.nuget\packages.config` into the `packages` folder.
    - Restore packages listed in `$(ProjectDir)\packages.config` into the `packages` folder. For each package specified, restore the package in parallel unless `-DisableParallelProcessing` is specified.

## Examples

```cli
# Restore packages for a solution file
nuget restore a.sln

# Restore packages for a solution file, using MSBuild version 14.0 to load the solution and its project(s)
nuget restore a.sln -MSBuildVersion 14

# Restore packages for a project's packages.config file, with the packages folder at the parent
nuget restore proj1\packages.config -PackagesDirectory ..\packages

# Restore packages for the solution in the current folder, specifying package sources
nuget restore -source "https://api.nuget.org/v3/index.json;https://www.myget.org/F/nuget"
```
