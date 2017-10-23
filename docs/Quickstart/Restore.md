---
# required metadata

title: NuGet Quick Guide to Package Restore | Microsoft Docs
author: kraigb
ms.author: kraigb
manager: ghogen
ms.date: 10/3/2017
ms.topic: article
ms.prod: nuget
ms.technology: null
ms.assetid: b70326a0-5bfc-4b7c-881d-7a7d5ebeeed5

# optional metadata

description: A brief description of how to restore NuGet packages in a project.
keywords: NuGet package restore, restoring packages
ms.reviewer:
- karann
- unnir

---

# Quick guide to package restore

You may encounter situations where you need to restore the NuGet packages referenced by a project. For example, a project in Visual Studio may show the following error:

```
This project references NuGet package(s) that are missing on this computer.
Use NuGet Package Restore to download them. The missing file is {name}.
```

Use any of the following methods to quickly restore packages:

[!INCLUDE[package-restore](../includes/package-restore.md)]

For additional details, see [Package restore](../Consume-Packages/Package-Restore.md).