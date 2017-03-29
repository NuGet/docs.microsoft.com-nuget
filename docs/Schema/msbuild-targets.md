﻿---
# required metadata

title: NuGet pack and restore as MSBuild targets | Microsoft Docs
author: kraigb
ms.author: kraigb
manager: ghogen
ms.date: 2/8/2017
ms.topic: article
ms.prod: nuget
#ms.service:
ms.technology: nuget
ms.assetid: 86f7e724-2509-4d7d-aa8d-4a3fb913ded6

# optional metadata

description: NuGet pack and restore can work directly as MSBuild targets with NuGet 4.0+.
keywords: NuGet and MSBuild, NuGet pack target, NuGet restore target
#ROBOTS:
#audience:
#ms.devlang:
ms.reviewer:
- karann
#ms.suite:
#ms.tgt_pltfrm:
#ms.custom:

---

# NuGet pack and restore as MSBuild targets

*NuGet 4.0+*

NuGet 4.0+ can work directly with the information in a `.csproj` file without requiring a separate `.nuspec` or `project.json` file. All the metadata that was previously stored in those configuration files can be instead stored in the `.csproj` file directly, as described here.

With MSBuild 15.1+, NuGet is also a first-class MSBuild citizen with the `pack` and `restore` targets as described below. These targets allow you to work with NuGet as you would with any other MSBuild task or target.

In this topic:

