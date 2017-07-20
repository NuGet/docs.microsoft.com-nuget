---
# required metadata

title: Overview and workflow of creating NuGet packages | Microsoft Docs
author: kraigb
ms.author: kraigb
manager: ghogen
ms.date: 7/20/2017
ms.topic: article
ms.prod: nuget
#ms.service:
ms.technology: null
ms.assetid: 3c60f920-457d-4f43-9efe-210c514e5242

# optional metadata

description: An overview of the process of creating and publishing a NuGet package, with links to other specific parts of the process.
keywords: NuGet package creation, NuGet creation overview, NuGet creation workflow, package creation workflow, package creation overview.
#ROBOTS:
#audience:
#ms.devlang:
ms.reviewer:
- karann
- unnir
#ms.suite:
#ms.tgt_pltfrm:
#ms.custom:

---
# Package creation workflow

Creating a package starts with the code you want to package and share with others, either through the public nuget.org gallery, a gallery hosted on Visual Studio Team Services, or a private gallery within your organization. The package can also include additional files such as a readme that is displayed when the package is installed, and can include transformations to certain project files.

A package can also serve only to pull in a number of other dependencies and not contain any code of its own, which is a convenient way to create a single package for an SDK that's composed of multiple independent packages. In other cases, a package may contain only symbol (`.pdb`) files to aid debugging.

> [!Note]
> When you create a package for use by other developers, it's important to understand that they are taking a dependency on your work. As such, creating and publishing a package also implies a commitment to fixing bugs and making other updates, or at the very least making the package available as open source so others can help to maintain it.

To learn and understand the creation process, start with [Creating a package](../create-packages/creating-a-package.md) which guides you through the core processes common to all packages. This includes deciding which assemblies to package, creating the `.nuspec` (manifest) file, choosing a package identity and version number, setting a package type, adding a readme, and including MSBuild props and targets. The topic ends with creating the package itself using the `nuget pack` command.

From there, you can consider a number of other options for your package:

-  [Supporting Multiple Target Frameworks](../create-packages/supporting-multiple-target-frameworks.md) describes how to create a package with multiple variants for different .NET Frameworks.
-  [Source and Config File Transformations](../create-packages/source-and-config-file-transformations.md) describes how you can do both one-way token replacements in files that are added to a project, and modify `web.config` and `app.config` with settings that are also backed out when the package is uninstalled.
-  [Creating Localized Packages](../create-packages/creating-localized-packages.md) describes how to structure a package with multiple language resources and how to use separate localized satellite packages.
-  [Pre-release Packages](../create-packages/prerelease-packages.md) demonstrates how to release alpha, beta, and rc packages to those customers who are interested.
-  [Native Packages](../create-packages/native-packages.md) describes the process for creating a package for C++ consumers.
-  [Symbol Packages](../create-packages/symbol-packages.md) offers guidance for supplying symbols for your library that allow consumers to step into your code while debugging.
-  [Package versioning](../schema/package-versioning.md) discusses how to identify the exact versions that you allow for your dependencies (other packages that you consume from your package).


When you're then ready to publish a package to nuget.org, follow the simple process in [Publish a package](../create-packages/publish-a-package.md).

If you want to use a private feed instead of nuget.org, see the [Hosting Packages Overview](../hosting-packages/overview.md)
