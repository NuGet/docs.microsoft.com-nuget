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
| dotnet.exe CLI<br/>`dotnet add package <package_name>` | (All platforms) Retrieves the package identified by \<package_name\>, expands its contents into a folder in the current directory, and adds a reference to the project file. Also retrieves and installs dependencies.<ul><li>[Install and use a package (dotnet CLI)](../quickstart/install-and-use-a-package-using-the-dotnet-cli.md)</li><li>[dotnet add package command](/dotnet/core/tools/dotnet-add-package)</li></ul> |
| Package Manager UI (Visual Studio) | (Windows and Mac) Provides a UI through which you can browse, select, and install packages and their dependencies into a project from a specified package source. Adds references to installed packages to the project file.<ul><li>[Install and use a package (Visual Studio)](../quickstart/install-and-use-a-package-in-visual-studio.md)</li><li>[Package Manager UI reference (Windows)](../tools/package-manager-ui.md)</li><li>[Including a NuGet package in your project (Mac)](/visualstudio/mac/nuget-walkthrough)</li></ul> |
| Package Manager Console (Visual Studio)<br/>`Install-Package <package_name>` | (Windows only) Retrieves and installs the package identified by \<package_name\> from a selected source into a specified project in the solution, then adds a reference to the project file. Also retrieves and installs dependencies.<ul><li>[Install and use a package (Visual Studio)](../quickstart/install-and-use-a-package-in-visual-studio.md)</li><li>[Package Manager Console guide](../tools/package-manager-console.md)</li></ul> |
| nuget.exe CLI<br/>`nuget install <package_name>` | (All platforms) Retrieves the package identified by \<package_name\> and expands its contents into a folder in the current directory; can also retrieve all packages listed in a `packages.config` file. Also retrieves and installs dependencies, but makes no changes to project files or `packages.config`.<ul><li>[install command](../tools/cli-ref-install.md)</li></ul> |

## What happens when a package is installed

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

## Related articles

- [Overview and workflow of package consumption](../consume-packages/overview-and-workflow.md)
- [Finding and choosing packages](../consume-packages/finding-and-choosing-packages.md)
- [Managing the NuGet cache and global-packages folders](managing-the-global-packages-and-cache-folders.md)
- [Configuring NuGet behavior](../consume-packages/configuring-nuget-behavior.md)
