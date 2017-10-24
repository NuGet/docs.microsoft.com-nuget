---
# required metadata

title: Troubleshooting NuGet Package Restore | Microsoft Docs
author: kraigb
ms.author: kraigb
manager: ghogen
ms.date: 10/24/2017
ms.topic: article
ms.prod: nuget
ms.technology: null
ms.assetid: b70326a0-5bfc-4b7c-881d-7a7d5ebeeed5

# optional metadata

description: A description of commong nuget restore errors and how to troubleshoot them.
keywords: NuGet package restore, restoring packages, troubleshooting, troubleshoot
ms.reviewer:
- karann
- unnir

---

# Troubleshooting package restore errors

> [!Note]
> This page focuses on common package restore errors and steps to resolve them. For how-to restore packages, see [Package restore](../Consume-Packages/Package-Restore.md#enabling-and-disabling-package-restore).

By default, building a project in Visual Studio automatically restores NuGet packages referenced in the project. However, builds will fail with if package restore is disabled in the **Tools > Options > NuGet Package Manager > Package Restore** settings and the necessary packages are not available on your computer. In these cases you may see the following errors:

```bash
This project references NuGet package(s) that are missing on this computer.
Use NuGet Package Restore to download them. The missing file is {name}.
```

```bash
One or more NuGet packages need to be restored but couldn't be because consent has
not been granted. To give consent, open the Visual Studio Options dialog, click on
the NuGet Package Manager node and check 'Allow NuGet to download missing packages
during build.' You can also give consent by setting the environment variable
'EnableNuGetPackageRestore' to 'true'. Missing packages: {name}	
```

To enable package restore, open **Tools > Options > NuGet Package Manager** and select the options for **Allow NuGet to download missing packages** and **Automatically check for missing packages during build in Visual Studio**:

![enable NuGet package restore in Tool/Options](../Consume-Packages/media/restore-01-autorestoreoptions.png)

