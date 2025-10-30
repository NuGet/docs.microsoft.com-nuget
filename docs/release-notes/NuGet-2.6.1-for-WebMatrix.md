---
title: NuGet 2.6.1 for WebMatrix Release Notes
description: Release notes for NuGet 2.6.1 for WebMatrix including known issues, bug fixes, added features, and DCRs.
author: JonDouglas
ms.author: jodou
ms.date: 11/11/2016
ms.topic: release-notes
---

# NuGet 2.6.1 for WebMatrix Release Notes

[NuGet 2.6 Release Notes](../release-notes/nuget-2.6.md) | [NuGet 2.7 Release Notes](../release-notes/nuget-2.7.md)

The NuGet team released an updated NuGet Package Manager extension for WebMatrix on March 26, 2014.  This update can be installed from the [WebMatrix Extension Gallery](https://blogs.iis.net/webmatrix/retiring-the-webmatrix-extensions-gallery) using the following steps:

1. Open WebMatrix 3
1. Click the Extensions icon in the Home ribbon
1. Select the Updates tab
1. Click to update NuGet Package Manager to 2.6.1
1. Close and restart WebMatrix 3

## Notable Changes

This extension update addresses two of the biggest issues users have faced consuming NuGet packages within WebMatrix.  The first was a NuGet schema version error and the second was a bug leading to zero-byte DLLs in the `bin` folder.

### NuGet Schema Version Error

Since WebMatrix 3 was released, new features have been introduced into NuGet that require a new schema version for the NuGet packages.  When trying to manage your NuGet packages in your web site, these new packages can lead to errors that you see in WebMatrix.

![An error occurred. The schema version is incompatible. Please upgrade NuGet to the latest version.](./media/NuGet-2.8/webmatrix-schema-version.png)

This latest release provides compatibility with the newest NuGet packages, preventing this error from occurring. New versions of packages including Microsoft.AspNet.WebPages can now be installed in WebMatrix.  Some of these packages were using NuGet features such as [XDT config transforms](../release-notes/nuget-2.6.md#xdt), which wasn't supported in WebMatrix until now.

### Zero-Byte DLLs in bin Folder

Some users have reported that after installing NuGet packages in WebMatrix that include DLLs that get copied to bin, that the DLLs show up in the `bin` folder as 0-byte files.  This breaks the application at runtime.

```[This issue](https://nuget.codeplex.com/workitem/4060)``` has now been fixed.

## Other Recent Improvements

When NuGet Package Manager 2.8 was released for Visual Studio, we also released NuGet Package Manager 2.5.0 for WebMatrix.  While this was mentioned in the [NuGet 2.8 Release Notes](../release-notes/nuget-2.8.md#webmatrix-nuget-client-updates), we didn't mention the specific new features that update introduced.

### Update All

You can now update all of your web site's packages in one step!  When you open the NuGet extension in WebMatrix, you see the list of all packages on the gallery, those installed, and the ones with updates available.  Previously, every package would have to be updated individually but now there is a useful "Update All" button that shows up on the Updates tab.

![Click Update All to update all packages with available updates](./media/NuGet-2.8/webmatrix-update-all.png)

### Overwrite Existing Files

When installing packages that contain files that already exist in your web site, NuGet has always just silently ignored those files (leaving your existing files alone).  This could lead to the impression that a package was installed or updated correctly when in fact it wasn't.  NuGet will now prompt for files to be overwritten.

![File Conflict Resolution](./media/NuGet-2.8/webmatrix-overwrite-file.png)
