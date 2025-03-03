---
title: NuGet 6.13 Release Notes
description: Release notes for NuGet 6.13 including new features, bug fixes, and DCRs.
author: Nigusu-Allehu
ms.author: nyenework
ms.date: 2/4/2025
ms.topic: conceptual
---

# NuGet 6.13 Release Notes

NuGet distribution vehicles:

| NuGet version | Available in Visual Studio version | Available in .NET SDK(s) |
|:---|:---|:---|
| [**6.13**](https://nuget.org/downloads) | [Visual Studio 2022 version 17.13](https://visualstudio.microsoft.com/downloads/) | [9.0.2xx](https://dotnet.microsoft.com/download/dotnet/9.0)<sup>1</sup> |

<sup>1</sup> Installed with Visual Studio 2022 with any .NET workload

## Summary: What's New in 6.13.1

NuGet 6.13.1 is available in Visual Studio 17.13.

* Support for new slnx solution format in dotnet nuget why and dotnet list package - [#14034](https://github.com/NuGet/Home/issues/14034)

## Summary: What's New in 6.13.0

NuGet 6.13.0 is available in the .NET 9.0.200 SDK.

* Support for credential providers deployed via .NET tools - [#12567](https://github.com/NuGet/Home/issues/12567)

* Opt-in feature: "Supplied by Platform", which removes packages that are supplied by the .NET platform from the dependency graph. This results in better performance and eliminates false positives for vulnerabilities in transitive dependencies. 

* dotnet nuget why should check RID specific packages - [#13718](https://github.com/NuGet/Home/issues/13718)

* Allow specifying the msbuild binlog path when invoking static graph restore to avoid modifying the environment - [#10789](https://github.com/NuGet/Home/issues/10789)

* New Dependency Resolver Fixes

  * New dependency resolver downloads more packages than before - [#13943](https://github.com/NuGet/Home/issues/13943)

  * New dependency resolver does not handle floating versions correctly in some situations - [#13992](https://github.com/NuGet/Home/issues/13992)

  * New dependency resolver erroneously reports NU1605 (downgrade) when using transitive pinning a direct dependency and a downgrade exists in a package graph - [#13938](https://github.com/NuGet/Home/issues/13938)

  * NuGet Restore restoring old versions of transitive dependencies when direct dependency does not have guidelines for user's targeted .NET Framework - [#13934](https://github.com/NuGet/Home/issues/13934)

  * Project and package in the same graph with the same name but different dependencies may lead to incorrect versions of the dependencies of that id  - [#13888](https://github.com/NuGet/Home/issues/13888)

* Package Manager UI in Visual Studio now shows embedded READMEs for NuGet packages, if available - [#12583](https://github.com/NuGet/Home/issues/12583)

### Issues fixed in this release

* Detect if restore used NuGetAudit or not for PackageReference projects - [#13778](https://github.com/NuGet/Home/issues/13778)

* Add indicator for deprecated and vulnerable packages to Package Details tab header. - [#13974](https://github.com/NuGet/Home/issues/13974)

* Navigation telemetry for 'Clear All NuGet Storage' Command in VS Settings - [#13968](https://github.com/NuGet/Home/issues/13968)

* Nuget Package Manager for Solution automatically selects transitive dependencies - [#13893](https://github.com/NuGet/Home/issues/13893)

* Debugging large repos with static graph-based restore is slow - [#13876](https://github.com/NuGet/Home/issues/13876)

* NuGet Client SDK packages updating to net8.0 - [#13842](https://github.com/NuGet/Home/issues/13842)

* Promote NU3043 warning to error in .NET 10 - [#13814](https://github.com/NuGet/Home/issues/13814)

* Setting "Allow format selection on first package install" meaning is unclear - [#14016](https://github.com/NuGet/Home/issues/14016)

* `dotnet nuget why` reports missing argument, even though it ran - [#13908](https://github.com/NuGet/Home/issues/13908)

* Spacing adjustments in Details Pane Tabs - [#13880](https://github.com/NuGet/Home/issues/13880)

* The focus border on the Details Pane Tab content is being truncated - [#13879](https://github.com/NuGet/Home/issues/13879)

* JAWS is reading the entire contents of the Package Details Tab when first visiting it - [#13878](https://github.com/NuGet/Home/issues/13878)

* ContentItemCollection.PopulateItemGroups unnecessarily allocates - [#13851](https://github.com/NuGet/Home/issues/13851)

* Read and write .nupkg.metadata files with System.Text.Json - [#13835](https://github.com/NuGet/Home/issues/13835)

* NuGet Fails in Containers When HOME Is Not Set - [#13834](https://github.com/NuGet/Home/issues/13834)

* Signing:  key not disposed - [#13823](https://github.com/NuGet/Home/issues/13823)

* Walk TFMs in parallel when collecting pack outputs - [#13776](https://github.com/NuGet/Home/issues/13776)

* PERF: NuGet Cloning operations are showing heavily in allocations during VS solution load - [#13647](https://github.com/NuGet/Home/issues/13647)

* Fetching Vulnerability Resources doesn't respect cancellation - [#13644](https://github.com/NuGet/Home/issues/13644)

* Wrong order of arguments in logs for centralized package version (string `Info_AddPkgCPM`) - [#13155](https://github.com/NuGet/Home/issues/13155)

* Satellite assemblies for three-letter languages are not copied from NuGet package - [#12253](https://github.com/NuGet/Home/issues/12253)

* Nuget pack doesn't support blank &lt;version&gt; in .nuspec even though version is passed on the command line - [#7987](https://github.com/NuGet/Home/issues/7987)

[List of commits in this release](https://github.com/NuGet/NuGet.Client/compare/6.13.1.3...6.12.3.1)

### Community contributions

Thank you to all the contributors who helped make this NuGet release awesome!

* [SimonCropp](https://github.com/NuGet/NuGet.Client/pull/6185)
  * [6185](https://github.com/NuGet/NuGet.Client/pull/6185) reduce memory in ManifestMetadata
  * [6168](https://github.com/NuGet/NuGet.Client/pull/6168) remove duplicate dictionary lookups
  * [6166](https://github.com/NuGet/NuGet.Client/pull/6166) remove redundant Count() in GlobalPackageFolderRepositories
  * [6165](https://github.com/NuGet/NuGet.Client/pull/6165) avoid Any call in GetCommandAttribute
  * [6167](https://github.com/NuGet/NuGet.Client/pull/6167) remove redundant casts
* [baronfel](https://github.com/NuGet/NuGet.Client/pull/6124)
  * [6124](https://github.com/NuGet/NuGet.Client/pull/6124) Expand Locale parser to support three-character language codes
  * [6018](https://github.com/NuGet/NuGet.Client/pull/6018) Update _WalkEachTargetPerFramework to walk TFMs in parallel
* [ToddGrun](https://github.com/NuGet/NuGet.Client/pull/6098)
  * [6098](https://github.com/NuGet/NuGet.Client/pull/6098) Modify ContentItemCollection.PopulateItemGroups to use pooling for highly allocated temporary data structures
  * [5930](https://github.com/NuGet/NuGet.Client/pull/5930) Attempt to move several data structures to be immutable
* [mthalman](https://github.com/NuGet/NuGet.Client/pull/6212)
  * [6212](https://github.com/NuGet/NuGet.Client/pull/6212) Fix formatting in GraphOperations
* [kasperk81](https://github.com/NuGet/NuGet.Client/pull/6072)
  * [6072](https://github.com/NuGet/NuGet.Client/pull/6072) add SpecialFolder.UserProfile fallback
* [MichaelSimons](https://github.com/NuGet/NuGet.Client/pull/6102)
  * [6102](https://github.com/NuGet/NuGet.Client/pull/6102) Update source-build team references
* [akoeplinger](https://github.com/NuGet/NuGet.Client/pull/6025)
  * [6025](https://github.com/NuGet/NuGet.Client/pull/6025) Fix typo in EnhancedHttpRetryHelper.cs
* [jimmylewis](https://github.com/NuGet/NuGet.Client/pull/6027)
  * [6027](https://github.com/NuGet/NuGet.Client/pull/6027) Refactor calls to EnsureVisualStudioHost() to a base [TestInitialize] method
  