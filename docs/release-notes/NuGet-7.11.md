---
title: NuGet 7.11 Release Notes
description: Release notes for NuGet 7.11 including new features, bug fixes, and DCRs.
author: nkolev92
ms.date: 10/16/2026
ms.topic: release-notes
ai-usage: ai-generated
---

# NuGet 7.11 Release Notes

NuGet distribution vehicles:

| NuGet version | Available in Visual Studio version | Available in .NET SDK(s) |
|:---|:---|:---|
| [**7.11.0**](https://nuget.org/downloads) | [Visual Studio 2026 version 18.11.0](https://visualstudio.microsoft.com/downloads/) | N/A |

> [!NOTE]
> Visual Studio-only release.
> NuGet SDK packages are published quarterly with .NET.

## Summary: What's new in 7.11.0

* Start fixing vulnerable packages with **Fix with GitHub Copilot** from the Package Manager UI's vulnerability notification ([NuGet/Home#15080](https://github.com/NuGet/Home/issues/15080)).

* Review package source mappings with GitHub Copilot from Visual Studio's Package Source Mapping settings ([NuGet/Home#15021](https://github.com/NuGet/Home/issues/15021)).

### Issues fixed in this release

* NuGet commands no longer throw when Visual Studio has no active project ([NuGet/Home#15059](https://github.com/NuGet/Home/issues/15059)).

* Restore no longer closes plugin log writers while they are still in use when plugin logging is enabled ([NuGet/Home#15046](https://github.com/NuGet/Home/issues/15046)).

* Restore preserves its shared concurrency throttle, preventing `SemaphoreFullException` when a concurrency limit is active ([NuGet/Home#15045](https://github.com/NuGet/Home/issues/15045)).

* Credential-provider plugins remain available across restores in the same MSBuild build, avoiding null-reference errors and authentication failures caused by premature teardown ([NuGet/Home#15044](https://github.com/NuGet/Home/issues/15044)).

* `NuGet.Configuration` can read and write package update cooldown settings ([NuGet/Home#15026](https://github.com/NuGet/Home/issues/15026)).
  This adds configuration APIs, not cooldown enforcement.

* Backspace in the Package Manager Console removes complete supplementary Unicode characters without corrupting the input ([NuGet/Home#15025](https://github.com/NuGet/Home/issues/15025)).

[List of commits in this release](https://github.com/NuGet/NuGet.Client/compare/7.10.0.40...7.11.0.24)

### Community contributions

Thank you to all the contributors who helped make this NuGet release awesome!

* [gekka](https://github.com/gekka) fixed culture-dependent tests ([NuGet/NuGet.Client#7580](https://github.com/NuGet/NuGet.Client/pull/7580)) and repaired `runTests.ps1` ([NuGet/NuGet.Client#7579](https://github.com/NuGet/NuGet.Client/pull/7579)).
* [Youssef1313](https://github.com/Youssef1313) fixed package-version fallback ([NuGet/NuGet.Client#7619](https://github.com/NuGet/NuGet.Client/pull/7619)) and property substitution when calculating package output paths ([NuGet/NuGet.Client#7612](https://github.com/NuGet/NuGet.Client/pull/7612)).
* [bribrothers](https://github.com/bribrothers) preserved supplementary Unicode characters in the Package Manager Console ([NuGet/NuGet.Client#7621](https://github.com/NuGet/NuGet.Client/pull/7621)).
* [nijulia](https://github.com/nijulia) updated the DartLab test pipeline template ([NuGet/NuGet.Client#7637](https://github.com/NuGet/NuGet.Client/pull/7637)).
* [JanProvaznik](https://github.com/JanProvaznik) made NuGet's shared-state cleanup build-scoped ([NuGet/NuGet.Client#7630](https://github.com/NuGet/NuGet.Client/pull/7630)).
* [baronfel](https://github.com/baronfel) restored reuse of existing project evaluations during Pack ([NuGet/NuGet.Client#7603](https://github.com/NuGet/NuGet.Client/pull/7603)).
