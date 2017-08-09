---
# required metadata

title: NuGet 4.0 RC Release Notes | Microsoft Docs
author: anangaur
ms.author: anangaur
manager: unnir
ms.date: 03/03/2017
ms.topic: article
ms.prod: nuget
#ms.service:
ms.technology: null
ms.assetid: 906cc4dd-7948-4e86-a093-21df830ce8c3

# optional metadata

description: Release notes for NuGet 4.0 RTM including known issues, bug fixes, added features, and DCRs.
keywords: NuGet 4.0 RTM release notes, bug fixes, known issues, added features, DCRs
#ROBOTS:
#audience:
#ms.devlang:
ms.reviewer:
- karann
- unnir
#ms.suite:
#ms.tgt_pltfrm:
#ms.custom:

---

# 4.3 Release Notes

[Visual Studio 2017](https://www.visualstudio.com/news/releasenotes/vs2017-relnotes) comes with NuGet 4.3 which adds support for .NET Core, has a bunch of quality fixes and improves performance. This release also brings several improvements like support for PackageReference, NuGet commands as MSBuild targets, background package restores, and more.

## Known issues

### NuGet restore may fail when you have multiple projects referencing another project in a solution

#### Issue:
NuGet restore may not work if, in a solution, you have project references to the same project with different casing or with different relative paths. [NuGet#4574](https://github.com/NuGet/Home/issues/4574)

#### Workaround:
Fix the casings or relative paths to be the same for all project references.


## Issues fixed in NuGet 4.3 RTM timeframe

**Bug:**

* msbuild /t:pack fails with The "DevelopmentDependency" parameter is not supported by the "PackTask" task - [#5584](https://github.com/NuGet/Home/issues/5584)

* nuget.org not added to %AppData%\NuGet\nuget.config if file already contains a packageSource - [#5477](https://github.com/NuGet/Home/issues/5477)

* NU1201 for netcoreapp2.0 -> net461 project dependency using new csproj format/sdk, but not for classic csproj - [#5461](https://github.com/NuGet/Home/issues/5461)

* SDK based projects, with SDK 2.0 restore causes a build error - [#5449](https://github.com/NuGet/Home/issues/5449)

* build\native\PackageName.targets isn't imported after installing package in C++ project - [#5439](https://github.com/NuGet/Home/issues/5439)

* Missing nuget restore error messages in VS Package Manager output window - [#5412](https://github.com/NuGet/Home/issues/5412)

* Regression: msbuild /t:pack fails with `The file '' to be packed was not found on disk.` - [#5408](https://github.com/NuGet/Home/issues/5408)

* NU1201 Does Not Produce a Library Id - [#5405](https://github.com/NuGet/Home/issues/5405)

* Restore on .NET Core 2.0 project imports .NET Framework 3.5 package targets - [#5149](https://github.com/NuGet/Home/issues/5149)

* [Test Failure] Some projects can't be created after installing NuGet client 4.3.0-preview1-2462 (Insertable) under dev folder - [#4952](https://github.com/NuGet/Home/issues/4952)

* msbuild /t:restore refuses to restore one particular project from a solution - [#4845](https://github.com/NuGet/Home/issues/4845)

* Directory structure for content files flattened if not adding Windows directory separator at the end of PackagePath - [#4795](https://github.com/NuGet/Home/issues/4795)

* Pack targets don't support setting developmentDependency - [#4694](https://github.com/NuGet/Home/issues/4694)

* RestoreManagerPackage being loaded synchronously which blocked UI thread and deadlocked VS - [#4679](https://github.com/NuGet/Home/issues/4679)

* If your solution has projectreferences that refer to the same project, with different casing, restore may not work. This also affects different relative paths, without a difference in casing - [#4574](https://github.com/NuGet/Home/issues/4574)

* Executables restored from NuGet packages are no longer executable with .NET Core "2.0" - [#4424](https://github.com/NuGet/Home/issues/4424)

* NuGet.exe swallows details of exception when parsing solution file - [#4411](https://github.com/NuGet/Home/issues/4411)

* Pack puts content files in wrong location if ContentTargetFolders contains a path that ends with '/' on Windows - [#4407](https://github.com/NuGet/Home/issues/4407)

* Can't restore a DotNetCliToolReference for a tools package that targets netcoreapp1.1 - [#4396](https://github.com/NuGet/Home/issues/4396)

* Nuget update CLI leaves the old package version condition in project file (C++) - [#2449](https://github.com/NuGet/Home/issues/2449)

**DCR:**

* Read DotnetCliToolTargetFramework  from CPS nomation - [#5397](https://github.com/NuGet/Home/issues/5397)

* TPMinV check should work for pj style UWP - [#4763](https://github.com/NuGet/Home/issues/4763)

* Improve UI description for AutoReferenced packages - [#4471](https://github.com/NuGet/Home/issues/4471)

* NuGet restore is selecting compile assets from runtime section. - [#4207](https://github.com/NuGet/Home/issues/4207)

* Put dependency diagnostics in the lock file - [#1599](https://github.com/NuGet/Home/issues/1599)

**Docs:**

* MSBuild restore and NuGet.config - [#5043](https://github.com/NuGet/Home/issues/5043)

* AutoReferenced docs and fwlink - [#4470](https://github.com/NuGet/Home/issues/4470)

**Feature:**

* dotnet.exe - additional nuget functionality - update - [#4965](https://github.com/NuGet/Home/issues/4965)

* NET Core 2.0: VS/Dotnet CLI should start using existing NuGet functionality: FallBack folders - [#4939](https://github.com/NuGet/Home/issues/4939)

* NET Core 2.0: Enable users to ignore specific restore warnings (or elevate to error) - [#4898](https://github.com/NuGet/Home/issues/4898)

* NET Core 2.0: CLI localized assemblies - [#4896](https://github.com/NuGet/Home/issues/4896)

* NET Core 2.0: register all warnings/errors to assets file (including PackageTargetFallback) - [#4895](https://github.com/NuGet/Home/issues/4895)

* Enable TFM support: NetStandard2.0, Tizen, Unity - [#4892](https://github.com/NuGet/Home/issues/4892)

* Reduce the number of NuGet.Core and NuGet.Client projects - [#2446](https://github.com/NuGet/Home/issues/2446)

* Add ability to mark nuget warnings as errors - [#2395](https://github.com/NuGet/Home/issues/2395)

## Links to GitHub issues fixed in RTM

[Issues List](https://github.com/NuGet/Home/issues?q=is%3Aissue+is%3Aclosed+milestone%3A%224.3")
