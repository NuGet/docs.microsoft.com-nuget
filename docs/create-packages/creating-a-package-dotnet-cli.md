---
title: Create a NuGet package using the dotnet CLI
description: A detailed guide to the process of designing and creating a NuGet package, including key decision points like files and versioning.
author: karann-msft
ms.author: karann
ms.date: 07/09/2019
ms.topic: conceptual
---

# Create a NuGet package using the dotnet CLI

No matter what your package does or what code it contains, you use one of the CLI tools, either `nuget.exe` or `dotnet.exe`, to package that functionality into a component that can be shared with and used by any number of other developers. To install the `dotnet` CLI, see [Install NuGet client tools](../install-nuget-client-tools.md). Starting in Visual Studio 2017, the dotnet CLI is included with .NET Core workloads.

For .NET Core and .NET Standard projects that use the [SDK-style format](../resources/check-project-format.md), and any other SDK-style projects, NuGet uses information in the project file directly to create a package. For detailed steps, see [Create .NET Standard Packages with dotnet CLI](../quickstart/create-and-publish-a-package-using-the-dotnet-cli.md), [Create .NET Standard Packages with Visual Studio](../quickstart/create-and-publish-a-package-using-visual-studio.md) or [NuGet pack and restore as MSBuild targets](../reference/msbuild-targets.md).

Technically speaking, a NuGet package is just a ZIP file that's been renamed with the `.nupkg` extension and whose contents match certain conventions. This topic describes the detailed process of creating a package that meets those conventions.

Packaging begins with the compiled code (assemblies), symbols, and/or other files that you want to deliver as a package (see [Overview and workflow](overview-and-workflow.md)). This process is independent from compiling or otherwise generating the files that go into the package, although you can draw from information in a project file to keep the compiled assemblies and packages in sync.

> [!Important]
> This topic applies to non-SDK-style projects, typically projects other than .NET Core and .NET Standard projects using Visual Studio 2017 and higher versions and NuGet 4.0+.

## Decide which files to package

TBD - is this needed? Do you need to include any special files?

## The role and structure of the .nuspec file

