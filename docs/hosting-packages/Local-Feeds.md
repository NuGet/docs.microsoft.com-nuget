---
title: Setting up Local NuGet Feeds
description: How to create a local feed for NuGet packages using folders on your local network
author: JonDouglas
ms.author: jodou
ms.date: 12/06/2017
ms.topic: how-to
---

# Local feeds

Local NuGet package feeds are simply hierarchical folder structures on your local network (or even just your own computer) in which you place packages. These feeds can then be used as package sources with all other NuGet operations using the CLI, the Package Manager UI, and the Package Manager Console.

To enable the source, add its pathname (such as `\\myserver\packages`) to the list of sources using the [Package Manager UI](../consume-packages/install-use-packages-visual-studio.md#package-sources) or the [`nuget sources`](../reference/cli-reference/cli-ref-sources.md) command.

> [!Note]
> Hierarchical folder structures are supported in NuGet 3.3+. Older versions of NuGet use only a single folder containing packages, with which performance is much lower than the hierarchical structure.

## Initializing and maintaining hierarchical folders

The hierarchical versioned folder tree has the following general structure:

```
\\myserver\packages
  └─<packageID>
    └─<version>
      ├─<packageID>.<version>.nupkg
      └─<other files>
```

NuGet creates this structure automatically when you use the [`nuget add`](../reference/cli-reference/cli-ref-add.md) command to copy a package to the feed:

```cli
nuget add new_package.1.0.0.nupkg -source \\myserver\packages
```

The `nuget add` command works with one package at a time, which can be inconvenient when setting up a feed with multiple packages.

In such cases, use the [`nuget init`](../reference/cli-reference/cli-ref-init.md) command to copy all packages in a folder to the feed as if you ran `nuget add` on each one individually. For example, the following command copies all packages from `c:\packages` to a hierarchical tree on `\\myserver\packages`:

```cli
nuget init c:\packages \\myserver\packages
```

As with the `add` command, `init` creates a folder for each package identifier, each of which contains a version number folder, within which is the appropriate package.
