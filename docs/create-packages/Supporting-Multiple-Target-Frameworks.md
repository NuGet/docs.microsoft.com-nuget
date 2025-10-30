---
title: Multi-targeting for NuGet Packages
description: Description of the various methods to target multiple .NET Framework versions from within a single NuGet package.
author: JonDouglas
ms.author: jodou
ms.date: 07/15/2019
ms.topic: concept-article
---

# Support multiple .NET versions

Many libraries target a specific version of the .NET Framework. For example, you might have one version of your library that's specific to UWP, and another version that takes advantage of features in .NET Framework 4.6. To accommodate this, NuGet supports putting multiple versions of the same library in a single package.

This article describes the layout of a NuGet package, regardless of how the package or assemblies are built (that is, the layout is the same whether using multiple non-SDK-style *.csproj* files and a custom *.nuspec* file, or a single multi-targeted SDK-style *.csproj*). For an SDK-style project, NuGet [pack targets](../reference/msbuild-targets.md) knows how the package must be layed out and automates putting the assemblies in the correct lib folders and creating dependency groups for each target framework (TFM). For detailed instructions, see [Support multiple .NET Framework versions in your project file](multiple-target-frameworks-project-file.md).

You must manually lay out the package as described in this article when using the convention-based working directory method described in [Creating a package](../create-packages/creating-a-package.md#from-a-convention-based-working-directory). For an SDK-style project, the automated method is recommended, but you may also choose to manually lay out the package as described in this article.

## Framework version folder structure

When building a package that contains only one version of a library or target multiple frameworks, you always make subfolders under `lib` using different case-sensitive framework names with the following convention:

```
lib\{framework name}[{version}]
```

For a complete list of supported names, see the [Target Frameworks reference](../reference/target-frameworks.md#supported-frameworks).

You should never have a version of the library that is not specific to a framework and placed directly in the root `lib` folder. (This capability was supported only with `packages.config`). Doing so would make the library compatible with any target framework and allow it to be installed anywhere, likely resulting in unexpected runtime errors. Adding assemblies in the root folder (such as `lib\abc.dll`) or subfolders (such as `lib\abc\abc.dll`) has been deprecated and is ignored when using the PackagesReference format.

For example, the following folder structure supports four versions of an assembly that are framework-specific:

```
\lib
    \net46
        \MyAssembly.dll
    \net461
        \MyAssembly.dll
    \uap
        \MyAssembly.dll
    \netcore
        \MyAssembly.dll
```

To easily include all these files when building the package, use a recursive `**` wildcard in the `<files>` section of your `.nuspec`:

```xml
<files>
    <file src="lib\**" target="lib/{framework name}[{version}]" />
</files>
```

### Architecture-specific folders

If you have architecture-specific assemblies, that is, separate assemblies that target ARM, x86, and x64, you must place them in a folder named `runtimes` within sub-folders named `{platform}-{architecture}\lib\{framework}` or `{platform}-{architecture}\native`. For example, the following folder structure would accommodate both native and managed DLLs targeting Windows 10 and the `uap10.0` framework:

```
\runtimes
    \win10-arm
        \native
        \lib\uap10.0
    \win10-x86
        \native
        \lib\uap10.0
    \win10-x64
        \native
        \lib\uap10.0
```

These assemblies will only be available at runtime, so if you want to provide the corresponding compile time assembly as well then have `AnyCPU` assembly in `/ref/{tfm}` folder. 

Please note, NuGet always picks these compile or runtime assets from one folder so if there are some compatible assets from `/ref` then `/lib` will be ignored to add compile-time assemblies. Similarly, if there are some compatible assets from `/runtimes` then also `/lib` will be ignored for runtime.

See [Create UWP Packages](../guides/create-uwp-packages.md) for an example of referencing these files in the `.nuspec` manifest.

Also, see [Packing a Windows store app component with NuGet](/archive/blogs/mim/packaging-a-windows-store-apps-component-with-nuget-part-2)

## Matching assembly versions and the target framework in a project

When NuGet installs a package that has multiple assembly versions, it tries to match the framework name of the assembly with the target framework of the project.

If a match is not found, NuGet copies the assembly for the highest version that is less than or equal to the project's target framework, if available. If no compatible assembly is found, NuGet returns an appropriate error message.

For example, consider the following folder structure in a package:

```
\lib
    \net45
        \MyAssembly.dll
    \net461
        \MyAssembly.dll
```

When installing this package in a project that targets .NET Framework 4.6, NuGet installs the assembly in the `net45` folder, because that's the highest available version that's less than or equal to 4.6.

If the project targets .NET Framework 4.6.1, on the other hand, NuGet installs the assembly in the `net461` folder.

If the project targets .NET framework 4.0 and earlier, NuGet throws an appropriate error message for not finding the compatible assembly.

## Grouping assemblies by framework version

NuGet copies assemblies from only a single library folder in the package. For example, suppose a package has the following folder structure:

```
\lib
    \net40
        \MyAssembly.dll (v1.0)
        \MyAssembly.Core.dll (v1.0)
    \net45
        \MyAssembly.dll (v2.0)
```

When the package is installed in a project that targets .NET Framework 4.5, `MyAssembly.dll` (v2.0) is the only assembly installed. `MyAssembly.Core.dll` (v1.0) is not installed because it's not listed in the `net45` folder. NuGet behaves this way because `MyAssembly.Core.dll` might have merged into version 2.0 of `MyAssembly.dll`.

If you want `MyAssembly.Core.dll` to be installed for .NET Framework 4.5, place a copy in the `net45` folder.

## Grouping assemblies by framework profile

NuGet also supports targeting a specific framework profile by appending a dash and the profile name to the end of the folder.

lib\{framework name}-{profile}

The supported profiles are as follows:

- `client`: Client Profile
- `full`: Full Profile
- `wp`: Windows Phone
- `cf`: Compact Framework

## Declaring dependencies (Advanced)

When packing a project file, NuGet tries to automatically generate the dependencies from the project. The information in this section about using a *.nuspec* file to declare dependencies is typically necessary for advanced scenarios only.

*(Version 2.0+)* You can declare package dependencies in the *.nuspec* corresponding to the target framework of the target project using `<group>` elements within the `<dependencies>` element. For more information, see [dependencies element](../reference/nuspec.md#dependencies-element).

Each group has an attribute named `targetFramework` and contains zero or more `<dependency>` elements. Those dependencies are installed together when the target framework is compatible with the project's framework profile. See [Target frameworks](../reference/target-frameworks.md) for the exact framework identifiers.

We recommend using one group per Target Framework Moniker (TFM) for files in the *lib/* and *ref/* folders.

The following example shows different variations of the `<group>` element:

```xml
<dependencies>

    <group targetFramework="net472">
        <dependency id="jQuery" version="1.10.2" />
        <dependency id="WebActivatorEx" version="2.2.0" />
    </group>

    <group targetFramework="net20">
    </group>

</dependencies>
```

## Determining which NuGet target to use

When packaging libraries targeting the Portable Class Library it can be tricky to determine which NuGet target you should use in your folder names and `.nuspec` file, especially if targeting only a subset of the PCL. The following external resources will help you with this:

- [Framework profiles in .NET](https://blog.stephencleary.com/2012/05/framework-profiles-in-net.html) (stephencleary.com)
- [Portable Class Library profiles](https://embed.plnkr.co/03ck2dCtnJogBKHJ9EjY/preview) (plnkr.co): Table enumerating PCL profiles and their equivalent NuGet targets
- [Portable Class Library profiles tool](https://github.com/StephenCleary/PortableLibraryProfiles) (github.com): command line tool for determining PCL profiles available on your system

## Content files and PowerShell scripts

> [!Warning]
> Mutable content files and script execution are available with the `packages.config` format only; they are deprecated with all other formats and should not be used for any new packages.

With `packages.config`, content files and PowerShell scripts can be grouped by target framework using the same folder convention inside the `content` and `tools` folders. For example:

```
\content
    \net46
        \MyContent.txt
    \net461
        \MyContent461.txt
    \uap
        \MyUWPContent.html
    \netcore
\tools
    init.ps1
    \net46
        install.ps1
        uninstall.ps1
    \uap
        install.ps1
        uninstall.ps1
```

If a framework folder is left empty, NuGet doesn't add assembly references or content files or run the PowerShell scripts for that framework.

> [!Note]
> Because `init.ps1` is executed at the solution level and not dependent on project, it must be placed directly under the `tools` folder. It's ignored if placed under a framework folder.
