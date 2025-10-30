---
title: NuGet 4.3 RTM Release Notes
description: Release notes for NuGet 4.3 RTM including known issues, bug fixes, added features, and DCRs.
author: JonDouglas
ms.author: jodou
ms.date: 08/14/2017
ms.topic: release-notes
ms.reviewer: anangaur
---

# NuGet 4.3 Release Notes

[Visual Studio 2017 15.3 RTW](https://www.visualstudio.com/news/releasenotes/vs2017-relnotes) comes with NuGet 4.3 RTM which adds support for new scenarios such as .NET Standard 2.0/.NET Core 2.0, contains many quality fixes, and improves performance. This release also brings several improvements like support for Semantic Versioning 2.0.0, MSBuild integration of NuGet warnings and errors, and more.

## Summary: What's New in 4.3.0

## Summary: What's New in 4.3.1

* Security Fix: Permissions on files created inside ~/.nuget are too open [#7673](https://github.com/NuGet/Home/issues/7673) [CVE-2019-0757](https://portal.msrc.microsoft.com/en-us/security-guidance/advisory/CVE-2019-0757)
* Security Fix: Files inside of NUPKGs can have a relative path above the NUPKG directory [#7906](https://github.com/NuGet/Home/issues/7906)

## Known issues

### NuGet restore may treat disabled package sources as enabled in some cases

#### Issue

The following restore command-line techniques treat disabled packages sources as enabled. [NuGet#5704](https://github.com/NuGet/Home/issues/5704)
- `msbuild /t:restore`
- `dotnet restore` (either with dotnet.exe that ships with VS, or the one that comes with NetCore SDK 2.0.0)

#### Workaround

1. Use Visual Studio (2017 15.3 or later) or NuGet.exe (v4.3.0 or later)
1. Delete your disabled source and continue to use msbuild or dotnet.exe.
1. For your solution, you could use "Clear" in NuGet.config and then define the sources necessary for that solution.

### While using Package Manager Console, 'Enter' key may not work

#### Issue

Occasionally, the enter key does not work in the Package Manager Console. If you see this, please check out the progress on the fix, and provide any additional helpful information about your repro steps. [NuGet#4204](https://github.com/NuGet/Home/issues/4204) [NuGet#4570](https://github.com/NuGet/Home/issues/4570)

#### Workaround

Restart Visual Studio and open the PMC before opening the solution. Alternatively, try deleting the `project.lock.json` and restoring again.

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

## Issues fixed in NuGet 4.3 RTM timeframe

[NuGet 4.0 RTM Release Notes](../release-notes/nuget-4.0-RTM.md) - Lists all the issues fixed for NuGet 4.0 RTM

### Features

- Improve NuGet Restore Perf - Implement smarter NoOp for command line restores and VS - [#5080](https://github.com/NuGet/Home/issues/5080)

- NET Core 2.0: VS/Dotnet CLI should start using existing NuGet functionality: FallBack folders - [#4939](https://github.com/NuGet/Home/issues/4939)

- NET Core 2.0: Enable users to ignore specific restore warnings (or elevate to error) - [#4898](https://github.com/NuGet/Home/issues/4898)

- NET Core 2.0: CLI localized assemblies - [#4896](https://github.com/NuGet/Home/issues/4896)

- NET Core 2.0: register all warnings/errors to assets file (including PackageTargetFallback) - [#4895](https://github.com/NuGet/Home/issues/4895)

- Enable TFM support: NetStandard2.0, Tizen - [#4892](https://github.com/NuGet/Home/issues/4892)

- Reduce the number of NuGet.Core and NuGet.Client projects (and thus DLLs) - [#2446](https://github.com/NuGet/Home/issues/2446)

- Add ability to mark nuget warnings as errors - [#2395](https://github.com/NuGet/Home/issues/2395)

### Bugs

- msbuild /t:pack fails with The "DevelopmentDependency" parameter is not supported by the "PackTask" task - [#5584](https://github.com/NuGet/Home/issues/5584)

- Directory structure for content files flattened if not adding Windows directory separator at the end of PackagePath - [#4795](https://github.com/NuGet/Home/issues/4795)

- netcore projects don't support setting as developmentDependency - [#4694](https://github.com/NuGet/Home/issues/4694)

- RestoreManagerPackage being loaded synchronously which blocked UI thread and deadlocked VS - [#4679](https://github.com/NuGet/Home/issues/4679)

- dotnet
  - dotnetcore Restore (& therefore msbuild /t:restore) skips projects with an explicit solution project dependency [#4578](https://github.com/NuGet/Home/issues/4578)

- If your solution has projectreferences that refer to the same project, with different casing, restore may not work. This also affects different relative paths, without a difference in casing - [#4574](https://github.com/NuGet/Home/issues/4574)

- Executables restored from NuGet packages are no longer executable with .NET Core 2.0 - [#4424](https://github.com/NuGet/Home/issues/4424)

- NuGet.exe swallows details of exception when parsing solution file - [#4411](https://github.com/NuGet/Home/issues/4411)

- Pack puts content files in wrong location if ContentTargetFolders contains a path that ends with '/' on Windows - [#4407](https://github.com/NuGet/Home/issues/4407)

- Can't restore a DotNetCliToolReference for a tools package that targets netcoreapp1.1 - [#4396](https://github.com/NuGet/Home/issues/4396)

- Nuget update CLI leaves the old package version condition in project file (C++) - [#2449](https://github.com/NuGet/Home/issues/2449)

### DCRs

- Read DotnetCliToolTargetFramework from CPS nomation - [#5397](https://github.com/NuGet/Home/issues/5397)

- TPMinV check should work for pj style UWP - [#4763](https://github.com/NuGet/Home/issues/4763)

- Improve UI description for AutoReferenced packages - [#4471](https://github.com/NuGet/Home/issues/4471)

- NuGet restore is selecting compile assets from runtime section. - [#4207](https://github.com/NuGet/Home/issues/4207)

- Put dependency diagnostics in the lock file - [#1599](https://github.com/NuGet/Home/issues/1599)

## Links to GitHub issues fixed in 4.3 RTM

[Issues List](https://github.com/NuGet/Home/issues?q=is%3Aissue+is%3Aclosed+milestone%3A%224.3")
