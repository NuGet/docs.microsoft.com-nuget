---
title: What happens when a package is installed?
description: Detailed information about the package installation process
author: JonDouglas
ms.author: jodou
ms.date: 06/20/2019
ms.topic: article
---

# What happens when a NuGet package is installed?

Simply said, the different NuGet tools typically create a reference to a package in the project file or `packages.config`, then perform a package restore, which effectively installs the package. The exception is `nuget install`, which only expands the package into a `packages` folder and does not modify any other files.

The general process is as follows:

1. (All tools except `nuget.exe`) Record the package identifier and version into the project file or `packages.config`.

   If the installation tool is Visual Studio or the dotnet CLI, the tool first attempts to install the package. If it's incompatible, the package is not added to the project file or `packages.config`.

2. Acquire the package:
   - Check if the package (by exact identifer and version number) is already installed in the *global-packages* folder as described on [Managing the global packages and cache folders](../consume-packages/managing-the-global-packages-and-cache-folders.md).

   - If the package is not in the *global-packages* folder, attempt to retrieve it from the sources listed in the [configuration files](../consume-packages/Configuring-NuGet-Behavior.md). [Package Source Mapping](../consume-packages/package-source-mapping.md) configurations are applied at this point. For online sources, attempt first to retrieve the package from the HTTP cache unless `-NoHttpCache` is specified with `nuget.exe` commands or `--no-http-cache` is specified with `dotnet restore`. (Visual Studio and `dotnet add package` always use the cache.) If a package is used from the cache, "CACHE" appears in the output. The cache has an expiration time of 30 minutes.

   - If the package has been specified using a [floating version](../consume-packages/Package-References-in-Project-Files.md#floating-versions), or without a minimum version, NuGet *will* contact all sources to figure out the best match.
   Example: `1.*`, `(, 2.0.0]`.

   - If the package is not in the HTTP cache, attempt to download it from the sources listed in the configuration. If a package is downloaded, "GET" and "OK" appear in the output. NuGet logs http traffic on normal verbosity.

   - If the package cannot be successfully acquired from any sources, installation fails at this point with an error such as [NU1103](../reference/errors-and-warnings/NU1103.md). Note that errors from `nuget.exe` commands show only the last source checked, but implies that the package wasn't available from any source.

   When acquiring the package, the order of sources in the NuGet configuration may apply:

   - NuGet checks sources local folder and network shares before checking HTTP sources.

3. Save a copy of the package and other information in the *http-cache* folder as described on [Managing the global packages and cache folders](../consume-packages/managing-the-global-packages-and-cache-folders.md).

4. If downloaded, install the package into the per-user *global-packages* folder. NuGet creates a subfolder for each package identifier, then creates subfolders for each installed version of the package.

5. NuGet installs package dependencies as required. This process might update package versions in the process, as described in [Dependency Resolution](../concepts/dependency-resolution.md).

6. Update other project files and folders:

    - For projects using PackageReference, update the package dependency graph stored in `obj/project.assets.json`. Package contents themselves are not copied into any project folder.
    - Update `app.config` and/or `web.config` if the package uses [source and config file transformations](../create-packages/source-and-config-file-transformations.md).

7. (Visual Studio only) Display the package's readme file, if available, in a Visual Studio window.

Enjoy your productive coding with NuGet packages!
