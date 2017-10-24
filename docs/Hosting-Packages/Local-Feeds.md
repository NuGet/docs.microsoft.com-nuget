---
# required metadata

title: Setting up Local NuGet Feeds | Microsoft Docs
author: kraigb
ms.author: kraigb
manager: ghogen
ms.date: 8/25/2017
ms.topic: article
ms.prod: nuget
#ms.service:
ms.technology: null
ms.assetid: 1354a527-d988-43d1-8dcf-6ce46ec5d3d4

# optional metadata

description: How to create a local feed for NuGet packages using folders on your local network
keywords: NuGet feed, NuGet gallery, local package feed
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
# Local feeds

Local NuGet package feeds are simply folders on your local network (or even just your own computer) in which you place packages. Local feeds can be a simple folder of packages, or a hierarchical folder structure that include version numbers. With NuGet 3.3 and above, using the hierarchical structure gives much better performance.

In all cases, enable the source by adding its pathname, such as `\\myserver\packages`, using the [Package Manager UI](../tools/package-manager-ui.md#package-sources) or the [`nuget sources`](../tools/cli-ref-sources.md) command.

## Initializing and maintaining hierarchical folders

With NuGet 3.3+, you realize much better performance by structuring the feed using a hierarchical versioned folder tree:

    \\myserver\packages
      └─<packageID>
        └─<version>
          ├─<packageID>.<version>.nupkg
          └─<other files>

NuGet creates this structure automatically when you use the [`nuget add`](../tools/cli-ref-add.md) command to copy packages to the feed:

```
nuget add new_package.1.0.0.nupkg -source \\myserver\packages
```

You can also use the [`nuget init`](../tools/cli-ref-init.md) command to copy multiple packages from a single folder to the feed. For example, the following command copies all packages from `c:\packages` to a hierarchical tree on `\\myserver\packages`:

```
nuget init c:\packages \\myserver\packages
```

Again, this creates a folder for each package identifier, each of which contains a version number folder, within which is the appropriate package.
