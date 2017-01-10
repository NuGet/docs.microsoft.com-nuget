---
# required metadata

title: NuGet 2.9-RC Release Notes | Microsoft Docs
author: karann
ms.author: karann
manager: ghogen
ms.date: 11/11/2016
ms.topic: article
ms.prod: nuget
#ms.service:
ms.technology: nuget
ms.assetid: 04d76a22-63b0-41d1-9c27-799f4b35058f

# optional metadata

#description: release notes 2.9
#keywords: release notes 2.9
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
# NuGet 2.9-RC Release Notes

[NuGet 2.8.7 Release Notes](../release-notes/nuget-2.8.7.md) | [NuGet 3.0 Preview Release Notes](../release-notes/nuget-3.0-preview.md)

NuGet 2.9 was released September 10, 2015 as an update to the 2.8.7 VSIX for Visual Studio 2012 and 2013.

### Updates in this release

* Now skipping processing packages if their contained *.nuspec document is malformed - [PR8](https://github.com/NuGet/NuGet2/pull/8)
* Corrected multipartwebrequest handling of \r\n for Unix/Linux scenarios - [776](https://github.com/NuGet/Home/issues/776)
* Corrected integration with build events in Visual Studio 2013 Community edition - [1180](https://github.com/NuGet/Home/issues/1180)


The complete list of fixes in this release can be found on GitHub in the [2.8.8 milestone](https://github.com/NuGet/Home/issues?q=milestone%3A2.8.8+is%3Aclosed)

Download the extension for:

* [Visual Studio 2012](https://dist.nuget.org/visualstudio-2012-vsix/v2.9-rc/NuGet.Tools.vsix)
* [Visual Studio 2013](https://dist.nuget.org/visualstudio-2013-vsix/v2.9-rc/NuGet.Tools.vsix)