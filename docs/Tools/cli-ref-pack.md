---
# required metadata

title: NuGet CLI pack command | Microsoft Docs
author: kraigb
ms.author: kraigb
manager: ghogen
ms.date: 10/24/2017
ms.topic: reference
ms.prod: nuget
ms.technology: null
ms.assetid: 55e9e4d2-8039-4e9b-bdd9-c8b3eb0e894b

# optional metadata

description: Reference for the nuget.exe pack command
keywords: nuget pack reference, pack command
ms.reviewer:
- karann-msft
- unniravindranathan

---

# pack command (NuGet CLI)

**Applies to:** package creation &bullet; **Supported versions:** 2.7+

Creates a NuGet package based on the specified `.nuspec` or project file. 

> [!Important]
> Under Mono, creating a package from a project file is not supported. You also need to adjust non-local paths in the `.nuspec` file to Unix-style paths, as nuget.exe doesn't convert Windows pathnames itself.

## Usage

```
nuget pack <nuspecPath | projectPath> [options]
```

where `<nuspecPath>` and `<projectPath>` specify the `.nuspec` or project file, respectively.

## Options

| Option | Description |
| --- | --- |
| BasePath | Sets the base path of the files defined in the `.nuspec` file. |
| Build | Specifies that the project should be built before building the package. |
| Exclude | Specifies one or more wildcard patterns to exclude when creating a package. |
| ExcludeEmptyDirectories | Prevents inclusion of empty directories when building the package. |
| ForceEnglishOutput | *(3.5+)* Forces nuget.exe to run using an invariant, English-based culture. |
| Help | Displays help information for the command. |
| IncludeReferencedProjects | Indicates that the built package should include referenced projects either as dependencies or as part of the package. If a referenced project has a corresponding `.nuspec` file that has the same name as the project, then that referenced project is added as a dependency. Otherwise, the referenced project is added as part of the package. |
| MinClientVersion | Set the *minClientVersion* attribute for the created package. This value will override the value of the existing *minClientVersion* attribute (if any) in the `.nuspec` file. |
| MSBuildPath | *(4.0+)* Specifies the path of MSBuild to use with the command, taking precedence over `-MSBuildVersion`. |
| MSBuildVersion | *(3.2+)* Specifies the version of MSBuild to be used with this command. Supported values are 4, 12, 14, 15. By default the MSBuild in your path is picked, otherwise it defaults to the highest installed version of MSBuild. |
| NoDefaultExcludes | Prevents default exclusion of NuGet package files and files and folders starting with a dot, such as `.svn` and `.gitignore`. |
| NoPackageAnalysis | Specifies that pack should not run package analysis after building the package. |
| OutputDirectory | Specifies the folder in which the created package is stored. If no folder is specified, the current folder is used. |
| Properties | Specifies a list of token=value pairs, separated by semicolons, where each occurrence of `$token$` in the `.nuspec` file will be replaced with the given value. Values can be strings in quotation marks. |
| Suffix | *(3.4.4+)* Appends a suffix to the internally generated version number, typically used for appending build or other pre-release identifiers. For example, using `-suffix nightly` will create a package with a version number like `1.2.3-nightly`. Suffixes must start with a letter to avoid warnings, errors, and potential incompatibilities with different versions of NuGet and the NuGet Package Manager. |
| Symbols | Specifies that the package contains sources and symbols. When used with a `.nuspec` file, this creates a regular NuGet package file and the corresponding symbols package. |
| Tool | Specifies that the output files of the project should be placed in the `tool` folder. |
| Verbosity | Specifies the amount of detail displayed in the output: *normal*, *quiet*, *detailed (2.5+)*. |
| Version | Overrides the version number from the `.nuspec` file. |

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

## Examples

```
nuget pack

nuget pack foo.nuspec

nuget pack foo.csproj

nuget pack foo.csproj -Build -Symbols -Properties owners=janedoe,xiaop;version="1.0.5"

# create a package from project foo.csproj, using MSBuild version 12 to build the project
nuget pack foo.csproj -Build -Symbols -Properties owners=janedoe,xiaop;version="1.0.5" -MSBuildVersion 12

nuget pack foo.nuspec -Version 2.1.0

nuget pack foo.nuspec -Version 1.0.0 -MinClientVersion 2.5
```
