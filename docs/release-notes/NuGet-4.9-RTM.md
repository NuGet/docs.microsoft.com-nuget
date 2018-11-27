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


## Summary: What's New 4.9.0

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

* Signing:  improve signing related error messages - [#6906](https://github.com/NuGet/Home/issues/6906)

* [Test Failure][zh-TW]String "Package Manager Console" doesn't localize on Package Manager Console  - [#6381](https://github.com/NuGet/Home/issues/6381)

* Error message around "Unable to find project information" should be a little more specific inside VS - [#5350](https://github.com/NuGet/Home/issues/5350)

* Unhelpful error message when incorrectly using nuspec version tag of nuget pack - [#2714](https://github.com/NuGet/Home/issues/2714)

* DCR - Signing:  support NuGet protocol: RepositorySignatures/4.9.0 resource - [#7421](https://github.com/NuGet/Home/issues/7421)

* DCR - .nupkg.metadata file will now be created during package extraction - contains "content-hash" - [#7283](https://github.com/NuGet/Home/issues/7283)

* DCR - Skip authenticode verification for plugins while executing on Mono - [#7222](https://github.com/NuGet/Home/issues/7222)

### Problem Data - should be marked as Feature/Bug/DCR or closedAs something

* 7105 Fix flaky test in package manager related to data analytics count labels: Octokit.Label Octokit.Label Octokit.Label 
* 6518 Fix NuGet's packages' metadata labels: Octokit.Label Octokit.Label Octokit.Label Octokit.Label 

[List of all issues fixed in this release 4.9](https://github.com/NuGet/Home/issues?q=is%3Aissue+is%3Aclosed+milestone%3A%224.9")

## Summary: What's New 4.9.1

* Add support for reading a writing to the nuget.config via a new command trusted-signers - [#7480](https://github.com/NuGet/Home/issues/7480)

## Known issues
### Foo bar something

#### Issue
Some more foo bar

#### Workaround
Foo bar workarond

## Issues fixed in this release

* 401 Unauthorized during build restore step - [#7527](https://github.com/NuGet/Home/issues/7527)

* [4.9.0] dotnet.exe/nuget.exe doesn't use credentials when source name contains a whitespace - [#7517](https://github.com/NuGet/Home/issues/7517)

* Unable to dotnet restore from a private VSTS feed w/ SDK 2.1.500 - the credentials are not encoded - [#7516](https://github.com/NuGet/Home/issues/7516)

* Fix license link generation - [#7515](https://github.com/NuGet/Home/issues/7515)

* Error codes regression for validating signatures - [#7492](https://github.com/NuGet/Home/issues/7492)

### None

* LicenseAcceptanceWindow and LicenseFileWindow Accessibility issues - [#7452](https://github.com/NuGet/Home/issues/7452)

* NuGet.Build.Tasks.Pack package does not have license information - [#7379](https://github.com/NuGet/Home/issues/7379)

* Should assemblies in /runtimes/{rid}/lib/{tfm} be added as references to target project? - [#7316](https://github.com/NuGet/Home/issues/7316)

### Problem Data - should be marked as Feature/Bug/DCR or closedAs something

* 7452 LicenseAcceptanceWindow and LicenseFileWindow Accessibility issues labels: Octokit.Label Octokit.Label Octokit.Label 
* 7379 NuGet.Build.Tasks.Pack package does not have license information labels: 
* 7316 Should assemblies in /runtimes/{rid}/lib/{tfm} be added as references to target project? labels: Octokit.Label Octokit.Label Octokit.Label 

[List of all issues fixed in this release 4.9.x](https://github.com/NuGet/Home/issues?q=is%3Aissue+is%3Aclosed+milestone%3A%224.9.x")
