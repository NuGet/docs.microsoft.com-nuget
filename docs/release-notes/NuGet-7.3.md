---
title: NuGet 7.3 Release Notes
description: Release notes for NuGet 7.3 including new features, bug fixes, and DCRs.
author: nkoev92
ms.date: 1/28/2026
ms.topic: release-notes
---

# NuGet 7.3 Release Notes

NuGet distribution vehicles:

| NuGet version | Available in Visual Studio version | Available in .NET SDK(s) |
|:---|:---|:---|
| [**7.3.0**](https://nuget.org/downloads) | [Visual Studio 2026 version 18.3.0](https://visualstudio.microsoft.com/downloads/) | [10.0.200](https://dotnet.microsoft.com/download/dotnet/10.0)<sup>1</sup> |

<sup>1</sup> Installed with Visual Studio <TODO: VSYear. Unknown version/year combo, consider updating the tool.> with any .NET workload

## Summary: What's New in 7.3.0

* Support for add/update/delete auditsources in Visual Studio options UI - [#13955](https://github.com/NuGet/Home/issues/13955)

* &quot;Manage NuGet Packages&quot; in vulnerability gold bar could take you to a view with vulnerability filter selected - [#13050](https://github.com/NuGet/Home/issues/13050)

* Vulnerability InfoBar in Visual Studio now has &quot;How to fix with GitHub Copilot&quot; link to NuGet&#39;s MCP Server documentation - [#14680](https://github.com/NuGet/Home/issues/14680)

* Add &quot;dotnet nuget download&quot; command - [#12513](https://github.com/NuGet/Home/issues/12513)

### Breaking changes

### Issues fixed in this release

* Use a deterministic order for adding files to nuget packages - [#14448](https://github.com/NuGet/Home/issues/14448)

* Audit Sources region is collapsed until needed in VS Options - [#14665](https://github.com/NuGet/Home/issues/14665)

* IAssetsLogMessage.TargetGraphs and IRestoreLogMessage.TargetGraphs should use the alias TargetFramework instead of the effective target framework - [#14528](https://github.com/NuGet/Home/issues/14528)

* [Localization] The string ‘Machine-wide package sources are provisioned by Visual Studio workloads and can only be enabled or disabled’ was not localized - [#14716](https://github.com/NuGet/Home/issues/14716)

* [Localization] The string ‘Use separate sources for vulnerability audit’ in ‘Options-&gt;NuGet Package Manager-&gt;Package Sources’ page was not localized - [#14715](https://github.com/NuGet/Home/issues/14715)

* Tooltips are not appearing via keyboard navigation on Package List - [#14683](https://github.com/NuGet/Home/issues/14683)

* Package Owners are no longer separated with a space in the Package Item ToolTip - [#14682](https://github.com/NuGet/Home/issues/14682)

* No warning NU1905 shows when no audit sources are configured however the info said it would in Options-&gt;NuGet Package Manager-&gt;Package Sources-&gt;Audit source part - [#14678](https://github.com/NuGet/Home/issues/14678)

* Static graph restore with duplicate PackageVersion crashes - [#14676](https://github.com/NuGet/Home/issues/14676)

* `dotnet list package` error `Sequence contains no matching element` targeting `netcoreapp3.1` when package with build targets that fails the build is part of the deps - [#14670](https://github.com/NuGet/Home/issues/14670)

* NuGet.Packaging should reference System.Security.Cryptography.Pkcs v8 for net8.0 targets - [#14636](https://github.com/NuGet/Home/issues/14636)

* &quot;Clear NuGet local resources&quot; button is hidden until NuGet VSIX loads - [#14625](https://github.com/NuGet/Home/issues/14625)

* Dependency resolver allocating and resizing HashSet - [#14611](https://github.com/NuGet/Home/issues/14611)

* NuGetEnvironment.GetFolderPath should cache results - [#14602](https://github.com/NuGet/Home/issues/14602)

* Use TargetAlias in AssetsFileDependencies snapshot - [#14584](https://github.com/NuGet/Home/issues/14584)

* LockFileTarget and IRestoreTargetGraph are labeled with an alias - [#14577](https://github.com/NuGet/Home/issues/14577)

* Cannot set `updatePackageLastAccessTime` config via `dotnet nuget config set` - [#14576](https://github.com/NuGet/Home/issues/14576)

* dotnet package update should support multiple projects - [#14309](https://github.com/NuGet/Home/issues/14309)

* Migrator (PC to PR) shouldn&#39;t suggest using project.json project - [#14240](https://github.com/NuGet/Home/issues/14240)

* Include prerelease checkbox is truncated in PM UI for any width - [#14221](https://github.com/NuGet/Home/issues/14221)

* `dotnet restore` fails to invalidate lock file and produce puzzling error messages - [#14198](https://github.com/NuGet/Home/issues/14198)

* dotnet list package --format=json outputs warning &quot;(A) : Auto-referenced package.&quot; - [#13080](https://github.com/NuGet/Home/issues/13080)

* [Bug Bash] The vulnerability filter “show only vulnerable” checkbox is truncated when the PM UI width is not wide enough to display all controls  - [#13002](https://github.com/NuGet/Home/issues/13002)

* [Bug]: NuGet.Common.dll RuntimeEnvironmentHelper.GetIsMacOSX throws a first-chance exception - [#11433](https://github.com/NuGet/Home/issues/11433)

* [Bug]: KeyNotFoundException in LockFileBuilder.CreateLockFile - [#11028](https://github.com/NuGet/Home/issues/11028)

[List of commits in this release](https://github.com/NuGet/NuGet.Client/compare/7.0.2.3...7.3.0.70)

### Community contributions

Thank you to all the contributors who helped make this NuGet release awesome!

* [SimonCropp](https://github.com/NuGet/NuGet.Client/pull/6719)
  * [6719](https://github.com/NuGet/NuGet.Client/pull/6719) Remove un used parameters
  * [6716](https://github.com/NuGet/NuGet.Client/pull/6716) Avoid the perf hit of using linq .Count()
  * [6721](https://github.com/NuGet/NuGet.Client/pull/6721) Remove un used members
* [omajid](https://github.com/NuGet/NuGet.Client/pull/6963)
  * [6963](https://github.com/NuGet/NuGet.Client/pull/6963) Ensure stable order of files in the nuget package
* [nareshjo](https://github.com/NuGet/NuGet.Client/pull/6994)
  * [6994](https://github.com/NuGet/NuGet.Client/pull/6994) Prevent dictionary allocations in LockFileItem.Equals
* [bradselw](https://github.com/NuGet/NuGet.Client/pull/6924)
  * [6924](https://github.com/NuGet/NuGet.Client/pull/6924) Addressing SFI-ES2.1.2 in DartLab pipelines
* [dkurepa](https://github.com/NuGet/NuGet.Client/pull/6951)
  * [6951](https://github.com/NuGet/NuGet.Client/pull/6951) Set source tag to VMR main
