--- 
# required metadata 
 
title: "NuGet 2.8.6 Release Notes | Microsoft Docs" 
author: harikmenon
ms.author: harikm 
manager: ghogen 
ms.date: 11/11/2016 
ms.topic: article 
ms.prod: nuget 
#ms.service: 
ms.technology: nuget 
ms.assetid: 920c551c-18a7-40f4-a32b-ce84de6ea766 
 
# optional metadata 
 
#description: release notes 2.8.6
#keywords: release notes 2.8.6
#ROBOTS: 
#audience: 
#ms.devlang: 
ms.reviewer:  
- karann 
- harikm 
#ms.suite:  
#ms.tgt_pltfrm: 
#ms.custom: 
 
---
# NuGet 2.8.6 Release Notes

[NuGet 2.8.5 Release Notes](../nuget/release-notes/nuget-2.8.5.md) | [NuGet 2.8.7 Release Notes](../nuget/release-notes/nuget-2.8.7.md)

NuGet 2.8.6 was released July 20, 2015 as a minor update to our 2.8.5 VSIX with some targeted fixes and improvements to support packages that may be delivered with support for the Windows 10 UWP development model.

This version of the NuGet package manager extension provides support for Visual Studio 2013 only.

In this release, the NuGet Package Manager dialog had support added for:

* Introduced the UAP Target Framework Moniker to support Windows 10 Application Development.
* NuGet protocol version 3 endpoints
* Support for [nuget.config](http://docs.nuget.org/consume/NuGet-Config-Settings) protocolVersion attribute on repository sources.  Default value is "2"
* Falling back to remote repository if a required package version is not available in the local cache 

  