A `.nuspec` file is not required to create packages for [SDK-style projects](../resources/check-project-format.md) (typically .NET Core and .NET Standard projects that use the [SDK attribute](/dotnet/core/tools/csproj#additions)). However, a `.nuspec` is generated when you create the package.

If you are creating a package using `dotnet.exe pack` or `msbuild pack target`, we recommend that you [include all the properties](../reference/msbuild-targets.md#pack-target) that are usually in the `.nuspec` file in the project file instead. However, you can instead choose to [use a `.nuspec` file to pack using `dotnet.exe` or `msbuild pack target`](../reference/msbuild-targets.md#packing-using-a-nuspec).

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
> Give the package an identifier that's unique across nuget.org or whatever host you're using.

You can also set the optional properties, such as `Title`, `PackageDescription`, and `PackageTags`, as described in [NuGet metadata properties](/dotnet/core/tools/csproj#nuget-metadata-properties).

> [!Note]
> For packages built for public consumption, pay special attention to the **PackageTags** property, as tags help others find your package and understand what it does.

For details on declaring dependencies and specifying version numbers, see [Package versioning](../reference/package-versioning.md). It is also possible to surface assets from dependencies directly in the package by using the `include` and `exclude` attributes on the `dependency` element. See [.nuspec Reference - Dependencies](../reference/nuspec.md#dependencies).

## Choose a unique package identifier and setting the version number

The package identifier and the version number are the two most important values in the project because they uniquely identify the exact code that's contained in the package.

**Best practices for the package identifier:**

- **Uniqueness**: The identifier must be unique across nuget.org or whatever gallery hosts the package. Before deciding on an identifier, search the applicable gallery to check if the name is already in use. To avoid conflicts, a good pattern is to use your company name as the first part of the identifier, such as `Contoso.`.
- **Namespace-like names**: Follow a pattern similar to namespaces in .NET, using dot notation instead of hyphens. For example, use `Contoso.Utility.UsefulStuff` rather than `Contoso-Utility-UsefulStuff` or `Contoso_Utility_UsefulStuff`. Consumers also find it helpful when the package identifier matches the namespaces used in the code.
- **Sample Packages**: If you produce a package of sample code that demonstrates how to use another package, attach `.Sample` as a suffix to the identifier, as in `Contoso.Utility.UsefulStuff.Sample`. (The sample package would of course have a dependency on the other package.) When creating a sample package, use the convention-based working directory method described earlier. In the `content` folder, arrange the sample code in a folder called `\Samples\<identifier>` as in `\Samples\Contoso.Utility.UsefulStuff.Sample`.

**Best practices for the package version:**

- In general, set the version of the package to match the library, though this is not strictly required. This is a simple matter when you limit a package to a single assembly, as described earlier in [Deciding which assemblies to package](#decide-which-assemblies-to-package). Overall, remember that NuGet itself deals with package versions when resolving dependencies, not assembly versions.
- When using a non-standard version scheme, be sure to consider the NuGet versioning rules as explained in [Package versioning](../reference/package-versioning.md).

> The following series of brief blog posts are also helpful to understand versioning:
>
> - [Part 1: Taking on DLL Hell](http://blog.davidebbo.com/2011/01/nuget-versioning-part-1-taking-on-dll.html)
> - [Part 2: The core algorithm](http://blog.davidebbo.com/2011/01/nuget-versioning-part-2-core-algorithm.html)
> - [Part 3: Unification via Binding Redirects](http://blog.davidebbo.com/2011/01/nuget-versioning-part-3-unification-via.html)

## Readme file

The readme is not required for SDK-style projects.

## Include MSBuild props and targets in a package

In some cases, you might want to add custom build targets or properties in projects that consume your package, such as running a custom tool or process during build. You do this by placing files in the form `<package_id>.targets` or `<package_id>.props` (such as `Contoso.Utility.UsefulStuff.targets`) within the `\build` folder of the project.

Files in the root `\build` folder are considered suitable for all target frameworks. To provide framework-specific files, first place them within appropriate subfolders, such as the following:

    \build
        \netstandard1.4
            \Contoso.Utility.UsefulStuff.props
            \Contoso.Utility.UsefulStuff.targets
        \net462
            \Contoso.Utility.UsefulStuff.props
            \Contoso.Utility.UsefulStuff.targets

Then in the `.nuspec` file, be sure to refer to these files in the `<files>` node:

```xml
<?xml version="1.0"?>
<package >
    <metadata minClientVersion="2.5">
    <!-- ... -->
    </metadata>
    <files>
        <!-- Include everything in \build -->
        <file src="build\**" target="build" />

        <!-- Other files -->
        <!-- ... -->
    </files>
</package>
```

Including MSBuild props and targets in a package was [introduced with NuGet 2.5](../release-notes/NuGet-2.5.md#automatic-import-of-msbuild-targets-and-props-files), therefore it is recommended to add the `minClientVersion="2.5"` attribute to the `metadata` element, to indicate the minimum NuGet client version required to consume the package.

When NuGet installs a package with `\build` files, it adds MSBuild `<Import>` elements in the project file pointing to the `.targets` and `.props` files. (`.props` is added at the top of the project file; `.targets` is added at the bottom.) A separate conditional MSBuild `<Import>` element is added for each target framework.

MSBuild `.props` and `.targets` files for cross-framework targeting can be placed in the `\buildMultiTargeting` folder. During package installation, NuGet adds the corresponding `<Import>` elements to the project file with the condition, that the target framework is not set (the MSBuild property `$(TargetFramework)` must be empty).

With NuGet 3.x, targets are not added to the project but are instead made available through the `project.lock.json`.

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

### Test package installation

Before publishing a package, you typically want to test the process of installing a package into a project. The tests make sure that the necessarily files all end up in their correct places in the project.

You can test installations manually in Visual Studio or on the command line using the normal [package installation steps](../consume-packages/overview-and-workflow.md#ways-to-install-a-nuget-package).

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
