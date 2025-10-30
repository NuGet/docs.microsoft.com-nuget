---
title: NuGet 6.8 Release Notes
description: Release notes for NuGet 6.8 including new features, bug fixes, and DCRs.
author: nkolev92
ms.author: nikolev
ms.date: 10/30/2023
ms.topic: release-notes
---

# NuGet 6.8 Release Notes

NuGet distribution vehicles:

| NuGet version | Available in Visual Studio version | Available in .NET SDK(s) |
|:---|:---|:---|
| [**6.8**](https://nuget.org/downloads) | [Visual Studio 2022 version 17.8](https://visualstudio.microsoft.com/downloads/) | [8.0.100](https://dotnet.microsoft.com/download/dotnet/8.0)<sup>1</sup> |
| [**6.8.1**](https://nuget.org/downloads) | [Visual Studio 2022 version 17.8](https://visualstudio.microsoft.com/downloads/) | [8.0.102](https://dotnet.microsoft.com/download/dotnet/8.0)<sup>1</sup> |


<sup>1</sup> Installed with Visual Studio 2022 with any .NET workload

## Summary: What's New in 6.8.1

* [Security]: Microsoft Security Advisory CVE-2024-0057 | NuGet Client Security Feature bypass Vulnerability - [#12653](https://github.com/NuGet/Home/issues/13241)

## Summary: What's New in 6.8

* [NuGetAudit](../concepts/Auditing-Packages.md) - notifications for package vulnerabilities
  * Warn when vulnerabilities are detected during PackageReference restore  - [#12289](https://github.com/NuGet/Home/issues/12289)
  * Show vulnerabilities in transitive packages for PackageReference type projects in PMUI - [#8756](https://github.com/NuGet/Home/issues/8756)
  * Show an infobar in Solution Explorer for any detected security vulnerabilities in a project or solution - [#12398](https://github.com/NuGet/Home/issues/12398)

* Add [`allowInsecureConnections`](../reference/nuget-config-file.md#packagesources) property for package sources in NuGet.config, allowing opt-out of "HTTPs everywhere" warnings - [#12786](https://github.com/NuGet/Home/issues/12786)

* Create Package Source Mappings during Installation/update through PM UI - [#11366](https://github.com/NuGet/Home/issues/11366)

* Conditional package updating is respected in Visual Studio [#5420](https://github.com/NuGet/NuGet.Client/pull/5420)

* Add [protocolVersion argument](../reference/cli-reference/cli-ref-sources.md#options) to nuget source add - [#9170](https://github.com/NuGet/Home/issues/9170)

* Signed package verification is [enabled by default on Linux in .NET 8 SDK](/dotnet/core/tools/nuget-signed-package-verification#linux) - [#11262](https://github.com/NuGet/Home/issues/11262)

### Known issues

* NuGetAuditMode doesn't work for SDK style projects in VS 17.8 - [#13003](https://github.com/NuGet/Home/issues/13003)

### NuGet SDK breaking changes

The following is a list of breaking changes in the NuGet SDK. If you are using NuGet tooling, such as Visual Studio or .NET SDK, you are not affected.

* Remove the NuGetOperationType from NuGet.PackageManagement, use NuGetProjectActionType instead - [#12866](https://github.com/NuGet/Home/issues/12866)

* Changing PackageVulnerabilityInfo severity from int to enum - [#12781](https://github.com/NuGet/Home/issues/12781)

* Add nullable annotations to NuGet.Common - [#12775](https://github.com/NuGet/Home/issues/12775)

* Obsolete Clone methods on immutable types - [#12669](https://github.com/NuGet/Home/issues/12669)

### Issues fixed in this release

* NuGetAudit should not warn when no vulnerability data is available - [#12875](https://github.com/NuGet/Home/issues/12875)

* NuGetAudit: read vulnerability files with System.Text.Json - [#12855](https://github.com/NuGet/Home/issues/12855)

* `PackageSourceMapping` API doesn't follow best practices for returning lists - [#12794](https://github.com/NuGet/Home/issues/12794)

* Signing:  enable `X509Chain.Build(...)` retry behavior by default - [#12592](https://github.com/NuGet/Home/issues/12592)

* NuGetAudit should check direct PackageReferences by default - [#12590](https://github.com/NuGet/Home/issues/12590)

* NuGetAudit should be on by default with the .NET 8 SDK - [#12568](https://github.com/NuGet/Home/issues/12568)

* Remove "Checking compatibility..." log messages from RestoreTask - [#10383](https://github.com/NuGet/Home/issues/10383)

* 16.10: remove package source 1.0 service. remove obsolete APIs (in nuget.configuration that we added in 16.8) - [#10015](https://github.com/NuGet/Home/issues/10015)

* Add more logging to NuGetSdkResolver - [#11445](https://github.com/NuGet/Home/issues/11445)

* Upgrade Newtonsoft.Json reference to 13.0.3 - [#12858](https://github.com/NuGet/Home/issues/12858)

* Add an API for checking vulnerability during packages.config restore - [#12852](https://github.com/NuGet/Home/issues/12852)

* VS Options add/remove package source icons aren't using VS2022 styling - [#12840](https://github.com/NuGet/Home/issues/12840)

* Package Source Mapping utility always appends package ID - [#12839](https://github.com/NuGet/Home/issues/12839)

* NuGetSdkResolver loads global.json multiple times during project load - [#12819](https://github.com/NuGet/Home/issues/12819)

* dotnet list package doesn't list requested versions when using CPM - [#12765](https://github.com/NuGet/Home/issues/12765)

* Fix case sensitivity of runtime dependency sets during merge - [#12757](https://github.com/NuGet/Home/issues/12757)

* dotnet list package errors with Object reference not set to an instance of an object - [#12755](https://github.com/NuGet/Home/issues/12755)

* Improve hashing and equality allocations/performance - [#12746](https://github.com/NuGet/Home/issues/12746)

* NuGetAudit severity bugs - [#12743](https://github.com/NuGet/Home/issues/12743)

* Lock contention thread pool issues caused by LoadSettings not passing settingsLoadingContext to LoadSettingsForSpecificConfigs - [#12737](https://github.com/NuGet/Home/issues/12737)

* NuGetAuditMode all warns about package versions that were upgraded (rejected) - [#12730](https://github.com/NuGet/Home/issues/12730)

* An error “unable to find metadata of PackageName.1.0.0” occurs when installing package with “packages.config” format - [#12723](https://github.com/NuGet/Home/issues/12723)

* WalkTreeRejectNodesOfRejectedNodes constantly triggering resizes of its tracker collection - [#12719](https://github.com/NuGet/Home/issues/12719)

* Reduce RuntimeGraph allocations as it's immutable - [#12717](https://github.com/NuGet/Home/issues/12717)

* Heavy allocations in NuGet.Commands.RestoreRunner.ExecuteAndCommitAsync|nuget.packaging.dll!NuGet.RuntimeModel.RuntimeDescription - [#12714](https://github.com/NuGet/Home/issues/12714)

* Heavy allocations in NuGet.Commands.RestoreRunner.ExecuteAndCommitAsync|nuget.versioning.dll!NuGet.Versioning.VersionFormatter.Format - [#12707](https://github.com/NuGet/Home/issues/12707)

* Remove allocations from PackageSource.Source setter - [#12692](https://github.com/NuGet/Home/issues/12692)

* ContentItemCollection.FindBestItemGroup boxing enumerator - [#12689](https://github.com/NuGet/Home/issues/12689)

* FrameworkNameProvider.GetVersionString boxing enumerator - [#12685](https://github.com/NuGet/Home/issues/12685)

* NuGet.Client allocates many instances of comparers - [#12680](https://github.com/NuGet/Home/issues/12680)

* GetContentFileFolderRelativeToFramework allocates too much - [#12668](https://github.com/NuGet/Home/issues/12668)

* Deprecated info will flash for less than one second in the right penal when clicking package “Microsoft.Net.Http” with a non-deprecated version in the package list - [#12661](https://github.com/NuGet/Home/issues/12661)

* CreateGraphNode has a high number of allocations - [#12641](https://github.com/NuGet/Home/issues/12641)

* The vulnerable label doesn’t show in the “version” dropdown box of “Browse” tab when searching for vulnerable packages  - [#12623](https://github.com/NuGet/Home/issues/12623)

* NuGet.Commands.LockFileBuilder  KeyNotFoundException Exception - [#12464](https://github.com/NuGet/Home/issues/12464)

* A PackageDownload without a version causes a NullReferenceException - [#12212](https://github.com/NuGet/Home/issues/12212)

* [Bug]: View License dialog does not display license content - [#12060](https://github.com/NuGet/Home/issues/12060)


* [Bug Bash] Only the embedded license content of the latest version can be loaded correctly in PM UI when there are multiple versions in the same package from local feeds - [#10670](https://github.com/NuGet/Home/issues/10670)

[List of commits in this release](https://github.com/NuGet/NuGet.Client/compare/6.8.0.131...6.7.0.127)

### Community contributions

Thank you to all the contributors who helped make this NuGet release awesome!

* [drewnoakes](https://github.com/NuGet/NuGet.Client/pull/5311)
  * [5311](https://github.com/NuGet/NuGet.Client/pull/5311) Null annotate PackageDependencyInfo
  * [5310](https://github.com/NuGet/NuGet.Client/pull/5310) Reduce size of LockFileTargetLibrary
  * [5304](https://github.com/NuGet/NuGet.Client/pull/5304) Improve hashing and equality allocations/performance
  * [5267](https://github.com/NuGet/NuGet.Client/pull/5267) Reduce allocations in NuGet.DependencyResolver.Tracker
  * [5232](https://github.com/NuGet/NuGet.Client/pull/5232) Reduce allocations in RuntimeGraph
  * [5279](https://github.com/NuGet/NuGet.Client/pull/5279) Reduce allocations in VersionRangeFormatter
  * [5248](https://github.com/NuGet/NuGet.Client/pull/5248) Reduce allocations in RuntimeDescription and RuntimeDependencySet
  * [5269](https://github.com/NuGet/NuGet.Client/pull/5269) Don't box enumerators in ContentItemCollection
  * [5250](https://github.com/NuGet/NuGet.Client/pull/5250) Don't allocate temporaries in FrameworkNameProvider.GetVersionString
  * [5271](https://github.com/NuGet/NuGet.Client/pull/5271) Remove allocations from PackageSource.Source setter
* [MichaelSimons](https://github.com/NuGet/NuGet.Client/pull/5418)
  * [5418](https://github.com/NuGet/NuGet.Client/pull/5418) Fix source-build CI regression
  * [5414](https://github.com/NuGet/NuGet.Client/pull/5414) Remove unnecessary source-build patch
* [mthalman](https://github.com/NuGet/NuGet.Client/pull/5385)
  * [5385](https://github.com/NuGet/NuGet.Client/pull/5385) Update Newtonsoft.Json from 13.0.1 to 13.0.3
* [timheuer](https://github.com/NuGet/NuGet.Client/pull/5375)
  * [5375](https://github.com/NuGet/NuGet.Client/pull/5375) Update VS Options add/remove package source icons to VS2022 styling
* [dotnokato](https://github.com/NuGet/NuGet.Client/pull/5002)
  * [5002](https://github.com/NuGet/NuGet.Client/pull/5002) CLI: Add -protocolVersion option to nuget sources add/update commands
* [oleksandr-didyk](https://github.com/NuGet/NuGet.Client/pull/5352)
  * [5352](https://github.com/NuGet/NuGet.Client/pull/5352) allow empty sb intermediate
* [drolevar](https://github.com/NuGet/NuGet.Client/pull/5346)
  * [5346](https://github.com/NuGet/NuGet.Client/pull/5346) Add .vdproj to the exclusion list
* [Greybird](https://github.com/NuGet/NuGet.Client/pull/5335)
  * [5335](https://github.com/NuGet/NuGet.Client/pull/5335) Remove projects from list package output
* [NikolaMilosavljevic](https://github.com/NuGet/NuGet.Client/pull/5322)
  * [5322](https://github.com/NuGet/NuGet.Client/pull/5322) Fix incorrect package version property for System.Security.Cryptograp…
* [vishavpandhi](https://github.com/NuGet/NuGet.Client/pull/5283)
  * [5283](https://github.com/NuGet/NuGet.Client/pull/5283) [DartLab B2B feature] dropname for base VS should be retrieved using the baseline.
* [v-chayan](https://github.com/NuGet/NuGet.Client/pull/5278)
  * [5278](https://github.com/NuGet/NuGet.Client/pull/5278) Remove redundant SourceBuildTrimNetFrameworkTargets property
* [marcin-krystianc](https://github.com/NuGet/NuGet.Client/pull/5293)
  * [5293](https://github.com/NuGet/NuGet.Client/pull/5293) DetectAndMarkAmbiguousCentralTransitiveDependencies should be exhaustive and deterministic
* [Erarndt](https://github.com/NuGet/NuGet.Client/pull/5218)
  * [5218](https://github.com/NuGet/NuGet.Client/pull/5218) Reduce some allocations in CreateGraphNode.
