---
title: NuGet 4.8 RTM Release Notes
description: Release notes for NuGet 4.8.1 including known issues, bug fixes, added features, and DCRs.
author: karann-msft
ms.author: karann
ms.date: 5/14/2018
ms.topic: conceptual
---

# NuGet 4.8 RTM Release Notes

[Visual Studio 2017 15.8 RTW](https://www.visualstudio.com/news/releasenotes/vs2017-relnotes) comes with [NuGet 4.8] functionality.

Command line versions of the same functionality are also available:
* NuGet.exe 4.8 is available from [nuget.org/downloads(https://nuget.org/downloads)].
* DotNet.exe also has NuGet functionality and is available with the .NET Core SDK 2.1.400 (https://www.microsoft.com/net/download/visual-studio-sdks)


## Summary: What's New in this Release





* We have augmented package signing to enable [Repository Signed packages](https://github.com/NuGet/Home/wiki/Repository-Signatures)
* With Visual Studio Version 15.7, we have introduced the capability to [migrate existing projects that use the packages.config format to use PackageReference](https://docs.microsoft.com/en-us/nuget/reference/migrate-packages-config-to-package-reference) instead.

## Features
* NuGet now supports authenication plugins which work in all our code paths: MsBuild, DotNet.exe, NuGet.exe and Visual Studio - including cross platform. The first generation of authentication plugins were not supported in MsBuild, DotNet.exe -- including cross platform. Note: VS 2017 15.9 Preview builds have a VSTS authentication plugin included. [#6486](https://github.com/NuGet/Home/issues/6486)
* MsBuild's SDK Resolver now builds as part of NuGet and installs with NuGet tools for VS. This will avoid versions getting out sync. [#6799](https://github.com/NuGet/Home/issues/6799)
* NuGet.exe now supports longfilenames on Windows 10 - [#6937](https://github.com/NuGet/Home/issues/6937)




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
#### Signing
* Signing:  incorrect URL check - [#7174](https://github.com/NuGet/Home/issues/7174)
* Repository signed package verification allows packages signed by different certificate - [#6884](https://github.com/NuGet/Home/issues/6884)
* Signing:  contentUrl MUST be HTTPS - [#6777](https://github.com/NuGet/Home/issues/6777)
* Signing:  SignedPackageVerifierSettings.VSClientDefaultPolicy is unused - [#6601](https://github.com/NuGet/Home/issues/6601)

#### Pack
* restore and build should not be needed when using dotnet.exe to pack nuspec - [#6866](https://github.com/NuGet/Home/issues/6866)
* Allow empty replacement tokens in NuspecProperties  - [#6722](https://github.com/NuGet/Home/issues/6722)


* The serialization of GetAuthenticationCredentialRequest is incorrect - [#6983](https://github.com/NuGet/Home/issues/6983)
* NuGet Visual Studio AsyncPackage fails to load when initialized off the UI thread - [#6976](https://github.com/NuGet/Home/issues/6976)
* RestoreSources are ignored for project with p2p references - [#6776](https://github.com/NuGet/Home/issues/6776)
* Update-Package ignores PackageReference version range - [#6775](https://github.com/NuGet/Home/issues/6775)
* Creating unit test project using .NET Framework template will open older project model with packages.config - [#6736](https://github.com/NuGet/Home/issues/6736)
* Expose NoDefaultExcludes in MSBuild task - [#6450](https://github.com/NuGet/Home/issues/6450)
* PackTask throws NullReferenceException when NuspecProperties is specified - [#4649](https://github.com/NuGet/Home/issues/4649)
* [Test Failure][Accessibility] String ‘Prerelease’ under package button is covered by its package description in PM UI - [#4504](https://github.com/NuGet/Home/issues/4504)
* [Test Failure][Accessibility] Package source drop down and settings button truncated when selecting ‘Microsoft Visual Studio Offline Packages’ in PM UI - [#4502](https://github.com/NuGet/Home/issues/4502)
* "update-package -reinstall" solution wide issue - [#3127](https://github.com/NuGet/Home/issues/3127)
* `Update-Package [packagename] -reinstall` reinstalls all packages instead of just the named one - [#737](https://github.com/NuGet/Home/issues/737)

[List of all issues fixed in this release](https://github.com/NuGet/Home/issues?q=is%3Aissue+is%3Aclosed+milestone%3A%224.8")
