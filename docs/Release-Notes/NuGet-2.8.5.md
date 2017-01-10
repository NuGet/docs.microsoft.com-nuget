---
# required metadata

title: NuGet 2.8.5 Release Notes | Microsoft Docs
author: karann
ms.author: karann
manager: ghogen
ms.date: 11/11/2016
ms.topic: article
ms.prod: nuget
#ms.service:
ms.technology: nuget
ms.assetid: 60b96b2e-2a61-4f10-b5ad-3299f5a6d453

# optional metadata

#description: release notes 2.8.5
#keywords: release notes 2.8.5
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
# NuGet 2.8.5 Release Notes

[NuGet 2.8.3 Release Notes](../release-notes/nuget-2.8.3.md) | [NuGet 2.8.6 Release Notes](../release-notes/nuget-2.8.6.md)

NuGet 2.8.5 was released March 30, 2015. It is a minor update to our 2.8.3 VSIX with some targeted fixes.

In this release, the support for NuGet Package Manager dialog was added for [DNX Target Framework Monikers](https://github.com/aspnet/dnx).  These new framework monikers that are supported include:

* **core50** - A 'base' target framework moniker (TFM) that is compatible with the Core CLR.
* **dnx452** - A TFM specific to DNX-based apps using the full 4.5.2 version of the framework
* **dnx46** - A TFM specific to DNX-based apps using the full 4.6 version of the framework
* **dnxcore50** - A TFM specific to DNX-based apps using the Core 5.0 version of the framework

One bug was fixed that prevented packages from installing into FSharp projects properly:

https://nuget.codeplex.com/workitem/4400