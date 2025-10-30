---
title: NuGet 2.8.6 Release Notes
description: Release notes for NuGet 2.8.6 including known issues, bug fixes, added features, and DCRs.
author: JonDouglas
ms.author: jodou
ms.date: 11/11/2016
ms.topic: release-notes
---

# NuGet 2.8.6 Release Notes

[NuGet 2.8.5 Release Notes](../release-notes/nuget-2.8.5.md) | [NuGet 2.8.7 Release Notes](../release-notes/nuget-2.8.7.md)

NuGet 2.8.6 was released July 20, 2015 as a minor update to our 2.8.5 VSIX with some targeted fixes and improvements to support packages that may be delivered with support for the Windows 10 UWP development model.

This version of the NuGet package manager extension provides support for Visual Studio 2013 only.

In this release, the NuGet Package Manager dialog had support added for:

* Introduced the UAP Target Framework Moniker to support Windows 10 Application Development.
* NuGet protocol version 3 endpoints
* Support for [Nuget.Config](../consume-packages/configuring-nuget-behavior.md) protocolVersion attribute on repository sources. Default value is "2"
* Falling back to remote repository if a required package version is not available in the local cache
