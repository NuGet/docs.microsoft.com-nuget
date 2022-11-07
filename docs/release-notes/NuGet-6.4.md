---
title: NuGet 6.4 Release Notes
description: Release notes for NuGet 6.4 including new features, bug fixes, and DCRs.
author: jebriede
ms.author: jebriede
ms.date: 10/27/2022
ms.topic: conceptual
---

# NuGet 6.4 Release Notes

NuGet distribution vehicles:

| NuGet version | Available in Visual Studio version | Available in .NET SDK(s) |
|:---|:---|:---|
| [**6.4**](https://nuget.org/downloads) | [Visual Studio 2022 version 17.4](https://visualstudio.microsoft.com/downloads/) | [7.0.100](https://dotnet.microsoft.com/download/dotnet-core/7.0)<sup>1</sup> |

<sup>1</sup> Installed with Visual Studio 17.4 with .NET Core workload

## Summary: What's New in 6.4

* Add ability to designate a package reference as used by every project in the repo - GlobalPackageReference [#10159](https://github.com/NuGet/Home/issues/10159)

* Improved performance while loading packages for all tabs in the Package Manager UI and solution restore - [#11923](https://github.com/NuGet/Home/issues/11923)

* Prompts for authentication with Azure Artifacts package sources in Visual Studio indicate that it is for NuGet purposes and contain specific source information.

### Issues fixed in this release

**DCRs:**

* [DCR]: Static graph-based restore should handle an AggregateException from MSBuild - [#12100](https://github.com/NuGet/Home/issues/12100)

* Signing:  use separate fallback certificate bundles for code signing and timestamping - [#12033](https://github.com/NuGet/Home/issues/12033)

* [DCR]: Central package management package source mapping should only look at configured feeds - [#11951](https://github.com/NuGet/Home/issues/11951)

* [DCR]: Remove preview message when using central package management - [#11950](https://github.com/NuGet/Home/issues/11950)

* [DCR]: Package Source Mapping API does not support saving  - [#11935](https://github.com/NuGet/Home/issues/11935)

* [DCR]: Plugin timeout defaults should be increased - [#11793](https://github.com/NuGet/Home/issues/11793)

* Regenerate dgspec when customer triggers VS Feedback - [#8605](https://github.com/NuGet/Home/issues/8605)

**Bugs:**

* Details for Installed MAUI packages are missing NuGet Project PM UI - [#12130](https://github.com/NuGet/Home/issues/12130)

* Static graph restore supports long paths on Windows - [#12121](https://github.com/NuGet/Home/issues/12121)

* TelemetryUtility.IsVsOfflineFeed fails to correctly identify the local feed with 64-bit windows - [#12110](https://github.com/NuGet/Home/issues/12110)

* [Bug]: IVsPackageInstallerServices APIs sometimes throw ProjectNotNominatedException - [#12103](https://github.com/NuGet/Home/issues/12103)

* [Bug]: The transitive package doesn’t show in “Installed” tab until reopening the solution - [#12102](https://github.com/NuGet/Home/issues/12102)

* [Bug]: Incorrect check for feed count when logging NU1507 warning about not using package source mapping - [#12095](https://github.com/NuGet/Home/issues/12095)

* [Bug]: User needs to login multiple times while executing dotnet list package from private feeds - [#12090](https://github.com/NuGet/Home/issues/12090)

* [Bug]: Rename VS NuGet Options "Clear NuGet Cache(s)" button - [#12076](https://github.com/NuGet/Home/issues/12076)

* nuget.exe help command has unlocalized strings - [#12067](https://github.com/NuGet/Home/issues/12067)

* Remove unused localized resources in nuget.exe - [#12066](https://github.com/NuGet/Home/issues/12066)

* [Bug]: NugetSDKResolver doesn't give detailed error messages when it fails - [#12049](https://github.com/NuGet/Home/issues/12049)

* [Bug]: Package signature validation fails on Linux due to missing 'thawte_Primary_Root_CA' in codesignctl.pem - [#12027](https://github.com/NuGet/Home/issues/12027)

* [Bug]: "An item with the same key has already been added" when migrating to CPM with `ProjectDependencies` in solution file - [#12021](https://github.com/NuGet/Home/issues/12021)

* [Bug]: Build failures in dev branch due to renaming of parameter from cpvmEnabled to centralPackageTransitivePinningEnabled  - [#12020](https://github.com/NuGet/Home/issues/12020)

* [Bug]: [Bug Bash] Other versions will lose after selecting a version in the custom version drop-down box for a while - [#11992](https://github.com/NuGet/Home/issues/11992)

* Remove extra layers of abstractions from IVsProjectAdapter, move RuntimeGraph specific methods from VSProject to LegacyPackageReferenceProject - [#11980](https://github.com/NuGet/Home/issues/11980)

* Reduce redundant SolutionDirectory calculation, special-case template wizard solution directory retrieval - [#11936](https://github.com/NuGet/Home/issues/11936)

* Make VS adapter ProjectDirectory sync, use IVsHierarchy only to generate the guids, avoid double casting VSProject4 - [#11928](https://github.com/NuGet/Home/issues/11928)

* [Bug]: NuGet.VisualStudio.Implementation.Extensibility.VsPathContextProvider.TryCreateContext fault - [#11918](https://github.com/NuGet/Home/issues/11918)

* [Bug]: Package version downgrade is not detected due to invalid transitive pinning - [#11760](https://github.com/NuGet/Home/issues/11760)

* _CleanPackageFiles target fails sporadically when (re)building - [#11710](https://github.com/NuGet/Home/issues/11710)

* Avoid calling CreateLockFileTargetLibrary twice when AssetTargetFallback is used - [#11654](https://github.com/NuGet/Home/issues/11654)

* Package source mapping should check for duplicate node keys - [#11573](https://github.com/NuGet/Home/issues/11573)

* VSSolutionManager.DoesNuGetSupportsAnyProjectAsync can exit at the first supported projec - [#11555](https://github.com/NuGet/Home/issues/11555)

* Review all sync ServiceLocator calls and move to async where possible - [#11203](https://github.com/NuGet/Home/issues/11203)

* [Bug Bash]The new designs of hovered-on menu between VS and NuGet are inconsistent - [#10978](https://github.com/NuGet/Home/issues/10978)

* [Bug]: Metadata like PrivateAssets does not flow from parent to transitively pinned dependency in CPM - [#10311](https://github.com/NuGet/Home/issues/10311)

**[List of commits in this release](https://github.com/NuGet/NuGet.Client/compare/6.3.1.1...6.4.0.123)**

### Community contributions

Thank you to all the contributors who helped make this NuGet release awesome!

* [kkirkfield](https://github.com/kkirkfield)
  * [4738](https://github.com/NuGet/NuGet.Client/pull/4738) Fix issue with _CleanPackageFiles target failing on rebuild
* [MichaelSimons](https://github.com/MichaelSimons)
  * [4737](https://github.com/NuGet/NuGet.Client/pull/4737) Tweak ApplySourceBuildPatchFiles target to support virtual mono repo (VMR)
* [marcin-krystianc](https://github.com/marcin-krystianc)
  * [4611](https://github.com/NuGet/NuGet.Client/pull/4611) Central transitive dependencies should be considered only for root nodes
* [Forgind](https://github.com/Forgind)
  * [4766](https://github.com/NuGet/NuGet.Client/pull/4766) Return warnings to log when NuGet SDK resolver fails
* [lbussell](https://github.com/lbussell)
  * [4742](https://github.com/NuGet/NuGet.Client/pull/4742) Update TFM to net7.0 for source-build
