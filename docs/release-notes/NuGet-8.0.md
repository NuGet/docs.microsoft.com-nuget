---
title: NuGet 8.0 Release Notes
description: Release notes for NuGet 8.0 including new features, bug fixes, and DCRs.
author: jebriede
ai-usage: ai-generated
ms.date: 11/06/2026
ms.topic: release-notes
---

# NuGet 8.0 Release Notes

NuGet distribution vehicles:

| NuGet version | Available in Visual Studio version | Available in .NET SDK(s) |
|:---|:---|:---|
| [**8.0.0**](https://nuget.org/downloads) | [Visual Studio 2026 version 18.12.0](https://visualstudio.microsoft.com/downloads/) | [11.0.100](https://dotnet.microsoft.com/download/dotnet/11.0)<sup>1</sup> |

<sup>1</sup> Installed with Visual Studio 2026 with any .NET workload

## Summary: What's New in 8.0.0

* Add `dotnet nuget stage push` to upload packages and symbols to a staging service - [#15083](https://github.com/NuGet/Home/issues/15083)

* Review package supply-chain security with Copilot from the Visual Studio Error List - [#15049](https://github.com/NuGet/Home/issues/15049)

* Record filtered analyzer assets in `project.assets.json` with opt-in `RestoreEnableAnalyzerAssets` for .NET 11+ projects - [#6279](https://github.com/NuGet/Home/issues/6279)

### Breaking changes

* Add nullable annotations to the following libraries, which can introduce compiler warnings or errors in consuming projects:
  * `NuGet.Resolver` - [#15069](https://github.com/NuGet/Home/issues/15069)
  * `NuGet.DependencyResolver.Core` - [#15060](https://github.com/NuGet/Home/issues/15060)
  * `NuGet.Credentials` - [#15012](https://github.com/NuGet/Home/issues/15012)

* Replace the .NET 8 target with .NET 10 in NuGet client libraries. Retain the .NET Framework 4.7.2 target - [#15062](https://github.com/NuGet/Home/issues/15062)

* Warn with NU5052 for non-standard package IDs in the following pack flows:
  * Classic `msbuild /t:pack` and `nuget.exe pack` - [#15013](https://github.com/NuGet/Home/issues/15013)
  * SDK pack when `SdkAnalysisLevel` is `11.0.100` or later - [#14949](https://github.com/NuGet/Home/issues/14949)

### Issues fixed in this release

* Add `NuGet.Protocol` APIs for package staging resources - [#15084](https://github.com/NuGet/Home/issues/15084)

* Warn when `dotnet package add` uses a non-standard package ID - [#15076](https://github.com/NuGet/Home/issues/15076)

* Remove the `NUGET_DISABLE_PACKAGEID_VALIDATION` escape hatch in .NET 11 - [#14485](https://github.com/NuGet/Home/issues/14485)

* Show vulnerability indicators for top-level packages when the selected feed is not NuGet.org - [#13051](https://github.com/NuGet/Home/issues/13051)

* Continue listing supported projects when a solution contains unsupported Docker Compose projects. The command still returns exit code 1 - [#12520](https://github.com/NuGet/Home/issues/12520)

* Prevent a null-reference error when updating a package source with `dotnet nuget update source` - [#15094](https://github.com/NuGet/Home/issues/15094)

* Preserve no-op restore on Windows when project paths differ only in letter case - [#15081](https://github.com/NuGet/Home/issues/15081)

* Fix package-reference read errors when listing packages after `dotnet package add` - [#15079](https://github.com/NuGet/Home/issues/15079)

* Prevent a null-reference error when opening Package Manager for solutions with many projects - [#15073](https://github.com/NuGet/Home/issues/15073)

* Prevent an endless Package Manager refresh loop when updating packages with Central Package Management - [#15072](https://github.com/NuGet/Home/issues/15072)

* Include audit-source vulnerability data in the Package Manager version list - [#15071](https://github.com/NuGet/Home/issues/15071)

* Make deterministic package timestamps opt-in again to preserve existing `PreserveNewest` behavior - [#15050](https://github.com/NuGet/Home/issues/15050)

* Report NU1020 when minimum publish age restrictions apply to floating versions during restore - [#15035](https://github.com/NuGet/Home/issues/15035)

* Configure package-source minimum publish ages and package exceptions in Visual Studio settings - [#15032](https://github.com/NuGet/Home/issues/15032)

* Configure minimum publish ages with `--min-publish-age-hours` in `dotnet nuget add source` and `dotnet nuget update source` - [#15031](https://github.com/NuGet/Home/issues/15031)

* Respect configured minimum publish ages when `dotnet package add` selects the latest package version - [#15028](https://github.com/NuGet/Home/issues/15028)

* Respect configured minimum publish ages in `dotnet package update`, including updates for vulnerable packages - [#15027](https://github.com/NuGet/Home/issues/15027)

* Add `nuget.config` settings and `NuGet.Configuration` APIs for minimum publish ages and package exceptions - [#15026](https://github.com/NuGet/Home/issues/15026)

* Close the Clear NuGet Local Resources window with the Esc key - [#15020](https://github.com/NuGet/Home/issues/15020)

* Use the .NET SDK root to locate certificate bundles in Native AOT applications - [#14980](https://github.com/NuGet/Home/issues/14980)

* Fix restore failures when a Visual Studio build starts immediately after opening a solution - [#14927](https://github.com/NuGet/Home/issues/14927)

* Request package-source credentials instead of proxy credentials when a package source requires authentication - [#14791](https://github.com/NuGet/Home/issues/14791)

* Avoid an incorrect Installed-tab vulnerability indicator when the audit source is a local path - [#14724](https://github.com/NuGet/Home/issues/14724)

* Improve pack output filename calculation when nuspec metadata overrides the package ID or version - [#14711](https://github.com/NuGet/Home/issues/14711)

* Avoid showing "no packages found" alongside vulnerable packages after opening Package Manager from Solution Explorer - [#14648](https://github.com/NuGet/Home/issues/14648)

* Preserve vulnerability indicators when merging local and remote package versions - [#14253](https://github.com/NuGet/Home/issues/14253)

* Show consistent transitive-package vulnerability information in package details and the version list - [#14004](https://github.com/NuGet/Home/issues/14004)

* Continue listing supported projects when a solution contains unsupported JavaScript `.esproj` projects. The command still returns exit code 1 - [#12817](https://github.com/NuGet/Home/issues/12817)

* Calculate pack output paths correctly when `OutputFileNamesWithoutVersion` is enabled - [#12644](https://github.com/NuGet/Home/issues/12644)

* Allow `PackageSourceProvider.UpdatePackageSource` to enable a previously disabled package source - [#12008](https://github.com/NuGet/Home/issues/12008)

* Reduce redundant project evaluations during pack while preserving no-build behavior - [#11530](https://github.com/NuGet/Home/issues/11530)

* Avoid a deadlock during credential-service initialization when the value factory needs the Visual Studio UI thread - [#8248](https://github.com/NuGet/Home/issues/8248)

* Support the new Copilot SDK backend for NuGet workflows in Visual Studio while retaining the legacy experience - [NuGet/NuGet.Client#7698](https://github.com/NuGet/NuGet.Client/pull/7698)

[List of commits in this release](https://github.com/NuGet/NuGet.Client/compare/7.11.0.24...7719b2079546f52df28ecb639ce114ca8ca82440)

### Community contributions

Thank you to all the contributors who helped make this NuGet release awesome!

* [eerhardt](https://github.com/eerhardt)
  * [7659](https://github.com/NuGet/NuGet.Client/pull/7659) Add Native AOT compatibility tests and durable trimming annotations
  * [7657](https://github.com/NuGet/NuGet.Client/pull/7657) Replace the .NET 8 library target with .NET 10
* [JasonWeinzierl](https://github.com/JasonWeinzierl)
  * [7671](https://github.com/NuGet/NuGet.Client/pull/7671) Continue package-list output after unsupported-project errors without changing the failing exit code
* [vaboca](https://github.com/vaboca)
  * [7699](https://github.com/NuGet/NuGet.Client/pull/7699) Preserve restore no-op for case-equivalent project paths
* [sean-mcmanus](https://github.com/sean-mcmanus)
  * [7681](https://github.com/NuGet/NuGet.Client/pull/7681) Avoid deadlock during credential service initialization
* [lifengl](https://github.com/lifengl)
  * [7676](https://github.com/NuGet/NuGet.Client/pull/7676) Remove obsolete UI-thread dependency from credential setup
* [agocke](https://github.com/agocke)
  * [7643](https://github.com/NuGet/NuGet.Client/pull/7643) Prevent extracted-file descriptors from leaking into child processes on Unix
