---
title: NuGet 3.0 RC Release Notes
description: Release notes for NuGet 3.0 RC including known issues, bug fixes, added features, and DCRs.
author: JonDouglas
ms.author: jodou
ms.date: 11/11/2016
ms.topic: release-notes
---

# NuGet 3.0 RC Release Notes

[NuGet 3.0 Beta Release Notes](../release-notes/nuget-3.0-beta.md) | [NuGet 3.0 RC2 Release Notes](../release-notes/nuget-3.0-RC2.md)

NuGet 3.0 RC was released on April 29, 2015 with the Visual Studio 2015 RC release. This release has a number of important bug fixes, performance improvements and updates to support the new frameworks.  It is only available for Visual Studio 2015.

### Continued Focus on Performance

Stability and performance of NuGet queries continue to be a hot topic that we are focusing on.  With this release, you should start to see very quick search operations in the NuGet UI and website.  We're monitoring the service and how you use the service so that we can continue to tune these operations.

## Significant Issues Resolved

In order to stabilize the NuGet clients, we resolved many issues as part of this release.  Here is just a brief list of some of the more important issues resolved:

* As part of the rename of the K framework for ASP.NET 5, framework monikers have been updated to handle dnx and dnxcore [link](https://github.com/NuGet/Home/issues/215)
* Added help documentation from links in the Visual Studio UI [link](https://github.com/NuGet/Home/issues/232)
* Better handling of complex references in `.nuspec` with comma-delimited framework references [link](https://github.com/NuGet/Home/issues/276)
* Fixed support for Japanese cultures [link](https://github.com/NuGet/Home/issues/253)
* Updated client to allow ASP.NET 5 projects to use new v3 endpoints [link](https://github.com/NuGet/Home/issues/219)
* Updated to better handle packages folder with source control [link](https://github.com/NuGet/Home/issues/56)
* Fixed support for satellite packages [link](https://github.com/NuGet/Home/issues/17)
* Corrected support for framework-specific content files [link](https://github.com/NuGet/Home/issues/18)

## GitHub presence overhaul

We've made some changes to our [source code repositories on GitHub](https://github.com/nuget/home).  If you have any issues with the NuGet Visual Studio client, the Powershell commands, or the command-line executable you can log those issues and monitor their progress on our [GitHub Home repository issues list](https://github.com/nuget/home/issues).  We are tracking issues for the gallery in our [GitHub NuGetGallery repository](https://github.com/nuget/NuGetGallery/issues).


## Stay Tuned

Please keep an eye on [our blog](https://blog.nuget.org) for more progress and announcements for NuGet 3.0!
