---
# required metadata

title: Multi-targeting for NuGet Packages | Microsoft Docs
author: kraigb
ms.author: kraigb
manager: ghogen
ms.date: 7/26/2017
ms.topic: article
ms.prod: nuget
#ms.service:
ms.technology: null
ms.assetid: 2646ffcd-83ae-4086-8915-a7fba3f53e45

# optional metadata

description: Description of the various methods to target multiple .NET Framework versions from within a single NuGet package.
keywords: NuGet package targeting, .NET Framework versions, NuGet and .NET, targeting multiple frameworks, NuGet package creation
#ROBOTS:
#audience:
#ms.devlang:
ms.reviewer:
- karann
- unnir
#ms.suite:
#ms.tgt_pltfrm:
#ms.custom:

---
# Supporting multiple .NET framework versions

*For .NET Core projects using NuGet 4.0+, see [NuGet pack and restore as MSBuild targets](../schema/msbuild-targets.md) for details on cross-targeting.*

Many libraries target a specific version of the .NET Framework. For example, you might have one version of your library that's specific to UWP, and another version that takes advantage of features in .NET Framework 4.6.

To accommodate this, NuGet supports putting multiple versions of the same library in a single package when using the convention-based working directory method described in [Creating a package](../create-packages/creating-a-package.md#from-a-convention-based-working-directory).

In this topic:

- [Framework version folder structure](#framework-version-folder-structure)
- [Matching assembly versions and the target framework in a project](#matching-assembly-versions-and-the-target-framework-in-a-project)
- [Grouping assemblies by framework version](#grouping-assemblies-by-framework-version)
- [Determining which NuGet target to use](#determining-which-nuget-target-to-use)
- [Content files and PowerShell scripts](#content-files-and-powershell-scripts)


## Framework version folder structure

When building a package that contains only one version of a library, you place its files in the `lib` folder of your project.

When specifically targeting multiple frameworks, you instead make subfolders under `lib` using different case-sensitive framework names with the following convention:

    lib\{framework name}[{version}]

For a complete list of supported names, see the [Target Frameworks reference](../schema/target-frameworks.md#supported-frameworks).

If you also have a version of the library that is not specific to a framework, place it in the root `lib` folder.

For example, the following folder structure supports five versions of an assembly, four that are framework-specific and one that is not:

    \lib
        MyAssembly.dll
        \net46
            \MyAssembly.dll
        \net461
            \MyAssembly.dll
        \uap
            \MyAssembly.dll
        \netcore
            \MyAssembly.dll

To easily include all these files when building the package, use a recursive `**` wildcard in the `<files>` section of your `.nuspec`:

```xml
<files>
    <file src="lib\**" target="lib" />
</files>
```

## Matching assembly versions and the target framework in a project

When NuGet installs a package that has multiple assembly versions, it tries to match the framework name of the assembly with the target framework of the project.

If a match is not found, NuGet copies the assembly for the highest version that is less than or equal to the project's target framework, using an assembly in the root `lib` folder, if present, as the fallback.

For example, consider the following folder structure in a package:

    \lib
        \MyAssembly.dll
        \net45
            \MyAssembly.dll
        \net461
            \MyAssembly.dll


When installing this package in a project that targets .NET Framework 4.6, NuGet installs the assembly in the `net45` folder, because that's the highest available version that's less than or equal to 4.6.

If the project targets .NET Framework 4.6.1, on the other hand, NuGet installs the assembly in the `net461` folder.

If the project targets .NET framework 4.0 and earlier, NuGet installs the assembly in the root `lib` folder.

## Grouping assemblies by framework version

NuGet copies assemblies from only a single library folder in the package. For example, suppose a package has the following folder structure:

    \lib
        \net40
            \MyAssembly.dll (v1.0)
            \MyAssembly.Core.dll (v1.0)
        \net45
            \MyAssembly.dll (v2.0)

When the package is installed in a project that targets .NET Framework 4.5, `MyAssembly.dll` (v2.0) is the only assembly installed. `MyAssembly.Core.dll` (v1.0) is not installed because it's not listed in the `net45` folder. NuGet behaves this way because `MyAssembly.Core.dll` might have merged into version 2.0 of `MyAssembly.dll`.

If you want `MyAssembly.Core.dll` to be installed for .NET Framework 4.5, place a copy in the `net45` folder.

The rule about copying assemblies from only one folder also applies to the root `lib` folder. Suppose a package has the following folder structure:

    \lib
        \MyAssembly.dll (v1.0)
        \MyAssembly.Core.dll (v1.0)
        \Net45
            \MyAssembly.dll (v2.0)

In projects that target .NET Framework 4.0 and earlier, NuGet copies both `MyAssembly.dll` (v1.0) and `MyAssembly.Core.dll` (v1.0) because their location in the package does not restrict them to a specific target. In projects that target .NET Framework 4.5, however, only `MyAssembly.dll` (v2.0) from the `net45` folder is copied. Again, to include `MyAssembly.Core.dll` for .NET Framework 4.5, place a copy of it in the `net45` folder.

## Grouping assemblies by framework profile

NuGet also supports targeting a specific framework profile by appending a dash and the profile name to the end of the folder.

    lib\{framework name}-{profile}

The supported profiles are as follows:

- `client`: Client Profile
- `full`: Full Profile
- `wp`: Windows Phone
- `cf`: Compact Framework

## Determining which NuGet target to use

When packaging libraries targeting the Portable Class Library it can be tricky to determine which NuGet target you should use in your folder names and `.nuspec` file, especially if targeting only a subset of the PCL. The following external resources will help you with this:

- [Framework profiles in .NET](http://blog.stephencleary.com/2012/05/framework-profiles-in-net.html) (stephenclearly.com)
- [Portable Class Library profiles](http://embed.plnkr.co/03ck2dCtnJogBKHJ9EjY/preview) (plnkr.co): Table enumerating PCL profiles and their equivalent NuGet targets
- [Portable Class Library profiles tool](https://github.com/StephenCleary/PortableLibraryProfiles) (github.com): command line tool for determining PCL profiles available on your system

## Content files and PowerShell scripts

> [!Warning]
> Mutable content file support and script execution is available in NuGet 2.x, but deprecated in NuGet 3.x and later.

With NuGet 2.x, content files and PowerShell scripts can be grouped by target framework using the same folder convention inside the `content` and `tools` folders. For example:

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

If a framework folder is left empty, NuGet doesn't add assembly references or content files or run the PowerShell scripts for that framework.

> [!Note]
> Because `init.ps1` is executed at the solution level and not dependent on project, it must be placed directly under the `tools` folder. It's ignored if placed under a framework folder.
