---
title: NuGet pack and restore as MSBuild targets
description: NuGet pack and restore can work directly as MSBuild targets with NuGet 4.0+.
author: nkolev92
ms.author: nikolev
ms.date: 09/02/2021
ms.topic: conceptual
no-loc: [NuGet, MSBuild, .nuspec, nuspec]
---

# NuGet pack and restore as MSBuild targets

*NuGet 4.0+*

With the [PackageReference](../consume-packages/package-references-in-project-files.md) format, NuGet 4.0+ can store all manifest metadata directly within a project file rather than using a separate `.nuspec` file.

With MSBuild 15.1+, NuGet is also a first-class MSBuild citizen with the `pack` and `restore` targets as described below. These targets allow you to work with NuGet as you would with any other MSBuild task or target. For instructions creating a NuGet package using MSBuild, see [Create a NuGet package using MSBuild](../create-packages/creating-a-package-msbuild.md). (For NuGet 3.x and earlier, you use the [pack](../reference/cli-reference/cli-ref-pack.md) and [restore](../reference/cli-reference/cli-ref-restore.md) commands through the NuGet CLI instead.)

## Target build order

Because `pack` and `restore` are  MSBuild targets, you can access them to enhance your workflow. For example, let's say you want to copy your package to a network share after packing it. You can do that by adding the following in your project file:

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

For .NET projects that use the `PackageReference` format, using `msbuild -t:pack` draws inputs from the project file to use in creating a NuGet package.

The following table describes the MSBuild properties that can be added to a project file within the first `<PropertyGroup>` node. You can make these edits easily in Visual Studio 2017 and later by right-clicking the project and selecting **Edit {project_name}** on the context menu. For convenience, the table is organized by the equivalent property in a [`.nuspec` file](../reference/nuspec.md).

> [!NOTE]
> `Owners` and `Summary` properties from `.nuspec` are not supported with MSBuild.

