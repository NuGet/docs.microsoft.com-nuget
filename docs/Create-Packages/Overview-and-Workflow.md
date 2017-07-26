---
# required metadata

title: Overview and workflow of creating NuGet packages | Microsoft Docs
author: kraigb
ms.author: kraigb
manager: ghogen
ms.date: 7/26/2017
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

Creating a package starts with the compiled code (typically .NET assemblies) that you want to package and share with others, either through the public nuget.org gallery or a private gallery within your organization. The package can also include additional files such as a readme that is displayed when the package is installed, and can include transformations to certain project files.

A package can also serve to only pull in any number of other dependencies, without containing any code of its own. Such a package is a convenient way to deliver an SDK that's composed of multiple independent packages. In other cases, a package may contain only symbol (`.pdb`) files to aid debugging.

> [!Note]
> When you create a package for use by other developers, it's important to understand that they are taking a dependency on your work. As such, creating and publishing a package also implies a commitment to fixing bugs and making other updates, or at the very least making the package available as open source so others can help to maintain it.

Whatever the case, creating a package begins with deciding which assemblies and other files to package. You then create a manifest file, referred to as a `.nuspec` file, to describe the package's contents along with its identifer, version number, copyright information, MSBuild props and targets, and much more.

When you've prepared all the necessary files in the appropriate folders and have created the appropriate `.nuspec` file, you then use the `nuget pack` command (or the[MSBuild pack target](../Schema/msbuild-targets.md)) to put everything together into a `.nupkg` file. You're then ready to deploy the package to whatever host makes it available to other developers.

> [!Tip]
> A NuGet package with the `.nupkg` extension is simple a ZIP file. To easily examine any package's contents, change the extension to `.zip` and expand its contents as usual. Just be sure to change the extension back to `.nupkg` before attempting to upload it to a host.

To learn and understand the creation process, start with [Creating a package](../create-packages/creating-a-package.md) which guides you through the core processes common to all packages. 

From there, you can consider a number of other options for your package:

-  [Supporting Multiple Target Frameworks](../create-packages/supporting-multiple-target-frameworks.md) describes how to create a package with multiple variants for different .NET Frameworks.
-  [Creating Localized Packages](../create-packages/creating-localized-packages.md) describes how to structure a package with multiple language resources and how to use separate localized satellite packages.
-  [Pre-release Packages](../create-packages/prerelease-packages.md) demonstrates how to release alpha, beta, and rc packages to those customers who are interested.
-  [Source and Config File Transformations](../create-packages/source-and-config-file-transformations.md) describes how you can do both one-way token replacements in files that are added to a project, and modify `web.config` and `app.config` with settings that are also backed out when the package is uninstalled.
-  [Symbol Packages](../create-packages/symbol-packages.md) offers guidance for supplying symbols for your library that allow consumers to step into your code while debugging.
-  [Dependency Versions](../create-packages/dependency-versions.md) discusses how to identify the exact versions that you allow for your dependencies (other packages that you consume from your package).
-  [Native Packages](../create-packages/native-packages.md) describes the process for creating a package for C++ consumers.

When you're then ready to publish a package to nuget.org, follow the simple process in [Publish a package](../create-packages/publish-a-package.md).

If you want to use a private feed instead of nuget.org, see the [Hosting Packages Overview](../hosting-packages/overview.md)
