---
title: Ways to Install NuGet Packages | Microsoft Docs
author: kraigb
ms.author: kraigb
manager: ghogen
ms.date: 02/12/2018
ms.topic: get-started-article
ms.prod: nuget
ms.technology: null
description: Describes the process of installing NuGet packages into a project, including what happens on disk and to applicable project files.
keywords: install NuGet, NuGet package consumption, installing NuGet packages, NuGet package references
ms.reviewer:
- karann-msft
- unniravindranathan
---

# Different ways to install a NuGet Package

NuGet packages are downloaded and installed using any of the following methods (see [Install NuGet client tools](../install-nuget-client-tools.md) if you don't have these installed already):

| Method | Description |
| --- | --- |
| dotnet.exe CLI<br/>`dotnet install <package_name>` | (All platforms) Downloads the package identified by \<package_name\>, expands its contents into a folder in the current directory, and adds a reference to the project file. Also downloads and installs dependencies.<ul><li>[Install and use a package (dotnet CLI)](../quickstart/install-and-use-a-package-using-the-dotnet-cli.md)</li><li>[dotnet commands](../tools/dotnet-commands.md)</li></ul> |
| Package Manager UI (Visual Studio) | (Windows and Mac) Provides a UI through which you can browse, select, and install packages and their dependencies into a project. Adds references to installed packages to the project file.<ul><li>[Install and use a package (Visual Studio)](../quickstart/install-and-use-a-package-in-visual-studio.md)</li><li>[Package Manager UI reference (Windows)](../tools/package-manager-ui.md)</li><li>[Including a NuGet package in your project (Mac)](/visualstudio/mac/nuget-walkthrough)</li></ul> |
| Package Manager Console (Visual Studio)<br/>`Install-Package <package_name>` | (Windows only) Downloads and installs the package identified by \<package_name\> into a specified project in the solution, then adds a reference to the project file. Also downloads and installs dependencies.<ul><li>[Install and use a package (Visual Studio)](../quickstart/install-and-use-a-package-in-visual-studio.md)</li><li>[Package Manager Console guide](../tools/package-manager-console.md)</li></ul> |
| nuget.exe CLI<br/>`nuget install <package_name>` | (All platforms) Downloads the package identified by \<package_name\> and expands its contents into a folder in the current directory; can also download all packages listed in a `packages.config` file. Also downloads and installs dependencies, but makes no changes to project files.<ul><li>[install command](../tools/cli-ref-install.md)</li></ul> |

## General install process

In general, NuGet does the following then asked to install a package:

1. Acquire the package:
    - Check if the requested package already exists in a cache (see [Managing the NuGet cache](managing-the-nuget-cache.md)).
    - If the package is not in the cache, attempt to download the package from the sources listed in the [configuration files](Configuring-NuGet-Behavior.md).
      - For projects using the `packages.config` reference format, NuGet uses the order of the sources in the configuration.
      - For projects using the PackageReference format, NuGet checks sources that are local folders first, then checks sources on network shares, then checks HTTP (Internet) sources.
      - In general, the order in which NuGet checks sources isn't particularly meaningful, because any given package with a specific identifier and version number is exactly the same on whatever source it's found.
    - If the package is successfully acquired from one of the sources, NuGet adds it to the cache. Otherwise, installation fails.

1. Expand the package into the project.
    - Expanding means unzipping the package's contents into an appropriate subfolder. A copy of the `.nupkg` itself is also placed in the subfolder.
    - If the package is being installed into a Visual Studio or .NET Core project, only the files relevant to the project's target framework are expanded. When installed using the nuget.exe command line, all assemblies are expanded.

1. If the package is installed within Visual Studio or the dotnet CLI, a reference is added to the appropriate project file (or `packages.config` for some project types in Visual Studio).

## Related topics

- [Overview and workflow of package consumption](../consume-packages/overview-and-workflow.md)
- [Finding and choosing packages](../consume-packages/finding-and-choosing-packages.md)
- [Configuring NuGet behavior](../consume-packages/configuring-nuget-behavior.md)
- [Managing the NuGet cache](managing-the-nuget-cache.md)
