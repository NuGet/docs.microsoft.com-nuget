---
title: NuGet 2.8.2 Release Notes
description: Release notes for NuGet 2.8.2 including known issues, bug fixes, added features, and DCRs.
author: JonDouglas
ms.author: jodou
ms.date: 11/11/2016
ms.topic: release-notes
---

# NuGet 2.8.2 Release Notes

[NuGet 2.8.1 Release Notes](../release-notes/nuget-2.8.1.md) | [NuGet 2.8.3 Release Notes](../release-notes/nuget-2.8.3.md)

NuGet 2.8.2 was released on May 22, 2014.  This release only included changes to the nuget.exe command-line, the NuGet.Server package and other NuGet packages.  The release did not include an updated Visual Studio extension or WebMatrix extension.

## Notable Updates

The most notable updates were in the nuget.exe command-line and the NuGet.Server package (for self-hosted NuGet feeds).

### Important nuget.exe Bug Fixes

1. ```[nuget.exe Push fails and keeps retrying](https://nuget.codeplex.com/workitem/4000)```
1. ```[nuget.exe Push does not send Basic Auth credentials correctly](https://nuget.codeplex.com/workitem/4109)```
1. ```[nuget.exe Push won't follow temporary redirect](https://nuget.codeplex.com/workitem/4050)```

### Important NuGet.Server Bug Fix

1. ```[Wrong value of IsAbsoluteLatestVersion returned by NuGet.Server](https://nuget.codeplex.com/workitem/4147)```

## Packages Updated

The nuget.exe command-line and NuGet.Server fixes are shipped as NuGet package updates.  There were other packages updated with 2.8.2 as well.

Here's the list of updated packages:

1. [NuGet.Core](https://www.nuget.org/packages/NuGet.Core/)
1. [NuGet.CommandLine](https://www.nuget.org/packages/NuGet.CommandLine/)
1. [NuGet.Server](https://www.nuget.org/packages/NuGet.Server/)
1. [NuGet.Build](https://www.nuget.org/packages/NuGet.Build/)
1. [NuGet.VisualStudio](https://www.nuget.org/packages/NuGet.VisualStudio/) (the package, not the extension)

## All Changes
There were 10 issues addressed in the release. For a full list of the work items fixed in NuGet 2.8.2, please view the ```[NuGet Issue Tracker for this release](https://nuget.codeplex.com/workitem/list/advanced?keyword=&status=All&type=All&priority=All&release=NuGet%202.8.2&assignedTo=All&component=All&sortField=LastUpdatedDate&sortDirection=Descending&page=0&reasonClosed=All)```.