- [Target build order](#target-build-order)
- [pack target](#pack-target)
- [pack scenarios](#pack-scenarios)
- [restore target](#restore-target)
- [PackageTargetFallback](#packagetargetfallback)

## Target build order

Because `pack` and `restore` are  MSBuild targets, you can access them to enhance your workflow. For example, let’s say you want to copy your package to a network share after packing it. You can do that by adding the following in your project file:

```xml
<Target Name="CopyPackage" AfterTargets="Pack">
    <Copy
        SourceFiles="$(OutputPath)\$(PackageId).$(PackageVersion).nupkg"
        DestinationFolder="\\myshare\packageshare\"
        />
</Target>
```

Similarly, you can write an MSBuild task, write your own target and consume NuGet properties in the MSBuild task.

## pack target

When using the pack target, that is, `msbuild /t:pack`, MSBuild draws its inputs from the `.csproj` file rather than `project.json` or `.nuspec` files. The table below describes the MSBuild properties that can be added to a `.csproj` file within the first `<PropertyGroup>` node. You can make these edits easily in Visual Studio 2017 and later by right-clicking the project and selecting **Edit {project_name}** on the context menu. For convenience the table is organized by the equivalent property in a [`.nuspec` file](../schema/nuspec.md).


| Attribute/NuSpec Value | MSBuild Property | Default | Notes |
|--------|--------|--------|--------|
| Id | PackageId | AssemblyName | $(AssemblyName) from MSBuild |
| Version | PackageVersion | Version | New $(Version) property from MSBuild, is semver compatible. Could be “1.0.0”, “1.0.0-beta”, or “1.0.0-beta-00345”. |
| VersionPrefix | PackageVersionPrefix | empty | Setting PackageVersion will overwrite PackageVersionPrefix |
| VersionSuffix | PackageVersionSuffix | empty | $(VersionSuffix) from MSBuild. Setting PackageVersion will overwrite PackageVersionSuffix | 
| Authors | Authors | Username of the current user | |
| Owners | N/A | Not present in NuSpec | |
| Title | Title | The PackageId| |
| Description | Description | "Package Description" | |
| Copyright | Copyright | empty | |
| RequireLicenseAcceptance | PackageRequireLicenseAcceptance | false | |
| LicenseUrl | PackageLicenseUrl | empty | |
| ProjectUrl | PackageProjectUrl | empty | |
| IconUrl | PackageIconUrl | empty | |
| Tags | PackageTags | empty | |
| ReleaseNotes | PackageReleaseNotes | empty | |
| RepositoryUrl | RepositoryUrl | empty | |
| RepositoryType | RepositoryType | empty | |
| PackageType | `<PackageType>DotNetCliTool, 1.0.0.0;Dependency, 2.0.0.0</PackageType>` | | |
| Summary | Not supported | | |


### pack target inputs

- IsPackable
- PackageVersion
- PackageId
- Authors
- Description
- Copyright
- PackageRequireLicenseAcceptance
- PackageLicenseUrl
- PackageProjectUrl
- PackageIconUrl
- PackageReleaseNotes
- PackageTags
- PackageOutputPath
- IncludeSymbols
- IncludeSource
- PackageTypes
- IsTool
- RepositoryUrl
- RepositoryType
- NoPackageAnalysis
- MinClientVersion
- IncludeBuildOutput
- IncludeContentInPack
- BuildOutputTargetFolder
- ContentTargetFolders
- NuspecFile
- NuspecBasePath
- NuspecProperties

## pack scenarios

### PackageIconUrl

As part of the change for [NuGet Issue 2582](https://github.com/NuGet/Home/issues/2582), `PackageIconUrl` will eventually be changed to `PackageIconUri` and can be relative path to a icon file which will included at the root of the resulting package.

### Output assemblies

The `nuget pack` command will copy output files with extensions `.exe`, `.dll`, `.xml`, `.winmd`, `.json`, and `.pri`. The output files that are copied depend on what MSBuild provides from the `BuiltOutputProjectGroup` target.

There are two MSBuild  properties that you can use in your project file or command line to control where output assemblies go:

- `IncludeBuildOutput`: A boolean that determines whether the build output assemblies should be included in the package.
- `BuildOutputTargetFolder`: Specifies the folder in which the output assemblies should be placed. The output assemblies (and other output files) are copied into their respective framework folders.

### Package references

See [Package References in Project Files](../consume-packages/package-references-in-project-files.md).

### Project to project references

Project to project references are considered by default as nuget package references, for example:

```xml
<ProjectReference Include="..\UwpLibrary2\UwpLibrary2.csproj"/>
```

You can also add the following metadata to your project reference:

```xml
<IncludeAssets>
<ExcludeAssets>
<PrivateAssets>
```

### Including content in a package

To include content, add extra metadata to the existing `<Content>` item. By default everything of type "Content" gets included in the package unless you override with entries like the following:

 ```xml
    <Content Include="..\win7-x64\libuv.txt">
        <Pack>false</Pack>
    </Content>
 ```

By default, everything gets added to the root of the `content` and `contentFiles\any\<target_framework>` folder within a package and preserves the relative folder structure, unless you specify a package path:

```xml
<Content Include="..\win7-x64\libuv.txt">        
    <Pack>true</Pack>
    <PackagePath>content\myfiles\</PackagePath>
</Content>
```

If you want to copy all your content to only a specific root folder(s) (instead of `content` and `contentFiles` both), you can use the MSBuild property `ContentTargetFolders`, which defaults to "content;contentFiles" but can be set to any other folder names. Note that just specifying "contentFiles" in `ContentTargetFolders` puts files under `contentFiles\any\<target_framework>` or `contentFiles\<language>\<target_framework>` based on `buildAction`.

`PackagePath` can be a semicolon-delimited set of target paths. Specifying an empty package path would add the file to the root of the package. For example, the following adds `libuv.txt` to `content\myfiles`, `content\samples`, and the package root:

```xml
<Content Include="..\win7-x64\libuv.txt">
    <Pack>true</Pack>
    <PackagePath>content\myfiles;content\sample;;</PackagePath>
</Content>
```

There is also an MSBuild property `$(IncludeContentInPack)`, which defaults to `true`. If this is set to `false` on any project, then the content from that project are not included in the nuget package.

> [!Note]
> Apart from Content items, the `<Pack>` and `<PackagePath>` metadata can also be set on files with a build action of Compile, EmbeddedResource, ApplicationDefinition, Page, Resource, SplashScreen, DesignData, DesignDataWithDesignTimeCreatableTypes, CodeAnalysisDictionary, AndroidAsset, AndroidResource, BundleResource or None.
>
> For pack to append the filename to your package path when using globbing patterns, your package path must end with the folder separator character, otherwise the package path is treated as the full path including the file name.

### IncludeSymbols

When using `MSBuild /t:pack /p:IncludeSymbols=true`, the corresponding `.pdb` files are copied along with other output files (`.dll`, `.exe`, `.winmd`, `.xml`, `.json`, `.pri`). Note that setting `IncludeSymbols=true` creates a regular package *and* a symbols package.

### IncludeSource

This is the same as `IncludeSymbols`, except that it copies source files along with `.pdb` files as well. All files of type `Compile` are copied over to `src\<ProjectName>\` preserving the relative path folder structure in the resulting package. The same also happens for source files of any `ProjectReference` which has `TreatAsPackageReference` set to `false`.

If a file of type Compile, is outside the project folder, then it is just added to `src\<ProjectName>\`.

### IsTool

When using `MSBuild /t:pack /p:IsTool=true`, all output files, as specified in the [Output Assemblies](#output-assemblies) scenario, are copied to the `tools` folder instead of the `lib` folder. Note that this is different from a `DotNetCliTool` which is specified by setting the `PackageType` in `.csproj` file.

### Packing using a .nuspec

You can use a `.nuspec` file to pack your project provided that you have a project file to import `NuGet.Build.Tasks.Pack.targets` so that the pack task can be executed. The following three MSBuild properties are relevant to packing using a `.nuspec`:

1. `NuspecFile`: relative or absolute path to the `.nuspec` file being used for packing.
1. `NuspecProperties`: a semicolon-separated list of key=value pairs. Due to the way MSBuild command-line parsing works, multiple properties must be specified as follows: `/p:NuspecProperties=\"key1=value1;key2=value2\"`.  
1. `NuspecBasePath`: Base path for the `.nuspec` file.

If using `dotnet.exe` to pack your project, use a command like the following:

```bash
dotnet pack <path to .csproj file> /p:NuspecFile=<path to nuspec file> /p:NuspecProperties=<> /p:NuspecBasePath=<Base path> 
```

If using MSBuild to pack your project, use a command like the following:

```bash
msbuild /t:pack <path to .csproj file> /p:NuspecFile=<path to nuspec file> /p:NuspecProperties=<> /p:NuspecBasePath=<Base path> 
```

## restore target

As part of the move to MSBuild, package restore becomes an MSBuild target, that is,  MSBuild /t:restore`. `nuget restore` and `dotnet restore` use this target to restore packages in .NET Core projects.

The restore target works as follows:

1. Read all project to project references
1. Read the project properties to find the intermediate folder and target frameworks
1. Pass msbuild data to NuGet.Build.Tasks.dll
1. Run restore
1. Download packages
1. Write assets file, targets, and props


### Restore properties

Additional restore settings may come from MSBuild properties; values are set from the command line.

| Property | Description |
|--------|--------|
| RestoreSources | Semicolon-delimited list of package sources |
| RestorePackagesPath | User packages folder path |
| RestoreDisableParallel | Limit downloads to one at a time |
| RestoreConfigFile | `Nuget.Config` file |
| RestoreNoCache | If true, avoids using the web cache |
| RestoreIgnoreFailedSource | If true, ignores failing or missing package sources |
| RestoreTaskAssemblyFile | Path to `NuGet.Build.Tasks.dll` |
| RestoreGraphProjectInput | Semicolon-delimited list of projects to restore, which should contain absolute paths. |
| RestoreOutputPath | Output folder, defaulting to the `obj` folder |

**Example**

```xml
<PropertyGroup>
<RestoreIgnoreFailedSource>true</RestoreIgnoreFailedSource>
<PropertyGroup>
```

### Restore outputs

Restore creates the following files in the build `obj` folder:

| File | Description |
|--------|--------|
| `project.assets.json` | Previously `project.lock.json` |
| `{projectName}.projectFileExtension.nuget.g.props` | References to MSBuild props contained in packages |
| `{projectName}.projectFileExtension.nuget.g.targets` | References to MSBuild targets contained in packages |

## PackageTargetFallback

`PackageTargetFallback` is the MSBuild equivalent of `Imports`.

### MSBuild syntax to support PackageTargetFallback

    <PackageTargetFallback Condition="'$(TargetFramework)'=='Net45'">portable-net45+win81</PackageTargetFallback>

`PackageTargetFallbacks` may have been set in one of Microsoft targets (we are considering), or other ones. If you'd like to add the list already provided, you can add additional values to the `PackageTargetFallback` property:

```xml
<PackageTargetFallback Condition="'$(TargetFramework)'=='Net45'">
    $(PackageTargetFallback);portable-net45+win8+wpa81+wp8
</PackageTargetFallback >
```

### Replacing one library from a restore graph

If a restore is bringing the wrong assembly, it is possible to exclude that packages default choice, and replace it with your own choice. First with a top level `PackageReference`, exclude all assets:

```xml
<PackageReference Include="Newtonsoft.Json" Version="9.0.1">
    <ExcludeAssets>All</ExcludeAssets>
</PackageReference>
```

Next, add your own reference to the appropriate local copy of the DLL:

```xml
<Reference Include="Newtonsoft.Json.dll" />
```
