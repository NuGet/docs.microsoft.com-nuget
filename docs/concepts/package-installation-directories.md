---
title: Package installation directories
description: Learn where NuGet installs packages and how the global-packages folder, fallback folders, and solution packages folder differ.
author: nkolev92
ms.date: 11/02/2026
ms.topic: concept-article
---

# Package installation directories

NuGet extracts packages into directories so your projects can use their contents.
The location depends on how your project references packages and how you configure NuGet.

## Global-packages folder

The global-packages folder is NuGet's de facto package installation directory.
When you restore a project that uses [PackageReference](../consume-packages/Package-References-in-Project-Files.md), NuGet uses packages directly from this folder instead of copying them into your project.
If another project needs the same package version, NuGet can reuse the installed copy without downloading or extracting it again.

Each package has a directory named after its package ID, with a subdirectory for each installed version.
NuGet converts the package ID and version to lowercase for these directory names and uses the [normalized version number](package-versioning.md#normalized-version-numbers).
For example, version `13.0.3` of `Newtonsoft.Json` is stored under `newtonsoft.json\13.0.3`.
The following example shows some of the files in that directory:

```text
packages\
`-- newtonsoft.json\
    `-- 13.0.3\
        |-- newtonsoft.json.13.0.3.nupkg
        |-- newtonsoft.json.13.0.3.nupkg.sha512
        |-- newtonsoft.json.nuspec
        |-- .nupkg.metadata
        `-- lib\
            `-- net6.0\
                `-- Newtonsoft.Json.dll
```

Package contents are extracted into directories such as `lib`, where projects can use them directly.
Extracted assemblies keep their original file names, such as `Newtonsoft.Json.dll`.

NuGet maintains the `.nupkg.sha512` and `.nupkg.metadata` files for its own use.
Don't rely on their formats remaining stable.

By default, the global-packages folder is `%userprofile%\.nuget\packages` on Windows and `~/.nuget/packages` on macOS and Linux.
For information about changing its location or removing packages, see [Manage global packages and cache folders](../consume-packages/managing-the-global-packages-and-cache-folders.md).

## Fallback package folders

Fallback package folders are additional directories that contain packages that are already installed.
They use the same directory layout as the global-packages folder, including the `.nupkg` files and extracted contents.

For PackageReference projects, NuGet first checks the global-packages folder for the required package ID and version, then checks the configured fallback folders.
If it finds the package in a fallback folder, your project uses it directly from that location.
NuGet doesn't need to download it or copy it into the global-packages folder.
If the package isn't in either location, NuGet retrieves it from a package source and installs it into the global-packages folder.

For example, you can prepare a fallback folder with commonly used packages as part of a build machine image.
Projects can reuse those packages without each user needing a separate installed copy.
Unlike a local package feed, a fallback folder must contain extracted packages, not just `.nupkg` files.

Configure these locations with the [`fallbackPackageFolders` section in `nuget.config`](../reference/nuget-config-file.md#fallbackpackagefolders-section).

## Solution packages folder

Projects that use [packages.config](../reference/packages-config.md) keep their installed packages in a separate directory, usually a `packages` folder next to the solution file.
Each package has a directory that combines its ID and version, such as `Newtonsoft.Json.13.0.3`.
These projects reference the package files in that directory rather than using them directly from the global-packages folder.

If a package is missing from the solution packages folder, NuGet copies it from the global-packages folder, and only downloads it if it's missing from both, as described in [What happens when a NuGet package is installed?](package-installation-process.md).

To change this location, set [`repositoryPath` in `nuget.config`](../reference/nuget-config-file.md#config-section).
