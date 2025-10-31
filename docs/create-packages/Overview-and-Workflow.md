---
title: Overview and workflow of creating NuGet packages
description: An overview of the process of creating and publishing a NuGet package, with links to other specific parts of the process.
author: JonDouglas
ms.author: jodou
ms.date: 07/26/2017
ms.topic: concept-article
---

# Package creation workflow

Creating a package starts with the compiled code (typically .NET assemblies) that you want to package and share with others, either through the public nuget.org gallery or a private gallery within your organization. The package can also include additional files such as a readme that is displayed when the package is installed, and can include transformations to certain project files.

A package can also serve to only pull in any number of other dependencies, without containing any code of its own. Such a package is a convenient way to deliver an SDK that's composed of multiple independent packages. In other cases, a package may contain only symbol (`.pdb`) files to aid debugging.

> [!Note]
> When you create a package for use by other developers, it's important to understand that they are taking a dependency on your work. As such, creating and publishing a package also implies a commitment to fixing bugs and making other updates, or at the very least making the package available as open source so others can help to maintain it.

Whatever the case, creating a package begins with deciding its identifier, version number, license, copyright information, and any other necessary content. Once done, you can use the "pack" command to put everything together into a `.nupkg` file. This file can be published to a NuGet feed, like nuget.org.

> [!Tip]
> A NuGet package with the `.nupkg` extension is simply a ZIP file. To easily examine any package's contents, change the extension to `.zip` and expand its contents as usual. Just be sure to change the extension back to `.nupkg` before attempting to upload it to a host.

To learn and understand the creation process, start with [Creating a package](../create-packages/creating-a-package.md) which guides you through the core processes common to all packages.

From there, you can consider a number of other options for your package:

- [Supporting Multiple Target Frameworks](../create-packages/supporting-multiple-target-frameworks.md) describes how to create a package with multiple variants for different .NET Frameworks.
- [Creating Localized Packages](../create-packages/creating-localized-packages.md) describes how to structure a package with multiple language resources and how to use separate localized satellite packages.
- [Pre-release Packages](../create-packages/prerelease-packages.md) demonstrates how to release alpha, beta, and rc packages to those customers who are interested.
- [Source and Config File Transformations](../create-packages/source-and-config-file-transformations.md) describes how you can do both one-way token replacements in files that are added to a project, and modify `web.config` and `app.config` with settings that are also backed out when the package is uninstalled.
- [Symbol Packages](../create-packages/symbol-packages-snupkg.md) offers guidance for supplying symbols for your library that allow consumers to step into your code while debugging.
- [Package versioning](../concepts/package-versioning.md) discusses how to identify the exact versions that you allow for your dependencies (other packages that you consume from your package).
- [Native Packages](../guides/native-packages.md) describes the process for creating a package for C++ consumers.
- [Signing Packages](../create-packages/sign-a-package.md) describes the process for adding a digital signature to a package.

When you're then ready to publish a package to nuget.org, follow the simple process in [Publish a package](../nuget-org/publish-a-package.md).

If you want to use a private feed instead of nuget.org, see the [Hosting Packages Overview](../hosting-packages/overview.md)
