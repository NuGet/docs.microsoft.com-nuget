---
title: NuGet 6.11 Release Notes
description: Release notes for NuGet 6.11 including new features, bug fixes, and DCRs.
author: mruizmares
ms.date: 8/1/2024
ms.topic: conceptual
---

# NuGet 6.11 Release Notes

NuGet distribution vehicles:

| NuGet version | Available in Visual Studio version | Available in .NET SDK(s) |
|:---|:---|:---|
| [**6.11**](https://nuget.org/downloads) | [Visual Studio 2022 version 17.11](https://visualstudio.microsoft.com/downloads/) | [<TODO: Full SDK Version>](https://dotnet.microsoft.com/download/dotnet/<TODO: SDKMajorMinorVersionOnly)<sup>1</sup> |

<sup>1</sup> Installed with Visual Studio 2022 with any .NET workload

## Summary: What's New in 6.11

* [Feature]: add dotnet nuget why to dotnet CLI - [#11943](https://github.com/NuGet/Home/issues/11943)

### Breaking changes

### Issues fixed in this release

* [DCR]: NuGet causes many ArgumentExceptions to be thrown & caught in VS - [#11535](https://github.com/NuGet/Home/issues/11535)

* Remove .NET Framework TFM from NuGet.CommandLine.XPlat - [#8452](https://github.com/NuGet/Home/issues/8452)

* Enable Nullable and throw in KnownOwnerViewModel - [#13425](https://github.com/NuGet/Home/issues/13425)

* `PackageSourceProvider.UpdatePackageSource` doesn't respect a value from `disableTLSCertificateValidation` attribute - [#13423](https://github.com/NuGet/Home/issues/13423)

[List of commits in this release](https://github.com/NuGet/NuGet.Client/compare/6.11.0.122...6.10.1.5)

### Community contributions

Thank you to all the contributors who helped make this NuGet release awesome!

* [seclerp](https://github.com/NuGet/NuGet.Client/pull/5783)
  * [5783](https://github.com/NuGet/NuGet.Client/pull/5783) Fix `PackageSourceProvider.UpdatePackageSource` doesn't respect a value from `DisableTLSCertificateValidation` field
  * [5767](https://github.com/NuGet/NuGet.Client/pull/5767) Fix `PackageSourceProvider.UpdatePackageSource` doesn't respect a value from `AllowInsecureConnection` field
* [mmitche](https://github.com/NuGet/NuGet.Client/pull/5749)
  * [5749](https://github.com/NuGet/NuGet.Client/pull/5749) Build NuGet from the VMR
  * [5752](https://github.com/NuGet/NuGet.Client/pull/5752) Fixup signing support conditional
* [ToddGrun](https://github.com/NuGet/NuGet.Client/pull/5859)
  * [5859](https://github.com/NuGet/NuGet.Client/pull/5859) Reduce allocations in GetPackageInfo by conditionally calling GetNupkgMetadataPath
* [omajid](https://github.com/NuGet/NuGet.Client/pull/5848)
  * [5848](https://github.com/NuGet/NuGet.Client/pull/5848) Dispose certificates on failure in SignatureUtility.GetCertificates
* [SimonCropp](https://github.com/NuGet/NuGet.Client/pull/5842)
  * [5842](https://github.com/NuGet/NuGet.Client/pull/5842) remove redundant .GetTypeInfo()
* [ryanmolden](https://github.com/NuGet/NuGet.Client/pull/5834)
  * [5834](https://github.com/NuGet/NuGet.Client/pull/5834) Eliminate first-chance ArgumentExceptions when querying a legacy project for properties that don't exist via DTE
* [hickford](https://github.com/NuGet/NuGet.Client/pull/5743)
  * [5743](https://github.com/NuGet/NuGet.Client/pull/5743) Correct message in the case that a higher version of package is already installed
* [ViktorHofer](https://github.com/NuGet/NuGet.Client/pull/5764)
  * [5764](https://github.com/NuGet/NuGet.Client/pull/5764) Use .NET SDK sourcelink integration
* [NikolaMilosavljevic](https://github.com/NuGet/NuGet.Client/pull/5738)
  * [5738](https://github.com/NuGet/NuGet.Client/pull/5738) Disable CA2022 errors
* [jv42](https://github.com/NuGet/NuGet.Client/pull/5717)
  * [5717](https://github.com/NuGet/NuGet.Client/pull/5717) Fixed NullReferenceException in ResolverComparer
