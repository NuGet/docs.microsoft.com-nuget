---
title: NuGet CLI pack command
description: Reference for the nuget.exe pack command
author: JonDouglas
ms.author: jodou
ms.date: 01/18/2018
ms.topic: reference
---

# pack command (NuGet CLI)

**Applies to:** package creation &bullet; **Supported versions:** 2.7+

Creates a NuGet package based on the specified [.nuspec](../nuspec.md) or project file. The `dotnet pack` command (see [dotnet Commands](../dotnet-Commands.md)) and `msbuild -t:pack` (see [MSBuild targets](../msbuild-targets.md)) may be used as alternates.

> [!Important]
> Use [`dotnet pack`](../dotnet-Commands.md) or [`msbuild -t:pack`](../msbuild-targets.md) for [PackageReference](../../consume-packages/package-references-in-project-files.md) based projects. Starting with NuGet version 6.5+, the pack command will error out when attempting to pack these project types. Earlier versions would attempt to pack, but the generated package may not be correct.
> Under Mono, creating a package from a project file is not supported. You also need to adjust non-local paths in the `.nuspec` file to Unix-style paths, as nuget.exe doesn't convert Windows pathnames itself.

## Usage

```cli
nuget pack <nuspecPath | projectPath> [options] [-Properties ...]
```

where `<nuspecPath>` and `<projectPath>` specify the `.nuspec` or project file, respectively.

## Options
- **`-BasePath`**

   Sets the base path of the files defined in the [.nuspec](../nuspec.md) file.

- **`-Build`**

  Specifies that the project should be built before building the package.

- **`-ConfigFile`**

  The NuGet configuration file to apply. If not specified, `%AppData%\NuGet\NuGet.Config` (Windows), or `~/.nuget/NuGet/NuGet.Config` or `~/.config/NuGet/NuGet.Config` (Mac/Linux) is used.

- **`-Exclude`**

  Specifies one or more wildcard patterns to exclude when creating a package. To specify more than one pattern, repeat the -Exclude flag. See example below.

- **`-ExcludeEmptyDirectories`**

  Prevents inclusion of empty directories when building the package.

- **`-ForceEnglishOutput`**

  *(3.5+)* Forces nuget.exe to run using an invariant, English-based culture.

- **`-?|-help`**

  Displays help information for the command.

- **`-IncludeReferencedProjects`**

  Indicates that the built package should include referenced projects either as dependencies or as part of the package. If a referenced project has a corresponding `.nuspec` file that has the same name as the project, then that referenced project is added as a dependency. Otherwise, the referenced project is added as part of the package.

- **`-InstallPackageToOutputPath`**

  Specify if the command should prepare the package output directory to support share as feed.

- **`-MinClientVersion`**

  Set the *minClientVersion* attribute for the created package. This value will override the value of the existing *minClientVersion* attribute (if any) in the `.nuspec` file.

- **`-MSBuildPath`**

  *(4.0+)* Specifies the path of MSBuild to use with the command, taking precedence over `-MSBuildVersion`.

- **`-MSBuildVersion`**

  *(3.2+)* Specifies the version of MSBuild to be used with this command. Supported values are 4, 12, 14, 15.1, 15.3, 15.4, 15.5, 15.6, 15.7, 15.8, 15.9. By default the MSBuild in your path is picked, otherwise it defaults to the highest installed version of MSBuild.

- **`-NoDefaultExcludes`**

  Prevents default exclusion of NuGet package files and files and folders starting with a dot, such as `.svn` and `.gitignore`.

- **`-NonInteractive`**

  Suppresses prompts for user input or confirmations.

- **`-NoPackageAnalysis`**

  Specifies that pack should not run package analysis after building the package.

- **`-OutputDirectory`**

  Specifies the folder in which the created package is stored. If no folder is specified, the current folder is used.

- **`-OutputFileNamesWithoutVersion`**

  Specify if the command should prepare the package output name without the version.

- **`-PackagesDirectory`**

  Specifies the packages folder.

