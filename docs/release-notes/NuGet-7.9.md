---
title: NuGet 7.9 Release Notes
description: Release notes for NuGet 7.9 including new features, bug fixes, and DCRs.
author: donnie-msft
ms.date: 7/31/2026
ms.topic: release-notes
---

# NuGet 7.9 Release Notes

NuGet distribution vehicles:

| NuGet version | Available in Visual Studio version | Available in .NET SDK(s) |
|:---|:---|:---|
| [**7.9.0**](https://nuget.org/downloads) | [Visual Studio 2026 version 18.9.0](https://visualstudio.microsoft.com/downloads/) | [10.0.400](https://dotnet.microsoft.com/download/dotnet/10.0)<sup>1</sup> |

<sup>1</sup> Installed with Visual Studio 2026 with any .NET workload

## Summary: What's New in 7.9.0

### Breaking changes

* Add deprecation warning for monoandroid TFMs - [#14828](https://github.com/NuGet/Home/issues/14828)

* Report real transitive reference versions in `dotnet nuget why`, not final resolved versions - [#13752](https://github.com/NuGet/Home/issues/13752)

### Issues fixed in this release

* Remove unused Package Reference upgrade InfoBar in PM UI - [#14968](https://github.com/NuGet/Home/issues/14968)

* Installed packages load all version info before returning, taking a long time to load on slow feeds - [#14964](https://github.com/NuGet/Home/issues/14964)

* Restore will start checking audit sources are secure HTTPS - [#14962](https://github.com/NuGet/Home/issues/14962)

* static graph restore doesn&#39;t handle relative paths for RestoreSource correctly - [#14904](https://github.com/NuGet/Home/issues/14904)

* nuget.exe restore `-MSBuildPath` crashes when pointing it to .NET SDK directory - [#14844](https://github.com/NuGet/Home/issues/14844)

* Show certificate CRL and OCSP URLs in `dotnet nuget verify` output - [#14780](https://github.com/NuGet/Home/issues/14780)

* dotnet nuget why on non-sdk style project&#39;s assets file doesn&#39;t work - [#14695](https://github.com/NuGet/Home/issues/14695)

* dotnet add package incorrectly adds PrivateAssets to the PackageVersion item - [#14601](https://github.com/NuGet/Home/issues/14601)

* Revisit deterministic pack - [#8601](https://github.com/NuGet/Home/issues/8601)

[List of commits in this release](https://github.com/NuGet/NuGet.Client/compare/7.8.0.59...7.9.0.83)

### Community contributions

Thank you to all the contributors who helped make this NuGet release awesome!

* [OvesN](https://github.com/NuGet/NuGet.Client/pull/7537)
  * [7537](https://github.com/NuGet/NuGet.Client/pull/7537) Mark GetRestoreProjectStyleTask as MSBuild-multithreadable
  * [7534](https://github.com/NuGet/NuGet.Client/pull/7534) Mark WarnForInvalidProjectsTask as MSBuild-multithreadable
  * [7536](https://github.com/NuGet/NuGet.Client/pull/7536) Migrate GetRestoreProjectReferencesTask to the multithreadable model
* [JanProvaznik](https://github.com/NuGet/NuGet.Client/pull/7509)
  * [7509](https://github.com/NuGet/NuGet.Client/pull/7509) Annotate trivial Nuget.Build.Tasks for multithreadable MSBuild execution  
  * [7508](https://github.com/NuGet/NuGet.Client/pull/7508) Bump MSBuild to 18.6.3 for NET compilation of Nuget.Build.Tasks.dll
* [sbomer](https://github.com/NuGet/NuGet.Client/pull/7511)
  * [7511](https://github.com/NuGet/NuGet.Client/pull/7511) Skip NU1703 for empty MonoAndroid assets
* [omajid](https://github.com/NuGet/NuGet.Client/pull/7410)
  * [7410](https://github.com/NuGet/NuGet.Client/pull/7410) Make PackageBuilder.CalcPsmdcpName deterministic
* [liliankasem](https://github.com/NuGet/NuGet.Client/pull/7395)
  * [7395](https://github.com/NuGet/NuGet.Client/pull/7395) Fix package type query parameter for V3 search, add capability check
* [nareshjo](https://github.com/NuGet/NuGet.Client/pull/7405)
  * [7405](https://github.com/NuGet/NuGet.Client/pull/7405) Fix VS allocation issue: NuGet restore content-file glob matching
* [333fred](https://github.com/NuGet/NuGet.Client/pull/7407)
  * [7407](https://github.com/NuGet/NuGet.Client/pull/7407) Fix nullable annotation on LoadSpecificSettings
