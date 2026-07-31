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

* Capability discovery for SearchFilter.PackageTypes across PackageSearchResource implementations - [#14936](https://github.com/NuGet/Home/issues/14936)

* Use a deterministic order for files when calculating psmdcp hash - [#14916](https://github.com/NuGet/Home/issues/14916)

* Feature ask client side -- Surface PackageType query parameter in the NuGet.org search API - [#8915](https://github.com/NuGet/Home/issues/8915)

### Breaking changes

* SearchFilter changing `PackageTypes` to `PackageType` - [#14941](https://github.com/NuGet/Home/issues/14941)

* Add nullable annotations to NuGet.Protocol - [#14851](https://github.com/NuGet/Home/issues/14851)

* Add deprecation warning for monoandroid TFMs - [#14828](https://github.com/NuGet/Home/issues/14828)

* Report real transitive reference versions in `dotnet nuget why`, not final resolved versions - [#13752](https://github.com/NuGet/Home/issues/13752)

### Issues fixed in this release

* `dotnet restore/build` should work correctly when its MSBuild tasks run repeatedly and in the same process - [#14958](https://github.com/NuGet/Home/issues/14958)

* Don't require the usage of `RootCommand` to wire up the NuGet System.CommandLine commands - [#14939](https://github.com/NuGet/Home/issues/14939)

* Remove unused Package Reference upgrade InfoBar in PM UI - [#14968](https://github.com/NuGet/Home/issues/14968)

* Installed packages load all version info before returning, taking a long time to load on slow feeds - [#14964](https://github.com/NuGet/Home/issues/14964)

* Restore will start checking audit sources are secure HTTPS - [#14962](https://github.com/NuGet/Home/issues/14962)

* RuntimeEnvironmentHelper: remove Lazy for basic platform checks - [#14951](https://github.com/NuGet/Home/issues/14951)

* Remove `feature switch || runtime check()` used in nuget.client repo - [#14944](https://github.com/NuGet/Home/issues/14944)

* The error “the JSON value could not be converted to System.Int64” occurs when selecting a valid devops source in the ‘Package Source’ dropdown list on PM UI - [#14943](https://github.com/NuGet/Home/issues/14943)

* Failed to update binding redirects error needs more information - [#14940](https://github.com/NuGet/Home/issues/14940)

* [AOT test] STJ code path failures in NuGet.Protocol: Vuln Severity and PackageDependencyGroupStjConverter - [#14938](https://github.com/NuGet/Home/issues/14938)

* A/B test STJ VS NSJ deserializations - [#14937](https://github.com/NuGet/Home/issues/14937)

* Remove `PackageSearchResourceV3(NuGet.Protocol.RawSearchResourceV3 searchResource) ` constructor - [#14935](https://github.com/NuGet/Home/issues/14935)

* Annotate Newtonsoft.Json deserialization code path - [#14934](https://github.com/NuGet/Home/issues/14934)

* [Bug Bash] The status of the new installed package which doesn’t match the package pattern was shown incorrectly in ‘Browse’ tab - [#14923](https://github.com/NuGet/Home/issues/14923)

* The NuGet Solver tool is still unavailable after the NuGet MCP Server is enabled from MCP Server Manager - [#14912](https://github.com/NuGet/Home/issues/14912)

* static graph restore doesn't handle relative paths for RestoreSource correctly - [#14904](https://github.com/NuGet/Home/issues/14904)

* Migrate NuGet's dotnet CLI commands to System.CommandLine - [#14848](https://github.com/NuGet/Home/issues/14848)

* nuget.exe restore `-MSBuildPath` crashes when pointing it to .NET SDK directory - [#14844](https://github.com/NuGet/Home/issues/14844)

* Show certificate CRL and OCSP URLs in `dotnet nuget verify` output - [#14780](https://github.com/NuGet/Home/issues/14780)

* dotnet nuget why on non-sdk style project's assets file doesn't work - [#14695](https://github.com/NuGet/Home/issues/14695)

* dotnet add package incorrectly adds PrivateAssets to the PackageVersion item - [#14601](https://github.com/NuGet/Home/issues/14601)

* [Bug Bash] The vulnerable warning icon shows incorrectly on the right panel of "Installed" tab in solution-level PM UI - [#14322](https://github.com/NuGet/Home/issues/14322)

* [Bug Bash] The warning icon on the right of installed vulnerable package version doesn’t show for the lower version in "Installed" tab and “Consolidate” tab of solution-level PM UI - [#14024](https://github.com/NuGet/Home/issues/14024)

* Inconsistent restore results based on restore order - [#13326](https://github.com/NuGet/Home/issues/13326)

* [Bug Bash][Unstable] An error “A project with ID ‘XXXXXXX-XXXXXXX’ was not found” occurred after installing a package with PackageReference format into a Console App (.NET Framework 4.8.1) - [#12397](https://github.com/NuGet/Home/issues/12397)

* Revisit deterministic pack - [#8601](https://github.com/NuGet/Home/issues/8601)

* RestoreLockedMode=true & RestoreForceEvaluate=true should error - [#8222](https://github.com/NuGet/Home/issues/8222)

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