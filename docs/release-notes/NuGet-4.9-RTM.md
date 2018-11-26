---
title: NuGet 4.9.0 RTM Release Notes
description: Release notes for NuGet 4.9.0 including known issues, bug fixes, added features, and DCRs.
author: karann-msft
ms.author: karann
ms.date: 11/20/2018
ms.topic: conceptual
---

# NuGet 4.9.1 RTM Release Notes

[Visual Studio 2017 15.9 RTW](https://www.visualstudio.com/news/releasenotes/vs2017-relnotes) comes with NuGet 4.9.1 functionality.


Command line versions of the same functionality are also available:
* NuGet.exe 4.9.0 - [nuget.org/downloads](https://nuget.org/downloads)
* DotNet.exe - [.NET Core SDK 2.1.500](https://www.microsoft.com/net/download/visual-studio-sdks)


## Summary: What's New in this Release

* NuGet License Expression Model - [#7301](https://github.com/NuGet/Home/issues/7301)

* Improve customer success with NuGet operations - [#7108](https://github.com/NuGet/Home/issues/7108)

* Signing: Enable ClientPolicies to require use of a set of Trusted Authors and Repos listed in NuGet.Config - [#6961](https://github.com/NuGet/Home/issues/6961)

* Enable opt-in "GeneratePathProperty" metadata on a PackageReference to generate a per package MSBuild property to "Foo.Bar\1.0\" directory - [#6949](https://github.com/NuGet/Home/issues/6949)

* create ".snupkg" files to contain symbols in pack -- enhance push to understand nuget protocol to accept snupkg files for symbol server - [#6878](https://github.com/NuGet/Home/issues/6878)

* NuGet credential plugin V2 - [#6642](https://github.com/NuGet/Home/issues/6642)

* Self-Contained NuGet Packages - License - [#4628](https://github.com/NuGet/Home/issues/4628)

## Known issues
### Foo bar something

#### Issue
Some more foo bar

#### Workaround
Foo bar workarond

## Issues fixed in this release

### Bugs

* Warnings elevated to errors (via WarnAsErrors) raised by PackageExtraction should never leave extracted package around - [#7445](https://github.com/NuGet/Home/issues/7445)

* Badly signed packages should not end up in the global packages folder - [#7423](https://github.com/NuGet/Home/issues/7423)

* binding redirect generation should not skip facade assemblies - [#7393](https://github.com/NuGet/Home/issues/7393)

* VersionRange Equals doesn't compare floating ranges - [#7324](https://github.com/NuGet/Home/issues/7324)

* Restore:  performance regression using new .NET Core 2.1 HTTP stack - [#7314](https://github.com/NuGet/Home/issues/7314)

* Update of a Package should not modify PrivateAssets of a PackageReference - [#7285](https://github.com/NuGet/Home/issues/7285)

* Signing:  signing should fail if a package has too many package entries (>65534) - [#7248](https://github.com/NuGet/Home/issues/7248)

* "dotnet nuget push" codepath should support the new credential provider - [#7233](https://github.com/NuGet/Home/issues/7233)

* Support executing plugins with invariant culture (as happens in docker) - [#7223](https://github.com/NuGet/Home/issues/7223)

* nuget sources add should not delete credentials from NuGet.config - [#7200](https://github.com/NuGet/Home/issues/7200)

* installing a devDependency PackageReference should default to excludeassets=compile - [#7084](https://github.com/NuGet/Home/issues/7084)

* fix migrator option to be displayed for all projects and show error if project is incompatible - [#6958](https://github.com/NuGet/Home/issues/6958)

* "dotnet add package" should commit the restore it performs to the assets file - [#6928](https://github.com/NuGet/Home/issues/6928)

* [Test Failure][zh-TW]String "Package Manager Console" doesn't localize on Package Manager Console  - [#6381](https://github.com/NuGet/Home/issues/6381)

* Error message around "Unable to find project information" should be a little more specific inside VS - [#5350](https://github.com/NuGet/Home/issues/5350)

* Unhelpful error message when incorrectly using nuspec version tag of nuget pack - [#2714](https://github.com/NuGet/Home/issues/2714)

### DCR

* Signing:  support NuGet protocol: RepositorySignatures/4.9.0 resource - [#7421](https://github.com/NuGet/Home/issues/7421)

* .nupkg.metadata file will now be created during package extraction - contains "content-hash" - [#7283](https://github.com/NuGet/Home/issues/7283)

* Skip authenticode verification for plugins while executing on Mono - [#7222](https://github.com/NuGet/Home/issues/7222)

### None

* Signing:  improve signing related error messages - [#6906](https://github.com/NuGet/Home/issues/6906)

* PackageReference projects cannot find SemVer 2.0.0 packages  - [#6625](https://github.com/NuGet/Home/issues/6625)

### Problem Data - should be marked as Feature/Bug/DCR or closedAs something
* 7105 Fix flaky test in package manager related to data analytics count labels: Octokit.Label Octokit.Label Octokit.Label 
* 6906 Signing:  improve signing related error messages labels: Octokit.Label Octokit.Label Octokit.Label 
* 6625 PackageReference projects cannot find SemVer 2.0.0 packages  labels: Octokit.Label Octokit.Label Octokit.Label 
* 6518 Fix NuGet's packages' metadata labels: Octokit.Label Octokit.Label Octokit.Label Octokit.Label 


[List of all issues fixed in this release](https://github.com/NuGet/Home/issues?q=is%3Aissue+is%3Aclosed+milestone%3A%224.9")
