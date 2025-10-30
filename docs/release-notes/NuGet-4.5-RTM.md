---
title: NuGet 4.5 RTM Release Notes
description: Release notes for NuGet 4.5 RTM including known issues, bug fixes, added features, and DCRs.
author: anangaur
ms.author: anangaur
ms.date: 12/4/2017
ms.topic: release-notes
---

# NuGet 4.5 Release Notes

[Visual Studio 2017 15.5 RTW](https://www.visualstudio.com/news/releasenotes/vs2017-relnotes) comes with [NuGet 4.5 RTM](https://dist.nuget.org/win-x86-commandline/v4.5.0/nuget.exe).

## Summary: What's New in 4.5.0

## Summary: What's New in 4.5.2

* Security Fix: Permissions on files created inside ~/.nuget are too open [#7673](https://github.com/NuGet/Home/issues/7673) [CVE-2019-0757](https://portal.msrc.microsoft.com/en-us/security-guidance/advisory/CVE-2019-0757)

## Summary: What's New in 4.5.3

* Security Fix: Files inside of NUPKGs can have a relative path above the NUPKG directory [#7906](https://github.com/NuGet/Home/issues/7906)

## Known issues

### Issues with .NET Standard 2.0 with .NET Framework & NuGet 

.NET Standard & its tooling was designed such that projects targeting .NET Framework 4.6.1 can consume NuGet packages & projects targeting .NET Standard 2.0 or earlier. [This document](https://github.com/dotnet/standard/issues/481) summarizes the issues around that scenario, the plan for addressing them, and workarounds you can deploy with today's state of the tooling.

### You are unable to view, add, or update DotNetCLITools, using Nuget Package Manager

#### Issue

NuGet Package Manager does not display and does not allow add/update of DotNetCLITools. [NuGet#4256](https://github.com/NuGet/Home/issues/4256)

#### Workaround

DotNetCLIToolReferences must be manually edited in your project file.

### Retargeting target framework version may lead to incomplete Intellisense

#### Issue

Retargeting target framework version may lead to incomplete Intellisense, in Visual Studio. This happens when you are using PackageReferences as the package manager format. [NuGet#4216](https://github.com/NuGet/Home/issues/4216)

#### Workaround

Do a manual restore.

### A package in a .NET Core project that contains an assembly with an invalid signature, can trigger an infinite restore loop

#### Issue

Occasionally, when you use a package that contains an assembly with an invalid signature or when the package version is set with 'DateTime' ticker, it causes the package auto-restore to run in an infinite loop [dotnet/project-system#1457](https://github.com/dotnet/project-system/issues/1457).

#### Workaround

There is no workaround at this time.

## Issues fixed in NuGet 4.5 RTM timeframe

For issues fixed in NuGet 4.4 RTM, please refer to [NuGet 4.4 RTM Release Notes](../release-notes/nuget-4.4-RTM.md) 

### Features

- Disable auto-push of symbols package - [#6113](https://github.com/NuGet/Home/issues/6113)

### Bugs

- [Regression] in 15.5p1: Portable0.0 is skipped - [#6105](https://github.com/NuGet/Home/issues/6105)
- Assets from packages are missing after restore - [#5995](https://github.com/NuGet/Home/issues/5995)
- Plugin credential providers do not work with URIs containing spaces - [#5982](https://github.com/NuGet/Home/issues/5982)
- If package failed to restore, error should be printed in the output even with Minimal verbosity ON - [#5658](https://github.com/NuGet/Home/issues/5658)
- dotnet
  - dotnetcore restore at solution-level doesn't follow ProjectReference with ReferenceOutputAssembly of false leading to random build failures - [#5490](https://github.com/NuGet/Home/issues/5490)
- Auto-complete in PMC works incorrectly with object methods - [#4800](https://github.com/NuGet/Home/issues/4800)
- nuget.exe restore fails with Visual Studio 2015 toolset - [#4713](https://github.com/NuGet/Home/issues/4713)
- perf - pmc is expensive to instantiate in vs2017 - [#4205](https://github.com/NuGet/Home/issues/4205)
- Slow to get dependency information on slow connection - [#4089](https://github.com/NuGet/Home/issues/4089)
- uninstall-package w/ -RemoveDependencies will fail if multiple packages share a common dependency - [#4026](https://github.com/NuGet/Home/issues/4026)
- Finalize NuGet.Core.nupkg for publishing - [#3581](https://github.com/NuGet/Home/issues/3581)
- NuGet pack resolves dependency ID from directory name when -IncludeProjectReferences is used for csproj + project.json - [#3566](https://github.com/NuGet/Home/issues/3566)
- The type initializer for 'NuGet.ProxyCache' threw an exception - [#3144](https://github.com/NuGet/Home/issues/3144)
- nuget restore performance issue with kudu - [#3087](https://github.com/NuGet/Home/issues/3087)
- UI Client fails to show any error or warning when search is ahead of registration blobs - [#2149](https://github.com/NuGet/Home/issues/2149)
- Get-Packages -Updates generates an incorrect query - [#2135](https://github.com/NuGet/Home/issues/2135)

## Links to GitHub issues fixed in 4.5 RTM

[Issues list](https://github.com/NuGet/Home/issues?q=is%3Aissue+milestone%3A4.5+is%3Aclosed)
