---
title: Overview and workflow of using NuGet packages
description: An overview of the process of consuming NuGet packages in a project, with links to other specific parts of the process.
author: karann-msft
ms.author: karann
ms.date: 03/22/2018
ms.topic: conceptual
---

# Package consumption workflow

Between nuget.org and private package galleries that your organization might establish, you can find tens of thousands of highly useful packages to use in your apps and services. But regardless of the source, consuming a package follows the same general workflow.

![Flow of going to a package source, finding a package, installing it in a project, then adding a using statement and calls to the package API](media/Overview-01-GeneralFlow.png)

\* _Visual Studio and `dotnet.exe` only. The `nuget install` command does not modify project files or the `packages.config` file; entries must be managed manually._

For further details, see [Finding and Choosing Packages](../consume-packages/finding-and-choosing-packages.md) and [What happens when a package is installed?](#what-happens-when-a-package-is-installed?).

NuGet remembers the identity and version number of each installed package, recording it in either the project file (using [PackageReference](../consume-packages/package-references-in-project-files.md)) or [`packages.config`](../reference/packages-config.md), depending on project type and your version of NuGet. With NuGet 4.0+, PackageReference is preferred, although this is configurable in Visual Studio through the [Package Manager UI options](../tools/package-manager-ui.md). In any case, you can look in the appropriate file at any time to see the full list of dependencies for your project.

> [!Tip]
> It's prudent to always check the license for each package you intend to use in your software. On nuget.org, you find a **License Info** link on the right side of each package's description page. If a package does not specify license terms, contact the package owner directly using the **Contact owners** link on the package page. Microsoft does not license any intellectual property to you from third party package providers and is not responsible for information provided by third parties.

When installing packages, NuGet typically checks if the package is already available from its cache. You can manually clear this cache from the command line, as described on [Managing the global packages and cache folders](../consume-packages/managing-the-global-packages-and-cache-folders.md).

NuGet also makes sure that the target frameworks supported by the package are compatible with your project. If the package does not contain compatible assemblies, NuGet displays an error. See [Resolving incompatible package errors](dependency-resolution.md#resolving-incompatible-package-errors).

When adding project code to a source repository, you typically don't include NuGet packages. Those who later clone the repository or otherwise acquire the project, including build agents on systems like Visual Studio Team Services, must restore the necessary packages prior to running a build:

![Flow of restoring NuGet packages by cloning a repository and using either a restore command](media/Overview-02-RestoreFlow.png)

[Package Restore](../consume-packages/package-restore.md) uses the information in the project file or `packages.config` to reinstall all dependencies. Note that there are differences in the process involved, as described in [Dependency Resolution](../consume-packages/dependency-resolution.md). Also, the diagram above does not show a restore command for the Package Manager Console because if you're with the Console you're already in the context of Visual Studio, which typically restores packages automatically and provides the solution-level command as shown.

Occasionally it's necessary to reinstall packages that are already included in a project, which may also reinstall dependencies. This is easy to do using the `nuget reinstall` command or the NuGet Package Manager Console. For details, see [Reinstalling and Updating Packages](../consume-packages/reinstalling-and-updating-packages.md).

Finally, NuGet's behavior is driven by `Nuget.Config` files. Multiple files can be used to centralize certain settings at different levels, as explained in [Configuring NuGet Behavior](../consume-packages/configuring-nuget-behavior.md).

# Ways to install a NuGet Package

NuGet packages are downloaded and installed using any of the methods in the following table. The package may be retrieved from a cache instead of downloaded.

| Tool | Description |
| --- | --- |
| [dotnet.exe CLI](install-use-packages-dotnet-cli.md) | (All platforms) CLI tool for .NET Core and .NET Standard libraries, and for SDK-style projects that target .NET Framework (see [SDK attribute](/dotnet/core/tools/csproj#additions)). Retrieves the package identified by \<package_name\>, expands its contents into a folder in the current directory, and adds a reference to the project file. Also retrieves and installs dependencies. |
| Visual Studio | (Windows and Mac) Provides a UI through which you can browse, select, and install packages and their dependencies into a project from a specified package source. Adds references to installed packages to the project file.<ul><li>[Install and manage packages using Visual Studio](../tools/package-manager-ui.md)</li><li>[Including a NuGet package in your project (Mac)](/visualstudio/mac/nuget-walkthrough)</li></ul> |
| [PowerShell in Visual Studio](../tools/package-manager-console.md) | (Windows only) Retrieves and installs the package identified by \<package_name\> from a selected source into a specified project in the solution, then adds a reference to the project file. Also retrieves and installs dependencies. |
| [nuget.exe CLI](install-use-packages-dotnet-cli.md) | (All platforms) CLI tool for .NET Framework libraries and non-SDK-style projects that target .NET Standard libraries. Retrieves the package identified by \<package_name\> and expands its contents into a folder in the current directory; can also retrieve all packages listed in a `packages.config` file. Also retrieves and installs dependencies, but makes no changes to project files or `packages.config`. |

## What happens when a package is installed?

Simply said, the different NuGet tools typically create a reference to a package in the project file or `packages.config`, then perform a package restore, which effectively installs the package. The exception is `nuget install`, which only expands the package into a `packages` folder and does not modify any other files.

The general process is as follows:

1. (All tools except `nuget.exe`) Record the package identifer and version into the project file or `packages.config`.

2. Acquire the package:
   - Check if the package (by exact identifer and version number) is already installed in the *global-packages* folder as described on [Managing the global packages and cache folders](managing-the-global-packages-and-cache-folders.md).

   - If the package is not in the *global-packages* folder, attempt to retrieve it from the sources listed listed in the [configuration files](Configuring-NuGet-Behavior.md). For online sources, attempt first to retrieve the package from the cache unless `-NoCache` is specified with `nuget.exe` commands or `--no-cache` is specified with `dotnet restore`. (Visual Studio and `dotnet add package` always use the cache.) If a package is used from the cache, "CACHE" appears in the output. The cache has an expiration time of 30 minutes.

   - If the package is not in the cache, attempt to download it from the sources listed in the configuration. If a package is downloaded, "GET" and "OK" appear in the output.

   - If the package cannot be successfully acquired from any sources, installation fails at this point with an error such as [NU1103](../reference/errors-and-warnings/NU1103.md). Note that errors from `nuget.exe` commands show only the last source checked, but implies that the package wasn't available from any source.

   When acquiring the package, the order of sources in the NuGet configuration may apply:

   - For projects using the PackageReference format, NuGet checks sources local folder and network shares before checking HTTP sources.

   - For projects using the `packages.config` management format, NuGet uses the order of the sources in the configuration. An exception is restore operations, in which case source ordering is ignored and NuGet uses the package from whichever source responds first.

   - In general, the order in which NuGet checks sources isn't particularly meaningful, because any given package with a specific identifier and version number is exactly the same on whatever source it's found.

3. (All tools except `nuget.exe`) Save a copy of the package and other information in the *http-cache* folder as described on [Managing the global packages and cache folders](managing-the-global-packages-and-cache-folders.md).

4. If downloaded, install the package into the per-user *global-packages* folder. NuGet creates a subfolder for each package identifier, then creates subfolders for each installed version of the package.

5. Update other project files and folders:

    - For projects using PackageReference, update the package dependency graph stored in `obj/project.assets.json`. Package contents themselves are not copied into any project folder.
    - For projects using `packages.config`, copy those parts of the expanded package that match the project's target framework into project's `packages` folder. (When using `nuget install`, the entire expanded package is copied because `nuget.exe` does not examine project files to identify the target framework.)
    - Update `app.config` and/or `web.config` if the package uses [source and config file transformations](../create-packages/source-and-config-file-transformations.md).

6. Install any down-level dependencies if not already present in the project. This process might update package versions in the process, as described in [Dependency Resolution](../consume-packages/dependency-resolution.md).

7. (Visual Studio only) Display the package's readme file, if available, in a Visual Studio window.

Enjoy your productive coding with NuGet packages!
