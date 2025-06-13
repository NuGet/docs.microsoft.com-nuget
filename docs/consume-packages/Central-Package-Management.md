---
title: Central Package Management
description: Manage your dependencies in a central location and learn how to get started with Central Package Management.
author: jondouglas
ms.author: jodou
ms.date: 05/09/2022
ms.topic: conceptual
---

# Central Package Management (CPM)

Dependency management is a core feature of NuGet.
While managing dependencies for a single project is straightforward, it becomes increasingly complex as the number of projects in a solution grows.

If you manage common dependencies for many different projects, you can leverage NuGet's Central Package Management (CPM) features to do all of this from a single, central location.

Historically, NuGet package dependencies have been managed in one of two ways:

- `packages.config` – An XML file used in older project types to maintain the list of packages referenced by the project.
- `<PackageReference />` – An XML element in MSBuild project files that defines NuGet package dependencies.

Starting with [NuGet 6.2](../release-notes/NuGet-6.2.md), you can centrally manage your dependencies in your projects with the addition of a `Directory.Packages.props` file and an MSBuild property.

This feature is available across all NuGet-integrated tooling, starting with the following versions:

* [Visual Studio 2022 17.2](https://visualstudio.microsoft.com/downloads/)
* [.NET SDK 6.0.300](https://dotnet.microsoft.com/download/dotnet/6.0)
* [nuget.exe 6.2.0](https://www.nuget.org/downloads)

Older tooling will ignore Central Package Management configurations and features.
To use this feature to the fullest extent, ensure all your build environments use the latest compatible tooling versions.

Central Package Management applies to all `<PackageReference>`-based MSBuild projects (including [legacy CSPROJ](https://github.com/dotnet/project-system/blob/main/docs/feature-comparison.md)) as long as compatible tooling is used.

## Enabling Central Package Management

To get started with Central Package Management, create a `Directory.Packages.props` file at the root of your repository and set the MSBuild property `ManagePackageVersionsCentrally` to `true`.

You can create it manually, or use the .NET CLI:

```shell
dotnet new packagesprops
```

Inside `Directory.Packages.props`, define `<PackageVersion />` elements to specify the package IDs and versions used by your projects.

```xml
<Project>
  <PropertyGroup>
    <ManagePackageVersionsCentrally>true</ManagePackageVersionsCentrally>
  </PropertyGroup>
  <ItemGroup>
    <PackageVersion Include="PackageA" Version="1.0.0" />
    <PackageVersion Include="PackageB" Version="2.0.0" />
  </ItemGroup>
</Project>
```

In each project file, define `<PackageReference />` elements without the `Version` attribute.
The version will be resolved from the corresponding `<PackageVersion />` entry in `Directory.Packages.props`.

```xml
<Project Sdk="Microsoft.NET.Sdk">
  <PropertyGroup>
    <TargetFramework>net6.0</TargetFramework>
  </PropertyGroup>
  <ItemGroup>
    <PackageReference Include="PackageA" />
  </ItemGroup>
</Project>
```

Now you're using Central Package Management and managing your versions in a central location!

### Using Different Versions for Different Target Frameworks

As NuGet packages evolve, package owners may drop support for older target frameworks.
This can cause issues for developers of libraries that still target older frameworks but want to reference newer versions of packages for newer target frameworks.

For example, if your project targets .NET Standard 2.0, .NET 8.0, and .NET Framework 4.7.2, but `PackageA` no longer supports .NET Standard 2.0 in its latest version, you can specify different versions for each target framework.

```xml
<Project Sdk="Microsoft.NET.Sdk">
  <PropertyGroup>
    <TargetFrameworks>netstandard2.0;net8.0;net472</TargetFrameworks>
  </PropertyGroup>
  <ItemGroup>
    <PackageReference Include="PackageA" />
  </ItemGroup>
</Project>
```

In this case, define different versions for each target framework in your `Directory.Packages.props` using [MSBuild conditions](/visualstudio/msbuild/msbuild-conditions):

```xml
<Project>
  <PropertyGroup>
    <ManagePackageVersionsCentrally>true</ManagePackageVersionsCentrally>
  </PropertyGroup>
  <ItemGroup>
    <PackageVersion Include="PackageA" Version="1.0.0" Condition="'$(TargetFramework)' == 'netstandard2.0'" />
    <PackageVersion Include="PackageA" Version="2.0.0" Condition="'$(TargetFramework)' == 'net8.0' Or '$(TargetFramework)' == 'net472'" />
  </ItemGroup>
</Project>
```

## Central Package Management Rules

The `Directory.Packages.props` file has specific rules regarding its location and context within a repository.
Only one `Directory.Packages.props` file is evaluated for a given project by default.

If you have multiple `Directory.Packages.props` files in your repository, the file closest to a given project's directory will be evaluated for it.
This allows extra control at various levels of your repository.

Consider the following repository directory structure:

```
📂 (root)
 ├─📄 Directory.Packages.props
 |
 ├─📂Solution1
 |  ├─ 📄Directory.Packages.props
 |  |
 |  └─ 📂 Project1
 |      └─📄Project1.csproj
 |
 └─ 📂 Solution2
    └─ 📂 Project2
        └─ 📄 Project2.csproj
```

`Project1.csproj` will use the `Directory.Packages.props` file in the `Repository\Solution1\` directory.
If you want to include settings from a parent `Directory.Packages.props`, you must manually import it.

```xml
<Project>
  <Import Project="$([MSBuild]::GetPathOfFileAbove(Directory.Packages.props, $(MSBuildThisFileDirectory)..))" />
  <ItemGroup>
    <PackageVersion Update="Newtonsoft.Json" Version="12.0.1" />
  </ItemGroup>
</Project>
```

`Project2.csproj` will evaluate the `Directory.Packages.props` file in the root directory.

> [!NOTE]
> MSBuild will only automatically import the first `Directory.Packages.props` file it finds in the project directory or any parent directory.
> If you have multiple such files, you must manually import parent files as needed.

## Getting Started

To fully onboard your repository, follow these steps:

1. Create a new file at the root of your repository named `Directory.Packages.props` that declares your centrally defined package versions and set the MSBuild property `ManagePackageVersionsCentrally` to `true`.
2. Declare `<PackageVersion />` items in your `Directory.Packages.props`.
3. Declare `<PackageReference />` items without `Version` attributes in your project files.

For an example of how Central Package Management may look, refer to our [samples repository](https://github.com/NuGet/Samples/tree/main/CentralPackageManagementExample).

## Pinning Transitive Packages to Different Versions

You can automatically override a transitive package version without an explicit top-level `<PackageReference />` item by opting into a feature known as transitive pinning.
This promotes a transitive dependency to a top-level dependency implicitly on your behalf when necessary.

Note that downgrades are not allowed when transitive pinning a package.
If you attempt to pin a package to a lower version than the one requested by your dependencies, restore will raise a [NU1109](../reference/errors-and-warnings/NU1109.md) error.

You can enable this feature by setting the MSBuild property `CentralPackageTransitivePinningEnabled` to `true` in a project or in a `Directory.Packages.props` or `Directory.Build.props` import file:

```xml
<PropertyGroup>
  <CentralPackageTransitivePinningEnabled>true</CentralPackageTransitivePinningEnabled>
</PropertyGroup>
```

### Transitive Pinning and Pack

When a package is transitively pinned, your project uses a higher version than the one requested by your dependencies.
If you create a package from your project, to ensure that your package will work, NuGet will promote the transitively pinned dependencies to explicit dependencies in the nuspec.

In the following example, `PackageA 1.0.0` has a dependency on `PackageB 1.0.0`.

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
    <CentralPackageTransitivePinningEnabled>true</CentralPackageTransitivePinningEnabled>
    <TargetFramework>net6.0</TargetFramework>
  </PropertyGroup>
  <ItemGroup>
    <PackageReference Include="PackageA" />
  </ItemGroup>
</Project>
```

When you use the pack command to create a package, both packages will appear in the dependency group.

```xml
<group targetFramework="net6.0">
  <dependency id="PackageA" version="1.0.0" exclude="Build,Analyzers" />
  <dependency id="PackageB" version="2.0.0" exclude="Build,Analyzers" />
</group>
```

Because of this, the use of transitive pinning should be carefully evaluated when authoring a library, as it may lead to dependencies you did not expect.

## Overriding Package Versions

You can override an individual package version by using the `VersionOverride` property on a `<PackageReference />` item.
This will take precedence over any centrally defined `<PackageVersion />`.

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

You can disable this feature by setting the MSBuild property `CentralPackageVersionOverrideEnabled` to `false` in a project or in a `Directory.Packages.props` or `Directory.Build.props` import file:

```xml
<PropertyGroup>
  <CentralPackageVersionOverrideEnabled>false</CentralPackageVersionOverrideEnabled>
</PropertyGroup>
```

When this feature is disabled, specifying a `VersionOverride` on any `<PackageReference />` item will result in an error at restore time indicating that the feature is disabled.

## Disabling Central Package Management

To disable Central Package Management for a specific project, set the MSBuild property `ManagePackageVersionsCentrally` to `false`:

```xml
<PropertyGroup>
  <ManagePackageVersionsCentrally>false</ManagePackageVersionsCentrally>
</PropertyGroup>
```

## Global Package References

> [!NOTE]
> This feature is only available in Visual Studio 2022 17.4 or higher, .NET SDK 7.0.100.preview7 or higher, and NuGet 6.4 or higher.

A global package reference is used to specify that a package will be used by every project in a repository.
This includes packages that do versioning, extend your build, or any other packages that are needed by all projects.
Global package references are added to the PackageReference item group with the following metadata:

* `IncludeAssets="Runtime;Build;Native;contentFiles;Analyzers"`<br/>
  This ensures that the package is only used as a development dependency and prevents it from being included as a compile-time assembly reference.
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

## NU1507 Warning When Using Multiple Package Sources

When using Central Package Management, NuGet will log an [`NU1507`](../reference/errors-and-warnings/nu1507.md) warning if more than one package source is defined in your configuration.
To resolve this warning, map your package sources with [package source mapping](https://aka.ms/nuget-package-source-mapping) or specify a single package source.

```
There are 3 package sources defined in your configuration. When using Central Package Management, please map your package sources with package source mapping (https://aka.ms/nuget-package-source-mapping) or specify a single package source.
```
