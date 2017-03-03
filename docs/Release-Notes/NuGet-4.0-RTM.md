---
# required metadata

title: NuGet 4.0 RC Release Notes | Microsoft Docs
author: anangaur-msft
ms.author: anangaur
manager: ghogen
ms.date: 11/17/2016
ms.topic: article
ms.prod: nuget
#ms.service:
ms.technology: nuget
ms.assetid: 906cc4dd-7948-4e86-a093-21df830ce8c3

# optional metadata

description: Release notes for NuGet 4.0 RTM including known issues, bug fixes, added features, and DCRs.
keywords: NuGet 4.0 RTM release notes, bug fixes, known issues, added features, DCRs
#ROBOTS:
#audience:
#ms.devlang:
ms.reviewer:
- kraigb
#ms.suite:
#ms.tgt_pltfrm:
#ms.custom:

---

# 4.0 RTM Release Notes

## Known issues


[NuGet 4.0 RC Release Notes](../release-notes/nuget-4.0-RC.md)

[NuGet 4.0 RTM for Visual Studio 2017]() TBD is focused on adding support for .NET Core scenarios, addressing key customer feedback and improving performance in a variety of scenarios. This release brings several improvements like support for PackageReference, NuGet commands as MSBuild targets, background package restore, and more.

[Issues List](https://github.com/NuGet/Home/issues?q=is%3Aissue+is%3Aclosed+milestone%3A%224.0 RTM")

**Bug:**

* Dependency name of project reference not taken from PackageId - [#4642](https://github.com/NuGet/Home/issues/4642)

* [Test Failure]Fail to create UWP project on Blend - [#4625](https://github.com/NuGet/Home/issues/4625)

* Entity Framework tools -- "No executable found matching command 'dotnet-ef'" - [#4600](https://github.com/NuGet/Home/issues/4600)

* NuGet restore in Visual Studio does not respect PackageId property of projects - [#4586](https://github.com/NuGet/Home/issues/4586)

* NuGet ProjectSystemCache error when adding package in vsix package - [#4545](https://github.com/NuGet/Home/issues/4545)

* LightweightSolutionLoad - gives file not found during restore - [#4544](https://github.com/NuGet/Home/issues/4544)

* Pack throws exception if IncludeSource is used in a project with multiple TFMs - [#4536](https://github.com/NuGet/Home/issues/4536)

* Build solution with ASP.NET Core web app, build does not start - [#4486](https://github.com/NuGet/Home/issues/4486)

* VS 2017 RC3 crashes on using update from Solution-wide package management - [#4474](https://github.com/NuGet/Home/issues/4474)

* Cannot uninstall newly installed package  - [#4435](https://github.com/NuGet/Home/issues/4435)

* When migrating to PackageRef, hybrid solutions have strange restore behavior - [#4433](https://github.com/NuGet/Home/issues/4433)

* Building soon after starting NuGet operation (install, update, restore), can cause VS to Hang - [#4420](https://github.com/NuGet/Home/issues/4420)

* [Watson] apphangb1: APPLICATION_HANG_BusyHang_cfffffff_Microsoft.VisualStudio.Shell.15.0.dll!Microsoft.VisualStudio.Shell.VsTaskLibraryHelper+__c__DisplayClass24_0_1+__AsVsTask_b__1_d[[System.__Canon,_mscorlib]].MoveNext - [#4371](https://github.com/NuGet/Home/issues/4371)

* add package command should add version as attribute instead of element - [#4325](https://github.com/NuGet/Home/issues/4325)

* Dotnet Restore foo.sln -- fails when configurations in SLN cause duplicate (but diff config) projects in restore graph - [#4316](https://github.com/NuGet/Home/issues/4316)

* PMC Script Compat? Create website use server HTTP or FTP, Show error in output window - [#4224](https://github.com/NuGet/Home/issues/4224)

* Content only packages - [#3668](https://github.com/NuGet/Home/issues/3668)

**DCR:**

* migrate vsix from v2 vsix to v3 vsix - [#4196](https://github.com/NuGet/Home/issues/4196)

* NuGet should have a mechanism for getting the path to the lock file in MSBuild - [#3351](https://github.com/NuGet/Home/issues/3351)

* Add build assets to the TFM compatibility check and assets file - [#3296](https://github.com/NuGet/Home/issues/3296)

**Feature:**

* Localize strings in NuGet.Core.sln - [#2041](https://github.com/NuGet/Home/issues/2041)



