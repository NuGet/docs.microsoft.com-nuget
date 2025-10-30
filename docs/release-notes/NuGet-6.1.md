---
title: NuGet 6.1 Release Notes
description: Release notes for NuGet 6.1 including new features, bug fixes, and DCRs.
author: zivkan
ms.author: zivkan
ms.date: 2/15/2022
ms.topic: release-notes
---

# NuGet 6.1 Release Notes

NuGet distribution vehicles:

| NuGet version | Available in Visual Studio version | Available in .NET SDK(s) |
|:---|:---|:---|
| [**6.1.0**](https://nuget.org/downloads) | [Visual Studio 2022 version 17.1](https://visualstudio.microsoft.com/downloads/) | [6.0.200](https://dotnet.microsoft.com/download/dotnet-core/6.0)<sup>1</sup> |

<sup>1</sup> Installed with Visual Studio 2022 with.NET Core workload

## Summary: What's New in 6.1

* Show subcommand help instead of main `dotnet nuget trust` command help for all cases - [#10788](https://github.com/NuGet/Home/issues/10788)

* Sort the package vulnerabilities in descending order in PMUI details pane - [#11091](https://github.com/NuGet/Home/issues/11091)

### Issues fixed in this release

**DCRs:**

* Disable nuget.exe pack for project.json by default, add a fallback env var to enable it - [#11214](https://github.com/NuGet/Home/issues/11214)

* [DCR]: Adjust compatibility rules for Apple TFMs in .NET  - [#11338](https://github.com/NuGet/Home/issues/11338)

* [DCR][No Customer Impact]: NuGetPackage (VS extension entry point) should not use DTEEvents - [#11360](https://github.com/NuGet/Home/issues/11360)

* [DCR]: Mitigate missing nuget.org when non-NuGet tool creates nuget.config without any sources - [#11387](https://github.com/NuGet/Home/issues/11387)

* [DCR]: NuGet.VisualStudio and NuGet.VisualStudio.Contracts to follow Visual Studio version numbers - [#11394](https://github.com/NuGet/Home/issues/11394)

* [DCR]: Deprecate VS Extensibility APIs that use System.Version - [#11412](https://github.com/NuGet/Home/issues/11412)

* [DCR]: Obsolete VS extensibility APIs that use System.Runtime.Versioning.FrameworkName - [#11419](https://github.com/NuGet/Home/issues/11419)

* Remove Mac Catalyst warning - [#11438](https://github.com/NuGet/Home/issues/11438)

* dotnet nuget push - Missing value for option - [#4864](https://github.com/NuGet/Home/issues/4864)

**Bugs:**

* Confusing restore output: it did some restore on one of the projects, but at the end it still prints "All packages are already installed and there is nothing to restore" - [#4376](https://github.com/NuGet/Home/issues/4376)

* Restore does not print enough info in the output when fails - [#6047](https://github.com/NuGet/Home/issues/6047)

* Minimal VS install has broken NuGet UI - [#8414](https://github.com/NuGet/Home/issues/8414)

* Visual Studio is unresponsive while using `Clear All NuGet cache(s)` feature - [#9831](https://github.com/NuGet/Home/issues/9831)

* Versions List in Details Pane is not kept in sync when changing Selected Package - [#10557](https://github.com/NuGet/Home/issues/10557)

* File Conflict dialog does not have access keys - [#10691](https://github.com/NuGet/Home/issues/10691)

* ContentItemCollection uses yield return which is causing over-allocation of Enumerators - [#10921](https://github.com/NuGet/Home/issues/10921)

* UIDelay: `nuget.packagemanagement.visualstudio.dll!NuGet.PackageManagement.VisualStudio.VsCoreProjectSystemReferenceReader+<GetProjectReferencesAsync>d__` - managed projects - [#11163](https://github.com/NuGet/Home/issues/11163)

* [Bug]: MSBuild restore is logging exception as warning - [#11179](https://github.com/NuGet/Home/issues/11179)

* [Bug]: String comparison approach used for Source and Namespaces is not consistent - [#11182](https://github.com/NuGet/Home/issues/11182)

* There is no tooltip for the “install” button on the right of a package in "Browse" tab - [#11189](https://github.com/NuGet/Home/issues/11189)

* Async Service Provider should be invoked on threadpool threads via the built in TService,TInterface extensions instead of custom casting - [#11200](https://github.com/NuGet/Home/issues/11200)

* [Bug]: VS crashes when package sources dropdown in PM UI has multiple package sources whose names are equal when compared using Culture Ignore Case - [#11241](https://github.com/NuGet/Home/issues/11241)

* The strings "ms"  and "sec" are not localized on Output - Package Manager window. - [#11297](https://github.com/NuGet/Home/issues/11297)

* Sort Package Source dropdown list using locale/culture setttings in PM UI - [#11298](https://github.com/NuGet/Home/issues/11298)

* Update SDPX license list from da7ecca to fafa781 - [#11309](https://github.com/NuGet/Home/issues/11309)

* [Bug]: NuGet.Localization isn't generated - [#11328](https://github.com/NuGet/Home/issues/11328)

* [Bug]:  assembly-loading MEF composition in NuGet.Tools VS package's synchronous event handlers can cause responsiveness delays - [#11334](https://github.com/NuGet/Home/issues/11334)

* [Bug]: NuGet SDK resolver should not throw if project path is NULL - [#11376](https://github.com/NuGet/Home/issues/11376)

* [Bug]: PackageSourceMapping inconsistencies should be reported - [#11385](https://github.com/NuGet/Home/issues/11385)

* [Bug]: `RegistryKeyUtility.GetValueFromRegistryKey()` has unused parameter, leading to incorrect results - [#11407](https://github.com/NuGet/Home/issues/11407)

* Reduce heap allocations in ResolverUtility.FindLibraryByVersionAsync - [#11409](https://github.com/NuGet/Home/issues/11409)

* [Bug]: Package Source Mapping matches found should not be logged for projects - [#11413](https://github.com/NuGet/Home/issues/11413)

* [Bug]: Avoid explicitly casting the result of GetService/GetServiceAsync, use the extension methods instead - [#11451](https://github.com/NuGet/Home/issues/11451)

* [Bug]: Package Source options in VS does not announce package sources or checkbox status - [#11482](https://github.com/NuGet/Home/issues/11482)

* [Bug]: VS package source options clears all checkboxes when adding or removing sources - [#11521](https://github.com/NuGet/Home/issues/11521)

**[List of all issues fixed in this release - 6.1](https://app.zenhub.com/workspaces/nuget-client-team-55aec9a240305cf007585881/reports/release?release=Z2lkOi8vcmFwdG9yL1JlbGVhc2UvNjY5ODY)**

## Known issues

### dotnet nuget push -n|--no-symbols or -d|--disable-buffering raises `error: File does not exist ...` exception. - [#11601](https://github.com/NuGet/Home/issues/11601)

#### Issue
Previously in order to use `-n|--no-symbols` and `-d|--disable-buffering` options with `dotnet nuget push` command requires passing of unnecessary random value after it. Removal of this unnecessary value can break your script by throwing exception with `error: File does not exist ...` even though actual push operation was successful.

#### Workaround
Use `-n|--no-symbols` and `-d|--disable-buffering` options standalone without any additional value parameter.

### Community contributions

Thank you to all the contributors who helped make this NuGet release awesome!

|Who|PRs|Issues|
|----|----|----|
[mairaw](https://github.com/mairaw) | [4336](https://github.com/NuGet/NuGet.Client/pull/4336) | Fix broken NuGet logo image - [#11390](https://github.com/NuGet/Home/issues/11390)

## Feedback welcome

Your feedback is important to us.  If there are any problems with this release, check our
[GitHub Issues](https://github.com/NuGet/Home/issues) and
[Visual Studio Developer Community](https://developercommunity.visualstudio.com/)
for existing issues.  For new issues within NuGet, please report a
[GitHub Issue](https://github.com/NuGet/Home/issues/new/choose).
For general NuGet experience issues, let us know via the
[Report a Problem](/visualstudio/ide/how-to-report-a-problem-with-visual-studio)
option found in your favorite IDE under **Help > Report a Problem**.