| Attribute/nuspec Value | MSBuild Property | Default | Notes |
|--------|--------|--------|--------|
| `Id` | `PackageId` | `$(AssemblyName)` | `$(AssemblyName)` from MSBuild |
| `Version` | `PackageVersion` | Version | This is semver compatible, for example `1.0.0`, `1.0.0-beta`, or `1.0.0-beta-00345`. Defaults to `Version` if not set. |
| `VersionPrefix` | `VersionPrefix` | empty | Setting `PackageVersion` overwrites `VersionPrefix` |
| `VersionSuffix` | `VersionSuffix` | empty | Setting `PackageVersion` overwrites `VersionSuffix` |
| `Authors` | `Authors` | Username of the current user | A semicolon-separated list of packages authors, matching the profile names on nuget.org. These are displayed in the NuGet Gallery on nuget.org and are used to cross-reference packages by the same authors. |
| `Owners` | N/A | Not present in nuspec |
| `Title` | `Title` | `$(PackageId)` | A human-friendly title of the package, typically used in UI displays as on nuget.org and the Package Manager in Visual Studio. |
| `Description` | `Description` | "Package Description" | A long description for the assembly. If `PackageDescription` is not specified, then this property is also used as the description of the package. |
| `Copyright` | `Copyright` | empty | Copyright details for the package. |
| `RequireLicenseAcceptance` | `PackageRequireLicenseAcceptance` | `false` | A Boolean value that specifies whether the client must prompt the consumer to accept the package license before installing the package. |
| `license` | `PackageLicenseExpression` | empty | Corresponds to `<license type="expression">`. See [Packing a license expression or a license file](#packing-a-license-expression-or-a-license-file). |
| `license` | `PackageLicenseFile` | empty | Path to a license file within the package if you're using a custom license or a license that hasn't been assigned an SPDX identifier. You need to explicitly pack the referenced license file. Corresponds to `<license type="file">`. See [Packing a license expression or a license file](#packing-a-license-expression-or-a-license-file). |
| `LicenseUrl` | `PackageLicenseUrl` | empty | `PackageLicenseUrl` is deprecated. Use `PackageLicenseExpression` or `PackageLicenseFile` instead. |
| `ProjectUrl` | `PackageProjectUrl` | empty | |
| `Icon` | `PackageIcon` | empty | A path to an image in the package to use as a package icon. You need to explicitly pack the referenced icon image file. For more information, see [Packing an icon image file](#packing-an-icon-image-file) and [`icon` metadata](./nuspec.md#icon). |
| `IconUrl` | `PackageIconUrl` | empty | `PackageIconUrl` is deprecated in favor of `PackageIcon`. However, for the best downlevel experience, you should specify `PackageIconUrl` in addition to `PackageIcon`. |
| `Readme` | `PackageReadmeFile` | empty | You need to explicitly pack the referenced readme file.|
| `Tags` | `PackageTags` | empty | A semicolon-delimited list of tags that designates the package. |
| `ReleaseNotes` | `PackageReleaseNotes` | empty | Release notes for the package. |
| `Repository/Url` | `RepositoryUrl` | empty | Repository URL used to clone or retrieve source code. Example: *https://github.com/NuGet/NuGet.Client.git*. |
| `Repository/Type` | `RepositoryType` | empty | Repository type. Examples: `git` (default), `tfs`. |
| `Repository/Branch` | `RepositoryBranch` | empty | Optional repository branch information. `RepositoryUrl` must also be specified for this property to be included. Example: *master* (NuGet 4.7.0+). |
| `Repository/Commit` | `RepositoryCommit` | empty | Optional repository commit or changeset to indicate which source the package was built against. `RepositoryUrl` must also be specified for this property to be included. Example: *0e4d1b598f350b3dc675018d539114d1328189ef* (NuGet 4.7.0+). |
| `PackageType` | `<PackageType>CustomType1, 1.0.0.0;CustomType2</PackageType>` | | Indicates the package's intended use. Package types use the same format as package IDs and are delimited by `;`. Package types may be versioned by appending a `,` and a [`Version`](/dotnet/api/system.version) string. See [Set a NuGet package type](../create-packages/set-package-type.md) (NuGet 3.5.0+). |
| `Summary` | Not supported | | |

### pack target inputs

| Property | Description |
| - | - |
| `IsPackable` | A Boolean value that specifies whether the project can be packed. The default value is `true`. |
| `SuppressDependenciesWhenPacking` | Set to `true` to suppress package dependencies from the generated NuGet package. |
| `PackageVersion` | Specifies the version that the resulting package will have. Accepts all forms of NuGet version string. Default is the value of `$(Version)`, that is, of the property `Version` in the project. |
| `PackageId` | Specifies the name for the resulting package. If not specified, the `pack` operation will default to using the `AssemblyName` or directory name as the name of the package. |
| `PackageDescription` | A long description of the package for UI display. |
| `Authors` | A semicolon-separated list of packages authors, matching the profile names on nuget.org. These are displayed in the NuGet Gallery on nuget.org and are used to cross-reference packages by the same authors. |
| `Description` | A long description for the assembly. If `PackageDescription` is not specified, then this property is also used as the description of the package. |
| `Copyright` | Copyright details for the package. |
| `PackageRequireLicenseAcceptance` | A Boolean value that specifies whether the client must prompt the consumer to accept the package license before installing the package. The default is `false`. |
| `DevelopmentDependency` | A Boolean value that specifies whether the package is marked as a development-only dependency, which prevents the package from being included as a dependency in other packages. With `PackageReference` (NuGet 4.8+), this flag also means that compile-time assets are excluded from compilation. For more information, see [DevelopmentDependency support for PackageReference](https://github.com/NuGet/Home/wiki/DevelopmentDependency-support-for-PackageReference). |
| `PackageLicenseExpression` | An [SPDX license identifier](https://spdx.org/licenses/) or expression, for example, `Apache-2.0`. For more information, see [Packing a license expression or a license file](#packing-a-license-expression-or-a-license-file). |
| `PackageLicenseFile` | Path to a license file within the package if you're using a custom license or a license that hasn't been assigned an SPDX identifier. |
| `PackageLicenseUrl` | `PackageLicenseUrl` is deprecated. Use `PackageLicenseExpression` or `PackageLicenseFile` instead. |
| `PackageProjectUrl` | |
| `PackageIcon` | Specifies the package icon path, relative to the root of the package. For more information, see [Packing an icon image file](#packing-an-icon-image-file). |
| `PackageReleaseNotes` | Release notes for the package. |
| `PackageReadmeFile` | Readme for the package. |
| `PackageTags` | A semicolon-delimited list of tags that designates the package. |
| `PackageOutputPath` | Determines the output path in which the packed package will be dropped. Default is `$(OutputPath)`. |
| `IncludeSymbols` | This Boolean value indicates whether the package should create an additional symbols package when the project is packed. The symbols package's format is controlled by the `SymbolPackageFormat` property. For more information, see [IncludeSymbols](#includesymbols). |
| `IncludeSource` | This Boolean value indicates whether the pack process should create a source package. The source package contains the library's source code as well as PDB files. Source files are put under the `src/ProjectName` directory in the resulting package file. For more information, see [IncludeSource](#includesource). |
| `PackageType` | |
| `IsTool` | Specifies whether all output files are copied to the *tools* folder instead of the *lib* folder. For more information, see [IsTool](#istool). |
| `RepositoryUrl` | Repository URL used to clone or retrieve source code. Example: *https://github.com/NuGet/NuGet.Client.git*. |
| `RepositoryType` | Repository type. Examples: `git` (default), `tfs`. |
| `RepositoryBranch` | Optional repository branch information. `RepositoryUrl` must also be specified for this property to be included. Example: *master* (NuGet 4.7.0+). |
| `RepositoryCommit` | Optional repository commit or changeset to indicate which source the package was built against. `RepositoryUrl` must also be specified for this property to be included. Example: *0e4d1b598f350b3dc675018d539114d1328189ef* (NuGet 4.7.0+). |
| `SymbolPackageFormat` | Specifies the format of the symbols package. If "symbols.nupkg", a legacy symbols package is created with a *.symbols.nupkg* extension containing PDBs, DLLs, and other output files. If "snupkg", a snupkg symbol package is created containing the portable PDBs. The default is "symbols.nupkg". |
| `NoPackageAnalysis` | Specifies that `pack` should not run package analysis after building the package. |
| `MinClientVersion` | Specifies the minimum version of the NuGet client that can install this package, enforced by nuget.exe and the Visual Studio Package Manager. |
| `IncludeBuildOutput` | This Boolean value specifies whether the build output assemblies should be packed into the *.nupkg* file or not. |
| `IncludeContentInPack` | This Boolean value specifies whether any items that have a type of `Content` are included in the resulting package automatically. The default is `true`. |
| `BuildOutputTargetFolder` | Specifies the folder where to place the output assemblies. The output assemblies (and other output files) are copied into their respective framework folders. For more information, see [Output assemblies](#output-assemblies). |
| `ContentTargetFolders` | Specifies the default location of where all the content files should go if `PackagePath` is not specified for them. The default value is "content;contentFiles". For more information, see [Including content in a package](#including-content-in-a-package). |
| `NuspecFile` | Relative or absolute path to the *.nuspec* file being used for packing. If specified, it's used **exclusively** for packaging information, and any information in the projects is not used. For more information, see [Packing using a .nuspec](#packing-using-a-nuspec-file). |
| `NuspecBasePath` | Base path for the *.nuspec* file. For more information, see [Packing using a .nuspec](#packing-using-a-nuspec-file). |
| `NuspecProperties` | Semicolon separated list of key=value pairs. For more information, see [Packing using a .nuspec](#packing-using-a-nuspec-file). |

## pack scenarios

### Suppressing dependencies

To suppress package dependencies from generated NuGet package, set `SuppressDependenciesWhenPacking` to `true` which will allow skipping all the dependencies from generated nupkg file.

### `PackageIconUrl`

`PackageIconUrl` is deprecated in favor of the [`PackageIcon`](#packageicon) property. Starting with NuGet 5.3 and Visual Studio 2019 version 16.3, `pack` raises the [NU5048](./errors-and-warnings/nu5048.md) warning if the package metadata only specifies `PackageIconUrl`.

### `PackageIcon`

> [!Tip]
> To maintain backward compatibility with clients and sources that don't yet support `PackageIcon`, specify both `PackageIcon` and `PackageIconUrl`. Visual Studio supports `PackageIcon` for packages coming from a folder-based source.

#### Packing an icon image file

When packing an icon image file, use `PackageIcon` property to specify the icon file path, relative to the root of the package. In addition, make sure that the file is included in the package. Image file size is limited to 1 MB. Supported file formats include JPEG and PNG. We recommend an image resolution of 128x128.

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

[Package Icon sample](https://github.com/NuGet/Samples/tree/main/PackageIconExample).

For the nuspec equivalent, take a look at [nuspec reference for icon](nuspec.md#icon).

### PackageReadmeFile

*Supported with **NuGet 5.10.0 preview 2** / **.NET SDK 5.0.300** and above*

When packing a readme file, you need to use the `PackageReadmeFile` property to specify the package path, relative to the root of the package. In addition to this, you need to make sure that the file is included in the package. Supported file formats include only Markdown (*.md*).

For example:

```xml
<PropertyGroup>
    ...
    <PackageReadmeFile>readme.md</PackageReadmeFile>
    ...
</PropertyGroup>

<ItemGroup>
    ...
    <None Include="docs\readme.md" Pack="true" PackagePath="\"/>
    ...
</ItemGroup>
```

For the nuspec equivalent, take a look at [nuspec reference for readme](nuspec.md#readme).

### Output assemblies

`nuget pack` copies output files with extensions `.exe`, `.dll`, `.xml`, `.winmd`, `.json`, and `.pri`. The output files that are copied depend on what MSBuild provides from the `BuiltOutputProjectGroup` target.

There are two MSBuild  properties that you can use in your project file or command line to control where output assemblies go:

- `IncludeBuildOutput`: A boolean that determines whether the build output assemblies should be included in the package.
- `BuildOutputTargetFolder`: Specifies the folder in which the output assemblies should be placed. The output assemblies (and other output files) are copied into their respective framework folders.

### Package references

See [Package References in Project Files](../consume-packages/package-references-in-project-files.md).

### Project to project references

Project to project references are considered by default as NuGet package references. For example:

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

When using a license expression, use the `PackageLicenseExpression` property. For a sample, see [License expression sample](https://github.com/NuGet/Samples/tree/main/PackageLicenseExpressionExample).

```xml
<PropertyGroup>
    <PackageLicenseExpression>MIT</PackageLicenseExpression>
</PropertyGroup>
```

To learn more about license expressions and licenses that are accepted by NuGet.org, see [license metadata](nuspec.md#license).

When packing a license file, use `PackageLicenseFile` property to specify the package path, relative to the root of the package. In addition, make sure that the file is included in the package. For example:

```xml
<PropertyGroup>
    <PackageLicenseFile>LICENSE.txt</PackageLicenseFile>
</PropertyGroup>

<ItemGroup>
    <None Include="licenses\LICENSE.txt" Pack="true" PackagePath=""/>
</ItemGroup>
```

For a sample, see [License file sample](https://github.com/NuGet/Samples/tree/main/PackageLicenseFileExample).

> [!NOTE]
> Only one of `PackageLicenseExpression`, `PackageLicenseFile`, and `PackageLicenseUrl` can be specified at a time.

### Packing a file without an extension

In some scenarios, like when packing a license file, you might want to include a file without an extension.
For historical reasons, NuGet & MSBuild treat paths without an extension as directories.

```xml
  <PropertyGroup>
    <TargetFrameworks>netstandard2.0</TargetFrameworks>
    <PackageLicenseFile>LICENSE</PackageLicenseFile>
  </PropertyGroup>

  <ItemGroup>
    <None Include="LICENSE" Pack="true" PackagePath=""/>
  </ItemGroup>  
```

[File without an extension sample](https://github.com/NuGet/Samples/blob/main/PackageLicenseFileExtensionlessExample/).

### IsTool

When using `MSBuild -t:pack -p:IsTool=true`, all output files, as specified in the [Output Assemblies](#output-assemblies) scenario, are copied to the `tools` folder instead of the `lib` folder. Note that this is different from a `DotNetCliTool` which is specified by setting the `PackageType` in `.csproj` file.

### Packing using a `.nuspec` file

Although it is recommended that you [include all the properties](../reference/msbuild-targets.md#pack-target) that are usually in the `.nuspec` file in the project file instead, you can choose to use a `.nuspec` file to pack your project. For a non-SDK-style project that uses `PackageReference`, you must import `NuGet.Build.Tasks.Pack.targets` so that the pack task can be executed. You still need to restore the project before you can pack a nuspec file. (An SDK-style project includes the pack targets by default.)

The target framework of the project file is irrelevant and not used when packing a nuspec. The following three MSBuild properties are relevant to packing using a `.nuspec`:

1. `NuspecFile`: relative or absolute path to the `.nuspec` file being used for packing.
1. `NuspecProperties`: a semicolon-separated list of key=value pairs. Due to the way MSBuild command-line parsing works, multiple properties must be specified as follows: `-p:NuspecProperties="key1=value1;key2=value2"`.  
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

#### `TargetsForTfmSpecificBuildOutput`

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

#### `TargetsForTfmSpecificContentInPackage`

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

The `restore` target works for projects using the PackageReference format.
`MSBuild 16.5+` also has [opt-in support](#restoring-packagereference-and-packagesconfig-projects-with-msbuild) for the `packages.config` format.

> [!NOTE]
> The `restore` target [should not be run](#restoring-and-building-with-one-msbuild-command) in combination with the `build` target.

### Restore properties

Additional restore settings may come from MSBuild properties in the project file. Values can also be set from the command line using the `-p:` switch (see Examples below).

| Property | Description |
|--------|--------|
| `RestoreSources` | Semicolon-delimited list of package sources. |
| `RestorePackagesPath` | User packages folder path. |
| `RestoreDisableParallel` | Limit downloads to one at a time. |
| `RestoreConfigFile` | Path to a `Nuget.Config` file to apply. |
| `RestoreNoCache` | If true, avoids using cached packages. See [Managing the global packages and cache folders](../consume-packages/managing-the-global-packages-and-cache-folders.md). |
| `RestoreIgnoreFailedSources` | If true, ignores failing or missing package sources. |
| `RestoreFallbackFolders` | Fallback folders, used in the same way the user packages folder is used. |
| `RestoreAdditionalProjectSources` | Additional sources to use during restore. |
| `RestoreAdditionalProjectFallbackFolders` | Additional fallback folders to use during restore. |
| `RestoreAdditionalProjectFallbackFoldersExcludes` | Excludes fallback folders specified in `RestoreAdditionalProjectFallbackFolders` |
| `RestoreTaskAssemblyFile` | Path to `NuGet.Build.Tasks.dll`. |
| `RestoreGraphProjectInput` | Semicolon-delimited list of projects to restore, which should contain absolute paths. |
| `RestoreUseSkipNonexistentTargets`  | When the projects are collected via MSBuild it determines whether they are collected using the `SkipNonexistentTargets` optimization. When not set, defaults to `true`. The consequence is a fail-fast behavior when a project's targets cannot be imported. |
| `MSBuildProjectExtensionsPath` | Output folder, defaulting to `BaseIntermediateOutputPath` and the `obj` folder. |
| `RestoreForce` | In PackageReference based projects, forces all dependencies to be resolved even if the last restore was successful. Specifying this flag is similar to deleting the `project.assets.json` file. This does not bypass the http-cache. |
| `RestorePackagesWithLockFile` | Opts into the usage of a lock file. |
| `RestoreLockedMode` | Run restore in locked mode. This means that restore will not reevaluate the dependencies. |
| `NuGetLockFilePath` | A custom location for the lock file. The default location is next to the project and is named `packages.lock.json`. |
| `RestoreForceEvaluate` | Forces restore to recompute the dependencies and update the lock file without any warning. |
| `RestorePackagesConfig` | An opt-in switch, that restores projects with packages.config. Support with `MSBuild -t:restore` only. |
| `RestoreRepositoryPath` | packages.config only. Specifies the packages directory to which the packages should be restored. `SolutionDirectory` will be used if not specified. |
| `RestoreUseStaticGraphEvaluation` | An opt-in switch to use static graph MSBuild evaluation instead of the standard evaluation. Static graph evaluation is an experimental feature that's significantly faster for large repos and solutions. |

The `ExcludeRestorePackageImports` property is an internal property used by NuGet.
It should not be modified or set in any MSBuild files.

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

### Restoring PackageReference and packages.config projects with MSBuild

With MSBuild 16.5+, packages.config are also supported for `msbuild -t:restore`.

```cli
msbuild -t:restore -p:RestorePackagesConfig=true
```

> [!NOTE]
> `packages.config` restore is only available with `MSBuild 16.5+`, and not with `dotnet.exe`

### Restoring with MSBuild static graph evaluation

> [!NOTE]
> With MSBuild 16.6+, NuGet has added an experimental feature to use static graph evaluation from the command line that significantly improves the restore time for large repositories.

```cli
msbuild -t:restore -p:RestoreUseStaticGraphEvaluation=true
```

Alternatively you can enable it by setting the property in a Directory.Build.Props.

```xml
<Project>
  <PropertyGroup>
    <RestoreUseStaticGraphEvaluation>true</RestoreUseStaticGraphEvaluation>
  </PropertyGroup>
</Project>
```

> [!NOTE]
> As of Visual Studio 2019.x and NuGet 5.x, this feature is considered experimental and opt-in. Follow [NuGet/Home#9803](https://github.com/NuGet/Home/issues/9803) for details on when this feature will be enabled by default.

Static graph restore changes the msbuild part of restore, the project reading and evaluation, but not the restore algorithm! The restore algorithm is the same across all NuGet tools (NuGet.exe, MSBuild.exe, dotnet.exe and Visual Studio).

In very few scenarios, static graph restore may behave differently from current restore and certain declared PackageReferences or ProjectReferences might be missing.

To ease your mind, as a one time check, when migrating to static graph restore, consider running:

```cli
msbuild.exe -t:restore -p:RestoreUseStaticGraphEvaluation=true
msbuild.exe -t:restore
```

NuGet should *not* report any changes. If you do see a discrepancy, please file an issue at [NuGet/Home](https://github.com/nuget/home/issues/new).

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
