---
# required metadata

title: NuGet pack and restore as MSBuild targets | Microsoft Docs
author: kraigb
ms.author: kraigb
manager: ghogen
ms.date: 11/17/2016
ms.topic: article
ms.prod: nuget
#ms.service:
ms.technology: nuget
ms.assetid: 86f7e724-2509-4d7d-aa8d-4a3fb913ded6

# optional metadata

#description: release notes 3.5 RTM
#keywords: release notes 3.5 RTM
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

NuGet 4.0+ can work directly with the information in a `.csproj` file without requring a separate `packages.config` or `project.json` file. All the metadata that was previously stored in those configuration files can be instead stores in the `.csproj` file directly, as described here.

NuGet is also a first-class MSBuild citizen with the `pack` and `restore` targets as described below. These targets allow you to work with NuGet as you would with any other MSBuild task or target.

In this topic:

- [Target build order](#target-build-order)
- [pack target](#pack-target)
- [pack scenarios](#pack-scenarios)
- [restore target](#restore-target)
- [PackageTargetFallback](#packagetargetfallback)

## Target build order

Because `pack` and `restore` are  MSBuild targets, you can access them to enhance your workflow. For example, let’s say you want to push your package to a network share after packing it. You can do that by adding the following in your project file:

    <Target Name="CopyPackage" AfterTargets="Pack">
        <Copy
            SourceFiles="$(OutputPath)\$(PackageId).$(PackageVersion).nupkg"
            DestinationFolder="\\myshare\packageshare\"
            />
    </Target>

Similarly, you can write an MSBuild task, write your own target and consume NuGet properties in the MSBuild task.

## pack target

In order for MSBuild to be able to gather all the inputs, all metadata from `project.json` and `.nuspec` will move into the `.csproj` file. The table below describes the MSBuild properties that can be added to a `.csproj` file within the first &lt;PropertyGroup&gt; node. You can make these edits easily in Visual Studio 2017 and later by right-clicking the project and selecting **Edit {project_name}** on the context menu. For convenience the table is organized by the equivalent property in a [`.nuspec` file](../schema/nuspec.md).


| Attribute/NuSpec Value | MSBuild Property | Default | Notes          
| Id | PackageId | AssemblyName | $(AssemblyName) from msbuild
| Version | PackageVersion | Version | New $(Version) property from msbuild, is semver compatible. Could be “1.0.0”, “1.0.0-beta”, or “1.0.0-beta-00345”.
| Authors | Authors | username of the current user will be the default value |
| Owners | N/A | Not present in NuSpec |
| Description | Description | "Package Description" |
| Copyright | Copyright | empty |
| RequireLicenseAcceptance | PackageRequireLicenseAcceptance | false |
| LicenseUrl | PackageLicenseUrl | empty |
| ProjectUrl | PackageProjectUrl | empty |
| IconUrl | PackageIconUrl | empty |
| Tags | PackageTags | empty |
| ReleaseNotes | PackageReleaseNotes | empty |
| RepositoryUrl | RepositoryUrl | empty |
| RepositoryType | RepositoryType | empty |
| PackageType | `&lt;PackageType&gt;DotNetCliTool, 1.0.0.0;Dependency, 2.0.0.0&lt;/PackageType&gt;` |  |
| Title | Not supported | |
| Summary | Not supported | |
| n/a | PackageOutputPath | The bin\{configuration} folder |
| n/a | Configuration | Debug |
| n/a | AssemblyName | |
| n/a | IncludeSymbols | |
| n/a | IncludeSource | | If true, there must also be a &lt;SourceFile&gt; node.
| n/a | IsTool | |
| n/a | NoPackageAnalysis | |
| n/a | MinClientVersion | |
| n/a | TargetPath<br>TargetFramework | | See <a href="#cross-targeting">cross-targeting</a> below.

## pack scenarios

### Output assemblies

The `nuget pack` command will copy the output files, that is, those with extensions `.exe`, `.dll`, `.xml`, and `.winmd`. The output files that are copied depend on what MSBuild provides from the `BuiltOutputProjectGroup` target.

There are two MSBuild  properties that you can use in your project file or command line to control where output assemblies go:

- `IncludeBuildOutput`: A boolean that determines whether the build output assemblies should be included in the package.
- `BuildOutputTargetFolder`: Specifies the folder in which the output assemblies should be placed. The output assemblies (and other output files) are copied into their respective framework folders.

### Package references

See [Package References in Project Files](../consume-packages/package-references-in-project-files.md).

### Including content in package

To include content, add extra metadata to the existing &lt;Content&gt; item. By default everything of type "Content" gets included in the package unless you override with entries like the following:

     <Content Include="..\win7-x64\libuv.txt">
         <Pack>false</Pack>
     </Content>

Everything gets added to the root of the `content` and `contentFiles` folder within a package and preserves the relative directory structure, unless you specify a package path:

    <Content Include="..\win7-x64\libuv.txt">
        <Pack>true</Pack>
        <PackagePath>content\myfiles</PackagePath>
     </Content>

`PackagePath` can be a semicolon-delimited set of target paths. Specifying an empty package path would add the file to the root of the package. For example, the following adds libuv.txt to content\myfiles, content\samples, and the package root:

    <Content Include="..\win7-x64\libuv.txt">
        <Pack>true</Pack>
        <PackagePath>content\myfiles;content\sample;;</PackagePath>
     </Content>

Packing of content files is also recursive. Content files from any project to project reference, which has `TreatAsPackageReference` set to `false`, are also copied in the similar manner and the same rules apply.

If you wish to prevent copying of a content from another project into your nuget package, use the following form:

    <Content Include="..\..\project2\readme.txt">
        <Pack>false</Pack>
     </Content>

Similarly, you can override the behavior in the referenced project and include a file to be packed which would have otherwise been excluded:

    <Content Include="..\..\project2\readme.txt">
        <Pack>true</Pack>
        <PackagePath>content\myfiles</PackagePath>
        <Visible>false</Visible>
    </Content>

Setting `Visible` to `false` prevents Visual Studio from showing the file in the Solution Explorer.

There is also an MSBuild property `$(IncludeContentInPack)`, which defaults to `true`. If this is set to `false` on any project, then the content from that project or it's project to project dependencies are not included in the nuget package.

> [!Note]
> Apart from Content items, the Pack and PackagePath metadata can also be set on files with a build action of Compile or None.
> For pack to append the filename to your package path, your package path must end with the directory separator character, otherwise the package path is treated as the full path including the file name.

### Cross-targeting

At present, target frameworks are defined in the `.csproj` file in an item list called `TargetFrameworks` where the identity maps to `$(TargetFrameworkIdentity),$(TargetFrameworkVersion)` (not using NuGet short names).

The `@(TargetPath)` will be a list of all the output paths (path to the output assembly) with their associated TargetFramework metadata. `nuget pack` will convert these full target framework names to short folder names in the resulting package.

### IncludeSymbols

When using `msbuild /t:pack /p:IncludeSymbols=true`, the corresponding pdb files are copied along with other output files (`.dll`, `.exe`, `.winmd`, `.xml`). Note that setting `IncludeSymbols=true` creates a regular package *and* a symbols package.

### IncludeSource

This is the same as `IncludeSymbols`, except that it copies source files along with pdbs as well. All files of type Compile are copied over to `src\&lt;ProjectName&gt;\` preserving the relative path directory structure in the resulting package. The same also happens for source files of any `ProjectReference` which has `TreatAsPackageReference` set to `false`.

If a file of type Compile, is outside the project folder, then it is just added to `src\&lt;ProjectName&gt;\`.

### IsTool

When using `msbuild /t:pack /p:IsTool=true`, all output files, as specified in the [Output Assemblies](#output-assemblies) scenario, are copied to the `tools` folder instead of the `lib` folder. Note that this is different from a `DotNetCliTool` which is specified by setting the `PackageType` in `.csproj` file.

## restore target

As part of the move to MSBuild, package restore becomes an MSBuild target; `nuget restore` and `dotnet restore` use this target to restore packages in .NET Core projects.

### Restore properties

Additional restore settings may come from MSBuild properties; values are set from the command line.

| Property | Description
| RestoreSources | List of package sources separated by semicolons
| RestorePackagesPath | User packages directory path
| RestoreDisableParallel | Limit downloads to one at a time
| RestoreConfigFile | nuget.config file
| RestoreNoCache | If true, avoids using the web cache
| RestoreIgnoreFailedSource | If true, ignores failing or missing package sources


### Restore outputs

Restore creates the following files in the build `obj` folder:

| File | Description
| project.assets.json | Previously project.lock.json
| {projectName}.projectFileExtension.nuget.g.props | References to msbuild targets contained in packages
| {projectName}.projectFileExtension.nuget.g.targets | References to msbuild props contained in packages

## PackageTargetFallback

`PackageTargetFallback` is the MSBuild equivalent of `Imports`.

### MSBuild syntax to support PackageTargetFallback

    <PackageTargetFallback Condition="'$(TargetFramework)'=='Net45'">portable-net45+win81</PackageTargetFallback>

`PackageTargetFallbacks` may have been set in one of Microsoft targets (we are considering), or other ones. If you'd like to add the list already provided, you can add additional values to the `PackageTargetFallback` property:

    <PackageTargetFallback Condition="'$(TargetFramework)'=='Net45'">
        $(PackageTargetFallback);portable-net45+win8+wpa81+wp8
    </PackageTargetFallback >

### Replacing one library from a restore graph

If a restore is bringing the wrong assembly, it is possible to exclude that packages default choice, and replace it with your own choice. First with a top level `PackageReference`, exclude all assets:

    <PackageReference Include="Newtonsoft.Json" Version="9.0.1">
        <ExcludeAssets>All</ExcludeAssets>
    </PackageReference>

Next, add your own reference to the appropriate local copy of the DLL:

    <Reference Include="Newtonsoft.Json.dll" />
