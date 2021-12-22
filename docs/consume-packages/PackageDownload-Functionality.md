---
title: PackageDownload
description: Describes the PackageDownload feature, which is a complement to PackageReference.
author: nkolev92
ms.author: nikolev
ms.date: 12/22/2021
ms.topic: conceptual
---

# PackageDownload

Starting with Visual Studio 2017 and .NET SDK 1.0.0, NuGet [PackageReference](..\Package-References-in-Project-Files.md) functionality was added.

`PackageReference` allows you to manage your package dependencies directly in your project file.
When you run restore, the transitive dependencies are resolved automatically and the applicable references are chosen for each package in the project graph.

In [5.3 of NuGet](..\release-notes\NuGet-5.3.md) a companion functionality was introduced for [.NET SDK-style projects](..\resources\check-project-format.md) called `PackageDownload`.

## PackageDownload specification

PackageDownload is a utility feature for all .NET SDK-style projects, and it works along side `PackageReference`.

- Packages acquired through PackageDownload will undergo the same [installation process](..concepts\package-installation-process.md) as packages acquired through PackageReference.
This means [package signatures](installing-signed-packages.md) are validated, [package source mapping](Package-Source-Mapping.md) is considered.
- When a package is specified using a PackageDownload item, its assemblies will not be automatically included in the project.
  - For example: If you installed Newtonsoft.Json as a PackageReference, you will be able to use its APIs in your project. If you installed Newtonsoft.jSon as a PackageDownload, you will not be able to use its APIs.
- When a package is specified using a PackageDownload item, its dependencies will not be automatically resolved.
- Packages specified using PackageDownload will not part of packages created using the pack command.
- Given that no transitive dependency resolution and no asset seletion is happening, attributes such as `PrivateAssets`, `ExcludeAssets`, `IncludeAssets` and `GeneratePathProperty` are *not* supported in PackageDownload. Only the version attribute is supported.
- PackageDownload only allows an exact version specification.
- PackageDownload allows you to download multiple versions of the same package. 

### PackageDownload limitations

Given that this is an advanced feature with limited applicability, it doesn't have support equivalent to PackageReference.

- There is no VisualStudio or dotnet.exe functionality to modify PackageDownload items. You can only change them manually in your project files.
- Commands such as `dotnet list package` will not indicate the presence of PackageDownload items.
- PackageDownload packages are *not* included in the packages lock file. 
- PackageDownload packages are *not* part of the packages created using the pack command. 
