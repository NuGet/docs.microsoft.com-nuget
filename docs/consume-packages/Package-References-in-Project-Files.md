---
title: NuGet PackageReference in project files
description: Details on NuGet PackageReference in project files as supported by NuGet 4.0+ and VS2017 and .NET Core 2.0
author: nkolev92
ms.author: nikolev
ms.date: 4/6/2022
ms.topic: conceptual
---

# `PackageReference` in project files

Package references, using `<PackageReference>` MSBuild items, specify NuGet package dependencies directly within project files, as opposed to having a separate `packages.config` file. Use of PackageReference doesn't affect other aspects of NuGet; for example, settings in `NuGet.Config` files (including package sources) are still applied as explained in [Common NuGet configurations](configuring-nuget-behavior.md).

With PackageReference, you can also use MSBuild conditions to choose package references per target framework, or other groupings. It also allows for fine-grained control over dependencies and content flow. (See For more details [NuGet pack and restore as MSBuild targets](../reference/msbuild-targets.md).)

## Project type support

By default, PackageReference is used for .NET Core projects, .NET Standard projects, and UWP projects targeting Windows 10 Build 15063 (Creators Update) and later, with the exception of C++ UWP projects. .NET Framework projects support PackageReference, but currently default to `packages.config`. To use PackageReference, [migrate](../consume-packages/migrate-packages-config-to-package-reference.md) the dependencies from `packages.config` into your project file, then remove packages.config.

