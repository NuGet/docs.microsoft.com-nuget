---
title: Central Package Management
description: Manage your dependencies in a central location and how you can get started with central package management.
author: jondouglas
ms.author: jodou
ms.date: 2/25/2022
ms.topic: conceptual
---

# Central Package Management

Dependency management is a core feature of NuGet. Managing dependencies for a single project can be easy. Managing dependencies for multi-project solutions can prove to be difficult as they start to scale in size and complexity. In situations where you manage common dependencies for many different projects, you can leverage NuGet's central package management features to do all of this from the ease of a single location.

Historically, NuGet package dependencies have been managed in two main locations:

- `packages.config` - An XML file used in older project types to maintain the list of packages referenced by the project.
- `<PackageReference />` - An XML element used in new project types that manages NuGet dependencies directly within project files.

Starting with [NuGet 7.0](..\release-notes\NuGet-7.0.md), you can centrally manage your dependencies in your projects with the addition of a `Directory.Packages.props` file.

The feature is available across all NuGet integrated tooling.

* [Visual Studio 2022 and later](https://visualstudio.microsoft.com/downloads/)
* [.NET SDK 7.0.000 and later](https://dotnet.microsoft.com/download/dotnet/7.0)
* [nuget.exe 7.0.0 and later](https://www.nuget.org/downloads)

Older tooling will ignore Central Package Management configurations and features. To use this feature to the fullest extent, ensure all your build environments use the latest compatible tooling versions.

Central Package Management will apply to all project types – including .NET Framework – as long as compatible tooling is used.

## Enabling Central Package Management

To get started with central package management, you can create a `Directory.Packages.props` file at the root of your solution.

Inside, you can define each of the respective package versions required of your solution using `<PackageVersion />` elements that define the package ID and version.

```xml
<Project>
  <ItemGroup>
    <PackageVersion Include="Newtonsoft.Json" Version="13.0.1" />
  </ItemGroup>
</Project>
```

Within a project of the solution, you can then use the respective `<PackageReference />` syntax you know and love, but without a `Version` attribute to infer the centrally managed version instead.

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

The `Directory.Packages.props` file has a number of rules with regards to where it's located in a repository's directory and its context. For the sake of simplicity, only one `Directory.Packages.props` file is evaluated for a given project.

What this means is that if you had multiple `Directory.Packages.props` files in your repository, the file that is closest to your project's directory will be evaluated for it. This allows you extra control at various levels of your repository.

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

- Project1 will evaluate the `Directory.Packages.props` file in the `Repository\Solution1\` directory.
- Project2 will evaluate the `Directory.Packages.props` file in the `Repository\` directory.

## Get started

To fully onboard your repository, consider taking these steps:

1. Create a new file at the root of your repository named `Directory.Packages.props` that declares your centrally defined package versions in.
2. Declare `<PackageVersion />` items in your `Directory.Packages.props`.
3. Declare `<PackageReference />` items without `Version` attributes in your project files.

<!--For an idea of how central package management may look like, refer to our [samples repo](https://github.com/NuGet/Samples/tree/main/CentralPackageManagementExample).-->

## Transitive pinning

You can automatically override a transitive package version even without an explicit top-level `<PackageReference />` by opting into a feature known as transitive pinning. This promotes a transitive dependency to a top-level dependency implicitly on your behalf.

You can enable this feature by setting the MSBuild property `CentralPackageTransitivePinningEnabled` to `true` in a project or `Directory.Build.props` import file:

```xml
  <PropertyGroup>
    <CentralPackageTransitivePinningEnabled>true</CentralPackageTransitivePinningEnabled>
  </PropertyGroup>
```

## Overriding package versions

You can override an individual package version by using the `VersionOverride` property on a `<PackageReference />` item. This overrides any `<PackageVersion />` defined centrally.

```xml
<Project>
  <ItemGroup>
    <PackageVersion Include="PackageA" Version="1.0.0" />
    <PackageVersion Include="PackageB" Version="2.0.0" />
  </ItemGroup>
<Project>
```

```xml
<Project Sdk="Microsoft.NET.Sdk">
  <PropertyGroup>
    <TargetFramework>net6.0</TargetFramework>
  </PropertyGroup>
  <ItemGroup>
    <PackageReference Include="PackageA" VersionOverride="3.0.0" />
  </ItemGroup>
<Project>
```

You can disable this feature by setting the MSBuild property `EnablePackageVersionOverride` to `false` in a project or `Directory.Build.props` import file:

```xml
  <PropertyGroup>
    <EnablePackageVersionOverride>false</EnablePackageVersionOverride>
  </PropertyGroup>
```

When this feature is disabled, specifying a `VersionOverride` on any `<PackageReference />` item will result in an error at restore time indicating that the feature is disabled.

## Disabling Central Package Management

If you'd like to disable central package management for any reason, you can disable this feature with the following:

```xml
<ManagePackageVersionsCentrally>false</ManagePackageVersionsCentrally>
```

> [!Note]
> This feature is in active development. We appreciate you trying it out and providing any feedback you may have at [NuGet/Home](https://github.com/nuget/home/issues).
>
> * There is currently no support in Visual Studio or the .NET CLI for Central Package Management.