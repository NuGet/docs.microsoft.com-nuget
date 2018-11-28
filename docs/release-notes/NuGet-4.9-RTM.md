---
title: NuGet 4.9 RTM Release Notes
description: Release notes for NuGet 4.9 including known issues, bug fixes, new features, and DCRs.
author: karann-msft
ms.author: karann
ms.date: 11/20/2018
ms.topic: conceptual
---

# NuGet 4.9 Release Notes

[Visual Studio 2017 15.9.0 RTW](https://www.visualstudio.com/news/releasenotes/vs2017-relnotes) comes with NuGet 4.9.0 functionality.


Command line versions of the same functionality are also available:
* NuGet.exe 4.9.x - [nuget.org/downloads](https://nuget.org/downloads)
* DotNet.exe - [.NET Core SDK 2.1.500](https://www.microsoft.com/net/download/visual-studio-sdks)


## Summary: What's New in 4.9.0

* Signing: Enable ClientPolicies to require use of a set of Trusted Authors and Repos listed in NuGet.Config - [#6961](https://github.com/NuGet/Home/issues/6961)

* create ".snupkg" files to contain symbols in pack -- enhance push to understand nuget protocol to accept snupkg files for symbol server - [#6878](https://github.com/NuGet/Home/issues/6878)

* NuGet credential plugin V2 - [#6642](https://github.com/NuGet/Home/issues/6642)

* Self-Contained NuGet Packages - License - [#4628](https://github.com/NuGet/Home/issues/4628)

* Enable opt-in "GeneratePathProperty" metadata on a PackageReference to generate a per package MSBuild property to "Foo.Bar\1.0\" directory - [#6949](https://github.com/NuGet/Home/issues/6949)

* Improve customer success with NuGet operations - [#7108](https://github.com/NuGet/Home/issues/7108)

### Issues fixed in this release

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

[List of all issues fixed in this release 4.9.0](https://github.com/NuGet/Home/issues?q=is%3Aissue+is%3Aclosed+milestone%3A%224.9") <br>

## Summary: What's New in 4.9.1

* Add support for reading a writing to the nuget.config via a new command trusted-signers - [#7480](https://github.com/NuGet/Home/issues/7480)

### Issues fixed in this release

* Fix license link generation - [#7515](https://github.com/NuGet/Home/issues/7515)

* Error codes regression for validating signatures - [#7492](https://github.com/NuGet/Home/issues/7492)

* NuGet.Build.Tasks.Pack package does not have license information - [#7379](https://github.com/NuGet/Home/issues/7379)

[List of all issues fixed in this release 4.9.1](https://github.com/NuGet/Home/issues?q=is%3Aissue+is%3Aclosed+milestone%3A%224.9.1")

## Known issues

### dotnet.exe/nuget.exe doesn't use credentials when source name contains a whitespace - [#7517](https://github.com/NuGet/Home/issues/7517)

#### Issue
When there is a whitespace in the source name, nuget.exe throws an error like `The ' ' character, hexadecimal value 0x20, cannot be included in a name.`

#### Workaround
Change the name of the source to not contain a whitespace.

### dotnet nuget push --interactive gives an error on Mac. - [#7519](https://github.com/NuGet/Home/issues/7519)

#### Issue
The `--interactive` argument is not being forwarded by the dotnet cli and results in the error `error: Missing value for option 'interactive'`

#### Workaround
Run any other dotnet command with the interactive option such as `dotnet restore --interactive` and authenticate. The authentication then might be cached by the credential provider. Then run `dotnet nuget push`.

### LicenseAcceptanceWindow and LicenseFileWindow Accessibility issues - [#7452](https://github.com/NuGet/Home/issues/7452)

#### Issue
The license acceptance window and license file window have accessibility issues with keyboard navigation and narration with screen reader and JAWS.

#### Workaround
No workaround.

### Packages in FallbackFolders installed by .NET Core SDK are custom installed, and fail signature validation. - [#7414](https://github.com/NuGet/Home/issues/7414)

#### Issue
When using dotnet 2.x to restore a projet that multi-targets netcoreapp 1.x and netcoreapp 2.x, the fallback folder is treated as a file feed. This means, when restoring, NuGet will pick the package from the fallback folder and try to install it into the global packages folder and do the usual signing validation which fails.

#### Workaround
Disable the usage of the fallback folder by setting the `RestoreAdditionalProjectSources` to nothing. `<RestoreAdditionalProjectSources/>` Use this with caution as it will cause a lot of packages to be downloaded from NuGet.org which otherwise would be have been restored from the fallback folder.
