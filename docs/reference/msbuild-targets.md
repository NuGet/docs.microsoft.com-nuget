---
title: NuGet pack and restore as MSBuild targets
description: NuGet pack and restore can work directly as MSBuild targets with NuGet 4.0+.
author: karann-msft
ms.author: karann
ms.date: 03/23/2018
ms.topic: conceptual
---

# NuGet pack and restore as MSBuild targets

*NuGet 4.0+*

With the [PackageReference](../consume-packages/package-references-in-project-files.md) format, NuGet 4.0+ can store all manifest metadata directly within a project file rather than using a separate `.nuspec` file.

With MSBuild 15.1+, NuGet is also a first-class MSBuild citizen with the `pack` and `restore` targets as described below. These targets allow you to work with NuGet as you would with any other MSBuild task or target. For instructions creating a NuGet package using MSBuild, see [Create a NuGet package using MSBuild](../create-packages/creating-a-package-msbuild.md). (For NuGet 3.x and earlier, you use the [pack](../reference/cli-reference/cli-ref-pack.md) and [restore](../reference/cli-reference/cli-ref-restore.md) commands through the NuGet CLI instead.)

## Target build order

Because `pack` and `restore` are  MSBuild targets, you can access them to enhance your workflow. For example, let’s say you want to copy your package to a network share after packing it. You can do that by adding the following in your project file:

```xml
<Target Name="CopyPackage" AfterTargets="Pack">
  <Copy
    SourceFiles="$(OutputPath)..\$(PackageId).$(PackageVersion).nupkg"
    DestinationFolder="\\myshare\packageshare\"
    />
</Target>
```

Similarly, you can write an MSBuild task, write your own target and consume NuGet properties in the MSBuild task.

> [!NOTE]
> `$(OutputPath)` is relative and expects that you are running the command from the project root.

## pack target

For .NET Standard projects using the PackageReference format, using `msbuild -t:pack` draws inputs from the project file to use in creating a NuGet package.

The table below describes the MSBuild properties that can be added to a project file within the first `<PropertyGroup>` node. You can make these edits easily in Visual Studio 2017 and later by right-clicking the project and selecting **Edit {project_name}** on the context menu. For convenience the table is organized by the equivalent property in a [`.nuspec` file](../reference/nuspec.md).

Note that the `Owners` and `Summary` properties from `.nuspec` are not supported with MSBuild.

| Attribute/NuSpec Value | MSBuild Property | Default | Notes |
|--------|--------|--------|--------|
| Id | PackageId | AssemblyName | $(AssemblyName) from MSBuild |
| Version | PackageVersion | Version | This is semver compatible, for example “1.0.0”, “1.0.0-beta”, or “1.0.0-beta-00345” |
| VersionPrefix | PackageVersionPrefix | empty | Setting PackageVersion overwrites PackageVersionPrefix |
| VersionSuffix | PackageVersionSuffix | empty | $(VersionSuffix) from MSBuild. Setting PackageVersion overwrites PackageVersionSuffix |
| Authors | Authors | Username of the current user | |
| Owners | N/A | Not present in NuSpec | |
| Title | Title | The PackageId| |
| Description | Description | "Package Description" | |
| Copyright | Copyright | empty | |
| RequireLicenseAcceptance | PackageRequireLicenseAcceptance | false | |
| license | PackageLicenseExpression | empty | Corresponds to `<license type="expression">` |
| license | PackageLicenseFile | empty | Corresponds to `<license type="file">`. You need to explicitly pack the referenced license file. |
| LicenseUrl | PackageLicenseUrl | empty | `PackageLicenseUrl` is deprecated, use the PackageLicenseExpression or PackageLicenseFile property |
| ProjectUrl | PackageProjectUrl | empty | |
| Icon | PackageIcon | empty | You need to explicitly pack the referenced icon image file.|
| IconUrl | PackageIconUrl | empty | For the best downlevel experience, `PackageIconUrl` should be specified in addition to `PackageIcon`. Longer term, `PackageIconUrl` will be deprecated. |
| Tags | PackageTags | empty | Tags are semi-colon delimited. |
| ReleaseNotes | PackageReleaseNotes | empty | |
| Repository/Url | RepositoryUrl | empty | Repository URL used to clone or retrieve source code. Example: *https://github.com/NuGet/NuGet.Client.git* |
| Repository/Type | RepositoryType | empty | Repository type. Examples: *git*, *tfs*. |
| Repository/Branch | RepositoryBranch | empty | Optional repository branch information. *RepositoryUrl* must also be specified for this property to be included. Example: *master* (NuGet 4.7.0+) |
| Repository/Commit | RepositoryCommit | empty | Optional repository commit or changeset to indicate which source the package was built against. *RepositoryUrl* must also be specified for this property to be included. Example: *0e4d1b598f350b3dc675018d539114d1328189ef* (NuGet 4.7.0+) |
| PackageType | `<PackageType>DotNetCliTool, 1.0.0.0;Dependency, 2.0.0.0</PackageType>` | | |
| Summary | Not supported | | |

