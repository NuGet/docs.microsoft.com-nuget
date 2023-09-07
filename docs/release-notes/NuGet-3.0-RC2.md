---
title: NuGet 3.0 RC2 Release Notes
description: Release notes for NuGet 3.0 RC2 including known issues, bug fixes, added features, and DCRs.
author: JonDouglas
ms.author: jodou
ms.date: 11/11/2016
ms.topic: conceptual
---

# NuGet 3.0 RC2 Release Notes

[NuGet 3.0 RC Release Notes](../release-notes/nuget-3.0-RC.md) | [NuGet 3.0 Release Notes](../release-notes/nuget-3.0.0.md)

NuGet 3.0 RC2 was released on June 3, 2015 as an interim release available from the Visual Studio 2015 Extension Gallery and [Codeplex](https://blog.nuget.org/releases/view/615507). This release has a number of important bug fixes and performance improvements that we felt were important to release before the completed Visual Studio 2015 release. This NuGet extension version is only available for Visual Studio 2015.

In total, we closed 158 issues in this release, and you can review the [complete list of issues on GitHub](https://github.com/NuGet/Home/issues?utf8=%E2%9C%93&q=is%3Aclosed+milestone%3A3.0.0-RTM+sort%3Aupdated-asc+updated%3A%3C%3D2015-06-01).

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

Download this [update to the NuGet extension](https://blog.nuget.org/releases/view/615507) from Codeplex and please keep an eye on [our blog](https://blog.nuget.org) for more progress and announcements for NuGet 3.0!