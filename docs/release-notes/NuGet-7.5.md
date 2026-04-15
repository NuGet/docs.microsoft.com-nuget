---
title: NuGet 7.5 Release Notes
description: Release notes for NuGet 7.5 including new features, bug fixes, and DCRs.
author: kartheekp-ms
ms.date: 03/06/2026
ms.topic: release-notes
---

# NuGet 7.5 Release Notes

NuGet distribution vehicles:

| NuGet version | Available in Visual Studio version | Available in .NET SDK(s) |
|:---|:---|:---|
| [**7.5.0**](https://nuget.org/downloads) | [Visual Studio 2026 version 18.5.0](https://visualstudio.microsoft.com/downloads/) | N/A |

## Summary: What's New in 7.5.0

* Ensure PM UI and PMC updating support aliased package changes - [#14539](https://github.com/NuGet/Home/issues/14539)

### Issues fixed in this release

* Ensure PM UI and PMC updating support aliased package changes - [#14539](https://github.com/NuGet/Home/issues/14539)
  * Change the assets file format to support aliasing - [#14534](https://github.com/NuGet/Home/issues/14534)
  * Ensure PackagesLockFile supports aliases - [#14538](https://github.com/NuGet/Home/issues/14538)
  * Ensure the .NET SDK can handle aliased assets file - [#14536](https://github.com/NuGet/Home/issues/14536)
  * Use .NET 11 preview 2 SDK for aliasing opt-in - [#7121](https://github.com/NuGet/NuGet.Client/pull/7121)

* Add net11.0 to FrameworkConstants.CommonFrameworks - [#14759](https://github.com/NuGet/Home/issues/14759)

* NullReferenceException in package pruning when using CPM and a version is not specified on a package - [#14749](https://github.com/NuGet/Home/issues/14749)

* NugetProjectServiceV1 brokered service is not usable from out-of-proc consumers - [#14732](https://github.com/NuGet/Home/issues/14732)

* [Bug Bash] The machine-wide package source should be shown immediately under the ‘Package Sources’ part after deleting all the package sources from ‘Options-&gt;NuGet Package Manager-&gt;Package Sources’ - [#14693](https://github.com/NuGet/Home/issues/14693)

* Make NuGet AOT compatible (InProgress) - [#14408](https://github.com/NuGet/Home/issues/14408)
  * AOT Compatible: ManifestVersionUtility - [#7128](https://github.com/NuGet/NuGet.Client/pull/7128)
  * AOT Compatible: Add overrideVersion to enforce -Version precedence - [#7133](https://github.com/NuGet/NuGet.Client/pull/7133)
  * AOT compatible: NuGet.DependencyResolver.Core - [#7138](https://github.com/NuGet/NuGet.Client/pull/7138)
  * AOT compatible: NuGet.LibraryModel - [#7140](https://github.com/NuGet/NuGet.Client/pull/7140)
  * AOT compatible: NuGet.PackageManagement - [#7141](https://github.com/NuGet/NuGet.Client/pull/7141)
  * AOT compatible: NuGet.Resolver - [#7142](https://github.com/NuGet/NuGet.Client/pull/7142)
  * AOT compatible: NuGet.Credentials - [#7143](https://github.com/NuGet/NuGet.Client/pull/7143)

* Improve readability as a follow up to e302692 - [#7107](https://github.com/NuGet/NuGet.Client/pull/7107)

* Reduce memory allocations when creating SourceRepository instances - [#7126](https://github.com/NuGet/NuGet.Client/pull/7126)

* Update schema version determination logic - [#7131](https://github.com/NuGet/NuGet.Client/pull/7131)

* Remove dead code from PackageSpecReferenceDependencyProvider and use PackageSpec TargetFrameworkInformation in GetReferenceNearestTargetFrameworkTask - [#7065](https://github.com/NuGet/NuGet.Client/pull/7065)

* Security Advisory | Defense in Depth update for NuGet Client - [14857](https://github.com/NuGet/Home/issues/14857)

[List of commits in this release](https://github.com/NuGet/NuGet.Client/compare/7.3.0.78...7.5.0.55)

### Community contributions

Thank you to all the contributors who helped make this NuGet release awesome!

* [nareshjo](https://github.com/nareshjo)
  * [7132](https://github.com/NuGet/NuGet.Client/pull/7132) Avoid redundant SourceRepository allocation on cache hit in CachingSourceProvider
  * [7089](https://github.com/NuGet/NuGet.Client/pull/7089) Reduce allocations in `ResolverMetadataClient.ProcessPackageVersion` by avoiding JArray enumerator allocations
  * [7095](https://github.com/NuGet/NuGet.Client/pull/7095) Pre-size List in TryGetPortableFrameworks to avoid array resize allocations
  * [7086](https://github.com/NuGet/NuGet.Client/pull/7086) Reduce allocations in JsonRuntimeFormat.ReadRuntimeDescription by pre-sizing List
  * [7083](https://github.com/NuGet/NuGet.Client/pull/7083) Reduce List resize allocations in PopulateItemGroups hot path
  * [7056](https://github.com/NuGet/NuGet.Client/pull/7056) Avoid LineInfoAnnotation allocations in MetadataFieldConverter JSON parsing
  * [7050](https://github.com/NuGet/NuGet.Client/pull/7050) Reduce allocation in GetNuGetProjects by providing capacity for List
  * [7037](https://github.com/NuGet/NuGet.Client/pull/7037) Reduce allocation in GetContentFileGroup by providing accurate capacity for List
  * [7035](https://github.com/NuGet/NuGet.Client/pull/7035) Reduce allocation in DownloadTimeoutStream.ReadAsync caused by avoiding unconditional string.Format allocation
  * [7026](https://github.com/NuGet/NuGet.Client/pull/7026) Fix list capacity calculation in GetGraphItemAsync that is leading to allocations
* [viceice](https://github.com/NuGet/viceice)
  * [7145](https://github.com/NuGet/NuGet.Client/pull/7145) Handle empty paths when locating extensions