---
title: Download packages with PackageDownload
description: Describes the PackageDownload feature, which is a complement to PackageReference.
author: nkolev92
ms.author: nikolev
ms.date: 12/22/2021
ms.topic: concept-article
---

# PackageDownload

Starting with Visual Studio 2017 and .NET SDK 1.0.0, NuGet [PackageReference](Package-References-in-Project-Files.md) functionality was added.

`PackageReference` allows you to manage your package dependencies directly in your project file.
When you run restore, the transitive dependencies are resolved automatically and the applicable references are chosen for each package in the project graph.

In [NuGet version 5.3](..\release-notes\NuGet-5.3.md) a companion feature was introduced for [.NET SDK-style projects](..\resources\check-project-format.md) called `PackageDownload`, which allows you to download the package without including its files in the project.

## PackageDownload specification

PackageDownload is a utility feature for all .NET SDK-style projects, and it works along side `PackageReference`.

`PackageDownload` items support different attributes compared to `PackageReference`. Only attributes listed in the below table are supported.

| Attributes | Description | Example |
|------------|-------------|---------|
| Version | Only exact versions, surrounded with `[]` are supported. Multiple versions can be specified separated by `;` | `[1.0.0]`, `[1.0.0];[2.0.0]` |

Packages acquired through PackageDownload will undergo the same [installation process](..\concepts\package-installation-process.md) as packages acquired through PackageReference.
This means [package signatures](installing-signed-packages.md) are validated, [package source mapping](Package-Source-Mapping.md) is considered.
All newly acquired PackageDownload packages will be installed in the global packages folder.

| Feature | PackageReference | PackageDownload |
|-|------------------|-----------------|
| Package assets selection | Assemblies from packages are automatically added to the project and can be used for compile and runtime | No assets from the package are included in the project. |
| Dependencies | Automatically resolved, and flattened to a single version | Not considered at all |
| pack | Included in the package specification | Not included in the package specification. |
| Transitivity | PackageReference items are automatically propagated to dependant projects | PackageDownload items are ignored by dependant projects |
| Version | Version ranges such as `1.0.0` or `[1.0.0, )` are supported. Exactly 1 version is allowed. | Only exact versions are supported. More than 1 version can be downloaded. |
| dotnet package list | All dependencies are included | PackageDownload packages are not shown by `dotnet package list`. |

Due to the fact that PackageDownload are not tied to the project in any way beyond acquisition, multiple versions of the same package can be downloaded.

### PackageDownload limitations

Given that this is an advanced feature with limited applicability, it doesn't have a tooling support equivalent to PackageReference.

- There is no VisualStudio or dotnet.exe functionality to modify PackageDownload items. You can only change them manually in your project files.
- dotnet package/reference add, remove, and list commands do not account for PackageDownload items.
- PackageDownload items are *not* part of the [packages lock file](package-references-in-project-files.md#locking-dependencies).

### PackageDownload applications

The primary application of PackageDownload is downloading packages that do not follow the traditional NuGet package structure and primarily carry build time dependencies.

Ideally, all your dependencies are expressed through PackageReference, but in scenarios where that's not possible, or often times not practical yet, you can use this feature to simply `download` packages to a certain location, in a similar way that you could achieve that with a `packages.config` file not tied to a project. 

Example:

```xml

<Project Sdk="Microsoft.Build.NoTargets/1.0.80"> <!-- This is not a project we want to build. -->

    <PropertyGroup>
        <RestorePackagesPath>packages/</RestorePackagesPath> <!-- Changes the global packages folder-->
        <MSBuildProjectExtensionsPath>$(RestorePackagesPath)obj/</MSBuildProjectExtensionsPath> <!-- It's still PackageReference, so project intermediates are still created. -->
        <TargetFramework>net5.0</TargetFramework> <!-- This is not super relevant, as long as your SDK version supports it. -->
        <DisableImplicitNuGetFallbackFolder>true</DisableImplicitNuGetFallbackFolder> <!-- If a package is resolved to a fallback folder, it may not be downloaded.-->
        <AutomaticallyUseReferenceAssemblyPackages>false</AutomaticallyUseReferenceAssemblyPackages> <!-- We don't want to build this project, so we do not need the reference assemblies for the framework we chose.-->
    </PropertyGroup>

    <ItemGroup>
        <PackageDownload Include="MySpecialPackage" version="[6.0.0]" />
    </ItemGroup>

</Project>
```