ASP.NET apps targeting the full .NET Framework include only [limited support](https://github.com/NuGet/Home/issues/5877) for PackageReference. C++ and JavaScript project types are unsupported.

## Adding a PackageReference

Add a dependency in your project file using the following syntax:

```xml
<ItemGroup>
    <!-- ... -->
    <PackageReference Include="Contoso.Utility.UsefulStuff" Version="3.6.0" />
    <!-- ... -->
</ItemGroup>
```

## Controlling dependency version

The convention for specifying the version of a package is the same as when using `packages.config`:

```xml
<ItemGroup>
    <!-- ... -->
    <PackageReference Include="Contoso.Utility.UsefulStuff" Version="3.6.0" />
    <!-- ... -->
</ItemGroup>
```

In the example above, 3.6.0 means any version that is >=3.6.0 with preference for the lowest version, as described on [Package versioning](../concepts/package-versioning.md#version-ranges).

## Using PackageReference for a project with no package dependencies

Advanced: If you have no packages installed in a project (no PackageReferences in project file and no packages.config file), but want the project to be restored as PackageReference style, you can set a Project property RestoreProjectStyle to PackageReference in your project file.

```xml
<PropertyGroup>
    <!--- ... -->
    <RestoreProjectStyle>PackageReference</RestoreProjectStyle>
    <!--- ... -->
</PropertyGroup>    
```

This may be useful, if you reference projects which are PackageReference styled (existing csproj or SDK-style projects). This will enable packages that those projects refer to, to be "transitively" referenced by your project.

## PackageReference and sources

In PackageReference projects, the transitive dependency versions are resolved at restore time. As such, in PackageReference projects all sources need to be available for all restores. 

## Floating Versions

[Floating versions](../concepts/dependency-resolution.md#floating-versions) are supported with `PackageReference`:

```xml
<ItemGroup>
    <!-- ... -->
    <PackageReference Include="Contoso.Utility.UsefulStuff" Version="3.6.*" />
    <PackageReference Include="Contoso.Utility.UsefulStuff" Version="3.6.0-beta.*" />
    <!-- ... -->
</ItemGroup>
```

## Controlling dependency assets

You might be using a dependency purely as a development harness and might not want to expose that to projects that will consume your package. In this scenario, you can use the `PrivateAssets` metadata to control this behavior.

```xml
<ItemGroup>
    <!-- ... -->

    <PackageReference Include="Contoso.Utility.UsefulStuff" Version="3.6.0">
        <PrivateAssets>all</PrivateAssets>
    </PackageReference>

    <!-- ... -->
</ItemGroup>
```

The following metadata tags control dependency assets:

| Tag | Description | Default Value |
| --- | --- | --- |
| IncludeAssets | These assets will be consumed | all |
| ExcludeAssets | These assets will not be consumed | none |
| PrivateAssets | These assets will be consumed but won't flow to the parent project | contentfiles;analyzers;build |

Allowable values for these tags are as follows, with multiple values separated by a semicolon except with `all` and `none` which must appear by themselves:

| Value | Description |
| --- | ---
| compile | Contents of the `lib` folder and controls whether your project can compile against the assemblies within the folder |
| runtime | Contents of the `lib` and `runtimes` folder and controls whether these assemblies will be copied out to the build output directory |
| contentFiles | Contents of the `contentfiles` folder |
| build | `.props` and `.targets` in the `build` folder |
| buildMultitargeting | *(4.0)* `.props` and `.targets` in the `buildMultitargeting` folder, for cross-framework targeting |
| buildTransitive | *(5.0+)* `.props` and `.targets` in the `buildTransitive` folder, for assets that flow transitively to any consuming project. See the [feature](https://github.com/NuGet/Home/wiki/Allow-package--authors-to-define-build-assets-transitive-behavior) page. |
| analyzers | .NET analyzers |
| native | Contents of the `native` folder |
| none | None of the above are used. |
| all | All of the above (except `none`) |

```xml
<ItemGroup>
    <!-- ... -->
    <!-- Everything except the content files will be consumed by the project -->
    <!-- Everything except content files and analyzers will flow to the parent project-->
    <PackageReference Include="Contoso.Utility.UsefulStuff" Version="3.6.0">
        <IncludeAssets>all</IncludeAssets> <!-- Default is `all`, can be omitted-->
        <ExcludeAssets>contentFiles</ExcludeAssets>
        <PrivateAssets>contentFiles;analyzers</PrivateAssets>
    </PackageReference>
    <!-- ... -->
    <!-- Everything except the compile will be consumed by the project -->
    <!-- Everything except contentFiles will flow to the parent project-->
    <PackageReference Include="Contoso.Utility.SomeOtherUsefulStuff" Version="3.6.0">
        <ExcludeAssets>compile</ExcludeAssets>
        <PrivateAssets>contentFiles</PrivateAssets>
    </PackageReference>
    <!-- ... -->
</ItemGroup>
```

Note that because `build` is not included with `PrivateAssets`, targets and props *will* flow to the parent project. Consider, for example, that the reference above is used in a project that builds a NuGet package called AppLogger. AppLogger can consume the targets and props from `Contoso.Utility.UsefulStuff`, as can projects that consume AppLogger.

> [!NOTE]
> When `developmentDependency` is set to `true` in a `.nuspec` file, this marks a package as a development-only dependency, which prevents the package from being included as a dependency in other packages. With PackageReference *(NuGet 4.8+)*, this flag also means that it will exclude compile-time assets from compilation. For more information, see [DevelopmentDependency support for PackageReference](https://github.com/NuGet/Home/wiki/DevelopmentDependency-support-for-PackageReference).

## Adding a PackageReference condition

You can use a condition to control whether a package is included, where conditions can use any MSBuild variable or a variable defined in the targets or props file. However, at presently, only the `TargetFramework` variable is supported.

For example, say you're targeting `netstandard1.4` as well as `net452` but have a dependency that is applicable only for `net452`. In this case you don't want a `netstandard1.4` project that's consuming your package to add that unnecessary dependency. To prevent this, you specify a condition on the `PackageReference` as follows:

```xml
<ItemGroup>
    <!-- ... -->
    <PackageReference Include="Newtonsoft.Json" Version="9.0.1" Condition="'$(TargetFramework)' == 'net452'" />
    <!-- ... -->
</ItemGroup>
```

A package built using this project will show that Newtonsoft.Json is included as a dependency only for a `net452` target:

![The result of applying a Condition on PackageReference with VS2017](media/PackageReference-Condition.png)

Conditions can also be applied at the `ItemGroup` level and will apply to all children `PackageReference` elements:

```xml
<ItemGroup Condition = "'$(TargetFramework)' == 'net452'">
    <!-- ... -->
    <PackageReference Include="Newtonsoft.Json" Version="9.0.1" />
    <PackageReference Include="Contoso.Utility.UsefulStuff" Version="3.6.0" />
    <!-- ... -->
</ItemGroup>
```

## GeneratePathProperty

This feature is available with NuGet **5.0** or above and with Visual Studio 2019 **16.0** or above.

Sometimes it is desirable to reference files in a package from an MSBuild target.
In `packages.config` based projects, the packages are installed in a folder relative to the project file. However in PackageReference, the packages are [consumed](../concepts/package-installation-process.md) from the *global-packages* folder, which can vary from machine to machine.

To bridge that gap, NuGet introduced a property that points to the location from which the package will be consumed.

Example:

```xml
  <ItemGroup>
      <PackageReference Include="Some.Package" Version="1.0.0" GeneratePathProperty="true" />
  </ItemGroup>

  <Target Name="TakeAction" AfterTargets="Build">
    <Exec Command="$(PkgSome_Package)\something.exe" />
  </Target>
````

Additionally NuGet will automatically generate properties for packages containing a tools folder.

```xml
  <ItemGroup>
      <PackageReference Include="Package.With.Tools" Version="1.0.0" />
  </ItemGroup>

  <Target Name="TakeAction" AfterTargets="Build">
    <Exec Command="$(PkgPackage_With_Tools)\tools\tool.exe" />
  </Target>
```

MSBuild properties and package identities do not have the same restrictions so the package identity needs to be changed to an MSBuild friendly name, prefixed by the word `Pkg`.
To verify the exact name of the property generated, look at the generated [nuget.g.props](../reference/msbuild-targets.md#restore-outputs) file.

## PackageReference Aliases

In some rare instances different packages will contain classes in the same namespace. Starting with NuGet 5.7 & Visual Studio 2019 Update 7, equivalent to ProjectReference, PackageReference supports [`Aliases`](/dotnet/api/microsoft.codeanalysis.projectreference.aliases).
By default no aliases are provided. When an alias is specified, *all* assemblies coming from the annotated package with need to be referenced with an alias.

You can look at sample usage at [NuGet\Samples](https://github.com/NuGet/Samples/tree/main/PackageReferenceAliasesExample)

In the project file, specify the aliases as follows:

```xml
  <ItemGroup>
    <PackageReference Include="NuGet.Versioning" Version="5.8.0" Aliases="ExampleAlias" />
  </ItemGroup>
```

and in the code use it as follows:

```cs
extern alias ExampleAlias;

namespace PackageReferenceAliasesExample
{
...
        {
            var version = ExampleAlias.NuGet.Versioning.NuGetVersion.Parse("5.0.0");
            Console.WriteLine($"Version : {version}");
        }
...
}

```

## NuGet warnings and errors

*This feature is available with NuGet **4.3** or above and with Visual Studio 2017 **15.3** or above.*

For many pack and restore scenarios, all NuGet warnings and errors are coded, and start with `NU****`. All NuGet warnings and errors are listed in the [reference](../reference/errors-and-warnings.md) documentation.

NuGet observes the following warning properties:

- `TreatWarningsAsErrors`, treat all warnings as errors
- `WarningsAsErrors`, treat specific warnings as errors
- `NoWarn`, hide specific warnings, either project-wide or package-wide.

Examples:

```xml
<PropertyGroup>
    <TreatWarningsAsErrors>true</TreatWarningsAsErrors>
</PropertyGroup>
...
<PropertyGroup>
    <WarningsAsErrors>$(WarningsAsErrors);NU1603;NU1605</WarningsAsErrors>
</PropertyGroup>
...
<PropertyGroup>
    <NoWarn>$(NoWarn);NU5124</NoWarn>
</PropertyGroup>
...
<ItemGroup>
    <PackageReference Include="Contoso.Package" Version="1.0.0" NoWarn="NU1605" />
</ItemGroup>
```

### Suppressing NuGet warnings

While it is recommended that you resolve all NuGet warnings during your pack and restore operations, in certain situations suppressing them is warranted.
To suppress a warning project wide, consider doing:

```xml
<PropertyGroup>
    <PackageVersion>5.0.0</PackageVersion>
    <NoWarn>$(NoWarn);NU5104</NoWarn>
</PropertyGroup>
<ItemGroup>
    <PackageReference Include="Contoso.Package" Version="1.0.0-beta.1"/>
</ItemGroup>
```

Sometimes warnings apply only to a certain package in the graph. We can choose to suppress that warning more selectively by adding a `NoWarn` on the PackageReference item. 

```xml
<PropertyGroup>
    <PackageVersion>5.0.0</PackageVersion>
</PropertyGroup>
<ItemGroup>
    <PackageReference Include="Contoso.Package" Version="1.0.0-beta.1" NoWarn="NU1603" />
</ItemGroup>
```

#### Suppressing NuGet package warnings in Visual Studio

When in Visual Studio, you can also [suppress warnings](/visualstudio/ide/how-to-suppress-compiler-warnings#suppress-warnings-for-nuget-packages
) through the IDE.

## Locking dependencies

*This feature is available with NuGet **4.9** or above and with Visual Studio 2017 **15.9** or above.*

Input to NuGet restore is a set of `PackageReference` items from the project file (top-level or direct dependencies) and the output is a full closure of all the package dependencies including transitive dependencies. NuGet tries to always produce the same full closure of package dependencies if the input PackageReference list has not changed. However, there are some scenarios where it is unable to do so. For example:

* When you use floating versions like `<PackageReference Include="My.Sample.Lib" Version="4.*"/>`. While the intention here is to float to the latest version on every restore of packages, there are scenarios where users require the graph to be locked to a certain latest version and float to a later version, if available, upon an explicit gesture.
* A newer version of the package matching PackageReference version requirements is published. E.g. 

  * Day 1: if you specified `<PackageReference Include="My.Sample.Lib" Version="4.0.0"/>` but the versions available on the 
  NuGet repositories were 4.1.0, 4.2.0 and 4.3.0. In this case, NuGet would have resolved to  4.1.0 (nearest minimum version)

  * Day 2: Version 4.0.0 gets published. NuGet will now find the exact match and start resolving to 4.0.0

* A given package version is removed from the repository. Though nuget.org does not allow package deletions, not all package repositories have this constraint. This results in NuGet finding the best match when it cannot resolve to the deleted version.

### Enabling the lock file

In order to persist the full closure of package dependencies you can opt-in to the lock file feature by setting the MSBuild property `RestorePackagesWithLockFile` for your project:

```xml
<PropertyGroup>
    <!--- ... -->
    <RestorePackagesWithLockFile>true</RestorePackagesWithLockFile>
    <!--- ... -->
</PropertyGroup>    
```

If this property is set, NuGet restore will generate a lock file - `packages.lock.json` file at the project root directory that lists all the package dependencies. 

> [!Note]
> Once a project has `packages.lock.json` file in its root directory, the lock file is always used with restore even if the property `RestorePackagesWithLockFile` is not set. So another way to opt-in to this feature is to create a dummy blank `packages.lock.json` file in the project's root directory.

### `restore` behavior with lock file
If a lock file is present for project, NuGet uses this lock file to run `restore`. NuGet does a quick check to see if there were any changes in the package dependencies as mentioned in the project file (or dependent projects' files) and if there were no changes it just restores the packages mentioned in the lock file. There is no re-evaluation of package dependencies.

If NuGet detects a change in the defined dependencies as mentioned in the project file(s), it re-evaluates the package graph and updates the lock file to reflect the new package closure for the project.

For CI/CD and other scenarios, where you would not want to change the package dependencies on the fly, you can do so by setting the `lockedmode` to `true`:

For dotnet.exe, run:

```
> dotnet.exe restore --locked-mode
```

For msbuild.exe, run:

```
> msbuild.exe -t:restore -p:RestoreLockedMode=true
```

You may also set this conditional MSBuild property in your project file:

```xml
<PropertyGroup>
    <!--- ... -->
    <RestoreLockedMode>true</RestoreLockedMode>
    <!--- ... -->
</PropertyGroup> 
```

If locked mode is `true`, restore will either restore the exact packages as listed in the lock file or fail if you updated the defined package dependencies for the project after lock file was created.

### Make lock file part of your source repository
If you are building an application, an executable and the project in question is at the start of the dependency chain then do check in the lock file to the source code repository so that NuGet can make use of it during restore.

However, if your project is a library project that you do not ship or a common code project on which other projects depend upon, you **should not** check in the lock file as part of your source code. There is no harm in keeping the lock file but the locked package dependencies for the common code project may not be used, as listed in the lock file, during the restore/build of a project that depends on this common-code project.

Eg.

```
ProjectA
  |------> PackageX 2.0.0
  |------> ProjectB
             |------>PackageX 1.0.0
```

If `ProjectA` has a dependency on a `PackageX` version `2.0.0` and also references `ProjectB` that depends on `PackageX` version `1.0.0`, then the lock file for `ProjectB` will list a dependency on `PackageX` version `1.0.0`. However, when `ProjectA` is built, its lock file will contain a dependency on `PackageX` version **`2.0.0`** and **not** `1.0.0` as listed in the lock file for `ProjectB`. Thus, the lock file of a common code project has little say over the packages resolved for projects that depend on it.

### Lock file extensibility

You can control various behaviors of restore with lock file as described below:

| NuGet.exe option | dotnet option | MSBuild equivalent option | Description |
|:--- |:--- |:--- |:--- |
| `-UseLockFile` |`--use-lock-file` | RestorePackagesWithLockFile | Opts into the usage of a lock file. |
| `-LockedMode` | `--locked-mode` | RestoreLockedMode | Enables locked mode for restore. This is useful in CI/CD scenarios where you want repeatable builds.|   
| `-ForceEvaluate` | `--force-evaluate` | RestoreForceEvaluate | This option is useful with packages with floating version defined in the project. By default, NuGet restore will not update the package version automatically upon each restore unless you run restore with this option. |
| `-LockFilePath` | `--lock-file-path` | NuGetLockFilePath | Defines a custom lock file location for a project. By default, NuGet supports `packages.lock.json` at the root directory. If you have multiple projects in the same directory, NuGet supports project specific lock file `packages.<project_name>.lock.json` |

## NuGet Dependency Resolver

The NuGet dependency resolver follows the [4 rules as described in the dependency resolution document](../../docs/concepts/Dependency-Resolution.md).

In order to improve the performance and scalability of the restore operation, the restore algorithm was rewritten in the 6.12 release.
As of the 6.12 release, the new restore algorithm is enabled by default for all PackageReference projects.
While the new restore algorithm is is functionally equivalent to the previous one, as with any software, bugs are possible.
To revert to the previous implementation, set the MSBuild property `RestoreUseLegacyDependencyResolver` to `true`.

Should you face restore failures in 6.12, .NET 9 or 17.12, that weren't reproducing in earlier versions, please [file an issue on GitHub](https://github.com/NuGet/Home/issues/).
Any potential discrepancies may manifest in different ways, such as during compilation or at runtime. There's also a chance that changes don't lead to failures, but just differences.
If you think you may be impacted by any changes, here are the steps you can take to verify whether the changes in the NuGet restore algorithm are the root cause.

Restore's intermediates generated in the `MSBuildProjectExtensionsPath` folder can help determine the equivalence between the 2 algorithms. Usually this is the `obj` folder of your build. You can use `msbuild.exe` or `dotnet.exe` for the next steps.

1. Remove the `obj` folder for your project.
1. Run `msbuild -t:restore`
1. Save the contents of the `obj` to a location indicating that it's the `new` behavior.
1. Run `msbuild -t:restore -p:RestoreUseLegacyDependencyResolver="true"`
1. Save the contents of the `obj` to a location indicating that it's the `legacy` behavior.
1. Analyze the files yourself, or create a NuGet/Home issue and we'll take a look.

If you follow the above method, there should be exactly 1 difference between the `project.assets.json` files:

```diff
      "projectStyle": "PackageReference",
+     "restoreUseLegacyDependencyResolver": true,
      "fallbackFolders": [
```

If there are any more differences, please [file an issue on GitHub](https://github.com/NuGet/Home/issues/) with all the details.

## AssetTargetFallback

The `AssetTargetFallback` property lets you specify additional compatible framework versions for projects that your project references and NuGet packages that your project consumes.

If you specify a package dependency using `PackageReference` but that package doesn't contain assets that are compatible with your projects's target framework, the `AssetTargetFallback` property comes into play. The compatibility of the referenced package is rechecked using each target framework that's specified in `AssetTargetFallback`.
When a `project` or a `package` is referenced through `AssetTargetFallback`, the [NU1701](../reference/errors-and-warnings/NU1701.md) warning will be raised.

Refer to the table below for examples of how `AssetTargetFallback` affects compatibility.

| Project framework | AssetTargetFallback | Package frameworks | Result |
|-------------------|---------------------|--------------------|--------|
| .NET Framework 4.7.2 | | .NET Standard 2.0 | .NET Standard 2.0 |
| .NET Core App 3.1 | | .NET Standard 2.0, .NET Framework 4.7.2 | .NET Standard 2.0 |
| .NET Core App 3.1 | | .NET Framework 4.7.2 | Incompatible, fail with [`NU1202`](../reference/errors-and-warnings/NU1202.md) |
| .NET Core App 3.1 | net472;net471 | .NET Framework 4.7.2 | .NET Framework 4.7.2 with [`NU1701`](../reference/errors-and-warnings/NU1701.md) |

Multiple frameworks can be specified using `;` as a delimiter. To add a fallback framework you can do the following:

```xml
<AssetTargetFallback Condition=" '$(TargetFramework)'=='netcoreapp3.1' ">
    $(AssetTargetFallback);net472;net471
</AssetTargetFallback>
```

You can leave off `$(AssetTargetFallback)` if you wish to overwrite, instead of add to the existing `AssetTargetFallback` values.

> [!NOTE]
> If you are using a [.NET SDK based project](/dotnet/core/sdk), appropriate `$(AssetTargetFallback)` values are configured and you do not need to set them manually.
>
> `$(PackageTargetFallback)` was an earlier feature that attempted to address this challenge, but it is fundamentally broken and *should* not be used. To migrate from `$(PackageTargetFallback)` to `$(AssetTargetFallback)`, simply change the property name.
