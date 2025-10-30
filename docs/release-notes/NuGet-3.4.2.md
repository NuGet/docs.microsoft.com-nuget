---
title: NuGet 3.4.2 Release Notes
description: Release notes for NuGet 3.4.2 including known issues, bug fixes, added features, and DCRs.
author: JonDouglas
ms.author: jodou
ms.date: 11/11/2016
ms.topic: release-notes
---

# NuGet 3.4.2 Release Notes

[NuGet 3.4.1 Release Notes](../release-notes/nuget-3.4.1.md) | [NuGet 3.4.3 Release Notes](../release-notes/nuget-3.4.3.md)

NuGet 3.4.2 was released on April 8, 2016 to address several issues that were identified in the 3.4 and 3.4.1 release.

## nuget.exe 3.4.2 RC is now available

You can download the release candidate of nuget.exe 3.4.2 [here](https://dist.nuget.org/index.html).

## Updates and Improvements

* We have significantly improved the performance of updates in a specific scenario, where updates on packages with deep dependency graphs took a really long time and hung Visual Studio.
* nuget restore without network traffic is 2.5x – 3x faster within Visual Studio.
* In addition to this change, we have fixed an issue where we were hitting the network twice when fetching the update count in the VS UI. This was partially responsible for some timeout issues customers experienced in 3.4/3.4.1.
* Added support for no_proxy setting

## Fixes

* Fixed an issue where nuget.org source was missing in NuGet settings or config after updating to 3.4.1.
* Fixed an issue where a casing change to FindPackagesById in 3.4.1 breaks Artifactory.
* Corrected an issue with FIPS that caused failures with NuGet restore with nuget.exe.
* Fixed a crash when browsing sources with invalid icon URL.
* Fixed issues with merging versions and entries from 'All Sources'.

## Known Issues in 3.4.2 Windows x86 Commandline (RC)

These issues will be fixed early next week before we hit RTM.

*  Running nuget restore on a solution will fail if the solution file is placed in a lower folder hierarchy than the project files.
*  Running nuget delete command on a package using the V2 feed will fail. Use V3 feed instead.


For the complete list of fixes and improvements in this release, check out the list of issues [here](https://github.com/NuGet/Home/issues?utf8=%E2%9C%93&q=is%3Aissue+milestone%3A3.4.2++is%3Aclosed+).
