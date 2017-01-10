---
# required metadata

title: NuGet 2.8.1 Release Notes | Microsoft Docs
author: karann
ms.author: karann
manager: ghogen
ms.date: 11/11/2016
ms.topic: article
ms.prod: nuget
#ms.service:
ms.technology: nuget
ms.assetid: eebbbae4-fc6a-4c05-9102-4ec66b4c64a4

# optional metadata

#description: release notes 2.8.1
#keywords: release notes 2.8.1
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
# NuGet 2.8.1 Release Notes

[NuGet 2.8 Release Notes](../release-notes/nuget-2.8.md) | [NuGet 2.8.2 Release Notes](../release-notes/nuget-2.8.2.md)

NuGet 2.8.1 was released on April 2, 2014.

## Notable features in the release

### Support for Windows Phone 8.1 Projects
This release now supports the following new target framework monikers which can be used to target Windows Phone 8.1 projects:

* WindowsPhone81 / WP81 (for Silverlight-based Windows Phone projects)
* WindowsPhoneApp81 / WPA81 (for WinRT-based Windows Phone App projects)

### Update of the NuGet WebMatrix Extension
This release updates the NuGet client found in WebMatrix to [NuGet.Core](https://www.nuget.org/packages/Nuget.Core/2.6.1) 2.6.1 and brings with it features such as XDT transformations. More importantly, the 2.6.1 core update enables the WebMatrix client to install NuGet packages which contain more recent versions of the nuspec schema, which includes the ASP.NET NuGet packages.

For more information about the WebMatrix Extension update, see those specific [release notes](../release-notes/nuget-2.6.1-for-WebMatrix.md).

### Bug Fixes
In addition to these features, this release of NuGet includes other bug fixes. There were 16 total issues addressed in the release. For a full list of the work items fixed in NuGet 2.8.1, please view the [NuGet Issue Tracker for this release](https://nuget.codeplex.com/workitem/list/advanced?keyword=&status=All&type=All&priority=All&release=NuGet%202.8.1&assignedTo=All&component=All&sortField=LastUpdatedDate&sortDirection=Descending&page=0&reasonClosed=All).

### Reshipping with Visual Studio "14" CTP
In [Visual Studio "14" CTP](http://www.visualstudio.com/en-us/downloads/visual-studio-14-ctp-vs) released on June 3rd 2014, NuGet 2.8.1 is shipped in the box. The features it support remain in-par with other 2.8.1 VSIXes such as the one for Visual Studio 2013.
