---
title: NuGet 4.6 RTM Release Notes
description: Release notes for NuGet 4.6.0 including known issues, bug fixes, added features, and DCRs.
author: anangaur
ms.author: anangaur
ms.date: 3/7/2018
ms.topic: release-notes
---

# NuGet 4.6 Release Notes

[Visual Studio 2017 15.6 RTW](https://www.visualstudio.com/news/releasenotes/vs2017-relnotes) comes with [NuGet 4.6.0](https://dist.nuget.org/win-x86-commandline/v4.6.0/nuget.exe).

## Summary: What's New in 4.6.0

* We have added support for [signing packages](../create-packages/sign-a-package.md).
* Visual Studio 2017 and nuget.exe now verifies package integrity before installing, restoring packages for [signed packages](../reference/signed-packages-reference.md).
* We have improved performance of successive restores.

## Summary: What's New in 4.6.3

* Security Fix: Permissions on files created inside ~/.nuget are too open [#7673](https://github.com/NuGet/Home/issues/7673) [CVE-2019-0757](https://portal.msrc.microsoft.com/en-us/security-guidance/advisory/CVE-2019-0757)

## Summary: What's New in 4.6.4

* Security Fix: Files inside of NUPKGs can have a relative path above the NUPKG directory [#7906](https://github.com/NuGet/Home/issues/7906)

## Known issues

### Issues with .NET Standard 2.0 with .NET Framework & NuGet 

.NET Standard & its tooling was designed such that projects targeting .NET Framework 4.6.1 can consume NuGet packages & projects targeting .NET Standard 2.0 or earlier. [This document](https://github.com/dotnet/standard/issues/481) summarizes the issues around that scenario, the plan for addressing them, and workarounds you can deploy with today's state of the tooling.

## Top issues fixed in this release

**Performance fixes**

* Don't write asset files when there is no change - [#6491](https://github.com/NuGet/Home/issues/6491)
* Restore causes extra MSBuild evaluations when child projects' TFM do not match with the parent project's - [#6311](https://github.com/NuGet/Home/issues/6311)
* Improve NoOp restore perf by optimizing dependency graph spec creation - [#6252](https://github.com/NuGet/Home/issues/6252)

**Bugs**

* Push to local folder leaves nupkg locked - [#6325](https://github.com/NuGet/Home/issues/6325)
* NuGet Plugin implementation:  multiple issues - [#6149](https://github.com/NuGet/Home/issues/6149)
* UIHang - Remove query service call from MEF initialization in VSSolutionManager - [#6110](https://github.com/NuGet/Home/issues/6110)
* Error reporting exception for cancelled package download task - [#6096](https://github.com/NuGet/Home/issues/6096)
* NuGet.exe replaces '+' with '%2B' in assembly name - [#5956](https://github.com/NuGet/Home/issues/5956)
* Fn+F1 does not take to the right help page for PM UI and Console - [#5912](https://github.com/NuGet/Home/issues/5912)
* VS NuGet writes absolute paths into project files under specific circumstances - [#5888](https://github.com/NuGet/Home/issues/5888)
* Fix 4.3 regression - Placeholders $product$ and $AssemblyGuid$ not replaced in contentfile through transformation - [#5880](https://github.com/NuGet/Home/issues/5880)
* dotnet restore with multiple sources crashes - [#5817](https://github.com/NuGet/Home/issues/5817)
* Pack should re-evaluate project versions to allow git versioning - [#4790](https://github.com/NuGet/Home/issues/4790)
* Improve hard to understand errors when you install an incompatible package - [#4555](https://github.com/NuGet/Home/issues/4555)
* TemplateWizard needs option to install packages as PackageReferences - [#4549](https://github.com/NuGet/Home/issues/4549)
* Package-delivered props files ignored when MSBuild.exe is run from outside a Developer Command Prompt - [#4530](https://github.com/NuGet/Home/issues/4530)
* Fix poor error message when referencing .NET Standard Library that is not applicable to project - [#4423](https://github.com/NuGet/Home/issues/4423)
* dotnet add package fails for package targeting portable profile with little guidance - [#4349](https://github.com/NuGet/Home/issues/4349)
* dotnet pack - version suffix missing from ProjectReference - [#4337](https://github.com/NuGet/Home/issues/4337)
* Build errors and VS crash with .NET Core template - [#3973](https://github.com/NuGet/Home/issues/3973)
* Unable to load the service index for source https:* - [#3681](https://github.com/NuGet/Home/issues/3681)
* nuget.exe list -allversions doesn't work - [#3441](https://github.com/NuGet/Home/issues/3441)
* Misleading dependency resolution error message - [#2984](https://github.com/NuGet/Home/issues/2984)
* nuget.exe restore doesn't produce .props and .targets files for .msbuildproj (regression in v3.3.0-3.4.4 upgrade) - [#2921](https://github.com/NuGet/Home/issues/2921)
* UI delay when updating a NuGet package with XAML file open - [#2878](https://github.com/NuGet/Home/issues/2878)
* WebSite project from IIS fails with illegal characters in path - [#2798](https://github.com/NuGet/Home/issues/2798)
* Nuget add hangs on CentOS - [#2708](https://github.com/NuGet/Home/issues/2708)
* Restore with packagesavemode -nupkg fails for json.net - [#2706](https://github.com/NuGet/Home/issues/2706)
* Package Manager filter not available in vs output window for restore command - [#2704](https://github.com/NuGet/Home/issues/2704)

[List of all issues fixed in this release](https://github.com/NuGet/Home/issues?q=is%3Aissue+is%3Aclosed+milestone%3A%224.6")
