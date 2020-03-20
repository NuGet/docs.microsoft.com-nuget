---
title: NuGet 5.5 Release Notes
description: Release notes for NuGet 5.5 including new features, bug fixes, and DCRs.
author: karann-msft
ms.author: karann
ms.date: 03/19/2020
ms.topic: conceptual
---

# NuGet 5.5 Release Notes

NuGet distribution vehicles:

| NuGet version | Available in Visual Studio version| Available in .NET SDK(s)|
|:---|:---|:---|
| [**5.5.0**](https://nuget.org/downloads) | [Visual Studio 2019 version 16.5](https://visualstudio.microsoft.com/downloads/) | [3.1.200](https://dotnet.microsoft.com/download/dotnet-core/3.1)<sup>1</sup> |

<sup>1</sup>Installed with Visual Studio 2019 with .NET Core workload

## Summary: What's New in 5.5

* Several accessibility fixes in PM UI

* dotnet.exe reliability fixes with cross platform authentication plugins

* msbuild /restore works with Packages.Config

**Feature:**

* add `dotnet nuget <add|remove|update|disable|enable|list> source` command - [#4126](https://github.com/NuGet/Home/issues/4126)

* Improve network diagnostics to understand real world http download perf in VS - [#8592](https://github.com/NuGet/Home/issues/8592)

* --skip-duplicate support for dotnet nuget push - [#8778](https://github.com/NuGet/Home/issues/8778)

* msbuild /restore works with Packages.Config - [#8506](https://github.com/NuGet/Home/issues/8506)

* Embedded Icons support in NuGet PackageManager UI - [#8189](https://github.com/NuGet/Home/issues/8189)

**DCR:**

* nuget.exe pack should no longer warn about packing a SemVer 2.0.0 package - [#5201](https://github.com/NuGet/Home/issues/5201)

* Logic in _GetRestoreProjectStyle should be in a task - [#8804](https://github.com/NuGet/Home/issues/8804)

* Deprecation info is not visible, by default, on installed tab - [#8541](https://github.com/NuGet/Home/issues/8541)

**Bug:**

* Rework Self-Updater with V3 Apis - [#4197](https://github.com/NuGet/Home/issues/4197)

* Wrong package dependency version If package dependency version is set to '*' - [#6697](https://github.com/NuGet/Home/issues/6697)

* ErrorUnsafePackageEntry error message is not pointing to source of problem - [#7505](https://github.com/NuGet/Home/issues/7505)

* dotnet restore failing with TaskCanceledException - [#7842](https://github.com/NuGet/Home/issues/7842)

* Lock file is not honored in "*" scenarios  - [#8073](https://github.com/NuGet/Home/issues/8073)

* NuGet.exe does not resolve to the latest version of a package when using * in PackageReference (MSBuild/Dotnet/VS restore do) - [#8432](https://github.com/NuGet/Home/issues/8432)

* dotnet list package with multi targeting WPF project - [#8463](https://github.com/NuGet/Home/issues/8463)

* Plugin:  "A task was cancelled" - problem with ADO authentication due to this. - [#8528](https://github.com/NuGet/Home/issues/8528)

* Improve ConcurrencyUtilities (reduce CPU usage) - [#8653](https://github.com/NuGet/Home/issues/8653)

* DG Spec for unloaded project scenarios should not be written in preview restores - [#8793](https://github.com/NuGet/Home/issues/8793)

* The Visual Studio NuGet packages (RestoreManagerPackage) needs to auto load on solution build events - [#8796](https://github.com/NuGet/Home/issues/8796)

* Deadlock in VSSettings init - [#8842](https://github.com/NuGet/Home/issues/8842)

* VisualStudio ToolBox is not populated from a NuGet package if a project is placed in a solution folder - [#8868](https://github.com/NuGet/Home/issues/8868)

* VS:  solution restore perpetually fails due to race condition - [#8881](https://github.com/NuGet/Home/issues/8881)

* Constant "loading.." on installed tab, and "searching <term>.." on updates tab - [#8890](https://github.com/NuGet/Home/issues/8890)

* Accessibility Fixes in PM UI - altText, etc... - [#9059](https://github.com/NuGet/Home/issues/9059)

* Missing Embedded Icons in VS PM UI after cache expires - [#9069](https://github.com/NuGet/Home/issues/9069)

* Review Accessibility bugs - [#9077](https://github.com/NuGet/Home/issues/9077)

* PMUI Tabs have Accessibility Issues - [#9078](https://github.com/NuGet/Home/issues/9078)

* FireAndForget PM UI startup - [#9112](https://github.com/NuGet/Home/issues/9112)

* Accessibility Fixes in PM UI - [#9157](https://github.com/NuGet/Home/issues/9157)

* Restore: IncludeExcludeFiles.Equals(...) implementation is incorrect - [#9167](https://github.com/NuGet/Home/issues/9167)

* Restore: PackageSpec.Clone() creates unequal clone - [#9211](https://github.com/NuGet/Home/issues/9211)

* Error list shown although "Always show Error List if build finishes with errors" is not checked - [#8190](https://github.com/NuGet/Home/issues/8190)

* Static Graph restore should not pass empty SolutionPath - [#9061](https://github.com/NuGet/Home/issues/9061)

* Restore: closure computed for each project 4 times - [#9042](https://github.com/NuGet/Home/issues/9042)

* Restore: DependencyGraphSpec.Load(...) does not need JObject - [#9040](https://github.com/NuGet/Home/issues/9040)

* Restore: large strings created on large object heap (LOH) - [#9031](https://github.com/NuGet/Home/issues/9031)