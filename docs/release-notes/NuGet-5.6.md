---
title: NuGet 5.6 Release Notes
description: Release notes for NuGet 5.6 including new features, bug fixes, and DCRs.
author: nkolev92
ms.author: nikolev
ms.date: 05/19/2020
ms.topic: conceptual
---

# NuGet 5.6 Release Notes

NuGet distribution vehicles:

| NuGet version | Available in Visual Studio version| Available in .NET SDK(s)|
|:---|:---|:---|
| [**5.6.0**](https://nuget.org/downloads) | [Visual Studio 2019 version 16.6](https://visualstudio.microsoft.com/downloads/) | [3.1.300](https://dotnet.microsoft.com/download/dotnet-core/3.1)<sup>1</sup> |

<sup>1</sup>Installed with Visual Studio 2019 with .NET Core workload

## Summary: What's New in 5.6

* Support prerelease packages with floating versions. `Version="*-*"`, `Version="1.*-*"`, and similar float to latest versions, including prerelease versions, within specified range  - [#912](https://github.com/NuGet/Home/issues/912)

### Issues fixed in this release

**Bugs:**

* `nuget push *.nupkg` fails when snupkg does not exist - [#8148](https://github.com/NuGet/Home/issues/8148)

* Pack, and several other code paths, fail dependent on locale. Use RegexOptions.CultureInvariant - [#8246](https://github.com/NuGet/Home/issues/8246)

* Perf: DG Spec for unloaded project scenarios should not be written in preview restores - [#8793](https://github.com/NuGet/Home/issues/8793)

* Restore: Improve performance by caching solution dependency graph spec - [#9201](https://github.com/NuGet/Home/issues/9201)

* PM UI doesn't work for SDK style projects after installing a package with PM Console - [#9203](https://github.com/NuGet/Home/issues/9203)

* Embedded icon can’t be shown in PM UI with local package feed - depending on / vs \ - [#9225](https://github.com/NuGet/Home/issues/9225)

* NuGetVersion.TryParseStrict() should return false if it fails to parse - [#9255](https://github.com/NuGet/Home/issues/9255)

* `nuget.exe push` help for `-source`, should suggest usage of source name, not source URL - [#9265](https://github.com/NuGet/Home/issues/9265)

* `dotnet nuget add package SourceUri`  creates bad default package source name - [#9277](https://github.com/NuGet/Home/issues/9277)

* Screen reader doesn't announce "Searching..." message when switching tabs - [#9307](https://github.com/NuGet/Home/issues/9307)

* Accessibility: Focus-rectangle color is not accessible PM UI tabs in dark theme - [#9336](https://github.com/NuGet/Home/issues/9336)

* nuget.exe 5.5 fails to restore with MSBuild 14 or below - [#9458](https://github.com/NuGet/Home/issues/9458)

* Don't log millisecond times in restore messages - [#8977](https://github.com/NuGet/Home/issues/8977)

* Make IOutputConsole async - [#9268](https://github.com/NuGet/Home/issues/9268)

* MSBuild version picking works poorly on some non-english cultures - [#9322](https://github.com/NuGet/Home/issues/9322)

* Missing format default on `dotnet nuget list source` - [#9337](https://github.com/NuGet/Home/issues/9337)

* Perf: RestoreOperationLogger has unnecessary thread affinity - [#9288](https://github.com/NuGet/Home/issues/9288)

* Automated creation of docs for `dotnet nuget` commands - [#9146](https://github.com/NuGet/Home/issues/9146)

* Default verbosity should not report each project's noop restore - [#8792](https://github.com/NuGet/Home/issues/8792)

* Support `-DependencyVersion` parameter for `NuGet.exe update`, similar to install command - [#7694](https://github.com/NuGet/Home/issues/7694)


**DCRs:**

* Add initial support for net5.0 target framework - [#9584](https://github.com/NuGet/Home/issues/9584)

* Sort packages by ID in the Updates tab of the PM UI - [#9278](https://github.com/NuGet/Home/issues/9278)


**[List of all issues fixed in this release - 5.6](https://app.zenhub.com/workspaces/nuget-client-team-55aec9a240305cf007585881/reports/release?release=5e3b2080c4b30708e48bf9f3)**
