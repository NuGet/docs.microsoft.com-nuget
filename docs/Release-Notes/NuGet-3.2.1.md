---
# required metadata

title: NuGet 3.2.1 Release Notes | Microsoft Docs
author: karann
ms.author: karann
manager: ghogen
ms.date: 11/11/2016
ms.topic: article
ms.prod: nuget
#ms.service:
ms.technology: nuget
ms.assetid: d27c3bb9-8db1-439a-a134-54e20b7a7766

# optional metadata

#description: release notes 3.2.1
#keywords: release notes 3.2.1
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


# NuGet 3.2.1 Release Notes

[NuGet 3.2 Release Notes](../release-notes/nuget-3.2.md) | [NuGet 3.3 Release Notes](../release-notes/nuget-3.3.md)

NuGet 3.2.1 for the command-line was released October 12, 2015 with a handful of optimizations and fixes for the 3.2 release and is available from [dist.nuget.org](http://dist.nuget.org/index.html).

## Improvements

* NuGet now uses the configuration file with the original casing of `NuGet.Config`.  This is important on case-sensitive operating systems [1427](https://github.com/NuGet/Home/issues/1427)
* NuGet restore will now ignore dnx projects (*.xproj) that should be processed with `dnu` [1227](https://github.com/NuGet/Home/issues/1227)
* Optimized network utilization when working with index.json and package registration data [1426](https://github.com/NuGet/Home/issues/1426)
* Improved resource download handling to be more robust with v2 services [1448](https://github.com/NuGet/Home/issues/1448)

## Fixes

* NuGet update correctly updates csproj/vcxproj references [1483](https://github.com/NuGet/Home/issues/1483)
* Now preventing a local .nuget folder from being created when a SpecialFolders.UserProfile cannot be located [1531](https://github.com/NuGet/Home/issues/1531)
* Improved handling of packages in local cache that are corrupted during download [1405](https://github.com/NuGet/Home/issues/1405) [1157](https://github.com/NuGet/Home/issues/1157)

A complete list of issues addressed for the command-line and Visual Studio extension can be found in the NuGet GitHub [3.2.1 milestone](https://github.com/NuGet/Home/issues?q=milestone%3A3.2.1+is%3Aclosed)

## Known Issues

We continue to track issues on our GitHub issues list which can be found at: [http://github.com/nuget/home/issues](http://github.com/nuget/home/issues)