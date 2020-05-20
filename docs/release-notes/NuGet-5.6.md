---
title: NuGet 5.6 Release Notes
description: Release notes for NuGet 5.6 including new features, bug fixes, and DCRs.
author: chgill-msft
ms.author: chgill
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

* Support pre-release packages with floating versions. Version="*-*" and similar. - [#912](https://github.com/NuGet/Home/issues/912)

### Issues fixed in this release

**Bug:**

* `dotnet nuget push` - Missing value for option - [#4864](https://github.com/NuGet/Home/issues/4864)

* `nuget push *.nupkg` fails wheh snupkg does not exist - [#8148](https://github.com/NuGet/Home/issues/8148)

* Pack, and several other code paths, fails dependant on locale. Use RegexOptions.CultureInvariant - [#8246](https://github.com/NuGet/Home/issues/8246)

* Perf: DG Spec for unloaded project scenarios should not be written in preview restores - [#8793](https://github.com/NuGet/Home/issues/8793)

* Restore:  improve performance by caching solution dependency graph spec - [#9201](https://github.com/NuGet/Home/issues/9201)

* PM UI doesn't work for sdk style projects after installing a package with PMConsole - [#9203](https://github.com/NuGet/Home/issues/9203)

* [Test Failure] Embedded icon can’t be shown in PM UI with local package feed - depending on / vs \ - [#9225](https://github.com/NuGet/Home/issues/9225)

* NuGetVersion.TryParseStrict() should return false if it fails to parse - [#9255](https://github.com/NuGet/Home/issues/9255)

* `nuget.exe push` help for `-source`, should be source name, not source uri - [#9265](https://github.com/NuGet/Home/issues/9265)

* `dotnet nuget add package SourceUri`  creates bad default package source name - [#9277](https://github.com/NuGet/Home/issues/9277)

* Screen reader doesn't announces "Searching..." message when switching tabs - [#9307](https://github.com/NuGet/Home/issues/9307)

* [Test Failure] Focus-rect color in themes for PM UI tabs - [#9336](https://github.com/NuGet/Home/issues/9336)

* Several accessibility bugs reported by users - [#9393](https://github.com/NuGet/Home/issues/9393)

* nuget.exe 5.5 fails to restore with MSBuild 14 or below - [#9458](https://github.com/NuGet/Home/issues/9458)

* Don't bother logging small millisecond times in restore messages - [#8977](https://github.com/NuGet/Home/issues/8977)

* Make IOutputConsole async - [#9268](https://github.com/NuGet/Home/issues/9268)

* MsBuild version picking works badly on some non-english cultures - [#9322](https://github.com/NuGet/Home/issues/9322)

* Missing format default on `dotnet nuget list source` - [#9337](https://github.com/NuGet/Home/issues/9337)

* Perf: RestoreOperationLogger has unnecessary thread affinity - [#9288](https://github.com/NuGet/Home/issues/9288)

* Automated creation of docs for `dotnet nuget` commands. - [#9146](https://github.com/NuGet/Home/issues/9146)

* Default verbosity should not report each project's noop restore. - [#8792](https://github.com/NuGet/Home/issues/8792)

* add support to `NuGet.exe update` for -DependencyVersion parameter, like install command - [#7694](https://github.com/NuGet/Home/issues/7694)


**DCR:**

* net5.0 tfm - initial support - [#9584](https://github.com/NuGet/Home/issues/9584)

* Sort packages by ID in the Updates tab of the Package Manager UI - [#9278](https://github.com/NuGet/Home/issues/9278)


**[List of all issues fixed in this release - 5.6](https://app.zenhub.com/workspaces/nuget-client-team-55aec9a240305cf007585881/reports/release?release=5e3b2080c4b30708e48bf9f3)**