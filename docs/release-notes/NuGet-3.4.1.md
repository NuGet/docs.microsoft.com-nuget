---
title: NuGet 3.4.1 Release Notes
description: Release notes for NuGet 3.4.1 including known issues, bug fixes, added features, and DCRs.
author: JonDouglas
ms.author: jodou
ms.date: 11/11/2016
ms.topic: release-notes
---

# NuGet 3.4.1 Release Notes

[NuGet 3.4 Release Notes](../release-notes/nuget-3.4.md) | [NuGet 3.4.2 Release Notes](../release-notes/nuget-3.4.2.md)

NuGet 3.4.1 was released March 30, 2016 at the same time as the Visual Studio 2015 Update 2 and Visual Studio 15 Preview Release to address several issues that were identified in the 3.4 release.

## Updates and Improvements

* Corrected an issue that prevented browsing packages from the Visual Studio UI with a minimum Visual Studio install
* Corrected an issue with Visual Studio locating `lucene.net.dll`
* All sources should not be the default repository source after a NuGet extension install or update.  You can opt-in to this feature from the configuration settings.

We continue to track issues on our GitHub issues list which can be found at: [https://github.com/nuget/home/issues](https://github.com/nuget/home/issues)
