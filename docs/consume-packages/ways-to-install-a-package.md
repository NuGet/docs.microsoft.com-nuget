---
title: Ways to install NuGet packages
description: Describes the process of installing NuGet packages into a project, including what happens on disk and to applicable project files.
author: karann-msft
ms.author: karann
ms.date: 02/12/2018
ms.topic: overview
---

# Different ways to install a NuGet Package

NuGet packages are downloaded and installed using any of the methods in the following table (see [Install NuGet client tools](../install-nuget-client-tools.md) if you don't have these installed already). The package may be retrieved from a cache instead of downloaded.

| Method | Description |
| --- | --- |
| dotnet.exe CLI | (All platforms) Retrieves the package identified by \<package_name\>, expands its contents into a folder in the current directory, and adds a reference to the project file. Also retrieves and installs dependencies.<ul><li>[Install and use a package (dotnet CLI)](../quickstart/install-and-use-a-package-using-the-dotnet-cli.md)</li><li>[dotnet add package command](/dotnet/core/tools/dotnet-add-package)</li></ul> |
| Package Manager UI (Visual Studio) | (Windows and Mac) Provides a UI through which you can browse, select, and install packages and their dependencies into a project from a specified package source. Adds references to installed packages to the project file.<ul><li>[Install and use a package (Visual Studio)](../quickstart/install-and-use-a-package-in-visual-studio.md)</li><li>[Package Manager UI reference (Windows)](../tools/package-manager-ui.md)</li><li>[Including a NuGet package in your project (Mac)](/visualstudio/mac/nuget-walkthrough)</li></ul> |
| Package Manager Console (Visual Studio)<br/>`Install-Package <package_name>` | (Windows only) Retrieves and installs the package identified by \<package_name\> from a selected source into a specified project in the solution, then adds a reference to the project file. Also retrieves and installs dependencies.<ul><li>[Install and use a package (Visual Studio)](../quickstart/install-and-use-a-package-in-visual-studio.md)</li><li>[Package Manager Console guide](../tools/package-manager-console.md)</li></ul> |
| nuget.exe CLI | (All platforms) CLI tool for .NET Framework libraries and non-SDK-style projects that target .NET Standard libraries. Retrieves the package identified by \<package_name\> and expands its contents into a folder in the current directory; can also retrieve all packages listed in a `packages.config` file. Also retrieves and installs dependencies, but makes no changes to project files or `packages.config`.<ul><li>[install command](../tools/cli-ref-install.md)</li></ul> |

## Related articles

- [Overview and workflow of package consumption](../consume-packages/overview-and-workflow.md)
- [Finding and choosing packages](../consume-packages/finding-and-choosing-packages.md)
- [Managing the NuGet cache and global-packages folders](managing-the-global-packages-and-cache-folders.md)
- [Configuring NuGet behavior](../consume-packages/configuring-nuget-behavior.md)
- [Installing signed packages](installing-signed-packages.md)
