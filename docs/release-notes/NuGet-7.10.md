---
title: NuGet 7.10 Release Notes
description: Release notes for NuGet 7.10 including new features, bug fixes, and DCRs.
author: nkolev92
ms.date: 09/09/2026
ms.topic: release-notes
---

# NuGet 7.10 Release Notes

NuGet distribution vehicles:

| NuGet version | Available in Visual Studio version | Available in .NET SDK(s) |
|:---|:---|:---|
| [**7.10.0**](https://nuget.org/downloads) | [Visual Studio 2026 version 18.10.0](https://visualstudio.microsoft.com/downloads/) | N/A |

> [!NOTE]
> Visual Studio-only release.
> NuGet SDK packages are published quarterly with .NET.

## Summary: What's New in 7.10.0

### Issues fixed in this release

* NuGet is restoring packages from feeds that are not configured - [#14974](https://github.com/NuGet/Home/issues/14974)

* Don&#39;t cause VS tabs to load when opening PM UI - [#14995](https://github.com/NuGet/Home/issues/14995)

* Properly annotate NuGet.Client in-proc MSBuild call sites for trimming (IL2026) - [#14987](https://github.com/NuGet/Home/issues/14987)

* Remove unused Product Update InfoBar &amp; services in PMC - [#14981](https://github.com/NuGet/Home/issues/14981)

* Show an error if Package Manager UI fails to load in VS - [#14977](https://github.com/NuGet/Home/issues/14977)

* `dotnet restore`&#160; reads .sha512/.nupkg.metadata for ALL cached versions of a package during version resolution - [#14963](https://github.com/NuGet/Home/issues/14963)

[List of commits in this release](https://github.com/NuGet/NuGet.Client/compare/7.9.0.83...7.10.0.40)

### Community contributions

Thank you to all the contributors who helped make this NuGet release awesome!

* [OvesN](https://github.com/NuGet/NuGet.Client/pull/7543)
  * [7543](https://github.com/NuGet/NuGet.Client/pull/7543) Migrate GetRestoreSettingsTask to the multithreadable task model
  * [7533](https://github.com/NuGet/NuGet.Client/pull/7533) Migrate GetRestoreSolutionProjectsTask to the multithreadable model
  * [7538](https://github.com/NuGet/NuGet.Client/pull/7538) Only mark GetRestoreProjectReferencesTask as multithreadable on .NET
* [JanProvaznik](https://github.com/NuGet/NuGet.Client/pull/7551)
  * [7551](https://github.com/NuGet/NuGet.Client/pull/7551) Refresh NuGet static state in a dedicated task before restore tasks run and allow RestoreTask to be multithreadable
  * [7507](https://github.com/NuGet/NuGet.Client/pull/7507) Make RestoreTask resettable across builds (NuGet/Home#14958 item 1)
* [mospake](https://github.com/NuGet/NuGet.Client/pull/7589)
  * [7589](https://github.com/NuGet/NuGet.Client/pull/7589) Migrate OptProf to CloudTest
* [baronfel](https://github.com/NuGet/NuGet.Client/pull/7541)
  * [7541](https://github.com/NuGet/NuGet.Client/pull/7541) Pack: reuse existing evaluations instead of forcing BuildProjectReferences=false
