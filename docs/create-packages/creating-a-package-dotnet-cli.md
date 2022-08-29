---
title: Create a NuGet package with the dotnet CLI
description: Read a detailed guide about the process of designing and creating a NuGet package, including key decision points like files and versioning.
author: JonDouglas
ms.author: jodou
ms.date: 07/14/2022
ms.topic: conceptual
---

# Create a NuGet package with the dotnet CLI

You create NuGet packages to share code with other developers to use in their projects. No matter what your code does or contains, you use a CLI tool, either `nuget.exe` or `dotnet.exe`, to package the code. For more information, see [Install NuGet client tools](../install-nuget-client-tools.md).

This article describes how to create a package by using the [dotnet CLI](). Starting in Visual Studio 2017, the dotnet CLI is included with all .NET and .NET Core workloads.

This topic applies only to .NET and other projects that use the [SDK-style format](../resources/check-project-format.md). For these projects, NuGet uses information from the project file to create a package. For quickstart tutorials, see [Create packages with the dotnet CLI](../quickstart/create-and-publish-a-package-using-the-dotnet-cli.md) or [Create packages with Visual Studio](../quickstart/create-and-publish-a-package-using-visual-studio.md).
The MSBuild `msbuild -t:pack` command is functionally equivalent to `dotnet pack`. To build with MSBuild, see [Create a NuGet package using MSBuild](creating-a-package-msbuild.md).

> [!NOTE]
> To create and publish packages for non-SDK-style projects, typically .NET Framework projects, see [Create a package using the nuget.exe CLI](create-package.md) or [Create and publish a package using Visual Studio (.NET Framework)](create-and-publish-a-package-using-visual-studio-net-framework.md).

## Set properties

The following properties are required to create a package.

- `PackageId`, the package identifier, which must be unique across the gallery that hosts the package. If not specified, the default value is `AssemblyName`.
- `Version`, a specific version number in the form *Major.Minor.Patch[-Suffix]* where *-Suffix* identifies [pre-release versions](prerelease-packages.md). If not specified, the default value is 1.0.0.
- The package title as it should appear on the host (like nuget.org)
- `Authors`, author and owner information. If not specified, the default value is `AssemblyName`.
- `Company`, your company name. If not specified, the default value is `AssemblyName`.

In Visual Studio, you can set these values in the project properties (right-click the project in Solution Explorer, choose **Properties**, and select the **Package** tab). You can also set these properties directly in the project files (`.csproj`).

```xml
<PropertyGroup>
  <PackageId>AppLogger</PackageId>
  <Version>1.0.0</Version>
  <Authors>your_name</Authors>
  <Company>your_company</Company>
</PropertyGroup>
```

> [!Important]
> Give the package an identifier that's unique across nuget.org or whatever package source you're using.

The following example shows a simple, complete project file with these properties included. (You can create a new default project using the `dotnet new classlib` command.)

```xml
<Project Sdk="Microsoft.NET.Sdk">
  <PropertyGroup>
    <TargetFramework>netstandard2.0</TargetFramework>
    <PackageId>AppLogger</PackageId>
    <Version>1.0.0</Version>
    <Authors>your_name</Authors>
    <Company>your_company</Company>
  </PropertyGroup>
</Project>
```

You can also set the optional properties, such as `Title`, `PackageDescription`, and `PackageTags`, as described in [MSBuild pack targets](../reference/msbuild-targets.md#pack-target), [Controlling dependency assets](../consume-packages/package-references-in-project-files.md#controlling-dependency-assets), and [NuGet metadata properties](/dotnet/core/tools/csproj#nuget-metadata-properties).

> [!NOTE]
> For packages built for public consumption, pay special attention to the **PackageTags** property, as tags help others find your package and understand what it does.

For details on declaring dependencies and specifying version numbers, see [Package references in project files](../consume-packages/package-references-in-project-files.md) and [Package versioning](../concepts/package-versioning.md). It is also possible to surface assets from dependencies directly in the package by using the `<IncludeAssets>` and `<ExcludeAssets>` attributes. For more information, see [Controlling dependency assets](../consume-packages/package-references-in-project-files.md#controlling-dependency-assets).

## Add an optional description field

[!INCLUDE [add description to package](includes/add-description.md)]

## Choose a unique package identifier and set the version number

[!INCLUDE [choose-package-id](includes/choose-package-id.md)]

## Run the pack command

To build the NuGet package or *.nupkg* file, run the `dotnet pack` command from the project folder, which also builds the project automatically.

```dotnetcli
dotnet pack
```

The output shows the path to the *.nupkg* file:

```output
Microsoft (R) Build Engine version 15.5.180.51428 for .NET Core
Copyright (C) Microsoft Corporation. All rights reserved.

  Restore completed in 29.91 ms for D:\proj\AppLoggerNet\AppLogger\AppLogger.csproj.
  AppLogger -> D:\proj\AppLoggerNet\AppLogger\bin\Debug\netstandard2.0\AppLogger.dll
  Successfully created package 'D:\proj\AppLoggerNet\AppLogger\bin\Debug\AppLogger.1.0.0.nupkg'.
```

### Automatically generate package on build

To automatically run `dotnet pack` when you run `dotnet build`, add the following line to your project file within `<PropertyGroup>`:

```xml
<GeneratePackageOnBuild>true</GeneratePackageOnBuild>
```

When you run `dotnet pack` on a solution, this packs all the projects in the solution that are packable ([\<IsPackable\>](/dotnet/core/tools/csproj#nuget-metadata-properties) property is set to `true`).

> [!NOTE]
> When you automatically generate the package, the time to pack increases the build time for your project.

### Test package installation

Before publishing a package, you typically want to test the process of installing a package into a project. The tests make sure that the necessary files all end up in their correct places in the project.

You can test installations manually in Visual Studio or on the command line using the normal [package installation steps](../consume-packages/overview-and-workflow.md#ways-to-install-a-nuget-package).

> [!IMPORTANT]
> Packages are immutable. If you correct a problem, change the contents of the package and pack again, when you retest you will still be using the old version of the package until you [clear your global packages](../consume-packages/managing-the-global-packages-and-cache-folders.md#clearing-local-folders) folder. This is especially relevant when testing packages that don't use a unique prerelease label on every build.

## Next steps

Once you create the package, you can publish the *.nupkg* file to the host of your choice.

[Publish a package](../nuget-org/publish-a-package.md).

You might also want to extend the capabilities of your package or otherwise support other scenarios as described in the following topics:

- [Package versioning](../concepts/package-versioning.md)
- [Support multiple target frameworks](../create-packages/multiple-target-frameworks-project-file.md)
- [Add a package icon](../reference/nuspec.md#icon)
- [Transformations of source and configuration files](../create-packages/source-and-config-file-transformations.md)
- [Localization](../create-packages/creating-localized-packages.md)
- [Pre-release versions](../create-packages/prerelease-packages.md)
- [Set package type](../create-packages/set-package-type.md)
- [MSBuild props and targets](../concepts/MSBuild-props-and-targets.md)
- [Create packages with COM interop assemblies](../create-packages/author-packages-with-COM-interop-assemblies.md)

Finally, there are additional package types to be aware of:

- [Native Packages](../guides/native-packages.md)
- [Symbol Packages](../create-packages/symbol-packages-snupkg.md)
