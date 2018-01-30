---
title: Ways to Install NuGet Packages | Microsoft Docs
author: kraigb
ms.author: kraigb
manager: ghogen
ms.date: 01/30/2018
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
| dotnet.exe CLI<br/>`dotnet install <package_name>` | Downloads the package identified by \<package_name\>, expands its contents into a folder in the current directory, and adds a reference to the project file. Dependencies are also downloaded and expanded.<br/><br/>References: <ul><li>[Install and use a package (dotnet CLI)](../quickstart/install-and-use-a-package-using-the-dotnet-cli.md)</li><li>[dotnet commands](../tools/dotnet-commands.md)</li></ul> |
| Package Manager UI (Visual Studio) | Provides a UI through which you can browse, select, and install packages into a project. Adds references to installed packages to the project file. | [Install and use a package (Visual Studio)](../quickstart/install-and-use-a-package-in-visual-studio.md)<br/><br/>[Package Manager UI reference](../tools/package-manager-ui.md) |
| Package Manager Console (Visual Studio)<br/>`Install-Package <package_name>` | Downloads and installs the package identified by \<package_name\> into a specified project in the solution, then add a reference to the project file.<br/><br/><ul><li>[Install and use a package (Visual Studio)](../quickstart/install-and-use-a-package-in-visual-studio.md)</li><li>[Package Manager Console guide](../tools/package-manager-console.md)</li></ul> |
| nuget.exe CLI<br/>`nuget install <package_name>` | Downloads the package identified by \<package_name\> and expands its contents into a folder in the current directory; can also download all packages listed in a `packages.config` file. Makes no changes to project files. Dependencies are also downloaded and expanded.<br/><br/> See [install command](../tools/cli-ref-install.md).|

## General install process

In general, NuGet does the following then asked to install a package:

1. Acquire the package:
    - Check if the requested package already exists in a cache (see [Managing the NuGet cache](managing-the-nuget-cache.md)).
    - If the package is not in the cache, attempt to download the package from the sources listed in the configuration files, starting with the first in the list. This behavior allows you to use private package feeds before looking for a package on nuget.org (see [Configuring NuGet behavior](configuring-nuget-behavior.md)).
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
