---
title: NuGet 6.12 Release Notes
description: Release notes for NuGet 6.12 including new features, bug fixes, and DCRs.
author: zivkan
ms.topic: conceptual
---

# NuGet 6.12 Release Notes

> [!NOTE]
> In response to developers' feedback to ensure builds continuity when updating to .NET SDK 9, we have reverted the default value of NuGetAuditMode to `direct` in Visual Studio 17.12.3 and .NET 9.0.101.

NuGet distribution vehicles:

| NuGet version | Available in Visual Studio version | Available in .NET SDK(s) |
|:---|:---|:---|
| [**6.12**](https://nuget.org/downloads) | [Visual Studio 2022 version 17.12](https://visualstudio.microsoft.com/downloads/) | [9.0.1xx](https://dotnet.microsoft.com/download/dotnet/9.0)<sup>1</sup> |

<sup>1</sup> Installed with Visual Studio 2022 with any .NET workload

## Known Issues

* Project and package in the same graph with the same name but different dependencies may lead to incorrect versions of the dependencies of that id [#13888](https://github.com/NuGet/Home/issues/13888)
* VS PM UI shows warning icon about package vulnerability even after upgrade [#13866](https://github.com/NuGet/Home/issues/13866)
* dotnet nuget why reports missing argument, even though it ran [#13908](https://github.com/NuGet/Home/issues/13908)

## Summary: What's New in 6.12.3

NuGet 6.12.3 is available in Visual Studio 17.12.5.

### Issues fixed in this release

* Small Solution PM UI size can cause a System.ArgumentException SolutionView.ListView_SizeChanged - [#13928](https://github.com/NuGet/Home/issues/13928)

## Summary: What's New in 6.12.1

NuGet 6.12.1 is available in Visual Studio 17.12.0 and the .NET 9.0.101 SDK.

### Issues fixed in this release

* Deserializing an empty version range in a package dependency fails in .NET SDK 9.0.100-rc.2 [#13869](https://github.com/NuGet/Home/issues/13869)

## Summary: What's New in 6.12

NuGet 6.12.0 is available in the .NET 9.0.100 SDK.

* Add new graph resolution algorithm for better performance with large graphs - [#13692](https://github.com/NuGet/Home/issues/13692)

* NuGetAudit raises warnings for vulnerable transitive packages by default when the .NET 9 SDK is installed [#13293](https://github.com/NuGet/Home/issues/13293)

* Change NuGetAuditMode default from direct to all, raising warnings for vulnerable transitive packages for non-SDK style projects - [#13584](https://github.com/NuGet/Home/issues/13584)

* Audit security vulnerabilities without adding nuget.org as package source - [#12698](https://github.com/NuGet/Home/issues/12698)

* Owner profile hyperlinks needed in Details Pane of PM UI - [#13686](https://github.com/NuGet/Home/issues/13686)

* Deprecate SHA-1 fingerprints usage in NuGet Sign commands in favor of SHA-2 family fingerprints [#13891](https://github.com/NuGet/Home/issues/13891)

* Bubble-up Known Vulnerability Indicators in Solution Explorer for Transitive Packages - [#13636](https://github.com/NuGet/Home/issues/13636)

* Enable Transitive Dependencies and vulnerabilities for Solution-level in Visual Studio - [#13216](https://github.com/NuGet/Home/issues/13216)

### Breaking changes

* Deprecate http usage: Promote from warning to error - [#13289](https://github.com/NuGet/Home/issues/13289)

### Issues fixed in this release

* Enable `dotnet nuget why` on non-SDK style projects - [#13576](https://github.com/NuGet/Home/issues/13576)

* NuGetAuditSuppress for packages.config - [#13575](https://github.com/NuGet/Home/issues/13575)

* Roll-out new breaking change process for SDK tools, respect SdkAnalysisLevel - [#13309](https://github.com/NuGet/Home/issues/13309)

* Add property for toggling the to the previous NuGet resolver: RestoreUseLegacyDependencyResolver - [#13700](https://github.com/NuGet/Home/issues/13700)

* Reduce allocations in TokenSegment.TryMatch - [#12728](https://github.com/NuGet/Home/issues/12728)

* Use `SDKAnalysisLevel` in restore "https everywhere: promote from warning to error" - [#13546](https://github.com/NuGet/Home/issues/13546)

* tweak wording of NU1603 - [#13446](https://github.com/NuGet/Home/issues/13446)

* Default Package icon shown even when embedded icon file exists on disk - [#13766](https://github.com/NuGet/Home/issues/13766)

* Navigation telemetry for hyperlinks: License, ReportAbuse, Readme, ProjectUrl - [#13749](https://github.com/NuGet/Home/issues/13749)

* Navigation telemetry for Owner Profile URLs in PM UI - [#13738](https://github.com/NuGet/Home/issues/13738)

* PM UI should show transitive path - [#13574](https://github.com/NuGet/Home/issues/13574)

* NuGetVersion should use a factory to intern parsed versions - [#13532](https://github.com/NuGet/Home/issues/13532)

* Remove NuGet.Packaging.Core code - [#13385](https://github.com/NuGet/Home/issues/13385)

* PM UI transitive dependencies should display all transitive dependencies, not just ones brought in through packages directly installed in a project - [#13060](https://github.com/NuGet/Home/issues/13060)

* Remove deprecated field "owners" from VS UI Details Pane - [#10666](https://github.com/NuGet/Home/issues/10666)

* "Value cannot be null; Parameter name: source" displays in error list when clicking installed tab in PM UI - [#13801](https://github.com/NuGet/Home/issues/13801)

* New dependency resolver does not properly handle missing package versions when using CPM - [#13788](https://github.com/NuGet/Home/issues/13788)

* Saving PackageManagementFormat throws Nullable object must have a value. - [#13773](https://github.com/NuGet/Home/issues/13773)

* ProjectReference causing PM UI to error with "Value cannot be null. Parameter name: frameworkIdentifier" - [#13737](https://github.com/NuGet/Home/issues/13737)

* LockFileUtils.CreateLockFileTargetProject allocates a lot - [#13712](https://github.com/NuGet/Home/issues/13712)

* ConvertToProjectPaths  causes extra allocations due to yield usage - [#13677](https://github.com/NuGet/Home/issues/13677)

* dotnet add package with CPM installs a different version than what gets restored - [#13657](https://github.com/NuGet/Home/issues/13657)

* `dotnet list package` does not work if project is using central package management system, after upgrading to `.NET 8.0` - [#13632](https://github.com/NuGet/Home/issues/13632)

* Add a log code NuGetAuditSuppress duplicate items - [#13620](https://github.com/NuGet/Home/issues/13620)

* Solution Explorer search can be broken by skipped dataflow updates - [#13619](https://github.com/NuGet/Home/issues/13619)

* Add nullability declarations to ResolverUtility and RemoteWalkContext - [#13617](https://github.com/NuGet/Home/issues/13617)

* Use of Obsolete X509Certificate2 ctor - [#13612](https://github.com/NuGet/Home/issues/13612)

* nuget restore warnings can't be suppressed with NoWarn in Visual Studio - [#13571](https://github.com/NuGet/Home/issues/13571)

* Restore may write nulls to project.assets.json  - [#13563](https://github.com/NuGet/Home/issues/13563)

* VS 17.10 - Error building projects with CPM explicitly enabled if ManagePackageVersionsCentrally is set to false in Directory.Build.props - [#13560](https://github.com/NuGet/Home/issues/13560)

* PERF: Version and VersionRange allocations are very prevalent in profiles of Roslyn solution load - [#13559](https://github.com/NuGet/Home/issues/13559)

* PERF: LockFileFormat is filled completely when common callers only need some of the data - [#13558](https://github.com/NuGet/Home/issues/13558)

* PERF: Unnecessary construction of LockFileItem.Properties dictionary - [#13557](https://github.com/NuGet/Home/issues/13557)

* Narator does not read the value of  `allowInsecureConnections`  - [#13555](https://github.com/NuGet/Home/issues/13555)

* NuGet fails because of invalid characters in User-Agent header - [#13531](https://github.com/NuGet/Home/issues/13531)

* 'why' and 'config' command does not show up in 'dotnet nuget --help' output - [#13517](https://github.com/NuGet/Home/issues/13517)

* allocation: nuget.protocol.dll!NuGet.Protocol.HttpCacheUtility+&lt;CreateCacheFileAsync&gt;d__.MoveNext|nuget.protocol.dll!NuGet.Protocol.PackageDependencyGroupConverter.ReadJson - [#13445](https://github.com/NuGet/Home/issues/13445)

* Reduce allocations in ContentItemCollection - [#12657](https://github.com/NuGet/Home/issues/12657)

* When a source isn't accessible, service index cannot be read issues suppress the internal message making it difficult to understand the root cause - [#12530](https://github.com/NuGet/Home/issues/12530)

* [Bug]: Extra space at start of package description in tooltip - [#12105](https://github.com/NuGet/Home/issues/12105)

* Map branch name from sourcelink to RepositoryBranch for NuGet pack - [#13625](https://github.com/NuGet/Home/issues/13625)

[List of commits in this release](https://github.com/NuGet/NuGet.Client/compare/6.12.1.1...6.11.1.2)

### Community contributions

Thank you to all the contributors who helped make this NuGet release awesome!

* [akoeplinger](https://github.com/NuGet/NuGet.Client/pull/6005)
  * [6005](https://github.com/NuGet/NuGet.Client/pull/6005) Improve build.sh and fixes for building on arm64 macOS
  * [5956](https://github.com/NuGet/NuGet.Client/pull/5956) Add System.Formats.Asn1 into Version.Details.xml
  * [5911](https://github.com/NuGet/NuGet.Client/pull/5911) Don't use obsolete X509Certificate2 constructor on net9.0
* [ToddGrun](https://github.com/NuGet/NuGet.Client/pull/5862)
  * [5862](https://github.com/NuGet/NuGet.Client/pull/5862) Reduce allocations for version / versionranges
  * [5857](https://github.com/NuGet/NuGet.Client/pull/5857) Reduce memory allocations during solution load in VS
  * [5861](https://github.com/NuGet/NuGet.Client/pull/5861) Defer LockFileItem.Properties dictionary construction until needed
* [KirillOsenkov](https://github.com/NuGet/NuGet.Client/pull/6008)
  * [6008](https://github.com/NuGet/NuGet.Client/pull/6008) Always debug RestoreTask and RestoreEx when environment variable is set
* [vernou](https://github.com/NuGet/NuGet.Client/pull/5982)
  * [5982](https://github.com/NuGet/NuGet.Client/pull/5982) Fix restore when a package is installed with a version specified in CPM
* [mthalman](https://github.com/NuGet/NuGet.Client/pull/5959)
  * [5959](https://github.com/NuGet/NuGet.Client/pull/5959) Allow override of System.Formats.Asn1 package version
* [MattKotsenas](https://github.com/NuGet/NuGet.Client/pull/5923)
  * [5923](https://github.com/NuGet/NuGet.Client/pull/5923) Map SourceBranchName from sourcelink to RepositoryBranch for NuGet pack
