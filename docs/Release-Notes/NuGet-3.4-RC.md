---
# required metadata

title: NuGet 3.4-RC Release Notes | Microsoft Docs
author: karann-msft-msft
ms.author: karann-msft
manager: ghogen
ms.date: 11/11/2016
ms.topic: article
ms.prod: nuget
#ms.service:
ms.technology: null
ms.assetid: 239d3d95-5a72-4fac-8389-b6deac27884d

# optional metadata

description: Release notes for NuGet 3.4 RC including known issues, bug fixes, added features, and DCRs.
keywords: NuGet 3.4 RC release notes, bug fixes, known issues, added features, DCRs
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

# NuGet 3.4-RC Release Notes

[NuGet 3.3 Release Notes](../release-notes/nuget-3.3.md) | [NuGet 3.4 Release Notes](../release-notes/nuget-3.4.md)

NuGet 3.4-RC was released March 3, 2016 alongside the Visual Studio 2015 Update 2 RC and was built with a few tenets in minds:

*  Cross-Platform support
*  Performance improvements
*  Minor UI improvements

The following features are available in this RC, with more planned for the 3.4 final release.

## New Features

* NuGet clients now support gzip content-encoding from repositories
* Support for PDBs from packages in xproj projects
* Support for iOS and Android build actions in the contentFiles element
* Support for the netstandard and netstandardapp framework monikers

## New User Interface Features

* Significant performance improvements especially on the Installed, Updates, and Consolidate tabs
* Installed and Updates tabs are now sorted alphabetically
* Added a Refresh button that allows a search to be refreshed

## Updates and Improvements

* Packages referenced in `project.json` that have a floating version will not update on every build. Instead, they will update only when forced to restore, clean, rebuild, or modify `project.json`.
* nuget.org repository sources are no longer forced into a project configuration when you use the NuGet configuration UI.
* NuGet no longer restores packages in shared projects nor writes a lock file.
* We've improved network failure and retry handling for unreachable or slow-to-respond servers.
* Keyboard and mouse behaviors are improved in the Visual Studio Package Manager UI.
* We now support the latest `project.json` schema in DNX.

## Known Issues

We continue to track issues on our GitHub issues list which can be found at: [http://github.com/nuget/home/issues](http://github.com/nuget/home/issues)