- **`-p|-Properties`**

  Should appear last on the command line after other options. Specifies a list of properties that override values in the project file; see [Common MSBuild Project Properties](/visualstudio/msbuild/common-msbuild-project-properties) for property names. The Properties argument here is a list of token=value pairs, separated by semicolons, where each occurrence of `$token$` in the `.nuspec` file will be replaced with the given value. Values can be strings in quotation marks. Note that for the "Configuration" property, the default is "Debug". To change to a Release configuration, use `-Properties Configuration=Release`. **In general**, Properties should be the same that were used during the corresponding project build, in order to avoid potentially strange behavior.

- **`-SolutionDirectory`**

  Specifies the solution directory.

- **`-Suffix`**

  *(3.4.4+)* Appends a suffix to the internally generated version number, typically used for appending build or other pre-release identifiers. For example, using `-suffix nightly` will create a package with a version number like `1.2.3-nightly`. Suffixes must start with a letter to avoid warnings, errors, and potential incompatibilities with different versions of NuGet and the NuGet Package Manager.

- **`-SymbolPackageFormat`**

  When creating a symbols package, allows to choose between the `snupkg` and `symbols.nupkg` format.

- **`-Symbols`**

  Specifies that the package contains sources and symbols. When used with a `.nuspec` file, this creates a regular NuGet package file and the corresponding symbols package. By default it creates a [legacy symbol package](../../create-packages/Symbol-Packages.md). The new recommended format for symbol packages is .snupkg. See [Creating symbol packages (.snupkg)](../../create-packages/Symbol-Packages-snupkg.md).

- **`-Tool`**

   Specifies that the output files of the project should be placed in the `tool` folder.

- **`-Verbosity [normal|quiet|detailed]`**

  Specifies the amount of detail displayed in the output: `normal` (the default), `quiet`, or `detailed`.

- **`-Version`**

  Overrides the version number from the `.nuspec` file.

Also see [Environment variables](cli-ref-environment-variables.md)

## Excluding development dependencies

Some NuGet packages are useful as development dependencies, which help you author your own library, but aren't necessarily needed as actual package dependencies.

The `pack` command will ignore `package` entries in `packages.config` that have the `developmentDependency` attribute set to `true`. These entries will not be include as a dependencies in the created package.

For example, consider the following `packages.config` file in the source project:

```xml
<?xml version="1.0" encoding="utf-8"?>
<packages>
    <package id="jQuery" version="1.5.2" />
    <package id="netfx-Guard" version="1.3.3.2" developmentDependency="true" />
    <package id="microsoft-web-helpers" version="1.15" />
</packages>
```

For this project, the package created by `nuget pack` will have a dependency on `jQuery` and `microsoft-web-helpers` but not `netfx-Guard`.

## Suppressing pack warnings

While it is recommended that you resolve all NuGet warnings during your pack operations, in certain situations suppressing them is warranted.

You can achieve that in the following way: 

> nuget.exe pack package.nuspec -Properties NoWarn=NU5104

## Examples

```cli
nuget pack

nuget pack foo.nuspec

nuget pack foo.csproj

nuget pack foo.csproj -Properties Configuration=Release

nuget pack foo.csproj -Build -Symbols -Properties owners=janedoe,xiaop;version="1.0.5"

# Create a package from project foo.csproj, using MSBuild version 12 to build the project
nuget pack foo.csproj -Build -Symbols -MSBuildVersion 12 -Properties owners=janedoe,xiaop;version="1.0.5"

# Create a package from project foo.nuspec and the corresponding symbol package using the new recommended format .snupkg
nuget pack foo.nuspec -Symbols -SymbolPackageFormat snupkg

nuget pack foo.nuspec -Version 2.1.0

nuget pack foo.nuspec -Version 1.0.0 -MinClientVersion 2.5

nuget pack Package.nuspec -exclude "*.exe" -exclude "*.bat"
```

> [!Note]
> The `pack` command for SDK-style projects is not supported, use `dotnet pack` or `msbuild -t:pack` to pack this those projects instead.
