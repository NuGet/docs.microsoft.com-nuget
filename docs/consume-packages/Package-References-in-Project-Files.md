---
title: NuGet PackageReference format (package references in project files)
description: Details on NuGet PackageReference in project files as supported by NuGet 4.0+ and VS2017 and .NET Core 2.0
author: karann-msft
ms.author: karann
ms.date: 03/16/2018
ms.topic: conceptual
---

# Package references (PackageReference) in project files

Package references, using the `PackageReference` node, manage NuGet dependencies directly within project files (as opposed to a separate `packages.config` file). Using PackageReference, as it's called, doesn't affect other aspects of NuGet; for example, settings in `NuGet.Config` files (including package sources) are still applied as explained in [Configuring NuGet Behavior](configuring-nuget-behavior.md).

With PackageReference, you can also use MSBuild conditions to choose package references per target framework, configuration, platform, or other groupings. It also allows for fine-grained control over dependencies and content flow. (See For more details [NuGet pack and restore as MSBuild targets](../reference/msbuild-targets.md).)

By default, PackageReference is used for .NET Core projects, .NET Standard projects, and UWP projects targeting Windows 10 Build 15063 (Creators Update) and later, with the exception of C++ UWP projects. .NET Framework projects support PackageReference, but currently default to `packages.config`. To use PackageReference, migrate the dependencies from `packages.config` into your project file, then remove packages.config.

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

In the example above, 3.6.0 means any version that is >=3.6.0 with preference for the lowest version, as described on [Package versioning](../reference/package-versioning.md#version-ranges-and-wildcards).

## Using PackageReference for a project with no PackageReferences
Advanced: If you have no packages installed in a project (no PackageReferences in project file and no packages.config file), but want the project to be restored as PackageReference style, you can set a Project property RestoreProjectStyle to PackageReference in your project file.
```xml
<PropertyGroup>
    <!--- ... -->
    <RestoreProjectStyle>PackageReference</RestoreProjectStyle>
    <!--- ... -->
</PropertyGroup>    
```
This may be useful, if you reference projects which are PackageReference styled (existing csproj or SDK-style projects). This will enable packages that those projects refer to, to be "transitively" referenced by your project.

## Floating Versions

[Floating versions](../consume-packages/dependency-resolution.md#floating-versions) are supported with `PackageReference`:

```xml
<ItemGroup>
    <!-- ... -->
    <PackageReference Include="Contoso.Utility.UsefulStuff" Version="3.6.*" />
    <PackageReference Include="Contoso.Utility.UsefulStuff" Version="3.6.0-beta*" />
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
| build | Props and targets in the `build` folder |
| analyzers | .NET analyzers |
| native | Contents of the `native` folder |
| none | None of the above are used. |
| all | All of the above (except `none`) |

In the following example, everything except the content files from the package would be consumed by the project and everything except content files and analyzers would flow to the parent project.

```xml
<ItemGroup>
    <!-- ... -->

    <PackageReference Include="Contoso.Utility.UsefulStuff" Version="3.6.0">
        <IncludeAssets>all</IncludeAssets>
        <ExcludeAssets>contentFiles</ExcludeAssets>
        <PrivateAssets>contentFiles;analyzers</PrivateAssets>
    </PackageReference>

    <!-- ... -->
</ItemGroup>
```

Note that because `build` is not included with `PrivateAssets`, targets and props *will* flow to the parent project. Consider, for example, that the reference above is used in a project that builds a NuGet package called AppLogger. AppLogger can consume the targets and props from `Contoso.Utility.UsefulStuff`, as can projects that consume AppLogger.

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

## Locking dependencies
*This feature is available with NuGet **4.9** or above and with Visual Studio 2017 **15.9 Preview 5** or above.*

Input to NuGet restore is a set of Package References from the project file (top-level or direct dependenices) and the output is a full closure of all the package dependencies including transitive dependencies. NuGet tries to always produce the same full closure of package dependencies if the input PackageReference list has not changed. However, there are some scenarios where it is unable to do so. For example:

* When you use floating versions like `<PackageReference Include="My.Sample.Lib" Version="4.*"/>`. While the intention here is to float to the latest version on every restore of packages, there are scenarios where users require the graph to be locked to a certain latest version and float to a later version, if available, upon an explicit gesture.
* A newer version of the package matching PackageReference version requirements is published. E.g. 

  * Day 1: if you specified `<PackageReference Include="My.Sample.Lib" Version="4.0.0"/>` but the versions available on the 
  NuGet repositories were 4.1.0, 4.2.0 and 4.3.0. In this case, NuGet would have resolved to  4.1.0 (nearest minimum version)

  * Day 2: Version 4.0.0 gets published. NuGet will now find the exact match and start resolving to 4.0.0

* A given package version is removed from the repository. Though nuget.org does not allow package deletions, not all package repositories have this constraints. This results in NuGet finding the best match when it cannot resolve to the deleted version.

### Enabling lock file
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

If NuGet detects a change in the defined dependenices as mentioned in the project file(s), it re-evaluates the package graph and updates the lock file to reflect the new package closure for the project.

For CI/CD and other scenarios, where you would not want to change the package dependenies on the fly, you can do so by setting the `lockedmode` to `true`:

For dotnet.exe, run:
```
> dotnet.exe restore --locked-mode
```

For msbuild.exe, run:
```
> msbuild.exe /t:restore /p:RestoreLockedMode=true
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
If you are building an application, an executable and the project in question is at the end of the dependency chain then do check in the lock file to the source code repository so that NuGet can make use of it during restore.

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

| Option | MSBuild equivalent option | 
|:---  |:--- |
| `--use-lock-file` | Bootstraps use of lock file for a project. You can alternatively set `RestorePackagesWithLockFile` property in the project file | 
| `--locked-mode` | Enables locked mode for restore. This is useful in CI/CD scenarios where you would like to get the erepeatable builds. This can be also by setting the `RestoreLockedMode` MSBuild property to `true` |  
| `--force-evaluate` | This option is useful with packages with floating version defined in the project. By default, NuGet restore will not update the package version automatically upon each restore unless you run restore with `--force-evaluate` option. |
| `--lock-file-path` | Defines a custom lock file location for a project. This can be also achieved by setting the MSBuild property `NuGetLockFilePath`. By default, NuGet supports `packages.lock.json` at the root directory. If you have multiple projects in the same directory, NuGet supports project specific lock file `packages.<project_name>.lock.json` |
