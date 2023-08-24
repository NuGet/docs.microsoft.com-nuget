---
title: Central Package Management
description: Manage your dependencies in a central location and how you can get started with central package management.
author: jondouglas
ms.author: jodou
ms.date: 05/09/2022
ms.topic: conceptual
---

# Central Package Management (CPM)

Dependency management is a core feature of NuGet. Managing dependencies for a single project can be easy. Managing dependencies for multi-project solutions
can prove to be difficult as they start to scale in size and complexity. In situations where you manage common dependencies for many different projects, you
can leverage NuGet's central package management (CPM) features to do all of this from the ease of a single location.

Historically, NuGet package dependencies have been managed in one of two locations:

- `packages.config` - An XML file used in older project types to maintain the list of packages referenced by the project.
- `<PackageReference />` - An XML element used in MSBuild projects defines NuGet package dependencies.

Starting with [NuGet 6.2](..\release-notes\NuGet-6.2.md), you can centrally manage your dependencies in your projects with the addition of a
`Directory.Packages.props` file and an MSBuild property.

The feature is available across all NuGet integrated tooling, starting with the following versions.

* [Visual Studio 2022 17.2](https://visualstudio.microsoft.com/downloads/)
* [.NET SDK 6.0.300](https://dotnet.microsoft.com/download/dotnet/6.0)
* [nuget.exe 6.2.0](https://www.nuget.org/downloads)

Older tooling will ignore central package management configurations and features. To use this feature to the fullest extent, ensure all your build environments
use the latest compatible tooling versions.

Central package management applies to all `<PackageReference>`-based MSBuild projects (including
[legacy CSPROJ](https://github.com/dotnet/project-system/blob/main/docs/feature-comparison.md)) as long as compatible tooling is used.

## Enabling Central Package Management

To get started with central package management, you must create a `Directory.Packages.props` file at the root of your repository and set the MSBuild property
`ManagePackageVersionsCentrally` to `true`.

Inside, you then define each of the respective package versions required of your projects using `<PackageVersion />` elements that define the package ID and
version.

```xml
<Project>
  <PropertyGroup>
    <ManagePackageVersionsCentrally>true</ManagePackageVersionsCentrally>
  </PropertyGroup>
  <ItemGroup>
    <PackageVersion Include="Newtonsoft.Json" Version="13.0.1" />
  </ItemGroup>
</Project>
```

For each project, you then define a `<PackageReference />` but omit the `Version` attribute since the version will be attained from a corresponding
`<PackageVersion />` item.

```xml
<Project Sdk="Microsoft.NET.Sdk">
  <PropertyGroup>
    <TargetFramework>net6.0</TargetFramework>
  </PropertyGroup>
  <ItemGroup>
    <PackageReference Include="Newtonsoft.Json" />
  </ItemGroup>
</Project>
```

Now you're using central package management and managing your versions in a central location!

## Central Package Management rules

The `Directory.Packages.props` file has a number of rules with regards to where it's located in a repository's directory and its context. For the sake of
simplicity, only one `Directory.Packages.props` file is evaluated for a given project.

What this means is that if you had multiple `Directory.Packages.props` files in your repository, the file that is closest to your project's directory will
be evaluated for it. This allows you extra control at various levels of your repository.

Here's an example, consider the following repository structure:

```
Repository
 |-- Directory.Packages.props
 |-- Solution1
     |-- Directory.Packages.props
     |-- Project1
 |-- Solution2
     |-- Project2
```

- Project1 will evaluate the `Directory.Packages.props` file in the `Repository\Solution1\` directory and it must manually import the next one if so desired.
  ```xml
  <Project>
    <Import Project="$([MSBuild]::GetPathOfFileAbove(Directory.Packages.props, $(MSBuildThisFileDirectory)..))" />
    <ItemGroup>
      <PackageVersion Update="Newtonsoft.Json" Version="12.0.1" />
    </ItemGroup>
  </Project>
  ```
- Project2 will evaluate the `Directory.Packages.props` file in the `Repository\` directory.

**Note:** MSBuild will not automatically import each `Directory.Packages.props` for you, only the first one closest to the project.  If you have multiple
`Directory.Packages.props`, you must import the parent one manually while the root `Directory.Packages.props` would not.

## Get started

To fully onboard your repository, consider taking these steps:

1. Create a new file at the root of your repository named `Directory.Packages.props` that declares your centrally defined package versions and set
 the MSBuild property `ManagePackageVersionsCentrally` to `true`.
2. Declare `<PackageVersion />` items in your `Directory.Packages.props`.
3. Declare `<PackageReference />` items without `Version` attributes in your project files.

For an idea of how central package management may look like, refer to our [samples repo](https://github.com/NuGet/Samples/tree/main/CentralPackageManagementExample).

## Transitive pinning

You can automatically override a transitive package version even without an explicit top-level `<PackageReference />` by opting into a feature known as
transitive pinning. This promotes a transitive dependency to a top-level dependency implicitly on your behalf when necessary.

You can enable this feature by setting the MSBuild property `CentralPackageTransitivePinningEnabled` to `true` in a project or in a `Directory.Packages.props`
or `Directory.Build.props` import file:

```xml
<PropertyGroup>
  <CentralPackageTransitivePinningEnabled>true</CentralPackageTransitivePinningEnabled>
</PropertyGroup>
```

## Overriding package versions

You can override an individual package version by using the `VersionOverride` property on a `<PackageReference />` item. This overrides any `<PackageVersion />`
defined centrally.

```xml
<Project>
  <ItemGroup>
    <PackageVersion Include="PackageA" Version="1.0.0" />
    <PackageVersion Include="PackageB" Version="2.0.0" />
  </ItemGroup>
</Project>
```

```xml
<Project Sdk="Microsoft.NET.Sdk">
  <PropertyGroup>
    <TargetFramework>net6.0</TargetFramework>
  </PropertyGroup>
  <ItemGroup>
    <PackageReference Include="PackageA" VersionOverride="3.0.0" />
  </ItemGroup>
</Project>
```

You can disable this feature by setting the MSBuild property `CentralPackageVersionOverrideEnabled` to `false` in a project or in a `Directory.Packages.props` or
`Directory.Build.props` import file:

```xml
<PropertyGroup>
  <CentralPackageVersionOverrideEnabled>false</CentralPackageVersionOverrideEnabled>
</PropertyGroup>
```

When this feature is disabled, specifying a `VersionOverride` on any `<PackageReference />` item will result in an error at restore time indicating that
the feature is disabled.

## Disabling Central Package Management

If you'd like to disable central package management for any a particular project, you can disable it by setting the MSBuild property
`ManagePackageVersionsCentrally` to `false`:

```xml
<PropertyGroup>
  <ManagePackageVersionsCentrally>false</ManagePackageVersionsCentrally>
</PropertyGroup>
```

## Global Package References
> [!Note]
> This feature is only available in Visual Studio 2022 17.4 or higher, .NET SDK 7.0.100.preview7 or higher, and NuGet 6.4 or higher.

A global package reference is used to specify that a package will be used by every project in a repository. This includes packages that do versioning, extend your build, or any other packages that are needed by all projects. Global package references are added to the PackageReference item group with the following metadata:

* `IncludeAssets="Runtime;Build;Native;contentFiles;Analyzers"`<br/>
  This ensures that the package is only used as a development dependency and prevents any compile-time assembly references.
* `PrivateAssets="All"`<br/>
  This prevents global package references from being picked up by downstream dependencies.


`GlobalPackageReference` items should be placed in your `Directory.Packages.props` to be used by every project in a repository:

```xml
<Project>
  <ItemGroup>
    <GlobalPackageReference Include="Nerdbank.GitVersioning" Version="3.5.109" />
  </ItemGroup>
</Project>
```

## Warning when using multiple package sources

When using central package management, you will see a `NU1507` warning if you have more than one package source defined in your configuration. To resolve
this warning, map your package sources with [package source mapping](https://aka.ms/nuget-package-source-mapping) or specify a single package source.

```
There are 3 package sources defined in your configuration. When using central package management, please map your package sources with package source mapping (https://aka.ms/nuget-package-source-mapping) or specify a single package source.
```



> [!Note]
> Central package management is in active development. We appreciate you trying it out and providing any feedback you may have at [NuGet/Home](https://github.com/nuget/home/issues).
