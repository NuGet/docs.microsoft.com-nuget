---
title: Create a NuGet package using the dotnet CLI
description: A detailed guide to the process of designing and creating a NuGet package, including key decision points like files and versioning.
author: karann-msft
ms.author: karann
ms.date: 07/09/2019
ms.topic: conceptual
---

# Create a NuGet package using the dotnet CLI

No matter what your package does or what code it contains, you use one of the CLI tools, either `nuget.exe` or `dotnet.exe`, to package that functionality into a component that can be shared with and used by any number of other developers. This article describes how to create a package using the dotnet CLI. To install the `dotnet` CLI, see [Install NuGet client tools](../install-nuget-client-tools.md). Starting in Visual Studio 2017, the dotnet CLI is included with .NET Core workloads.

For .NET Core and .NET Standard projects that use the [SDK-style format](../resources/check-project-format.md), and any other SDK-style projects, NuGet uses information in the project file directly to create a package. For detailed steps, see [Create .NET Standard Packages with dotnet CLI](../quickstart/create-and-publish-a-package-using-the-dotnet-cli.md), [Create .NET Standard Packages with Visual Studio](../quickstart/create-and-publish-a-package-using-visual-studio.md) or [NuGet pack and restore as MSBuild targets](../reference/msbuild-targets.md).

`msbuild -t:pack` is functionality equivalent to `dotnet pack`. To build with MSBuild, the concepts are the same as described in this article, but the command line commands are slightly different.

> [!Important]
> This topic applies to [SDK-style](../resources/check-project-format.md) projects, typically .NET Core and .NET Standard projects.

## Set properties

The following properties are required to create a package.

- `PackageId`, the package identifier, which must be unique across the gallery that hosts the package.
- `Version`, a specific version number in the form *Major.Minor.Patch[-Suffix]* where *-Suffix* identifies [pre-release versions](prerelease-packages.md)
- The package title as it should appear on the host (like nuget.org)
- `Authors`, author and owner information.
- `Company`, your company name.

In Visual Studio, you can set these values in the project properties (right-click the project in Solution Explorer, choose **Properties**, and select the **Package** tab). You can also set these properties directly in the project files (`.csproj`).

    ```xml
    <PropertyGroup>
      ...
      <PackageId>AppLogger</PackageId>
      <Version>1.0.0</Version>
      <Authors>your_name</Authors>
      <Company>your_company</Company>
    </PropertyGroup>
    ```

> [!Important]
> Give the package an identifier that's unique across nuget.org or whatever package source you're using.

You can also set the optional properties, such as `Title`, `PackageDescription`, and `PackageTags`, as described in [MSBuild pack targets](../reference/msbuild-targets.md#pack-target), [Controlling dependency assets](../consume-packages/package-references-in-project-files.md#controlling-dependency-assets), and [NuGet metadata properties](/dotnet/core/tools/csproj#nuget-metadata-properties).

> [!NOTE]
> For packages built for public consumption, pay special attention to the **PackageTags** property, as tags help others find your package and understand what it does.

For details on declaring dependencies and specifying version numbers, see [Package versioning](../reference/package-versioning.md). It is also possible to surface assets from dependencies directly in the package by using the `<IncludeAssets>` and `<ExcludeAssets>` attributes. For more information, seee [Controlling dependency assets](../consume-packages/package-references-in-project-files.md#controlling-dependency-assets).

## Choose a unique package identifier and setting the version number

The package identifier and the version number are the two most important values in the project because they uniquely identify the exact code that's contained in the package.

**Best practices for the package identifier:**

- **Uniqueness**: The identifier must be unique across nuget.org or whatever gallery hosts the package. Before deciding on an identifier, search the applicable gallery to check if the name is already in use. To avoid conflicts, a good pattern is to use your company name as the first part of the identifier, such as `Contoso.`.
- **Namespace-like names**: Follow a pattern similar to namespaces in .NET, using dot notation instead of hyphens. For example, use `Contoso.Utility.UsefulStuff` rather than `Contoso-Utility-UsefulStuff` or `Contoso_Utility_UsefulStuff`. Consumers also find it helpful when the package identifier matches the namespaces used in the code.
- **Sample Packages**: If you produce a package of sample code that demonstrates how to use another package, attach `.Sample` as a suffix to the identifier, as in `Contoso.Utility.UsefulStuff.Sample`. (The sample package would of course have a dependency on the other package.) When creating a sample package, use the `contentFiles` value in `<IncludeAssets>`. In the `content` folder, arrange the sample code in a folder called `\Samples\<identifier>` as in `\Samples\Contoso.Utility.UsefulStuff.Sample`.

**Best practices for the package version:**

- In general, set the version of the package to match the project (or assembly), though this is not strictly required. This is a simple matter when you limit a package to a single assembly. Overall, remember that NuGet itself deals with package versions when resolving dependencies, not assembly versions.
- When using a non-standard version scheme, be sure to consider the NuGet versioning rules as explained in [Package versioning](../reference/package-versioning.md).

> The following series of brief blog posts are also helpful to understand versioning:
>
> - [Part 1: Taking on DLL Hell](http://blog.davidebbo.com/2011/01/nuget-versioning-part-1-taking-on-dll.html)
> - [Part 2: The core algorithm](http://blog.davidebbo.com/2011/01/nuget-versioning-part-2-core-algorithm.html)
> - [Part 3: Unification via Binding Redirects](http://blog.davidebbo.com/2011/01/nuget-versioning-part-3-unification-via.html)

## Include MSBuild props and targets in a package (Advanced)

In some cases, you might want to add custom build targets or properties in projects that consume your package, such as running a custom tool or process during build. You can do this by placing files in the form `<package_id>.targets` or `<package_id>.props` (such as `Contoso.Utility.UsefulStuff.targets`) within the `\build` folder of the project.

In the project file, use `<IncludeAssets>` or `<PrivateAssets>` and set the value to `build`. For more information, see [Controlling dependency assets](../consume-packages/package-references-in-project-files.md#controlling-dependency-assets) and [Additions to the csproj format](/dotnet/core/tools/csproj#additions).

Files in the root `\build` folder are considered suitable for all target frameworks. To provide framework-specific files, first place them within appropriate subfolders, such as the following:

    \build
        \netstandard2.0
            \Contoso.Utility.UsefulStuff.props
            \Contoso.Utility.UsefulStuff.targets
        \net462
            \Contoso.Utility.UsefulStuff.props
            \Contoso.Utility.UsefulStuff.targets

## Run the pack command

To build a NuGet package (a `.nupkg` file) from the project, run the `dotnet pack` command, which also builds the project automatically:

```cli
# Uses the project file in the current folder by default
dotnet pack
```

The output shows the path to the `.nupkg` file:

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

When you run `dotnet pack` on a solution, this packs all the projects in the solution that are packable ([<IsPackable>](/dotnet/core/tools/csproj#nuget-metadata-properties) property is set to `true`.

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

- [Package versioning](../reference/package-versioning.md)
- [Support multiple target frameworks](../create-packages/supporting-multiple-target-frameworks.md)
- [Transformations of source and configuration files](../create-packages/source-and-config-file-transformations.md)
- [Localization](../create-packages/creating-localized-packages.md)
- [Pre-release versions](../create-packages/prerelease-packages.md)
- [Set package type](../create-packages/set-package-type.md)
- [Create packages with COM interop assemblies](../create-packages/author-packages-with-COM-interop-assemblies.md)

Finally, there are additional package types to be aware of:

- [Native Packages](../create-packages/native-packages.md)
- [Symbol Packages](../create-packages/symbol-packages.md)
