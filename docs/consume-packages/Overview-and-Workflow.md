---
title: Overview and workflow of using NuGet packages
description: An overview of the process of consuming NuGet packages in a project, with links to other specific parts of the process.
author: JonDouglas
ms.author: jodou
ms.date: 03/22/2018
ms.topic: concept-article
---

# Package consumption workflow

Between nuget.org and private package galleries that your organization might establish, you can find tens of thousands of highly useful packages to use in your apps and services. But regardless of the source, consuming a package follows the same general workflow.

![Flow of going to a package source, finding a package, installing it in a project, then adding a using statement and calls to the package API](media/Overview-01-GeneralFlow.png)

\* _Visual Studio and `dotnet.exe` only. The `nuget install` command does not modify project files or the `packages.config` file; entries must be managed manually._

For further details, see [Finding and Choosing Packages](../consume-packages/finding-and-choosing-packages.md) and [What happens when a package is installed?](../concepts/package-installation-process.md).

NuGet remembers the identity and version number of each installed package, recording it in either the project file (using [PackageReference](../consume-packages/package-references-in-project-files.md)) or [`packages.config`](../reference/packages-config.md), depending on project type and your version of NuGet. With NuGet 4.0+, PackageReference is preferred, although this is configurable in Visual Studio through the [Package Manager UI](install-use-packages-visual-studio.md). In any case, you can look in the appropriate file at any time to see the full list of dependencies for your project.

> [!Tip]
> It's prudent to always check the license for each package you intend to use in your software. On nuget.org, you find a **License Info** link on the right side of each package's description page. If a package does not specify license terms, contact the package owner directly using the **Contact owners** link on the package page. Microsoft does not license any intellectual property to you from third party package providers and is not responsible for information provided by third parties.

When installing packages, NuGet typically checks if the package is already available from its cache. You can manually clear this cache from the command line, as described on [Managing the global packages and cache folders](../consume-packages/managing-the-global-packages-and-cache-folders.md).

NuGet also makes sure that the target frameworks supported by the package are compatible with your project. If the package does not contain compatible assemblies, NuGet displays an error. See [Resolving incompatible package errors](../concepts/dependency-resolution.md#resolving-incompatible-package-errors).

When adding project code to a source repository, you typically don't include NuGet packages. Those who later clone the repository or otherwise acquire the project, including build agents on systems like Visual Studio Team Services, must restore the necessary packages prior to running a build:

![Flow of restoring NuGet packages by cloning a repository and using either a restore command](media/Overview-02-RestoreFlow.png)

[Package Restore](../consume-packages/package-restore.md) uses the information in the project file or `packages.config` to reinstall all dependencies. Note that there are differences in the process involved, as described in [Dependency Resolution](../concepts/dependency-resolution.md). Also, the diagram above does not show a restore command for the Package Manager Console because if you're with the Console you're already in the context of Visual Studio, which typically restores packages automatically and provides the solution-level command as shown.

Occasionally it's necessary to reinstall packages that are already included in a project, which may also reinstall dependencies. This is easy to do using the `nuget reinstall` command or the NuGet Package Manager Console. For details, see [Reinstalling and Updating Packages](../consume-packages/reinstalling-and-updating-packages.md).

Finally, NuGet's behavior is driven by `NuGet.Config` files. Multiple files can be used to centralize certain settings at different levels, as explained in [Configuring NuGet Behavior](../consume-packages/configuring-nuget-behavior.md).

## Ways to install a NuGet Package

NuGet packages are downloaded and installed using any of the methods in the following table.

| Tool | Platforms | Description |
| --- | --- | --- |
| [dotnet CLI](install-use-packages-dotnet-cli.md) | All | CLI tool for .NET Core and .NET Standard libraries, and for SDK-style projects that target .NET Framework (see [SDK attribute](/dotnet/core/tools/csproj#additions)). Retrieves the package identified by \<package_name\> and adds a reference to the project file. Also retrieves and installs dependencies. |
| Visual Studio | Windows and Mac | Provides a UI through which you can browse, select, and install packages and their dependencies into a project from a specified package source. Adds references to installed packages to the project file.<ul><li>[Install and manage packages using Visual Studio](install-use-packages-visual-studio.md)</li><li>[Including a NuGet package in your project (Mac)](/visualstudio/mac/nuget-walkthrough)</li></ul> |
| [Package Manager Console (Visual Studio)](install-use-packages-powershell.md) | Windows only | Retrieves and installs the package identified by \<package_name\> from a selected source into a specified project in the solution, then adds a reference to the project file. Also retrieves and installs dependencies. |
| [nuget.exe CLI](install-use-packages-nuget-cli.md) | All | CLI tool for .NET Framework libraries and non-SDK-style projects that target .NET Standard libraries. Retrieves the package identified by \<package_name\> and expands its contents into a folder in the current directory; can also retrieve all packages listed in a `packages.config` file. Also retrieves and installs dependencies, but makes no changes to project files or `packages.config`. |
