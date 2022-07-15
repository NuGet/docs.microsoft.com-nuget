---
title: Create a NuGet package using MSBuild
description: A detailed guide to the process of designing and creating a NuGet package using MSBuild, including key decision points like files and versioning.
author: JonDouglas
ms.author: jodou
ms.date: 02/20/2020
ms.topic: conceptual
---

# Create a NuGet package using MSBuild

When you create a NuGet package from your code, you package that functionality into a component that can be shared with and used by any number of other developers. This article describes how to create a package using MSBuild. MSBuild comes preinstalled with every Visual Studio workload that contains NuGet. Additionally you can also use MSBuild through the dotnet CLI with [dotnet msbuild](/dotnet/core/tools/dotnet-msbuild).

For .NET Core and .NET Standard projects that use the [SDK-style format](../resources/check-project-format.md), and any other SDK-style projects, NuGet uses information in the project file directly to create a package.  For a non-SDK-style project that uses `<PackageReference>`, NuGet also uses the project file to create a package.

SDK-style projects have the pack functionality available by default. For non SDK-style PackageReference projects, you need to add the NuGet.Build.Tasks.Pack package to the project dependencies. For detailed information about MSBuild pack targets, see [NuGet pack and restore as MSBuild targets](../reference/msbuild-targets.md).

The command that creates a package, `msbuild -t:pack`, is functionally equivalent to `dotnet pack`.

> [!IMPORTANT]
> This topic applies to [SDK-style](../resources/check-project-format.md) projects, typically .NET Core and .NET Standard projects, and to non-SDK-style projects that use PackageReference.

## Set properties

The following properties are required to create a package.

- `PackageId`, the package identifier, which must be unique across the gallery that hosts the package. If not specified, the default value is `AssemblyName`.
- `Version`, a specific version number in the form *Major.Minor.Patch[-Suffix]* where *-Suffix* identifies [pre-release versions](prerelease-packages.md). If not specified, the default value is 1.0.0.
- The package title as it should appear on the host (like nuget.org)
- `Authors`, author and owner information. If not specified, the default value is `AssemblyName`.
- `Company`, your company name. If not specified, the default value is `AssemblyName`.

Additionally if you are packing non-SDK-style projects that use PackageReference, the following is required:

- `PackageOutputPath`, the output folder for the package generated when calling pack.

In Visual Studio, you can set these values in the project properties (right-click the project in Solution Explorer, choose **Properties**, and select the **Package** tab). You can also set these properties directly in the project files (*.csproj*).

```xml
<PropertyGroup>
  <PackageId>ClassLibDotNetStandard</PackageId>
  <Version>1.0.0</Version>
  <Authors>your_name</Authors>
  <Company>your_company</Company>
</PropertyGroup>
```

> [!Important]
> Give the package an identifier that's unique across nuget.org or whatever package source you're using.

The following example shows a simple, complete project file with these properties included.

```xml
<Project Sdk="Microsoft.NET.Sdk">
  <PropertyGroup>
    <TargetFramework>netstandard2.0</TargetFramework>
    <PackageId>ClassLibDotNetStandard</PackageId>
    <Version>1.0.0</Version>
    <Authors>your_name</Authors>
    <Company>your_company</Company>
  </PropertyGroup>
</Project>
```

