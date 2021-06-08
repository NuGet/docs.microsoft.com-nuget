---
title: NuGet CLI update command
description: Reference for the nuget.exe update command
author: JonDouglas
ms.author: jodou
ms.date: 12/07/2017
ms.topic: reference
---

# update command (NuGet CLI)

**Applies to:** package consumption &bullet; **Supported versions:** all

Updates all packages in a project (using `packages.config`) to their latest available versions. It is recommended to run ['restore'](cli-ref-restore.md) before running the `update`. (To update an individual package, use [`nuget install`](cli-ref-install.md) without specifying a version number, in which case NuGet installs the latest version.)

Note: `update` does not work with the CLI running under Mono (Mac OSX or Linux) or when using the PackageReference format.

The `update` command also updates assembly references in the project file, provided those references already exist. If an updated package has an added assembly, a new reference is *not* added. New package dependencies also don't have their assembly references added. To include these operations as part of an update, update the package in Visual Studio using the Package Manager UI or the Package Manager Console.

This command can also be used to update nuget.exe itself using the *-self* flag.

## Usage

```cli
nuget update <configPath> [options]
```

where `<configPath>` identifies either a `packages.config` or solution file that lists the project's dependencies.

## Options

- **`-ConfigFile`**

  The NuGet configuration file to apply. If not specified, `%AppData%\NuGet\NuGet.Config` (Windows), or `~/.nuget/NuGet/NuGet.Config` or `~/.config/NuGet/NuGet.Config` (Mac/Linux) is used.
  
- **`-DependencyVersion [Lowest, HighestPatch, HighestMinor, Highest, Ignore]`**

  Specifies the version of the dependency packages to use, which can be one of the following:<br/><ul><li>*Lowest* (default): the lowest version</li><li>*HighestPatch*: the version with the lowest major, lowest minor, highest patch</li><li>*HighestMinor*: the version with the lowest major, highest minor, highest patch</li><li>*Highest*: the highest version</li><li>*Ignore*: No dependency packages will be used</li></ul>

- **`-FileConflictAction [PromptUser, Overwrite, Ignore]`**

  Specifies the default action when a file from a package already exists in the target project. Set to `Overwrite` to always overwrite files. Set to `Ignore` to skip files.

  The `PromptUser` action, the default, will prompt for each conflicting file unless `OverwriteAll` or `IgnoreAll` is provided, which will apply to all remaining files.

- **`-ForceEnglishOutput`**

  *(3.5+)* Forces nuget.exe to run using an invariant, English-based culture.

- **`-?|-help`**

  Displays help information for the command.

- **`-Id`**

  Specifies a list of package IDs to update.

- **`-MSBuildPath`**

  *(4.0+)* Specifies the path of MSBuild to use with the command, taking precedence over `-MSBuildVersion`.

- **`-MSBuildVersion`**

  *(3.2+)* Specifies the version of MSBuild to be used with this command. Supported values are 4, 12, 14, 15.1, 15.3, 15.4, 15.5, 15.6, 15.7, 15.8, 15.9. By default the MSBuild in your path is picked, otherwise it defaults to the highest installed version of MSBuild.

- **`-NonInteractive`**

  Suppresses prompts for user input or confirmations.

- **`-PreRelease`**

  Allows updating to prerelease versions. This flag is not required when updating prerelease packages that are already installed.

- **`-RepositoryPath`**

  Specifies the local folder where packages are installed.

- **`-Safe`**

  Specifies that only updates with the highest version available within the same major and minor version as the installed package will be installed.

- **`-Self`**

  Updates `nuget.exe` to the latest version. `-Source` can be used however all other arguments are ignored. If no source is provided, checks `nuget.org` for updates regardless of `NuGet.Config` settings.

- **`-Source`**

  Specifies the list of package sources (as URLs) to use for the updates. If omitted, the command uses the sources provided in configuration files, see [Common NuGet configurations](../../consume-packages/configuring-nuget-behavior.md).

- **`-Verbosity [normal|quiet|detailed]`**

  Specifies the amount of detail displayed in the output: `normal` (the default), `quiet`, or `detailed`.

- **`-Version`**

  When used with one package ID, specifies the version of the package to update.

Also see [Environment variables](cli-ref-environment-variables.md)

## Examples

```cli
nuget update

# update packages installed in solution.sln, using MSBuild version 14.0 to load the solution and its project(s).
nuget update solution.sln -MSBuildVersion 14

nuget update -safe

nuget update -self
```
