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

* Treat TargetFramework(s) values as aliases - [#5154](https://github.com/NuGet/Home/issues/5154)
  * This feature enables building for the same framework multiple times, enabling scenarios such as generating runtime specific assemblies for the same target framework, as well as making running benchmarks on different versions of the same package easier.
  * [Learn more about TargetFramework aliases](/nuget/reference/target-frameworks#targetframework-values-are-aliases)

* Pack is aliased-framework-aware - [#14751](https://github.com/NuGet/Home/issues/14751)
  * When a project has multiple TargetFramework aliases that resolve to the same framework, `dotnet pack` now detects the ambiguity and raises NU5051 with an actionable error message instead of producing unexpected output.

* Package management APIs for file-based apps - [#14390](https://github.com/NuGet/Home/issues/14390)
  * NuGet now exposes APIs that `dotnet package add`, `list`, `remove`, and `update` use for file-based apps that reference packages with `#:package` directives in C# source files.

* Read push API key from environment variable - [#12539](https://github.com/NuGet/Home/issues/12539)
  * `dotnet nuget push` can now read the API key from an environment variable, avoiding the need to pass secrets on the command line or store them in configuration files.

### Issues fixed in this release

* `nuget push` sends CI platform in user-agent header - [#14740](https://github.com/NuGet/Home/issues/14740)
  * `nuget.exe push` now includes the CI platform (Azure DevOps, GitHub Actions, and others) in the HTTP user-agent header, allowing package sources to identify where pushes originate.

* `dotnet add package --no-restore` with Central Package Management no longer produces NU1008 - [#12552](https://github.com/NuGet/Home/issues/12552)
  * When using Central Package Management, `dotnet add package --no-restore` now correctly adds the `PackageReference` without a `Version` attribute instead of producing a restore error.

* Fix `Add-Migration` error in Package Manager Console - [#14862](https://github.com/NuGet/Home/issues/14862)
  * Running `Add-Migration` in the NuGet Package Manager Console no longer throws a "GetProjectFromHierarchy must be called on the UI thread" error.

* `NuGetAuditSuppress` with packages.config now supports multiple suppressions - [#14825](https://github.com/NuGet/Home/issues/14825)
  * Previously, only the first `NuGetAuditSuppress` entry was honored in packages.config projects. All suppressions are now applied correctly.

* Fix context menu theming on Package Manager UI search box - [#14799](https://github.com/NuGet/Home/issues/14799)
  * The right-click context menu on the search control in the NuGet Package Manager UI now follows the Visual Studio color theme.

* Fix NuGetProjectServiceV1 for out-of-process consumers - [#14732](https://github.com/NuGet/Home/issues/14732)
  * The `NuGetProjectServiceV1` brokered service now uses the correct serialization settings, making it usable from out-of-process Visual Studio extensions.

* Fix context menu theming on Package Manager UI copy menus - [#14704](https://github.com/NuGet/Home/issues/14704)
  * The right-click copy context menus in the Package Manager UI Package Details tab now follow the Visual Studio color theme.

* `dotnet list package --vulnerable` now shows vulnerabilities for deprecated packages - [#14477](https://github.com/NuGet/Home/issues/14477)
  * Previously, vulnerability information was not displayed for package versions that were both vulnerable and deprecated. Both statuses are now reported.

* `dotnet list package` resolves conditional TargetFramework values correctly - [#14339](https://github.com/NuGet/Home/issues/14339)
  * `dotnet list package` no longer fails when a project uses a TargetFramework property value that matches a real framework moniker, such as `net9.0-windows` with conditional `PackageReference` elements.

* Improved NU1107 error message with Central Package Management and transitive pinning - [#12277](https://github.com/NuGet/Home/issues/12277)
  * The NU1107 version conflict error now provides relevant guidance when Central Package Management with transitive pinning is enabled, instead of suggesting actions that don't apply in that configuration.

* Fix NU1004 for cross-framework references with locked mode - [#12010](https://github.com/NuGet/Home/issues/12010)
  * Restoring with `--locked-mode` no longer produces a false NU1004 error when a .NET project references a .NET Framework project.

* Fix intermittent "Failed to resolve SDK" error during parallel restores - [#10935](https://github.com/NuGet/Home/issues/10935)
  * Parallel `dotnet` restore processes no longer intermittently fail with "Failed to resolve SDK" when the package is already installed in the global packages folder.

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
