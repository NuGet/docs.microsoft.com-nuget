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

<sup>1</sup> Installed with Visual Studio 2022 with.NET Core workload

## Summary: What's New in 6.2

* Add TFM for .NET nanoFramework - [#10800](https://github.com/NuGet/Home/issues/10800)

* [Feature]: Require package source mapping when using CPM - [#11505](https://github.com/NuGet/Home/issues/11505)

* [Feature]: Allow overriding a centrally defined package version - [#11516](https://github.com/NuGet/Home/issues/11516)

* [Feature]: Add support for a dedicated environment variable providing the NuGetScratch path. - [#11671](https://github.com/NuGet/Home/issues/11671)

* [Feature]: Add IVsNuGetProjectUpdateEvents in Visual Studio, reporting of restore changes for PackageReference based projects.  - [#9782](https://github.com/NuGet/Home/issues/9782)  - [See documentation](https://docs.microsoft.com/nuget/visual-studio-extensibility/nuget-api-in-visual-studio#ivsnugetprojectupdateevents-interface)

* Project A referencing package B via AssetTargetFallback, doesn't use that same AssetTargetFallback to pull B's dependency package C - [#5957](https://github.com/NuGet/Home/issues/5957) - [More information](https://github.com/NuGet/Samples/tree/main/AssetTargetFallbackTransitiveDependencies)

### Issues fixed in this release

**DCRs:**

* Make LocalPackageFileCache methods virtual - [#10325](https://github.com/NuGet/Home/issues/10325)

* NuGetScratch lock files are not cleaned up - [#10679](https://github.com/NuGet/Home/issues/10679)

* AutoCompleteResourceV3 does not use the supplied logger - [#11272](https://github.com/NuGet/Home/issues/11272)

* [DCR]: Mitigate missing nuget.org when non-NuGet tool creates nuget.config without any sources - [#11387](https://github.com/NuGet/Home/issues/11387)

* Add Author to the tooltip for a package in the packages list of PM UI - [#11499](https://github.com/NuGet/Home/issues/11499)

* Remove NU5049  - [#11598](https://github.com/NuGet/Home/issues/11598)

**Bugs:**

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

* [Regression]: Performance regression for cold restores in .NET 5.0.x - [#11031](https://github.com/NuGet/Home/issues/11031)

* [Bug]: Package extraction sometimes fails with "file in use by another process" - [#11373](https://github.com/NuGet/Home/issues/11373)

* Add progress reporting during package installation - [#11432](https://github.com/NuGet/Home/issues/11432)

* [Bug]: Reduce string allocations in restore code path - [#11475](https://github.com/NuGet/Home/issues/11475)

* [Bug]: Errors NU3028 and NU3037 when restoring NuGet packages on FreeBSD - [#11481](https://github.com/NuGet/Home/issues/11481)

* [Responsiveness] RestoreOperationLogger blocking large number of thread pool threads trying to get access to the output window pane - [#11501](https://github.com/NuGet/Home/issues/11501)

* [Bug]: Race Condition Creating Plugin Log Files - [#11517](https://github.com/NuGet/Home/issues/11517)

* [Responsiveness] Package Management UI can consume large number of threads all searching the disk, it needs to run from long running thread - [#11570](https://github.com/NuGet/Home/issues/11570)

* [Responsiveness] Package Management UI can consume large number of threads all searching the disk (up to 316 threads), use cancellation token at subroutines - [#11599](https://github.com/NuGet/Home/issues/11599)

* [Bug]: NU1004 in Visual Studio, but not command line (lock files in locked mode) - [#11639](https://github.com/NuGet/Home/issues/11639)

* [Bug]: new warning for package source mappings doesn't pass a value for the resource string placeholder - [#11709](https://github.com/NuGet/Home/issues/11709)