### pack target inputs

- IsPackable
- SuppressDependenciesWhenPacking
- PackageVersion
- PackageId
- Authors
- Description
- Copyright
- PackageRequireLicenseAcceptance
- DevelopmentDependency
- PackageLicenseExpression
- PackageLicenseFile
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
- RepositoryBranch
- RepositoryCommit
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

### Suppress dependencies

To suppress package dependencies from generated NuGet package, set `SuppressDependenciesWhenPacking` to `true` which will allow skipping all the dependencies from generated nupkg file.

### PackageIconUrl

`PackageIconUrl` will be deprecated in favor of the new [`PackageIcon`](#packageicon) property.

Starting with NuGet 5.3 & Visual Studio 2019 version 16.3, `pack` will raise [NU5048](./errors-and-warnings/nu5048.md) warning if the package metadata only specifies `PackageIconUrl`.

### PackageIcon

> [!Tip]
> You should specify both `PackageIcon` and `PackageIconUrl` to maintain backward compatibility with clients and sources that do not yet support `PackageIcon`. Visual Studio will support `PackageIcon` for packages coming from a folder-based source in a future release.

#### Packing an icon image file

When packing an icon image file, you need to use `PackageIcon` property to specify the package path, relative to the root of the package. In addition, you need to make sure that the file is included in the package. Image file size is limited to 1 MB. Supported file formats include JPEG and PNG. We recommend an image resolution of 64x64.

For example:

```xml
<PropertyGroup>
    ...
    <PackageIcon>icon.png</PackageIcon>
    ...
</PropertyGroup>

<ItemGroup>
    ...
    <None Include="images\icon.png" Pack="true" PackagePath="\"/>
    ...
</ItemGroup>
```

[Package Icon sample](https://github.com/NuGet/Samples/tree/master/PackageIconExample).

For the nuspec equivalent, take a look at [nuspec reference for icon](nuspec.md#icon).

### Output assemblies

`nuget pack` copies output files with extensions `.exe`, `.dll`, `.xml`, `.winmd`, `.json`, and `.pri`. The output files that are copied depend on what MSBuild provides from the `BuiltOutputProjectGroup` target.

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

Other pack specific metadata that you can set on any of the above items includes ```<PackageCopyToOutput>``` and ```<PackageFlatten>``` which sets ```CopyToOutput``` and ```Flatten``` values on the ```contentFiles``` entry in the output nuspec.

> [!Note]
> Apart from Content items, the `<Pack>` and `<PackagePath>` metadata can also be set on files with a build action of Compile, EmbeddedResource, ApplicationDefinition, Page, Resource, SplashScreen, DesignData, DesignDataWithDesignTimeCreateableTypes, CodeAnalysisDictionary, AndroidAsset, AndroidResource, BundleResource or None.
>
> For pack to append the filename to your package path when using globbing patterns, your package path must end with the folder separator character, otherwise the package path is treated as the full path including the file name.

### IncludeSymbols

When using `MSBuild -t:pack -p:IncludeSymbols=true`, the corresponding `.pdb` files are copied along with other output files (`.dll`, `.exe`, `.winmd`, `.xml`, `.json`, `.pri`). Note that setting `IncludeSymbols=true` creates a regular package *and* a symbols package.

### IncludeSource

This is the same as `IncludeSymbols`, except that it copies source files along with `.pdb` files as well. All files of type `Compile` are copied over to `src\<ProjectName>\` preserving the relative path folder structure in the resulting package. The same also happens for source files of any `ProjectReference` which has `TreatAsPackageReference` set to `false`.

If a file of type Compile, is outside the project folder, then it's just added to `src\<ProjectName>\`.

### Packing a license expression or a license file

When using a license expression, the PackageLicenseExpression property should be used. 
[License expression sample](https://github.com/NuGet/Samples/tree/master/PackageLicenseExpressionExample).

```xml
<PropertyGroup>
    <PackageLicenseExpression>MIT</PackageLicenseExpression>
</PropertyGroup>
```

[Learn more about license expressions and licenses that are accepted by NuGet.org](nuspec.md#license).

When packing a license file, you need to use PackageLicenseFile property to specify the package path, relative to the root of the package. In addition, you need to make sure that the file is included in the package. For example:

```xml
<PropertyGroup>
    <PackageLicenseFile>LICENSE.txt</PackageLicenseFile>
</PropertyGroup>

<ItemGroup>
    <None Include="licenses\LICENSE.txt" Pack="true" PackagePath=""/>
</ItemGroup>
```

[License file sample](https://github.com/NuGet/Samples/tree/master/PackageLicenseFileExample).

### IsTool

When using `MSBuild -t:pack -p:IsTool=true`, all output files, as specified in the [Output Assemblies](#output-assemblies) scenario, are copied to the `tools` folder instead of the `lib` folder. Note that this is different from a `DotNetCliTool` which is specified by setting the `PackageType` in `.csproj` file.

### Packing using a .nuspec

Although it is recommended that you [include all the properties](../reference/msbuild-targets.md#pack-target) that are usually in the `.nuspec` file in the project file instead, you can choose to use a `.nuspec` file to pack your project. For a non-SDK-style project that uses `PackageReference`, you must import `NuGet.Build.Tasks.Pack.targets` so that the pack task can be executed. You still need to restore the project before you can pack a nuspec file. (An SDK-style project includes the pack targets by default.)

The target framework of the project file is irrelevant and not used when packing a nuspec. The following three MSBuild properties are relevant to packing using a `.nuspec`:

1. `NuspecFile`: relative or absolute path to the `.nuspec` file being used for packing.
1. `NuspecProperties`: a semicolon-separated list of key=value pairs. Due to the way MSBuild command-line parsing works, multiple properties must be specified as follows: `-p:NuspecProperties=\"key1=value1;key2=value2\"`.  
1. `NuspecBasePath`: Base path for the `.nuspec` file.

If using `dotnet.exe` to pack your project, use a command like the following:

```dotnetcli
dotnet pack <path to .csproj file> -p:NuspecFile=<path to nuspec file> -p:NuspecProperties=<> -p:NuspecBasePath=<Base path> 
```

If using MSBuild to pack your project, use a command like the following:

```cli
msbuild -t:pack <path to .csproj file> -p:NuspecFile=<path to nuspec file> -p:NuspecProperties=<> -p:NuspecBasePath=<Base path> 
```

Please note that packing a nuspec using dotnet.exe or msbuild also leads to building the project by default. This can be avoided by passing ```--no-build``` property to dotnet.exe, which is the equivalent of setting ```<NoBuild>true</NoBuild> ``` in your project file, along with setting ```<IncludeBuildOutput>false</IncludeBuildOutput> ``` in the project file.

An example of a *.csproj* file to pack a nuspec file is:

```xml
<Project Sdk="Microsoft.NET.Sdk">
  <PropertyGroup>
    <TargetFramework>netstandard2.0</TargetFramework>
    <NoBuild>true</NoBuild>
    <IncludeBuildOutput>false</IncludeBuildOutput>
    <NuspecFile>PATH_TO_NUSPEC_FILE</NuspecFile>
    <NuspecProperties>add nuspec properties here</NuspecProperties>
    <NuspecBasePath>optional to provide</NuspecBasePath>
  </PropertyGroup>
</Project>
```

### Advanced extension points to create customized package

The `pack` target provides two extension points that run in the inner, target framework specific build. The extension points support including target framework specific content and assemblies into a package:

- `TargetsForTfmSpecificBuildOutput` target: Use for files inside the `lib` folder or a folder specified using `BuildOutputTargetFolder`.
- `TargetsForTfmSpecificContentInPackage` target: Use for files outside the `BuildOutputTargetFolder`.

#### TargetsForTfmSpecificBuildOutput

Write a custom target and specify it as the value of the `$(TargetsForTfmSpecificBuildOutput)` property. For any files that need to go into the `BuildOutputTargetFolder` (lib by default), the target should write those files into the ItemGroup `BuildOutputInPackage` and set the following two metadata values:

- `FinalOutputPath`: The absolute path of the file; if not provided, the Identity is used to evaluate source path.
- `TargetPath`:  (Optional) Set when the file needs to go into a subfolder within `lib\<TargetFramework>` , like satellite assemblies that go under their respective culture folders. Defaults to the name of the file.

Example:

```xml
<PropertyGroup>
  <TargetsForTfmSpecificBuildOutput>$(TargetsForTfmSpecificBuildOutput);GetMyPackageFiles</TargetsForTfmSpecificBuildOutput>
</PropertyGroup>

<Target Name="GetMyPackageFiles">
  <ItemGroup>
    <BuildOutputInPackage Include="$(OutputPath)cs\$(AssemblyName).resources.dll">
        <TargetPath>cs</TargetPath>
    </BuildOutputInPackage>
  </ItemGroup>
</Target>
```

#### TargetsForTfmSpecificContentInPackage

Write a custom target and specify it as the value of the `$(TargetsForTfmSpecificContentInPackage)` property. For any files to include in the package, the target should write those files into the ItemGroup `TfmSpecificPackageFile` and set the following optional metadata:

- `PackagePath`: Path where the file should be output in the package. NuGet issues a warning if more than one file is added to the same package path.
- `BuildAction`: The build action to assign to the file, required only if the package path is in the `contentFiles` folder. Defaults to "None".

An example:
```xml
<PropertyGroup>
  <TargetsForTfmSpecificContentInPackage>$(TargetsForTfmSpecificContentInPackage);CustomContentTarget</TargetsForTfmSpecificContentInPackage>
</PropertyGroup>

<Target Name="CustomContentTarget">
  <ItemGroup>
    <TfmSpecificPackageFile Include="abc.txt">
      <PackagePath>mycontent/$(TargetFramework)</PackagePath>
    </TfmSpecificPackageFile>
    <TfmSpecificPackageFile Include="Extensions/ext.txt" Condition="'$(TargetFramework)' == 'net46'">
      <PackagePath>net46content</PackagePath>
    </TfmSpecificPackageFile>  
  </ItemGroup>
</Target>  
```

## restore target

`MSBuild -t:restore` (which `nuget restore` and `dotnet restore` use with .NET Core projects), restores packages referenced in the project file as follows:

1. Read all project to project references
1. Read the project properties to find the intermediate folder and target frameworks
1. Pass MSBuild data to NuGet.Build.Tasks.dll
1. Run restore
1. Download packages
1. Write assets file, targets, and props

The `restore` target works **only** for projects using the PackageReference format. It does **not** work for projects using the `packages.config` format; use [nuget restore](../reference/cli-reference/cli-ref-restore.md) instead.

### Restore properties

Additional restore settings may come from MSBuild properties in the project file. Values can also be set from the command line using the `-p:` switch (see Examples below).

| Property | Description |
|--------|--------|
| RestoreSources | Semicolon-delimited list of package sources. |
| RestorePackagesPath | User packages folder path. |
| RestoreDisableParallel | Limit downloads to one at a time. |
| RestoreConfigFile | Path to a `Nuget.Config` file to apply. |
| RestoreNoCache | If true, avoids using cached packages. See [Managing the global packages and cache folders](../consume-packages/managing-the-global-packages-and-cache-folders.md). |
| RestoreIgnoreFailedSources | If true, ignores failing or missing package sources. |
| RestoreFallbackFolders | Fallback folders, used in the same way the user packages folder is used. |
| RestoreAdditionalProjectSources | Additional sources to use during restore. |
| RestoreAdditionalProjectFallbackFolders | Additional fallback folders to use during restore. |
| RestoreAdditionalProjectFallbackFoldersExcludes | Excludes fallback folders specified in `RestoreAdditionalProjectFallbackFolders` |
| RestoreTaskAssemblyFile | Path to `NuGet.Build.Tasks.dll`. |
| RestoreGraphProjectInput | Semicolon-delimited list of projects to restore, which should contain absolute paths. |
| RestoreUseSkipNonexistentTargets  | When the projects are collected via MSBuild it determines whether they are collected using the `SkipNonexistentTargets` optimization. When not set, defaults to `true`. The consequence is a fail-fast behavior when a project's targets cannot be imported. |
| MSBuildProjectExtensionsPath | Output folder, defaulting to `BaseIntermediateOutputPath` and the `obj` folder. |
| RestoreForce | In PackageReference based projects, forces all dependencies to be resolved even if the last restore was successful. Specifying this flag is similar to deleting the `project.assets.json` file. This does not bypass the http-cache. |
| RestorePackagesWithLockFile | Opts into the usage of a lock file. |
| RestoreLockedMode | Run restore in locked mode. This means that restore will not reevaluate the dependencies. |
| NuGetLockFilePath | A custom location for the lock file. The default location is next to the project and is named `packages.lock.json`. |
| RestoreForceEvaluate | Forces restore to recompute the dependencies and update the lock file without any warning. | 

#### Examples

Command line:

```cli
msbuild -t:restore -p:RestoreConfigFile=<path>
```

Project file:

```xml
<PropertyGroup>
    <RestoreIgnoreFailedSource>true</RestoreIgnoreFailedSource>
</PropertyGroup>
```

### Restore outputs

Restore creates the following files in the build `obj` folder:

| File | Description |
|--------|--------|
| `project.assets.json` | Contains the dependency graph of all package references. |
| `{projectName}.projectFileExtension.nuget.g.props` | References to MSBuild props contained in packages |
| `{projectName}.projectFileExtension.nuget.g.targets` | References to MSBuild targets contained in packages |

### Restoring and building with one MSBuild command

Due to the fact that NuGet can restore packages that bring down MSBuild targets and props, the restore and build evaluations are run with different global properties.
This means that the following will have an unpredictable and often incorrect behavior.

```cli
msbuild -t:restore,build
```

 Instead the recommended approach is:

```cli
msbuild -t:build -restore
```

The same logic applies to other targets similar to `build`.

### PackageTargetFallback

The `PackageTargetFallback` element allows you to specify a set of compatible targets to be used when restoring packages. It's designed to allow packages that use a dotnet [TxM](../reference/target-frameworks.md) to work with compatible packages that don't declare a dotnet TxM. That is, if your project uses the dotnet TxM, then all the packages it depends on must also have a dotnet TxM, unless you add the `<PackageTargetFallback>` to your project in order to allow non-dotnet platforms to be compatible with dotnet.

For example, if the project is using the `netstandard1.6` TxM, and a dependent package contains only `lib/net45/a.dll` and `lib/portable-net45+win81/a.dll`, then the project will fail to build. If what you want to bring in is the latter DLL, then you can add a `PackageTargetFallback` as follows to say that the `portable-net45+win81` DLL is compatible:

```xml
<PackageTargetFallback Condition="'$(TargetFramework)'=='netstandard1.6'">
    portable-net45+win81
</PackageTargetFallback>
```

To declare a fallback for all targets in your project, leave off the `Condition` attribute. You can also extend any existing `PackageTargetFallback` by including `$(PackageTargetFallback)` as shown here:

```xml
<PackageTargetFallback>
    $(PackageTargetFallback);portable-net45+win81
</PackageTargetFallback >
```

### Replacing one library from a restore graph

If a restore is bringing the wrong assembly, it's possible to exclude that packages default choice, and replace it with your own choice. First with a top level `PackageReference`, exclude all assets:

```xml
<PackageReference Include="Newtonsoft.Json" Version="9.0.1">
  <ExcludeAssets>All</ExcludeAssets>
</PackageReference>
```

Next, add your own reference to the appropriate local copy of the DLL:

```xml
<Reference Include="Newtonsoft.Json.dll" />
```
