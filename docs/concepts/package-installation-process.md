---
title: What happens when a package is installed?
description: Detailed information about the package installation process
author: karann-msft
ms.author: karann
ms.date: 0/20/2019
ms.topic: conceptual
---

# What happens when a NuGet package is installed?

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
