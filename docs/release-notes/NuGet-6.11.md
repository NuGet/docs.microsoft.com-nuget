---
title: NuGet 6.11 Release Notes
description: Release notes for NuGet 6.11 including new features, bug fixes, and DCRs.
author: martinrrm
ms.date: 8/13/2024
ms.topic: conceptual
---
# NuGet 6.11 Release Notes

NuGet distribution vehicles:

| NuGet version | Available in Visual Studio version | Available in .NET SDK(s) |
|:---|:---|:---|
| [**6.11**](https://nuget.org/downloads) | [Visual Studio 2022 version 17.11](https://visualstudio.microsoft.com/downloads/) | [8.0.4xx](https://dotnet.microsoft.com/download/dotnet/8.0)<sup>1</sup> |

<sup>1</sup> Installed with Visual Studio 2022 with any .NET workload

## Summary: What's New in 6.11

* Suppress NuGetAudit warnings for specific advisories for PackageReference projects - [#13679](https://github.com/NuGet/Home/issues/13679)

* Add `--allow-insecure-connections` option to dotnet SDK - [#13396](https://github.com/NuGet/Home/issues/13396)

* Swap authors for owners in Visual Studio Package Manager UI - [#12501](https://github.com/NuGet/Home/issues/12501)

* [Feature]: add dotnet nuget why to dotnet CLI - [#11943](https://github.com/NuGet/Home/issues/11943)

* NuGet cannot restore from HTTPS sources that have SSL certificate problems - [#4387](https://github.com/NuGet/Home/issues/4387)

### Breaking changes

* `MSBuildRestoreUtility.GetRestoreAuditProperties` needs a breaking change to read `NuGetAuditSuppress` items - [#13313](https://github.com/NuGet/Home/issues/13313)

* NuGetAudit should check transitive packages by default when the .NET 9 SDK is installed - [#13293](https://github.com/NuGet/Home/issues/13293)

### Issues fixed in this release

* IVsNuGetProjectUpdateEvents.ProjectUpdateStarted or ProjectUpdateFinished should only provide a list of files that will get changed. - [#13413](https://github.com/NuGet/Home/issues/13413)

* `dotnet nuget` commands should not output usage information on all errors - [#13251](https://github.com/NuGet/Home/issues/13251)

* [DCR]: NuGet causes many ArgumentExceptions to be thrown & caught in VS - [#11535](https://github.com/NuGet/Home/issues/11535)

* Remove .NET Framework TFM from NuGet.CommandLine.XPlat - [#8452](https://github.com/NuGet/Home/issues/8452)

* PERF: GetPackageInfo allocates by unnecessarily calling GetNupkgMetadataPath - [#13556](https://github.com/NuGet/Home/issues/13556)

* 'dotnet nuget why' crashes when using --framework option - [#13547](https://github.com/NuGet/Home/issues/13547)

* SignatureUtility.GetCertificates in NuGet.Client can skip calling Dispose on error - [#13535](https://github.com/NuGet/Home/issues/13535)

* 'dotnet nuget why' does not work when a directory is provided for the 'Path' argument - [#13527](https://github.com/NuGet/Home/issues/13527)

* NuGet IntelliCode Package Suggestions are missing Author in packages list - [#13515](https://github.com/NuGet/Home/issues/13515)

* Rebuilding in VS causes unnecessary restores - [#13505](https://github.com/NuGet/Home/issues/13505)

* MSB4181: The "Restore Task" task returned false but did not log an error. - [#13460](https://github.com/NuGet/Home/issues/13460)

* TaskResultCache incorrectly shares the same lock object for all the keys. - [#13448](https://github.com/NuGet/Home/issues/13448)

* Calls to CompareTo and Equals should not allocate - [#13442](https://github.com/NuGet/Home/issues/13442)

* Enable Nullable and throw in KnownOwnerViewModel - [#13425](https://github.com/NuGet/Home/issues/13425)

* `PackageSourceProvider.UpdatePackageSource` doesn't respect a value from `disableTLSCertificateValidation` attribute - [#13423](https://github.com/NuGet/Home/issues/13423)

* `PackageSourceProvider.UpdatePackageSource` doesn't respect a value from `AllowInsecureConnection` field - [#13418](https://github.com/NuGet/Home/issues/13418)

* "nuget install -ExcludeVersion" inaccurate reports "already installed" when trying to install older version - [#13334](https://github.com/NuGet/Home/issues/13334)

* Vulnerability InfoBar remains visible in the Solution Explorer after closing solution - [#13055](https://github.com/NuGet/Home/issues/13055)

[List of commits in this release](https://github.com/NuGet/NuGet.Client/compare/6.11.0.122...6.10.1.5)

### Community contributions

Thank you to all the contributors who helped make this NuGet release awesome!

* [seclerp](https://github.com/seclerp)
  * [5783](https://github.com/NuGet/NuGet.Client/pull/5783) Fix `PackageSourceProvider.UpdatePackageSource` doesn't respect a value from `DisableTLSCertificateValidation` field
  * [5767](https://github.com/NuGet/NuGet.Client/pull/5767) Fix `PackageSourceProvider.UpdatePackageSource` doesn't respect a value from `AllowInsecureConnection` field
* [mmitche](https://github.com/mmitche)
  * [5749](https://github.com/NuGet/NuGet.Client/pull/5749) Build NuGet from the VMR
  * [5752](https://github.com/NuGet/NuGet.Client/pull/5752) Fixup signing support conditional
* [ToddGrun](https://github.com/ToddGrun)
  * [5859](https://github.com/NuGet/NuGet.Client/pull/5859) Reduce allocations in GetPackageInfo by conditionally calling GetNupkgMetadataPath
* [omajid](https://github.com/omajid)
  * [5848](https://github.com/NuGet/NuGet.Client/pull/5848) Dispose certificates on failure in SignatureUtility.GetCertificates
* [SimonCropp](https://github.com/SimonCropp)
  * [5842](https://github.com/NuGet/NuGet.Client/pull/5842) remove redundant .GetTypeInfo()
* [ryanmolden](https://github.com/ryanmolden)
  * [5834](https://github.com/NuGet/NuGet.Client/pull/5834) Eliminate first-chance ArgumentExceptions when querying a legacy project for properties that don't exist via DTE
* [hickford](https://github.com/hickford)
  * [5743](https://github.com/NuGet/NuGet.Client/pull/5743) Correct message in the case that a higher version of package is already installed
* [ViktorHofer](https://github.com/ViktorHofer)
  * [5764](https://github.com/NuGet/NuGet.Client/pull/5764) Use .NET SDK sourcelink integration
* [NikolaMilosavljevic](https://github.com/NikolaMilosavljevic)
  * [5738](https://github.com/NuGet/NuGet.Client/pull/5738) Disable CA2022 errors
* [jv42](https://github.com/jv42)
  * [5717](https://github.com/NuGet/NuGet.Client/pull/5717) Fixed NullReferenceException in ResolverComparer
