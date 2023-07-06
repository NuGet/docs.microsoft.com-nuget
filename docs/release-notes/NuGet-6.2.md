---
title: NuGet 6.2 Release Notes
description: Release notes for NuGet 6.2 including new features, bug fixes, and DCRs.
author: martinrrm
ms.author: mruizmares
ms.date: 5/9/2022
ms.topic: conceptual
---

# NuGet 6.2 Release Notes

NuGet distribution vehicles:

| NuGet version | Available in Visual Studio version | Available in .NET SDK(s) |
|:---|:---|:---|
| [**6.2.0**](https://nuget.org/downloads) | [Visual Studio 2022 version 17.2](https://visualstudio.microsoft.com/downloads/) | [6.0.300](https://dotnet.microsoft.com/download/dotnet-core/6.0)<sup>1</sup> |
| [**6.2.1**](https://nuget.org/downloads) | [Visual Studio 2022 version 17.2.4](https://visualstudio.microsoft.com/downloads/) | [6.0.301](https://dotnet.microsoft.com/download/dotnet-core/6.0)<sup>1</sup> |
| [**6.2.2**](https://nuget.org/downloads) | [Visual Studio 2022 version 17.2](https://visualstudio.microsoft.com/downloads/) | [6.0.305](https://dotnet.microsoft.com/download/dotnet-core/6.0)<sup>1</sup> |
| [**6.2.4**](https://nuget.org/downloads) | N/A | [6.0.313](https://dotnet.microsoft.com/download/dotnet-core/6.0)<sup>1</sup> |

<sup>1</sup> Installed with Visual Studio 2022 with .NET Core workload

## Summary: What's New in 6.2.4

* [Security]: Microsoft Security Advisory CVE-2023-29337 | NuGet Client Remote Code Execution Vulnerability - [#12653](https://github.com/NuGet/Home/issues/12653)

> [!NOTE]
> There is a behavior breaking change on Linux. The temp folder where NuGet stores temporary files during its various operations is changed from `/tmp/NuGetScratch` to `/tmp/NuGetScratch<username>` on Linux. E.g. for user User1, the temp folder will be `/tmp/NuGetScratchUser1`.

## Summary: What's New in 6.2.2

* [Security]: Microsoft Security Advisory CVE 2022-41032 | .NET Elevation of Privilege Vulnerability - [#12149](https://github.com/NuGet/Home/issues/12149)

## Summary: What's New in 6.2.1

* [Security]: Microsoft Security Advisory CVE 2022-30184 | .NET Information Disclosure Vulnerability - [#11883](https://github.com/NuGet/Home/issues/11883)

## Summary: What's New in 6.2

* Add TFM for .NET nanoFramework - [#10800](https://github.com/NuGet/Home/issues/10800)

* [Feature]: Require package source mapping when using CPM - [#11505](https://github.com/NuGet/Home/issues/11505)

* [Feature]: Allow overriding a centrally defined package version - [#11516](https://github.com/NuGet/Home/issues/11516)

* [Feature]: Add IVsNuGetProjectUpdateEvents in Visual Studio, reporting of restore changes for PackageReference based projects.  - [#9782](https://github.com/NuGet/Home/issues/9782)  - [See documentation](../visual-studio-extensibility/nuget-api-in-visual-studio.md#ivsnugetprojectupdateevents-interface)

* Project A referencing package B via AssetTargetFallback, doesn't use that same AssetTargetFallback to pull B's dependency package C - [#5957](https://github.com/NuGet/Home/issues/5957) - [More information](https://github.com/NuGet/Samples/tree/main/AssetTargetFallbackTransitiveDependencies)

### Issues fixed in this release

**DCRs:**

* Make LocalPackageFileCache methods virtual - [#10325](https://github.com/NuGet/Home/issues/10325)

* NuGetScratch lock files are not cleaned up - [#10679](https://github.com/NuGet/Home/issues/10679)

* AutoCompleteResourceV3 does not use the supplied logger - [#11272](https://github.com/NuGet/Home/issues/11272)

* Add Author to the tooltip for a package in the packages list of PM UI - [#11499](https://github.com/NuGet/Home/issues/11499)

* Remove unused code NU5049 - [#11598](https://github.com/NuGet/Home/issues/11598)

**Bugs:**

* Revert mitigation of missing nuget.org when other tools create nuget.config [#11616](https://github.com/NuGet/Home/issues/11616)

* Add support for grouping to the InfiniteScrollList, allowing it to be enabled or disabled - [#10748](https://github.com/NuGet/Home/issues/10748)

* Make the InfiniteScrollList grouping sections expandable and collapsible - [#10749](https://github.com/NuGet/Home/issues/10749)

* Read and store the transitive origins of a package while reading installed packages from assets file - [#10751](https://github.com/NuGet/Home/issues/10751)

* Add caching of the transitive dependencies data pulled from the lockfile (assets file) - [#10752](https://github.com/NuGet/Home/issues/10752)

* Surface the transitive packages and its transitive origins through the search layer - [#11486](https://github.com/NuGet/Home/issues/11486)

* NuGet.exe list from local packages folder does not work with the AllVersion flag - [#4537](https://github.com/NuGet/Home/issues/4537)

* Errors due to missing/failing sources are inconsistently shown in solution explorer vs the error list  - [#7245](https://github.com/NuGet/Home/issues/7245)

* Arrow keys in NuGet PM UI Sources editing doesn't change order of persistence - [#8315](https://github.com/NuGet/Home/issues/8315)

* PackageReference ungracefully handles duplicate Runtime Identifiers in csproj PackageReference  - [#9290](https://github.com/NuGet/Home/issues/9290)

* RestoreIgnoreFailedSources=true still gives warnings - [#9765](https://github.com/NuGet/Home/issues/9765)

* Introduce a warning for null/empty version range (new or reuse NU1604) - [#9767](https://github.com/NuGet/Home/issues/9767)

* NuGet again throwing exceptions "authors is required" "description is required", ignoring csproj/nuspec replacement tokens - [#9954](https://github.com/NuGet/Home/issues/9954)

* [Bug]: Package extraction sometimes fails with "file in use by another process" - [#11373](https://github.com/NuGet/Home/issues/11373)

* Add progress reporting during package installation - [#11432](https://github.com/NuGet/Home/issues/11432)

* [Bug]: Reduce string allocations in restore code path - [#11475](https://github.com/NuGet/Home/issues/11475)

* [Responsiveness] RestoreOperationLogger blocking large number of thread pool threads trying to get access to the output window pane - [#11501](https://github.com/NuGet/Home/issues/11501)

* [Responsiveness] Package Management UI can consume large number of threads all searching the disk, it needs to run from long running thread - [#11570](https://github.com/NuGet/Home/issues/11570)

* [Responsiveness] Package Management UI can consume large number of threads all searching the disk (up to 316 threads), use cancellation token at subroutines - [#11599](https://github.com/NuGet/Home/issues/11599)

* [Bug]: NU1004 in Visual Studio, but not command line (lock files in locked mode) - [#11639](https://github.com/NuGet/Home/issues/11639)

* [Bug]: new warning for package source mappings doesn't pass a value for the resource string placeholder - [#11709](https://github.com/NuGet/Home/issues/11709)


**[List of commits in this release](https://github.com/NuGet/NuGet.Client/compare/6.1.0.80%5E...6.2.0.146)**

### Community contributions

Thank you to all the contributors who helped make this NuGet release awesome!

|Who|PRs|Issues|
|----|----|----|
[MarkKharitonov](https://github.com/MarkKharitonov) | [4511](https://github.com/nuget/nuget.client/pull/4511) | [Feature]: Add support for a dedicated environment variable providing the NuGetScratch path. - [#11671](https://github.com/NuGet/Home/issues/11671)
[mfkl](https://github.com/mfkl) | [4222](https://github.com/nuget/nuget.client/pull/4222) | A better cache clean-up and expiration policy - [#4980](https://github.com/NuGet/Home/issues/4980)
[dfederm](https://github.com/dfederm) | [4504](https://github.com/nuget/nuget.client/pull/4504) | Static Graph restore uses Project.FromFile + Project.CreateInstance instead of ProjectInstance.FromFile directly - [#11675](https://github.com/NuGet/Home/issues/11675)
[crummel](https://github.com/crummel) | [4404](https://github.com/nuget/nuget.client/pull/4404) | [main] Backport source-build patches to repos. [#2708](https://github.com/dotnet/source-build/issues/2708)
[mjolka](https://github.com/mjolka) | [4475](https://github.com/nuget/nuget.client/pull/4475) | Very slow restore when using NoWarn in single project that has lots of dependents - [#11222](https://github.com/NuGet/Home/issues/11222)
[marcin-krystianc](https://github.com/marcin-krystianc) | [4488](https://github.com/nuget/nuget.client/pull/4488) | dotnet integration pack test IL issue - [#11454](https://github.com/NuGet/Home/issues/11454)
[marcin-krystianc](https://github.com/marcin-krystianc) | [4025](https://github.com/nuget/nuget.client/pull/4025) | Restore fails with NU1106 for solution that uses StaticGraph and CPVM - [#10327](https://github.com/NuGet/Home/issues/10327); [Feature]: Add option to allow versions of transitive dependencies to be overridden - [#10389](https://github.com/NuGet/Home/issues/10389)
[davkean](https://github.com/davkean) | [4483](https://github.com/nuget/nuget.client/pull/4483) | Remove unneeded allocations when parsing assets file [#11648](https://github.com/NuGet/Home/issues/11648)
[reynoldsbd](https://github.com/reynoldsbd) | [4458](https://github.com/nuget/nuget.client/pull/4458) | [Bug]: Race Condition Creating Plugin Log Files - [#11517](https://github.com/NuGet/Home/issues/11517)
[tintoy](https://github.com/tintoy) | [4287](https://github.com/nuget/nuget.client/pull/4287) | AutoCompleteResourceV3 does not use the supplied logger - [#11272](https://github.com/NuGet/Home/issues/11272)
[davkean](https://github.com/davkean) | [4440](https://github.com/nuget/nuget.client/pull/4440) | Improve VS and NuGet performance by making some methods non-asynchronous - [#11816](https://github.com/NuGet/Home/issues/11816)
[davkean](https://github.com/davkean) | [4439](https://github.com/nuget/nuget.client/pull/4439) | Redundant calls to get VsHierarchy in NuGet VS code - [#11817](https://github.com/NuGet/Home/issues/11817)
[davkean](https://github.com/davkean) | [4432](https://github.com/nuget/nuget.client/pull/4432) | Avoid double-checking for supported projects - [#11554](https://github.com/NuGet/Home/issues/11554)
[dfederm](https://github.com/dfederm) | [4393](https://github.com/nuget/nuget.client/pull/4393) | [Bug]: Static graph restore binlog doesn't log task inputs - [#11484](https://github.com/NuGet/Home/issues/11484)
[drewnoakes](https://github.com/drewnoakes) | [4390](https://github.com/nuget/nuget.client/pull/4390) | Show package .props and .targets files in Solution Explorer [#7838](https://github.com/dotnet/project-system/issues/7838)
[drewnoakes](https://github.com/drewnoakes) | [4386](https://github.com/nuget/nuget.client/pull/4386) | Solution Explorer search is not showing package contents - [#7834](https://github.com/dotnet/project-system/issues/7834)
[marcin-krystianc](https://github.com/marcin-krystianc) | [4186](https://github.com/nuget/nuget.client/pull/4186) | [Regression]: Performance regression for cold restores in .NET 5.0.x [#11031](https://github.com/NuGet/Home/issues/11031)
[joperator](https://github.com/joperator) | [4389](https://github.com/nuget/nuget.client/pull/4389) | [Bug]: Errors NU3028 and NU3037 when restoring NuGet packages on FreeBSD - [#11481](https://github.com/NuGet/Home/issues/11481)
[AndreiTimisescu](https://github.com/AndreiTimisescu) | [3779](https://github.com/nuget/nuget.client/pull/3779) | Make LocalPackageFileCache methods virtual - [#10325](https://github.com/NuGet/Home/issues/10325)
[tmds](https://github.com/tmds) | [4123](https://github.com/nuget/nuget.client/pull/4123) | NuGetScratch lock files are not cleaned up - [#10679](https://github.com/NuGet/Home/issues/10679)

## Feedback welcome

Your feedback is important to us. If there are any problems with this release, check our
[GitHub Issues](https://github.com/NuGet/Home/issues) and
[Visual Studio Developer Community](https://developercommunity.visualstudio.com/)
for existing issues.  For new issues within NuGet, please report a
[GitHub Issue](https://github.com/NuGet/Home/issues/new/choose).
For general NuGet experience issues, let us know via the
[Report a Problem](/visualstudio/ide/how-to-report-a-problem-with-visual-studio)
option found in your favorite IDE under **Help > Report a Problem**.
