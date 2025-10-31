---
title: NuGet 2.9-RC Release Notes
description: Release notes for NuGet 2.9 RC including known issues, bug fixes, added features, and DCRs.
author: JonDouglas
ms.author: jodou
ms.date: 11/11/2016
ms.topic: release-notes
---

# NuGet 2.9-RC Release Notes

[NuGet 2.8.7 Release Notes](../release-notes/nuget-2.8.7.md) | [NuGet 3.0 Preview Release Notes](../release-notes/nuget-3.0-preview.md)

NuGet 2.9 was released September 10, 2015 as an update to the 2.8.7 VSIX for Visual Studio 2012 and 2013.

### Updates in this release

* Now skipping processing packages if their contained `.nuspec` document is malformed - [PR8](https://github.com/NuGet/NuGet2/pull/8)
* Corrected multipartwebrequest handling of \r\n for Unix/Linux scenarios - [776](https://github.com/NuGet/Home/issues/776)
* Corrected integration with build events in Visual Studio 2013 Community edition - [1180](https://github.com/NuGet/Home/issues/1180)


The complete list of fixes in this release can be found on GitHub in the [2.8.8 milestone](https://github.com/NuGet/Home/issues?q=milestone%3A2.8.8+is%3Aclosed)
