---
# required metadata

title: Overview and workflow of using NuGet packages | Microsoft Docs
author: kraigb
ms.author: kraigb
manager: ghogen
ms.date: 6/6/2017
ms.topic: article
ms.prod: nuget
#ms.service:
ms.technology: null
ms.assetid: 3c60f920-457d-4f43-9efe-210c514e5242

# optional metadata

description: An overview of the process of consuming NuGet packages in a project, with links to other specific parts of the process.
keywords: NuGet package consumption, NuGet consumption overview, NuGet consumption workflow, package consumption workflow, package consumption overview
#ROBOTS:
#audience:
#ms.devlang:
ms.reviewer:
- karann-msft
- unniravindranathan
#ms.suite:
#ms.tgt_pltfrm:
#ms.custom:

---

# Package consumption workflow

Between nuget.org and private package galleries that your organization might establish, you can find tens of thousands of highly useful packages to use in your apps and services. But regardless of the source, consuming a package follows the same general workflow as shown below. For details, see [Finding and Choosing Packages](../consume-packages/finding-and-choosing-packages.md) and the [Use a Package quickstart](../quickstart/use-a-package.md).

![Flow of going to a package source, finding a package, installing it in a project, then adding a using statement and calls to the package API](media/Overview-01-GeneralFlow.png)

\* _Except with `nuget install` from the command-line, in which case it's necessary to edit the configuration files by hand. See the [install command reference](../tools/nuget-exe-cli-reference.md#install)._

NuGet remembers the identity and version number of each installed package, recording it in either `packages.config`, the project file, or a `project.json` file in your project root, depending on project type and your version of NuGet. With NuGet 4.0+, [storing dependencies in the project file](../consume-packages/package-references-in-project-files.md) is the default (except for UWP projects targeting Windows 10 RS1). In any case, you can look in the appropriate file at any time to see the full list of dependencies for your project.

> [!Tip]
> It's prudent to always check the license for each package you intend to use in your software. On nuget.org, you'll find a **License** link on the left side of each package's description page.

When installing packages, NuGet typically checks if the package is already available from its cache. You can manually clear this cache from the command line, as described on [Managing the NuGet cache](../consume-packages/managing-the-nuget-cache.md).

NuGet also makes sure that the target frameworks supported by the package are compatible with your project. If the package does not contain compatible assemblies, NuGet will give you an error message. See [Resolving incompatible package errors](dependency-resolution.md#resolving-incompatible-package-errors).

When adding project code to a source repository, you typically don't include NuGet packages. Those who later clone the repository, which includes build agents on systems like Visual Studio Team Services, must restore the necessary packages prior to running a build:

![Flow of restoring NuGet packages by cloning a repository and using either a restore command](media/Overview-02-RestoreFlow.png)

[Package Restore](../consume-packages/package-restore.md) uses the information in the project file, `packages.config`, `project.json` to reinstall all dependencies. Note that there are differences in the process involved, as described in [Dependency Resolution](../consume-packages/dependency-resolution.md).

Occasionally it's necessary to reinstall packages that are already included in a project, which may also reinstall dependencies. This is easy to do using the `reinstall` command via the NuGet command line or the NuGet Package Manager Console. For details, see [Reinstalling and Updating Packages](../consume-packages/reinstalling-and-updating-packages.md).

Finally, NuGet's behavior is driven by `Nuget.Config` configuration files. Multiple files can be used to centralize certain settings at different levels, as explained in [Configuring NuGet Behavior](../consume-packages/configuring-nuget-behavior.md).

Enjoy your productive coding with NuGet packages!
