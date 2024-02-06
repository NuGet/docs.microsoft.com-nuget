---
title: NuGet 6.9 Release Notes
description: Release notes for NuGet 6.9 including new features, bug fixes, and DCRs.
author: jgonz120
ms.author: jongonza
ms.date: 2/1/2024
ms.topic: conceptual
---

# NuGet 6.9 Release Notes

NuGet distribution vehicles:

| NuGet version | Available in Visual Studio version | Available in .NET SDK(s) |
|:---|:---|:---|
| [**6.9**](https://nuget.org/downloads) | [Visual Studio 2022 version 17.9](https://visualstudio.microsoft.com/downloads/) | [8.0.200](https://dotnet.microsoft.com/download/dotnet/8.0)<sup>1</sup> |

<sup>1</sup> Installed with Visual Studio 2022 with any .NET workload

## Summary: What's New in 6.9

* Support for dotnet search command (equivalent to nuget.exe list, later search) - [#6060](https://github.com/NuGet/Home/issues/6060) [#5138](https://github.com/NuGet/Home/issues/5138)

* PM UI multi targeting experience is incomplete - [#4681](https://github.com/NuGet/Home/issues/4681)

### Breaking changes

* Add nullable annotations to NuGet.LibraryModel - [#12889](https://github.com/NuGet/Home/issues/12889)

### Issues fixed in this release

* NuGetAudit should not download vulnerabilities database when project does not use any packages - [#13073](https://github.com/NuGet/Home/issues/13073)

* Static graph-based restore should not enumerate every item's metadata - [#13049](https://github.com/NuGet/Home/issues/13049)

* Migrate NuGet.CommandLine.XPlat package search to use System.CommandLine - [#13031](https://github.com/NuGet/Home/issues/13031)

* Add `--format`, `--verbosity` , `configfile` options to `dotnet package search` - [#12978](https://github.com/NuGet/Home/issues/12978)

* Set NuGetAudit defaults in MSBuild - [#12960](https://github.com/NuGet/Home/issues/12960)

* RestoreTask: Control whether to embed files in the binlog - [#12957](https://github.com/NuGet/Home/issues/12957)

* Create NU Error Code for Package Source Mapping & GPF conflicts - [#12953](https://github.com/NuGet/Home/issues/12953)

* [DCR]: Allow floating versions with Central Package Management (CPM) - [#10432](https://github.com/NuGet/Home/issues/10432)

* Static Graph restore won't warn for invalid projects - [#9300](https://github.com/NuGet/Home/issues/9300)

* Rename no-cache to no-http-cache - [#9180](https://github.com/NuGet/Home/issues/9180)

* VS Package Manager Console should close Text View - [#13104](https://github.com/NuGet/Home/issues/13104)

* Vulnerability indicator shows up on dependent project if parent project has transitive vulnerabilities - [#13068](https://github.com/NuGet/Home/issues/13068)

* Conditional updating when *all*  packages are conditioned is broken - [#13034](https://github.com/NuGet/Home/issues/13034)

* Perf issue in AssetsFileDependenciesDataSource - [#13019](https://github.com/NuGet/Home/issues/13019)

* The `NuGetPackageSearchService.GetDeprecationMetadataAsync` in `NuGet.PackageManagement.VisualStudio` is dead code - [#13007](https://github.com/NuGet/Home/issues/13007)

* Vulnerabilities filter shows incorrectly on “Brower” tab when the default tab was “Browse” instead of “Installed” for the previous opening of solution PM UI   - [#12974](https://github.com/NuGet/Home/issues/12974)

* HTTP 401 after some time in VS - [#12961](https://github.com/NuGet/Home/issues/12961)

* [NuGet.Versioning] SemanticVersion.HasMetadata should indicate that Metadata is not null when true - [#12949](https://github.com/NuGet/Home/issues/12949)

* TelemetryUtility.ToJsonArrayOfTimingsInSeconds returns incorrect json array on locales having comma as decimal separator - [#12915](https://github.com/NuGet/Home/issues/12915)

* Static graph-based restore does not respect Interactive option when loading projects - [#12907](https://github.com/NuGet/Home/issues/12907)

* Vulnerabilities InfoBar link to `Manage NuGet Packages` is truncated - [#12835](https://github.com/NuGet/Home/issues/12835)

* NuGet.Build.Tasks caches CredentialProvider device flow timeout. - [#12540](https://github.com/NuGet/Home/issues/12540)

* "error: Sequence contains no matching element" when listing outdated packages - [#12256](https://github.com/NuGet/Home/issues/12256)

* [Bug]: Process argument string is too long when publishing in Visual Studio with static graph enabled - [#11968](https://github.com/NuGet/Home/issues/11968)

* [Bug]: PM UI cannot uninstall packages in multitargeting projects - [#11914](https://github.com/NuGet/Home/issues/11914)

* When a package is installed in the global packages folder, add details about the package location - [#11447](https://github.com/NuGet/Home/issues/11447)

* NuGet should handle duplicate nomination data better.  - [#8749](https://github.com/NuGet/Home/issues/8749)

[List of commits in this release](https://github.com/NuGet/NuGet.Client/compare/6.9.0.74...6.8.0.131)

### Community contributions

Thank you to all the contributors who helped make this NuGet release awesome!

* [KirillOsenkov](https://github.com/NuGet/NuGet.Client/pull/5494)
  * [5494](https://github.com/NuGet/NuGet.Client/pull/5494) Control embedding Restore files in binlog
  * [5498](https://github.com/NuGet/NuGet.Client/pull/5498) Don't log task inputs and outputs when binary logger is enabled
* [Erarndt](https://github.com/NuGet/NuGet.Client/pull/5535)
  * [5535](https://github.com/NuGet/NuGet.Client/pull/5535) Unroll Linq usage in FilterDependencyProvidersForLibrary
  * [5531](https://github.com/NuGet/NuGet.Client/pull/5531) Reduce allocations in calls to CreateGraphNode()
* [dotnokato](https://github.com/NuGet/NuGet.Client/pull/5442)
  * [5442](https://github.com/NuGet/NuGet.Client/pull/5442) Fix tests failing when run on systems with non-English language/locale settings
  * [5441](https://github.com/NuGet/NuGet.Client/pull/5441) Fix incorrect json array returned for locales with comma as decimal separator in TelemetryUtility.ToJsonArrayOfTimingsInSeconds
* [ellahathaway](https://github.com/NuGet/NuGet.Client/pull/5543)
  * [5543](https://github.com/NuGet/NuGet.Client/pull/5543) Shorten source-build inner clone paths
* [jasonmalinowski](https://github.com/NuGet/NuGet.Client/pull/5533)
  * [5533](https://github.com/NuGet/NuGet.Client/pull/5533) Output a more debuggable message if a single value isn't specified
* [mthalman](https://github.com/NuGet/NuGet.Client/pull/5511)
  * [5511](https://github.com/NuGet/NuGet.Client/pull/5511) Target net9.0 for .NET source build
* [NikolaMilosavljevic](https://github.com/NuGet/NuGet.Client/pull/5496)
  * [5496](https://github.com/NuGet/NuGet.Client/pull/5496) Eliminate obsolete API warnings/errors in product source-build
* [amis92](https://github.com/NuGet/NuGet.Client/pull/5465)
  * [5465](https://github.com/NuGet/NuGet.Client/pull/5465) Add MemberNotNullWhen to SemanticVersion.HasMetadata