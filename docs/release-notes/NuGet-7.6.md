---
title: NuGet 7.6 Release Notes
description: Release notes for NuGet 7.6 including new features, bug fixes, and DCRs.
author: jeffkl
ms.date: 4/28/2026
ms.topic: release-notes
---

# NuGet 7.6 Release Notes

NuGet distribution vehicles:

| NuGet version | Available in Visual Studio version | Available in .NET SDK(s) |
|:---|:---|:---|
| [**7.6.0**](https://nuget.org/downloads) | [Visual Studio 2026 version 18.6.0](https://visualstudio.microsoft.com/downloads/) | [10.0.300](https://dotnet.microsoft.com/download/dotnet/10.0)<sup>1</sup> |

<sup>1</sup> Installed with Visual Studio 2026 with any .NET workload

## Summary: What's New in 7.6.0

* Get push API key from environment variable - [#12539](https://github.com/NuGet/Home/issues/12539)

### Issues fixed in this release

* Show certificate CRL and OCSP URLs in `dotnet nuget verify` output - [#14780](https://github.com/NuGet/Home/issues/14780)

* Add package and update package treat framework as an alias - [#14540](https://github.com/NuGet/Home/issues/14540)

* Treat TargetFramework(s) values as aliases - [#5154](https://github.com/NuGet/Home/issues/5154)

* Context menu on Search control in PM UI is missing VS color theming - [#14799](https://github.com/NuGet/Home/issues/14799)

* NugetProjectServiceV1 brokered service is not usable from out-of-proc consumers - [#14732](https://github.com/NuGet/Home/issues/14732)

* No vulns shown for vulnerable &amp; deprecated package version when using dotnet list package --vulnerable - [#14477](https://github.com/NuGet/Home/issues/14477)

[List of commits in this release](https://github.com/NuGet/NuGet.Client/compare/7.5.0.55...7.6.0.59)

### Community contributions

Thank you to all the contributors who helped make this NuGet release awesome!

* [nareshjo](https://github.com/NuGet/NuGet.Client/pull/7237)
  * [7237](https://github.com/NuGet/NuGet.Client/pull/7237) Reduce allocations in LicenseExpressionTokenizer.HasValidCharacters by caching Regex instance
  * [7174](https://github.com/NuGet/NuGet.Client/pull/7174) Resolve dependency graph items reduce allocation size
* [jjonescz](https://github.com/NuGet/NuGet.Client/pull/7233)
  * [7233](https://github.com/NuGet/NuGet.Client/pull/7233) Make virtual project builder parameter required in MSBuildAPIUtility
  * [7169](https://github.com/NuGet/NuGet.Client/pull/7169) Add support for file-based apps to the XPlat CLI
* [SimonCropp](https://github.com/NuGet/NuGet.Client/pull/7224)
  * [7224](https://github.com/NuGet/NuGet.Client/pull/7224) Use Ordinal string comparison for TargetAlias
* [elantiguamsft](https://github.com/NuGet/NuGet.Client/pull/7201)
  * [7201](https://github.com/NuGet/NuGet.Client/pull/7201) Add --allow-untrusted-root flag to nuget sign and dotnet nuget sign
* [slang25](https://github.com/NuGet/NuGet.Client/pull/7148)
  * [7148](https://github.com/NuGet/NuGet.Client/pull/7148) Fix `dotnet add package --no-restore` ignoring Central Package Management

