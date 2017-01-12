---
# required metadata

title: Setting up Local NuGet Feeds | Microsoft Docs
author: kraigb
ms.author: kraigb
manager: ghogen
ms.date: 1/9/2017
ms.topic: article
ms.prod: nuget
#ms.service:
ms.technology: nuget
ms.assetid: 1354a527-d988-43d1-8dcf-6ce46ec5d3d4

# optional metadata

#description:
#keywords:
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
# Local Feeds

Local NuGet package feeds are simply folders on your local network in which you place packages (or even just a folder on your own machine). Local feeds can be either a simple folder of packages, or a hierarchical folder structure that include version numbers. With NuGet 3.3 and above, using the hierarchical structure gives much better performance.

In these cases, you enable the source by simply adding the pathname, such as `\\myserver\packages` through the [Package Manager UI](../tools/package-manager-ui.md#package-sources) or the command line using [`nuget sources`](../tools/nuget-exe-cli-reference.md#sources).

## Initializing and maintaining hierarchical folders

With NuGet 3.3 and above, you'll realize much better performance by structuring the feed using a hierarchical versioned folder tree:

    \\myserver\packages
      └─<packageID>
        └─<version>
          ├─<packageID>.<version>.nupkg
          └─<other files>

NuGet will create this structure automatically when you use the [`nuget add`](../tools/nuget-exe-cli-reference.md#add) command to copy packages to the feed:

```bash
    nuget add new_package.1.0.0.nupkg -source \\myserver\packages
```

You can also use the [`nuget init`](../tools/nuget-exe-cli-reference.md#init) command to copy multiple packages from a single folder to the feed. For example, the following command copies all packages from `c:\packages` to a hierarchical tree on `\\myserver\packages`:

```bash
    nuget init c:\packages \\myserver\packages
```

Again, this will create a folder for each package identifier, each of which will contain a version number folder, within which will be the appropriate package.
