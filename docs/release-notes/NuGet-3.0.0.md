---
title: NuGet 3.0 Release Notes
description: Release notes for NuGet 3.0.0 including known issues, bug fixes, added features, and DCRs.
author: JonDouglas
ms.author: jodou
ms.date: 11/11/2016
ms.topic: conceptual
---

# NuGet 3.0 Release Notes

[NuGet 3.0 RC2 Release Notes](../release-notes/nuget-3.0-RC2.md) | [NuGet 3.1 Release Notes](../release-notes/nuget-3.1.md)

NuGet 3.0 was released on July 20, 2015 as a bundle extension to Visual Studio 2015. We pushed to deliver this release with Visual Studio so that the complete updated NuGet 3.0 experience would be available for new Visual Studio users. This NuGet extension version is only available for Visual Studio 2015.

We recommend those developers that have access to the Visual Studio gallery update to the latest version that is available, as we are publishing an update shortly after the release of Visual Studio 2015 that contains support for Windows 10 development.

In total, we closed 240 issues in the 3.0 release, and you can review the [complete list of issues on GitHub](https://github.com/NuGet/Home/issues?q=milestone%3A3.0.0-RTM+is%3Aclosed).

## Known Issues

There were a number of known issues delivered with this release, and all of these items are fixed in our scheduled 3.1 release to coincide with the release of Windows 10 on July 29th.  You are able to update your Visual Studio extension from the gallery on or after that date to fix these known issues.

*  Translation is not provided for the "Do not show this again" label on the preview window and the "Authors" label in the package description window.
*  When you working on a project by using TFS source control, NuGet cannot present the package manager user interface if the Nuget.Config file is marked as read-only.
   * **Workaround** Check out the file from TFS.
*  Text in the yellow "restart bar" in the NuGet Powershell window is not visible when you use the Visual Studio dark theme.
   * **Workaround** Use the Visual Studio light theme.


## Summary of top issues resolved

* [Frequent network update calls when package manager window refreshes](https://github.com/NuGet/Home/issues/515)
* [Delayed scroll when changing to installed view in package manager](https://github.com/NuGet/Home/issues/519)
* [Network calls should be run on a background thread](https://github.com/NuGet/Home/issues/516)
* [Added 'Do not show preview window' checkbox](https://github.com/NuGet/Home/issues/566)
* [Added process throttling to reduce processor usage](https://github.com/NuGet/Home/issues/356)
* Improved portable-class-library reference handling
    * [https://github.com/NuGet/Home/issues/562](https://github.com/NuGet/Home/issues/562)
    * [https://github.com/NuGet/Home/issues/454](https://github.com/NuGet/Home/issues/454)
    * [https://github.com/NuGet/Home/issues/440](https://github.com/NuGet/Home/issues/440)
* [Autocomplete service was case sensitive](https://github.com/NuGet/Home/issues/198)
* [Update to reintroduce basic auth credentials](https://github.com/NuGet/Home/issues/456)
* [Improved error logging](https://github.com/NuGet/Home/issues/407)
* [Improved powershell error messages when calling Update-Package](https://github.com/NuGet/Home/issues/5)
* [Fixed the 'Learn about Options' link to prevent crashing on Windows 10](https://github.com/NuGet/Home/issues/822)
* [Remember pre-release checkbox setting](https://github.com/NuGet/Home/issues/732)
* [Improved gather performance by caching results across projects in a solution](https://github.com/NuGet/Home/issues/721)
* [Multiple Packages can be gathered in parallel](https://github.com/NuGet/Home/issues/713)
* [Removed install-package -force command](https://github.com/NuGet/Home/issues/697)

Please keep an eye on [our blog](https://blog.nuget.org) for more progress and announcements as we get ready to deliver support for Windows 10 development.