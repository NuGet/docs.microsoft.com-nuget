---
title: NuGet 6.1 Release Notes
description: Release notes for NuGet 6.1 including new features, bug fixes, and DCRs.
author: zivkan
ms.author: zivkan
ms.date: 2/10/2022
ms.topic: conceptual
---

# NuGet 6.1 Release Notes

NuGet distribution vehicles:

| NuGet version | Available in Visual Studio version | Available in .NET SDK(s) |
|:---|:---|:---|
| [**6.1.0**](https://nuget.org/downloads) | [Visual Studio 2022 version 17.1](https://visualstudio.microsoft.com/downloads/) | [6.0.200](https://dotnet.microsoft.com/download/dotnet-core/6.0)<sup>1</sup> |

<sup>1</sup> Installed with Visual Studio 2022 with.NET Core workload

## Summary: What's New in 6.1

* Show help only for the error itself has to do with a command, argument, or option being invalid with `dotnet nuget` command - [#10788](https://github.com/NuGet/Home/issues/10788)

* Sort the package vulnerabilities in descending order in PMUI details pane - [#11091](https://github.com/NuGet/Home/issues/11091)

### Issues fixed in this release

**DCRs:**

* Disable nuget.exe pack for project.json by default, add a fallback env var to enable it - [#11214](https://github.com/NuGet/Home/issues/11214)

* [DCR][No Customer Impact]: NuGetPackage (VS extension entry point) should not use DTEEvents - [#11360](https://github.com/NuGet/Home/issues/11360)

* Remove Mac Catalyst warning - [#11438](https://github.com/NuGet/Home/issues/11438)

**Bugs:**

* Minimal VS install has broken NuGet UI - [#8414](https://github.com/NuGet/Home/issues/8414)

* Versions List in Details Pane is not kept in sync when changing Selected Package - [#10557](https://github.com/NuGet/Home/issues/10557)

* File Conflict dialog does not have access keys - [#10691](https://github.com/NuGet/Home/issues/10691)

* UIDelay: nuget.packagemanagement.visualstudio.dll!NuGet.PackageManagement.VisualStudio.VsCoreProjectSystemReferenceReader+<GetProjectReferencesAsync>d__ - managed projects - [#11163](https://github.com/NuGet/Home/issues/11163)

* [Bug]: MSBuild restore is logging exception as warning - [#11179](https://github.com/NuGet/Home/issues/11179)

* [Bug]: String comparison approach used for Source and Namespaces is not consistent - [#11182](https://github.com/NuGet/Home/issues/11182)

* Async Service Provider should be invoked on threadpool threads via the built in TService,TInterface extensions instead of custom casting - [#11200](https://github.com/NuGet/Home/issues/11200)

* [Bug]: VS crashes when package sources dropdown in PM UI has multiple package sources whose names are equal when compared using Culture Ignore Case - [#11241](https://github.com/NuGet/Home/issues/11241)

* The strings "ms"  and "sec" are not localized on Output - Package Manager window. - [#11297](https://github.com/NuGet/Home/issues/11297)

* Update SDPX license list from da7ecca to fafa781 - [#11309](https://github.com/NuGet/Home/issues/11309)

* [Bug]: NuGet SDK resolver should not throw if project path is NULL - [#11376](https://github.com/NuGet/Home/issues/11376)

* [Bug]: PackageSourceMapping inconsistencies should be reported - [#11385](https://github.com/NuGet/Home/issues/11385)

* [Bug]: `RegistryKeyUtility.GetValueFromRegistryKey()` has unused parameter, leading to incorrect results - [#11407](https://github.com/NuGet/Home/issues/11407)

* Reduce heap allocations in ResolverUtility.FindLibraryByVersionAsync - [#11409](https://github.com/NuGet/Home/issues/11409)

* [Bug]: Package Source Mapping matches found should not be logged for projects - [#11413](https://github.com/NuGet/Home/issues/11413)

* [Bug]: Avoid explicitly casting the result of GetService/GetServiceAsync, use the extension methods instead - [#11451](https://github.com/NuGet/Home/issues/11451)

* [Bug]: Package Source options in VS does not announce package sources or checkbox status - [#11482](https://github.com/NuGet/Home/issues/11482)

* [Bug]: VS package source options clears all checkboxes when adding or removing sources - [#11521](https://github.com/NuGet/Home/issues/11521)

**StillOpens:**

* Remove project.json pack - [#7931](https://github.com/NuGet/Home/issues/7931)

* Async implementation of IVS Package Installer/Uninstaller services - [#8896](https://github.com/NuGet/Home/issues/8896)

**Nones:**

* [5.x] Make restore types that are project.json-specific obsolete - [#9149](https://github.com/NuGet/Home/issues/9149)

**[List of all issues fixed in this release - 6.1](https://app.zenhub.com/workspaces/nuget-client-team-55aec9a240305cf007585881/reports/release?release=Z2lkOi8vcmFwdG9yL1JlbGVhc2UvNjY5ODY)**
