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

* --skip-duplicate support for dotnet nuget push

* msbuild /restore works with Packages.Config

* Change the way we read XML files

**Bug:**

* Rework Self-Updater with V3 Apis - [#4197](https://github.com/NuGet/Home/issues/4197)

* Wrong package dependency version If package dependency version is set to '*' - [#6697](https://github.com/NuGet/Home/issues/6697)

* ErrorUnsafePackageEntry error message is not pointing to source of problem - [#7505](https://github.com/NuGet/Home/issues/7505)

* NuGet 4.9.2 fails to install packages from nuget.org with "Canceled" error - [#7699](https://github.com/NuGet/Home/issues/7699)

* dotnet restore failing with TaskCanceledException - [#7842](https://github.com/NuGet/Home/issues/7842)

* Lock file is not honored in "*" scenarios  - [#8073](https://github.com/NuGet/Home/issues/8073)

* NuGet.exe does not resolve to the latest version of a package when using * in PackageReference (MSBuild/Dotnet/VS restore do) - [#8432](https://github.com/NuGet/Home/issues/8432)

* dotnet list package with multi targeting WPF project - [#8463](https://github.com/NuGet/Home/issues/8463)

* Plugin:  "A task was cancelled" - problem with ADO authentication due to this. - [#8528](https://github.com/NuGet/Home/issues/8528)

* Improve ConcurrencyUtilities (reduce CPU usage) - [#8653](https://github.com/NuGet/Home/issues/8653)

* vs/nuget/nugetaction telemetry doesn't adequately capture cancellation - [#8782](https://github.com/NuGet/Home/issues/8782)

* vs/nuget/nugetaction doesn't log the packages added, removed, or updated - [#8783](https://github.com/NuGet/Home/issues/8783)

* DG Spec for unloaded project scenarios should not be written in preview restores - [#8793](https://github.com/NuGet/Home/issues/8793)

* The Visual Studio NuGet packages (RestoreManagerPackage) needs to auto load on solution build events - [#8796](https://github.com/NuGet/Home/issues/8796)

*  [Test Failure] Package icon becomes a default icon after installing ‘Newtonsoft.Json’ with latest versoin - [#8814](https://github.com/NuGet/Home/issues/8814)

* Logic in RestoreTask should be refactored into a utility - [#8829](https://github.com/NuGet/Home/issues/8829)

* Deadlock in VSSettings init - [#8842](https://github.com/NuGet/Home/issues/8842)

* "An item with the same key has already been added" when restoring in Visual Studio - [#8847](https://github.com/NuGet/Home/issues/8847)

* DownloadTimeoutStream should not block by calling .Result - [#8853](https://github.com/NuGet/Home/issues/8853)

* RestoreSettingsUtils needs overload to use new SettingsLoadingContext - [#8855](https://github.com/NuGet/Home/issues/8855)

* VisualStudio ToolBox is not populated from a NuGet package if a project is placed in a solution folder - [#8868](https://github.com/NuGet/Home/issues/8868)

* VS:  solution restore perpetually fails due to race condition - [#8881](https://github.com/NuGet/Home/issues/8881)

* Constant "loading.." on installed tab, and "searching <term>.." on updates tab - [#8890](https://github.com/NuGet/Home/issues/8890)

* ProjectReference becomes dependency in nuspec instead of including it in nupkg - [#8960](https://github.com/NuGet/Home/issues/8960)

* Update the NuGet VS depedencies to the recent 16.4 ones so we can use the Service Broker APIs. - [#8967](https://github.com/NuGet/Home/issues/8967)

* Address VSSDK004, VSSDK005 & VSSDK006 analyzer warnings - [#8974](https://github.com/NuGet/Home/issues/8974)

* Address VSTHRD100 - [#8975](https://github.com/NuGet/Home/issues/8975)

* Address VSTHRD010 and VSTHRD109 warnings - [#8978](https://github.com/NuGet/Home/issues/8978)

* NuGet client tools strategy with multiple feeds on packages with version floats - [#8992](https://github.com/NuGet/Home/issues/8992)

* hashed package names in telemetry should be lower cased - [#8995](https://github.com/NuGet/Home/issues/8995)

* Accessibility Fixes in PM UI - altText, etc... - [#9059](https://github.com/NuGet/Home/issues/9059)

* Missing Embedded Icons in VS PM UI after cache expires - [#9069](https://github.com/NuGet/Home/issues/9069)

* Icon missing after package install in VS PM UI - [#9072](https://github.com/NuGet/Home/issues/9072)

* Review Accessibility bugs - [#9077](https://github.com/NuGet/Home/issues/9077)

* PMUI Tabs have Accessibility Issues - [#9078](https://github.com/NuGet/Home/issues/9078)

* Review JoinableTaskFactory.Run( usage - [#9096](https://github.com/NuGet/Home/issues/9096)

* FireAndForget PM UI startup - [#9112](https://github.com/NuGet/Home/issues/9112)

* Missing Icon in PM UI Details Pane - [#9113](https://github.com/NuGet/Home/issues/9113)

* Accessibility Fixes in PM UI - [#9157](https://github.com/NuGet/Home/issues/9157)

* Restore:  IncludeExcludeFiles.Equals(...) implementation is incorrect - [#9167](https://github.com/NuGet/Home/issues/9167)

* Restore:  PackageSpec.Clone() creates unequal clone - [#9211](https://github.com/NuGet/Home/issues/9211)

**Feature:**

* add `dotnet nuget <add|remove|update|disable|enable|list> source` command - [#4126](https://github.com/NuGet/Home/issues/4126)

* Deprecation info is not visible, by default, on installed tab - [#8541](https://github.com/NuGet/Home/issues/8541)

* Improve network diagnostics to understand real world http download perf in VS - [#8592](https://github.com/NuGet/Home/issues/8592)

* --skip-duplicate support for dotnet nuget push - [#8778](https://github.com/NuGet/Home/issues/8778)

* msbuild /restore works with Packages.Config - [#8506](https://github.com/NuGet/Home/issues/8506)

**DCR:**

* nuget.exe pack should no longer warn about packing a SemVer 2.0.0 package - [#5201](https://github.com/NuGet/Home/issues/5201)

* NuGet PackageManager UI handles icons from <icon/> and <iconUrl/> - [#8189](https://github.com/NuGet/Home/issues/8189)

* Logic in _GetRestoreProjectStyle should be in a task - [#8804](https://github.com/NuGet/Home/issues/8804)

**None:**

* Error list shown although "Always show Error List if build finishes with errors" is not checked - [#8190](https://github.com/NuGet/Home/issues/8190)

* Floating version "*" retrieves incorrect nuget package version (not latest) - [#8333](https://github.com/NuGet/Home/issues/8333)

* Temporary fix on patching SDK for System.Security.Cryptography.Pkcs.dll  - [#8508](https://github.com/NuGet/Home/issues/8508)

* Code files (.pp) in ContentFiles folder not added to consuming PackageReference project - [#8718](https://github.com/NuGet/Home/issues/8718)

* ExcludeRestorePackageImports=true should not exclude package path properties - [#8840](https://github.com/NuGet/Home/issues/8840)

* Nuget package can be installed multiple times in different versions - [#8865](https://github.com/NuGet/Home/issues/8865)

* Add more settings tests - completely cover project\sln level support across products - [#8927](https://github.com/NuGet/Home/issues/8927)

* [Test Failure]Confusing supported .NET Framework version v0.0 show in the error NU1202 message when installing a package into a project with incompatible framework - [#8965](https://github.com/NuGet/Home/issues/8965)

* SpecCommand tests depend on the current year - [#8984](https://github.com/NuGet/Home/issues/8984)

* Restore:  large strings created on large object heap (LOH) - [#9031](https://github.com/NuGet/Home/issues/9031)

* Make the dotnet.exe functional tests runnable from Visual Studio, speed up the Dotnet.Integration.Test suite by 40% - [#9036](https://github.com/NuGet/Home/issues/9036)

* Restore:  DependencyGraphSpec.Load(...) does not need JObject - [#9040](https://github.com/NuGet/Home/issues/9040)

* Restore:  closure computed for each project 4 times - [#9042](https://github.com/NuGet/Home/issues/9042)

* Static Graph restore should not pass empty SolutionPath - [#9061](https://github.com/NuGet/Home/issues/9061)

* NuGet.Build.Tasks.Console should have its own unit test assembly - [#9065](https://github.com/NuGet/Home/issues/9065)

* Add unit tests for RestoreTaskExe - [#9067](https://github.com/NuGet/Home/issues/9067)

* Delete TestEnvironment class in SDK resolver test assembly - [#9102](https://github.com/NuGet/Home/issues/9102)