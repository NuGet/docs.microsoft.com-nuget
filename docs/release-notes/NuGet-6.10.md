---
title: NuGet 6.10 Release Notes
description: Release notes for NuGet 6.10 including new features, bug fixes, and DCRs.
author: kartheekp-ms
ms.date: 5/13/2024
ms.topic: conceptual
---

# NuGet 6.10 Release Notes

NuGet distribution vehicles:

| NuGet version | Available in Visual Studio version | Available in .NET SDK(s) |
|:---|:---|:---|
| [**6.10.1**](https://nuget.org/downloads) | [Visual Studio 2022 version 17.10](https://visualstudio.microsoft.com/downloads/) | [8.0.3xx](https://dotnet.microsoft.com/download/dotnet/8.0)<sup>1</sup> |

<sup>1</sup> Installed with Visual Studio 2022 with any .NET workload

## Summary: What's New in 6.10.1

* [Feature]: add dotnet nuget config command - [#12469](https://github.com/NuGet/Home/issues/12469)

* Warn when vulnerabilities are detected during packages.config restore - [#12307](https://github.com/NuGet/Home/issues/12307)

* Display a vulnerability info bar when there's vulnerabilities in packages.config based projects. - [#13271](https://github.com/NuGet/Home/issues/13271)

* [Bug]: VS Credential Provider Incorrectly Setting Value of `isRetry` - [#11210](https://github.com/NuGet/Home/issues/11210)

* dotnet list package --vulnerable requires constant login to 3rd party nuget feed - [#12456](https://github.com/NuGet/Home/issues/12456)

* [Feature]: add command dotnet config set - [#12476](https://github.com/NuGet/Home/issues/12476)

### 6.10.0 Known issues

NuGet.exe 6.10.0 and Visual Studio 17.10.0 may have failures during NuGet operations for projects using packages.config under the following scenarios:

* Two or more projects in the solution have the same name
* Two or more projects in the solution use the same packages.config file (the project files exist in the same directory)

These issues have been fixed in NuGet.exe 6.10.1 and Visual Studio 17.10.2.

Public tracking issues and discussions can be found in the following locations:

* [Cannot nuget restore after updating visual studio community to 17.10.0. An item with the same key has already been added.](https://developercommunity.visualstudio.com/t/Cannot-nuget-restore-after-updating-visu/10665602)
* [Visual Studio and PMC restore/update fails when multiple packages.config projects in the solution share the same name (An item with the same key has already been added)](https://github.com/NuGet/Home/issues/13465)
* [##[error]The nuget command failed with exit code(1) and error(An item with the same key has already been added.](https://github.com/NuGet/Home/issues/13456)

### Breaking changes

* Add nullable annotations to NuGet.Configuration - [#13250](https://github.com/NuGet/Home/issues/13250)

* [Dotnet Package Search] The search result of the package should be “version” instead of “latestVersion” when executing command “dotnet package search \<Package Name\> --exact-match --format json” - [#13158](https://github.com/NuGet/Home/issues/13158)

* ResolvedDependencyKey should be struct to reduce memory allocations - [#13138](https://github.com/NuGet/Home/issues/13138)

* [DCR]: Central Package Management - Respect .props file as a way to opt-in to the feature. - [#11834](https://github.com/NuGet/Home/issues/11834)

* Remove NuGet.Packaging.Core - [#12495](https://github.com/NuGet/Home/issues/12495)

### Issues fixed in this release

* Warn when vulnerabilities are detected during packages.config restore in CLI scenarios. - [#13253](https://github.com/NuGet/Home/issues/13253)


* Add event tracing to restore to make it possible to measure performance - [#13274](https://github.com/NuGet/Home/issues/13274)

* Read auditSources from nuget.config files - [#13211](https://github.com/NuGet/Home/issues/13211)

* Promote from warning to error in list package - [#13323](https://github.com/NuGet/Home/issues/13323)

* SourceRepository.GetResourceAsync should be cancellable. - [#13234](https://github.com/NuGet/Home/issues/13234)

* CreateWalkAsync should not be recursive - [#13222](https://github.com/NuGet/Home/issues/13222)

* ProjectModel.HashObjectWriter.OnFlush is using a SHA512 hash versus a cheaper hash which seems like overkill - [#13214](https://github.com/NuGet/Home/issues/13214)

* Owner package metadata is an array in JSON but a string in Protocol types - [#13186](https://github.com/NuGet/Home/issues/13186)

* dotnet package search --verbosity detailed output table too wide - [#13162](https://github.com/NuGet/Home/issues/13162)

* Calls in SetWarningProperties() have allocation overhead due to multiple enumeration - [#13151](https://github.com/NuGet/Home/issues/13151)

* Use of ConcurrentStack in object pool implementation creates large amounts of allocations - [#13147](https://github.com/NuGet/Home/issues/13147)

* IsBestVersion boxes HashSet\<T\> enumerators resulting in lots of allocation overhead during restore. - [#13146](https://github.com/NuGet/Home/issues/13146)

* Deprecate NuGet.exe list in favor of NuGet.exe search - [#7912](https://github.com/NuGet/Home/issues/7912)

* Visual Studio and PMC restore/update fails when multiple packages.config projects in the solution share the same name (An item with the same key has already been added) - [#13465](https://github.com/NuGet/Home/issues/13465)

* ##[error]The nuget command failed with exit code(1) and error(An item with the same key has already been added. - [#13456](https://github.com/NuGet/Home/issues/13456)

* [Dotnet Package Search] An unhandled exception is thrown when searching with “--verbosity detailed” and “--format json” - [#13300](https://github.com/NuGet/Home/issues/13300)

* `dotnet package search` crashes on .NET 9 preview 2 nightly - [#13286](https://github.com/NuGet/Home/issues/13286)

* Use the StringBuilderPool rather than allocating a new StringBuilder - [#13285](https://github.com/NuGet/Home/issues/13285)

* Pass in an appropriate size for List\<T\> - [#13284](https://github.com/NuGet/Home/issues/13284)

* JsonTextWriter allocates a large number strings - [#13283](https://github.com/NuGet/Home/issues/13283)

* Usage of StringBuilder.Append() allocates when appending an int - [#13282](https://github.com/NuGet/Home/issues/13282)

* Unnecessary boxing of struct enumerators - [#13281](https://github.com/NuGet/Home/issues/13281)

* Process objects should be disposed so that the finalizer doesn't run - [#13280](https://github.com/NuGet/Home/issues/13280)

* Avoid boxing enumerators for collections - [#13279](https://github.com/NuGet/Home/issues/13279)

* [.NET 9 Preview 3] no-op restore is no longer a no-op - [#13269](https://github.com/NuGet/Home/issues/13269)

* Reduce allocations in calling IVsProjectAdpater.IsCapabilityMatchAsync - [#13268](https://github.com/NuGet/Home/issues/13268)

* Consolidate packages.config restore implementations by minimizing discrepancies - [#13233](https://github.com/NuGet/Home/issues/13233)

* Unroll LINQ usage to trim some allocations in AddMSBuildAssets - [#13223](https://github.com/NuGet/Home/issues/13223)

* PackageSpecWriter is calling Any on an ICollection\<T\>/IList\<T\> instances instead of .Count - [#13213](https://github.com/NuGet/Home/issues/13213)

* StringBuilder.Append(int) causes unnecessary allocations - [#13207](https://github.com/NuGet/Home/issues/13207)

* Caching task results can cause all continuations to occur on the same thread - [#13206](https://github.com/NuGet/Home/issues/13206)

* LibraryDependency creates a collection that is mostly empty - [#13184](https://github.com/NuGet/Home/issues/13184)

* PackageSpecWriter should write out original string for versions instead of allocating a new one - [#13183](https://github.com/NuGet/Home/issues/13183)

* SetCentralDependencies is calling OrderBy without specifying comparison defaulting to cultural-sensitive compare - [#13182](https://github.com/NuGet/Home/issues/13182)

* Search results in json format from dotnet package search should not include fields for which no values are provided - [#13166](https://github.com/NuGet/Home/issues/13166)

* The search result of the package should be “totalDownloads” instead of “total downloads” when executing command “dotnet package search \<Package Name\> --format json” - [#13165](https://github.com/NuGet/Home/issues/13165)

* [dotnet package search] the failure to load a serivce index should be an error and not a warning.  - [#13163](https://github.com/NuGet/Home/issues/13163)

* dotnet package search format shows help output in addition to a json file - [#13161](https://github.com/NuGet/Home/issues/13161)

* Cancelling static graph-based restore does not end the restore process - [#13140](https://github.com/NuGet/Home/issues/13140)

* Use string.Contains instead of IEnumerable.Contains in VersionRange parsing - [#13124](https://github.com/NuGet/Home/issues/13124)

* Static graph restore doesn't restore packages.config only solution - [#13109](https://github.com/NuGet/Home/issues/13109)

* NuGet restore always touched the project.assets.json file even no content is changed when it needs log error messages. - [#13098](https://github.com/NuGet/Home/issues/13098)

* Audit at restore time must not throw and fail the operation - [#13085](https://github.com/NuGet/Home/issues/13085)

* Getting "An item with the same key has already been added" error when restoring. - [#13067](https://github.com/NuGet/Home/issues/13067)

* PM UI Scrolling no longer loads additional packages. - [#13063](https://github.com/NuGet/Home/issues/13063)

* NuGet restore cache check is no longer using file existence cache - [#13058](https://github.com/NuGet/Home/issues/13058)

* Adding a reference to an esproj from an ASP.Net project results in a NU1105 error - [#12986](https://github.com/NuGet/Home/issues/12986)

* RemoteDependencyWalker allocates a lot due to the fact that it's called recursively - [#12748](https://github.com/NuGet/Home/issues/12748)

* Stop using JObject in assets file reading to reduce allocations - [#12715](https://github.com/NuGet/Home/issues/12715)

* dotnet list package --vulnerable requires constant login to 3rd party nuget feed - [#12456](https://github.com/NuGet/Home/issues/12456)

* [Bug]: Canceling msbuild restore is slow when invalid/unreachable source configured - [#11813](https://github.com/NuGet/Home/issues/11813)

* [Bug]: VS Credential Provider Incorrectly Setting Value of `isRetry` - [#11210](https://github.com/NuGet/Home/issues/11210)

* Restore:  excessive deep cloning of ProjectSpec - [#9041](https://github.com/NuGet/Home/issues/9041)

[List of commits in this release](https://github.com/NuGet/NuGet.Client/compare/6.9.1.3...6.10.1.5)

### Community contributions

Thank you to all the contributors who helped make this NuGet release awesome!

* [Erarndt](https://github.com/NuGet/NuGet.Client/pull/5659)
  * [5659](https://github.com/NuGet/NuGet.Client/pull/5659) Reduce boxing when enumerating lists
  * [5662](https://github.com/NuGet/NuGet.Client/pull/5662) Use StringBuilder.AppendInt() in more places to reduce allocations when appending integers to a StringBuilder
  * [5663](https://github.com/NuGet/NuGet.Client/pull/5663) Remove allocations from TextWriter.NewLine implementation
  * [5665](https://github.com/NuGet/NuGet.Client/pull/5665) Use pooled StringBuilder
  * [5661](https://github.com/NuGet/NuGet.Client/pull/5661) Avoid enumerator boxing in PackageSpecificWarningProperties.AddRangeOfCodes and TransitiveNoWarnUtils.AddToSeen
  * [5660](https://github.com/NuGet/NuGet.Client/pull/5660) Ensure that Process objects are disposed when launching authentication plug-ins
  * [5664](https://github.com/NuGet/NuGet.Client/pull/5664) Specify list size in TopologicalSortUtility.CalculateRelationships
  * [5624](https://github.com/NuGet/NuGet.Client/pull/5624) Switch CreateNodeAsync to an iterative approach
  * [5584](https://github.com/NuGet/NuGet.Client/pull/5584) Unroll LINQ usage to trim some allocations in AddMSBuildAssets
  * [5574](https://github.com/NuGet/NuGet.Client/pull/5574) Unroll several LINQ calls
  * [5593](https://github.com/NuGet/NuGet.Client/pull/5593) Further reduce allocations in CreateGraphNodeAsync
  * [5592](https://github.com/NuGet/NuGet.Client/pull/5592) Avoid multiple enumeration when writing Json objects
  * [5600](https://github.com/NuGet/NuGet.Client/pull/5600) Avoid intermediate string allocation caused by StringBuilder Append(i…
  * [5555](https://github.com/NuGet/NuGet.Client/pull/5555) Unroll Linq in GetFlags
  * [5588](https://github.com/NuGet/NuGet.Client/pull/5588) Avoid boxing HashSet Enumerator in IsBestVerion
  * [5589](https://github.com/NuGet/NuGet.Client/pull/5589) Update the pool implementation to use a stack with a lock to avoid al…
  * [5568](https://github.com/NuGet/NuGet.Client/pull/5568) Change ResolvedDependencyKey to a struct
  * [5553](https://github.com/NuGet/NuGet.Client/pull/5553) Avoid some allocations in GetCompatibilityData
  * [5554](https://github.com/NuGet/NuGet.Client/pull/5554) Switch from Tuple to ValueTuple for Dictionary keys
  * [5552](https://github.com/NuGet/NuGet.Client/pull/5552) Avoid creating the capture class for calls to WithExtension
  * [5556](https://github.com/NuGet/NuGet.Client/pull/5556) Switch from ConcurrentDictionary to Dictionary with lock to avoid rep…
  * [5551](https://github.com/NuGet/NuGet.Client/pull/5551) Use string.Contains instead of IEnumerable.Contains
  * [5550](https://github.com/NuGet/NuGet.Client/pull/5550) Avoid boxing List\<T\> enumerator
* [GenelleM](https://github.com/NuGet/NuGet.Client/pull/5655)
  * [5655](https://github.com/NuGet/NuGet.Client/pull/5655) Add 64-bit non crypto hash algo for dgspec uniqueness computation
  * [5629](https://github.com/NuGet/NuGet.Client/pull/5629) Replace calls to Any() on ICollection in PackageSpecWriter with Count > 0 Issue #13213
  * [5619](https://github.com/NuGet/NuGet.Client/pull/5619) Reduces some CPU time usage in SetCentralDependencies 
* [NikolaMilosavljevic](https://github.com/NuGet/NuGet.Client/pull/5673)
  * [5673](https://github.com/NuGet/NuGet.Client/pull/5673) Enable publishing in VMR
  * [5625](https://github.com/NuGet/NuGet.Client/pull/5625) Eliminate System.CommandLine prebuilt package
* [brianrob](https://github.com/NuGet/NuGet.Client/pull/5650)
  * [5650](https://github.com/NuGet/NuGet.Client/pull/5650) Add Restore Instrumentation