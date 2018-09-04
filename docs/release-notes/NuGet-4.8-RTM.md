---
title: NuGet 4.8 RTM Release Notes
description: Release notes for NuGet 4.8.1 including known issues, bug fixes, added features, and DCRs.
author: karann-msft
ms.author: karann
ms.date: 5/14/2018
ms.topic: conceptual
---

# NuGet 4.7 RTM Release Notes

[Visual Studio 2017 15.8 RTW](https://www.visualstudio.com/news/releasenotes/vs2017-relnotes) comes with [NuGet 4.8.0](https://dist.nuget.org/win-x86-commandline/v4.8.0/nuget.exe).

## Summary: What's New in this Release

* We have augmented package signing to enable [Repository Signed packages](https://github.com/NuGet/Home/wiki/Repository-Signatures)

* With Visual Studio Version 15.7, we have introduced the capability to [migrate existing projects that use the packages.config format to use PackageReference](https://docs.microsoft.com/en-us/nuget/reference/migrate-packages-config-to-package-reference) instead.

## Features
**TODO - convert features to summary of what's new in this release**

* Consider how to solve the credential plugin + proxy scenario - [#6923](https://github.com/NuGet/Home/issues/6923)
* Package 'Microsoft.ApplicationInsights.AzureWebSites' installing failing with ASP.NET project - [#6892](https://github.com/NuGet/Home/issues/6892)
* Bug jail for 15.8 - [#6882](https://github.com/NuGet/Home/issues/6882)
* NuGet client perf measurement - [#6879](https://github.com/NuGet/Home/issues/6879)
* ship msbuild sdk resolver as part of nuget.client - [#6799](https://github.com/NuGet/Home/issues/6799)
* Add caching of capabilities for the plugin - [#6796](https://github.com/NuGet/Home/issues/6796)
* Consider adding a logging contract to the plugin protocol - [#6762](https://github.com/NuGet/Home/issues/6762)
* Address perf cost of discovering plugins - [#6744](https://github.com/NuGet/Home/issues/6744)
* Protocol Cleanup - timeouts, plugin ordering, instantiation xplat - [#6706](https://github.com/NuGet/Home/issues/6706)
* Discovery of plugins - [#6704](https://github.com/NuGet/Home/issues/6704)

## Known issues
**TODO - below is a sample known issue**

### The `Migrate packages.config to PackageReference...` option is not available in the right-click context menu

#### Issue

When a project is first opened, NuGet may not have initialized until a NuGet operation is performed. This causes the migration option to not show up in the right-click context menu on `packages.config` or `References`.

#### Workaround

Perform any one of the following NuGet actions:
* Open the Package Manager UI - Right-click on `References` and select `Manage NuGet Packages...`
* Open the Package Manager Console - From `Tools > NuGet Package Manager`, select `Package Manager Console`
* Run NuGet restore - Right-click on the solution node in the Solution Explorer and select `Restore NuGet Packages`
* Build the project which also triggers NuGet restore

You should now be able to see the migration option. Note that this option is not supported and will not show up for ASP.NET and C++ project types.

## Top issues fixed in this release
**TODO - Triage the list below**

### Bugs

* Signing:  incorrect URL check - [#7174](https://github.com/NuGet/Home/issues/7174)
* The serialization of GetAuthenticationCredentialRequest is incorrect - [#6983](https://github.com/NuGet/Home/issues/6983)
* NuGet Visual Studio AsyncPackage fails to load when initialized off the UI thread - [#6976](https://github.com/NuGet/Home/issues/6976)
* Fix NuGet.exe to support longfilenames on Windows 10 - [#6937](https://github.com/NuGet/Home/issues/6937)
* Repository signed package verification allows packages signed by different certificate - [#6884](https://github.com/NuGet/Home/issues/6884)
* restore and build should not be needed when using dotnet.exe to pack nuspec - [#6866](https://github.com/NuGet/Home/issues/6866)
* Signing:  contentUrl MUST be HTTPS - [#6777](https://github.com/NuGet/Home/issues/6777)
* RestoreSources are ignored for project with p2p references - [#6776](https://github.com/NuGet/Home/issues/6776)
* Update-Package ignores PackageReference version range - [#6775](https://github.com/NuGet/Home/issues/6775)
* Creating unit test project using .NET Framework template will open older project model with packages.config - [#6736](https://github.com/NuGet/Home/issues/6736)
* Allow empty replacement tokens in NuspecProperties  - [#6722](https://github.com/NuGet/Home/issues/6722)
* Signing:  SignedPackageVerifierSettings.VSClientDefaultPolicy is unused - [#6601](https://github.com/NuGet/Home/issues/6601)
* Expose NoDefaultExcludes in MSBuild task - [#6450](https://github.com/NuGet/Home/issues/6450)
* PackTask throws NullReferenceException when NuspecProperties is specified - [#4649](https://github.com/NuGet/Home/issues/4649)
* [Test Failure][Accessibility] String ‘Prerelease’ under package button is covered by its package description in PM UI - [#4504](https://github.com/NuGet/Home/issues/4504)
* [Test Failure][Accessibility] Package source drop down and settings button truncated when selecting ‘Microsoft Visual Studio Offline Packages’ in PM UI - [#4502](https://github.com/NuGet/Home/issues/4502)
* "update-package -reinstall" solution wide issue - [#3127](https://github.com/NuGet/Home/issues/3127)
* `Update-Package [packagename] -reinstall` reinstalls all packages instead of just the named one - [#737](https://github.com/NuGet/Home/issues/737)

[List of all issues fixed in this release](https://github.com/NuGet/Home/issues?q=is%3Aissue+is%3Aclosed+milestone%3A%224.8")
