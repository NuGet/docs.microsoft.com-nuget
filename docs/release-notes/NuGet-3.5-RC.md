---
title: 3.5 RC Release Notes
description: Release notes for NuGet 3.5 RC including known issues, bug fixes, added features, and DCRs.
author: JonDouglas
ms.author: jodou
ms.date: 11/11/2016
ms.topic: release-notes
---

# NuGet 3.5 RC Release Notes

[NuGet 3.5-Beta2 Release Notes](../release-notes/nuget-3.5-Beta2.md) | [NuGet 3.5-RTM Release Notes](../release-notes/nuget-3.5-RTM.md)

3.5 release is focused on improving quality and performance of NuGet clients. In addition, we have shipped a few features like support for [Fallback folders](https://github.com/NuGet/Home/issues/2899), [PackageType](https://github.com/NuGet/Home/issues/2476) support in `.nuspec` and more.

[Issues List](https://github.com/NuGet/Home/issues?q=is%3Aissue+is%3Aclosed+milestone%3A%223.5%20RC")

## Bug Fixes

* Install/restore of a package fails with "Package contains multiple `.nuspec` files." - [#3231](https://github.com/NuGet/Home/issues/3231)

* nuget pack forcefully adds `.tt` files to content folder no matter what - [#3203](https://github.com/NuGet/Home/issues/3203)

* nuget pack csproj (with `project.json`) crashes if there are no packOptions and owner in JSON file - [#3180](https://github.com/NuGet/Home/issues/3180)

* nuget pack for `project.json` ignores packOptions tags like summary, authors , owners etc - [#3161](https://github.com/NuGet/Home/issues/3161)

* nuget pack ignores dependencies in output `.nuspec` for `project.json` - [#3145](https://github.com/NuGet/Home/issues/3145)

* Updating multiple packages with rollback leaves the project in a broken state - [#3139](https://github.com/NuGet/Home/issues/3139)

* ContentFiles under any are not added for netstandard projects - [#3118](https://github.com/NuGet/Home/issues/3118)

* Cannot package library targeting .Net Standard correctly - [#3108](https://github.com/NuGet/Home/issues/3108)

* File -> New Project -> Class Library (Portable) project fails in VS2015 and Dev15 - [#3094](https://github.com/NuGet/Home/issues/3094)

* NuGet error - 1.0.0-* is not a valid version string - [#3070](https://github.com/NuGet/Home/issues/3070)

* Find-Package fails to display but Install-Package works - [#3068](https://github.com/NuGet/Home/issues/3068)

* Error when "Install-Package jquery.validation" on dev15 - [#3061](https://github.com/NuGet/Home/issues/3061)

* When installed VS 2015 update 3 on a VS that uses NuGet version 3.5.0 error occurs - [#3053](https://github.com/NuGet/Home/issues/3053)

* Package manager UI: Doesn't display new version after updating a package - [#3041](https://github.com/NuGet/Home/issues/3041)

* -ApiKey on delete command line is not read/sent in 3.5.0-beta - [#3037](https://github.com/NuGet/Home/issues/3037)

* Incorrect string: A stable release of a package should not have on a prerelease dependency. - [#3030](https://github.com/NuGet/Home/issues/3030)

* Creating PCL (net46 and windows 10) project get NullRef exception. - [#3014](https://github.com/NuGet/Home/issues/3014)

* Nuget update should provide informative message when a higher version is restricted by allowedVersions constraint - [#3013](https://github.com/NuGet/Home/issues/3013)

* Credential plugin exited with error -1 / error downloading package when using credential providers with multiple sources - [#2885](https://github.com/NuGet/Home/issues/2885)

* nuget pack - Missing Newtonsoft.Json package dependency - [#2876](https://github.com/NuGet/Home/issues/2876)

* Bug in ExecuteSynchronizedCore on Linux/MacOS + Mono - [#2860](https://github.com/NuGet/Home/issues/2860)

* VS doesn't support environment variables in repositoryPath (nuget.exe does) - [#2763](https://github.com/NuGet/Home/issues/2763)

* Fix Accessibility Issues - [#2745](https://github.com/NuGet/Home/issues/2745)

* Portable frameworks with hyphenated profiles are rejected. - [#2734](https://github.com/NuGet/Home/issues/2734)

* NuGet package manager should make it clear that options list in packages detail does not apply to `project.json` - [#2665](https://github.com/NuGet/Home/issues/2665)

* NuGet 3.3.0 update fails with 'An additional constraint ... defined in packages.config prevents this operation.' - [#1816](https://github.com/NuGet/Home/issues/1816)

* Installing package from a local source that doesn't exist throws a bogus message - [#1674](https://github.com/NuGet/Home/issues/1674)

* "Upgrade vailable" filter shows upgrades that violate the version constraint - [#1094](https://github.com/NuGet/Home/issues/1094)

## Performance Improvements

* Performance: Improve ContentModel target framework parsing - [#3162](https://github.com/NuGet/Home/issues/3162)

* Performance: Avoid reading `runtime.json` files for restores that do not have RIDs [#3150](https://github.com/NuGet/Home/issues/3150). On CI machines, restore of a sample ASP.NET Web Application reduced from over 15 secs to 3 secs.

* Performance: Package Manager Console init.ps1 load time [#2956](https://github.com/NuGet/Home/issues/2956). Time to open PackageManagerConsole improved in some cases from 132s to 10s.

* Solve ReSharper performance issues in NuGet Update - [#3044](https://github.com/NuGet/Home/issues/3044): On a sample project, time taken to install packages reduced from 140s to 68s.

## DCRs

* NuGet needs to let users know that upgrading/installing in a dotnet tfm based PCL could cause issues - [#3138](https://github.com/NuGet/Home/issues/3138)

* Warn bad install/upgrade for project w/ tfm="dotnet" - [#3137](https://github.com/NuGet/Home/issues/3137)

* Add netcoreapp11 and netstandard17 support - [#2998](https://github.com/NuGet/Home/issues/2998)

* Print NuGet-Warning header contents to console in nuget.exe - [#2934](https://github.com/NuGet/Home/issues/2934)

* Leverage AssemblyMetadata attribute for `.nuspec` token replacements - [#2851](https://github.com/NuGet/Home/issues/2851)

* Remove the locked property from the lock file - [#2379](https://github.com/NuGet/Home/issues/2379)

* Symbol packages should not ever be used in install or update #2807

## Features

* Support for fallback package folders - [#2899](https://github.com/NuGet/Home/issues/2899)

* Design and implement a notion of package type to support tool packages - [#2476](https://github.com/NuGet/Home/issues/2476)

* API to get the path to the global packages folder - [#2403](https://github.com/NuGet/Home/issues/2403)

* Native packages update support - [#1291](https://github.com/NuGet/Home/issues/1291)