You can also set the optional properties, such as `Title`, `PackageDescription`, and `PackageTags`, as described in [MSBuild pack targets](../reference/msbuild-targets.md#pack-target), [Controlling dependency assets](../consume-packages/package-references-in-project-files.md#controlling-dependency-assets), and [NuGet metadata properties](/dotnet/core/tools/csproj#nuget-metadata-properties).

> [!NOTE]
> For packages built for public consumption, pay special attention to the **PackageTags** property, as tags help others find your package and understand what it does.

For details on declaring dependencies and specifying version numbers, see [Package references in project files](../consume-packages/package-references-in-project-files.md) and [Package versioning](../concepts/package-versioning.md). It is also possible to surface assets from dependencies directly in the package by using the `<IncludeAssets>` and `<ExcludeAssets>` attributes. For more information, seee [Controlling dependency assets](../consume-packages/package-references-in-project-files.md#controlling-dependency-assets).

## Add an optional description field

[!INCLUDE [add description to package](includes/add-description.md)]

## Choose a unique package identifier and set the version number

[!INCLUDE [choose-package-id](includes/choose-package-id.md)]

## Add the NuGet.Build.Tasks.Pack package

If you are using MSBuild with a non-SDK-style project and PackageReference, add the NuGet.Build.Tasks.Pack package to your project.

1. Open the project file and add the following after the `<PropertyGroup>` element:

   ```xml
   <ItemGroup>
     <!-- ... -->
     <PackageReference Include="NuGet.Build.Tasks.Pack" Version="5.2.0"/>
     <!-- ... -->
   </ItemGroup>
   ```

2. Open a Developer command prompt (In the **Search** box, type **Developer command prompt**).

   You typically want to start the Developer Command Prompt for Visual Studio from the **Start** menu, as it will be configured with all the necessary paths for MSBuild.

3. Switch to the folder containing the project file and type the following command to install the NuGet.Build.Tasks.Pack package.

   ```cmd
   # Uses the project file in the current folder by default
   msbuild -t:restore
   ```

   Make sure that the MSBuild output indicates that the build completed successfully.

## Run the msbuild -t:pack command

To build a NuGet package (a `.nupkg` file) from the project, run the `msbuild -t:pack` command, which also builds the project automatically:

In the Developer command prompt for Visual Studio, type the following command:

```cmd
# Uses the project file in the current folder by default
msbuild -t:pack
```

The output shows the path to the `.nupkg` file.

```output
Microsoft (R) Build Engine version 16.1.76+g14b0a930a7 for .NET Framework
Copyright (C) Microsoft Corporation. All rights reserved.

Build started 8/5/2019 3:09:15 PM.
Project "C:\Users\username\source\repos\ClassLib_DotNetStandard\ClassLib_DotNetStandard.csproj" on node 1 (pack target(s)).
GenerateTargetFrameworkMonikerAttribute:
Skipping target "GenerateTargetFrameworkMonikerAttribute" because all output files are up-to-date with respect to the input files.
CoreCompile:
  ...
CopyFilesToOutputDirectory:
  Copying file from "C:\Users\username\source\repos\ClassLib_DotNetStandard\obj\Debug\netstandard2.0\ClassLib_DotNetStandard.dll" to "C:\Use
  rs\username\source\repos\ClassLib_DotNetStandard\bin\Debug\netstandard2.0\ClassLib_DotNetStandard.dll".
  ClassLib_DotNetStandard -> C:\Users\username\source\repos\ClassLib_DotNetStandard\bin\Debug\netstandard2.0\ClassLib_DotNetStandard.dll
  Copying file from "C:\Users\username\source\repos\ClassLib_DotNetStandard\obj\Debug\netstandard2.0\ClassLib_DotNetStandard.pdb" to "C:\Use
  rs\username\source\repos\ClassLib_DotNetStandard\bin\Debug\netstandard2.0\ClassLib_DotNetStandard.pdb".
GenerateNuspec:
  Successfully created package 'C:\Users\username\source\repos\ClassLib_DotNetStandard\bin\Debug\AppLogger.1.0.0.nupkg'.
Done Building Project "C:\Users\username\source\repos\ClassLib_DotNetStandard\ClassLib_DotNetStandard.csproj" (pack target(s)).

Build succeeded.
    0 Warning(s)
    0 Error(s)

Time Elapsed 00:00:01.21
```

### Automatically generate package on build

To automatically run `msbuild -t:pack` when you build or restore the project, add the following line to your project file within `<PropertyGroup>`:

```xml
<GeneratePackageOnBuild>true</GeneratePackageOnBuild>
```

When you run `msbuild -t:pack` on a solution, this packs all the projects in the solution that are packable ([`<IsPackable>`](/dotnet/core/tools/csproj#nuget-metadata-properties) property is set to `true`).

> [!NOTE]
> When you automatically generate the package, the time to pack increases the build time for your project.

### Test package installation

Before publishing a package, you typically want to test the process of installing a package into a project. The tests make sure that the necessarily files all end up in their correct places in the project.

You can test installations manually in Visual Studio or on the command line using the normal [package installation steps](../consume-packages/overview-and-workflow.md#ways-to-install-a-nuget-package).

> [!IMPORTANT]
> Packages are immutable. If you correct a problem, change the contents of the package and pack again, when you retest you will still be using the old version of the package until you [clear your global packages](../consume-packages/managing-the-global-packages-and-cache-folders.md#clearing-local-folders) folder. This is especially relevant when testing packages that don't use a unique prerelease label on every build.

## Next Steps

Once you've created a package, which is a `.nupkg` file, you can publish it to the gallery of your choice as described on [Publishing a Package](../nuget-org/publish-a-package.md).

You might also want to extend the capabilities of your package or otherwise support other scenarios as described in the following topics:

- [NuGet pack and restore as MSBuild targets](../reference/msbuild-targets.md)
- [Package versioning](../concepts/package-versioning.md)
- [Support multiple target frameworks](../create-packages/multiple-target-frameworks-project-file.md)
- [Transformations of source and configuration files](../create-packages/source-and-config-file-transformations.md)
- [Localization](../create-packages/creating-localized-packages.md)
- [Pre-release versions](../create-packages/prerelease-packages.md)
- [Set package type](../create-packages/set-package-type.md)
- [MSBuild props and targets](../concepts/MSBuild-props-and-targets.md)
- [Create packages with COM interop assemblies](../create-packages/author-packages-with-COM-interop-assemblies.md)

Finally, there are additional package types to be aware of:

- [Native Packages](../guides/native-packages.md)
- [Symbol Packages](../create-packages/symbol-packages-snupkg.md)
