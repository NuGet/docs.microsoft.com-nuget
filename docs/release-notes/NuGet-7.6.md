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

* Pack with aliased frameworks should allow combining build outputs from the same framework into a single pack - [#14751](https://github.com/NuGet/Home/issues/14751)

* [Design spec][OIDC] Enrich telemetry for nuget.exe push to include about where the push is from (Azure DevOps, GitHub Actions, etc) - [#14740](https://github.com/NuGet/Home/issues/14740)

* APIs for package management of file-based apps - [#14390](https://github.com/NuGet/Home/issues/14390)

* Get push API key from environment variable - [#12539](https://github.com/NuGet/Home/issues/12539)

### Issues fixed in this release

* List package should use TargetFramework property as a key it its output instead of the effective TargetFramework; Introduce a V2 version of the list package output - [#14762](https://github.com/NuGet/Home/issues/14762)

* Block aliases with path separator - [#14752](https://github.com/NuGet/Home/issues/14752)

* Add package and update package treat framework as an alias - [#14540](https://github.com/NuGet/Home/issues/14540)

* [CPM] dotnet add package with --no-restore causes NU1008 - [#12552](https://github.com/NuGet/Home/issues/12552)

* Treat TargetFramework(s) values as aliases - [#5154](https://github.com/NuGet/Home/issues/5154)

* Error while using Add-Migration in  nuget console in Visual Studio Insiders 18.6 - [#14862](https://github.com/NuGet/Home/issues/14862)

* `&lt;NuGetAuditSuppress&gt;` with packages.config projects doesn&#39;t work for more than one suppression - [#14825](https://github.com/NuGet/Home/issues/14825)

* GetReferenceFrameworkNearestTask does not use aliases for disambiguation when using AssetTargetFallback - [#14807](https://github.com/NuGet/Home/issues/14807)

* Aliasing opt-in is broken due to a versioning change in .NET 10.0.3xx - [#14805](https://github.com/NuGet/Home/issues/14805)

* Context menu on Search control in PM UI is missing VS color theming - [#14799](https://github.com/NuGet/Home/issues/14799)

* When using aliased frameworks, package conditions are not generated correctly - [#14796](https://github.com/NuGet/Home/issues/14796)

* NugetProjectServiceV1 brokered service is not usable from out-of-proc consumers - [#14732](https://github.com/NuGet/Home/issues/14732)

* Context menus to Copy text from PM UI are missing VS color theming - [#14704](https://github.com/NuGet/Home/issues/14704)

* Ensure seamless experience with aliasing when packing - [#14535](https://github.com/NuGet/Home/issues/14535)

* No vulns shown for vulnerable &amp; deprecated package version when using dotnet list package --vulnerable - [#14477](https://github.com/NuGet/Home/issues/14477)

* `dotnet list package` invalid TFM resolution; does not work when TargetFramework property matches real framework. - [#14339](https://github.com/NuGet/Home/issues/14339)

* NU1107 message provides poor guidance when CPVM + transitive pinning is enabled - [#12277](https://github.com/NuGet/Home/issues/12277)

* [Bug]: NU1004: The project Xxx has no compatible target framework for .NET vs .NET Framework cross reference with -LockedMode - [#12010](https://github.com/NuGet/Home/issues/12010)

* Random error: Failed to resolve SDK &#39;package/version&#39;. Package restore was successful but a package with the ID of &quot;package&quot; was not installed. - [#10935](https://github.com/NuGet/Home/issues/10935)

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
