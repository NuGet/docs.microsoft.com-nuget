---
title: NuGet 4.8 RTM Release Notes
description: Release notes for NuGet 4.8.1 including known issues, bug fixes, added features, and DCRs.
author: JonDouglas
ms.author: jodou
ms.date: 5/14/2018
ms.topic: release-notes
---

# NuGet 4.8 Release Notes

[Visual Studio 2017 15.8 RTW](https://www.visualstudio.com/news/releasenotes/vs2017-relnotes) comes with NuGet 4.8 functionality.


Command line versions of the same functionality are also available:
* NuGet.exe 4.8 - [nuget.org/downloads](https://nuget.org/downloads)
* DotNet.exe - [.NET Core SDK 2.1.400](https://www.microsoft.com/net/download/visual-studio-sdks)


## Summary: What's New in 4.8.0
* NuGet.exe now supports longfilenames on Windows 10 - [#6937](https://github.com/NuGet/Home/issues/6937)
* Authentication plugins now work across MsBuild, DotNet.exe, NuGet.exe and Visual Studio, including cross platform. The first generation of authentication plugins were not supported in MsBuild, DotNet.exe. Note: VS 2017 15.9 Preview builds have a VSTS authentication plugin included. [#6486](https://github.com/NuGet/Home/issues/6486)
* MsBuild's SDK Resolver now builds as part of NuGet and installs with NuGet tools for VS. This will avoid versions getting out sync. [#6799](https://github.com/NuGet/Home/issues/6799)
* PackageReference now supports DevelopmentDependency metadata - [#4125](https://github.com/NuGet/Home/issues/4125)

## Summary: What's New in 4.8.2

* Security Fix: Permissions on files created inside ~/.nuget are too open [#7673](https://github.com/NuGet/Home/issues/7673) [CVE-2019-0757](https://portal.msrc.microsoft.com/en-us/security-guidance/advisory/CVE-2019-0757)

## Known issues
### Installing signed packages on a CI machine or in an offline environment takes longer than usual

#### Issue
If the machine has restricted internet access (such as a build machine in a CI/CD scenario), installing/restoring a signed nuget package will result a warning ([NU3028](../reference/errors-and-warnings/nu3028.md)) since the revocation servers are not reachable. This is expected. However, in some cases, this may have unintended concequences such as the package install/restore taking longer than usual.

#### Workaround
Update to Visual Studio 15.8.4 and NuGet.exe 4.8.1 where we introduced an environment variable to switch the revocation check mode.
Setting the `NUGET_CERT_REVOCATION_MODE` environment variable to `offline` will force NuGet to check the revocation status of the certificate only against the cached certificate revocation list, and NuGet will not attempt to reach revocation servers. When the revocation check mode is set to `offline`, the warning will be downgraded to an info.

> [!Warning]
> It is not recommended to switch the revocation check mode to offline under normal cirumstances. Doing so will cause NuGet to skip online revocation check and perform only an offline revocation check against the cached certificate revocation list which may be out of date. This means packages where the signing certificate may have been revoked, will continue to be installed/restored, which otherwise would have failed revocation check and would not have been installed.

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
Note: This has been fixed in VS 2017 15.9 Preview 3

## Issues fixed in this release

### Bugs
#### Signing
* Signing: Installing signed package in offline environment [#7008](https://github.com/NuGet/Home/issues/7008) -- Fixed in 4.8.1
* Signing:  incorrect URL check - [#7174](https://github.com/NuGet/Home/issues/7174)
* Signing:  check package integrity in RepositorySignatureVerifier when package is repository countersigned - [#6926](https://github.com/NuGet/Home/issues/6926)
* "Package Integrity check failed." should have package ID in message (and error code) - [#6944](https://github.com/NuGet/Home/issues/6944)
* Repository signed package verification allows packages signed by different certificate - [#6884](https://github.com/NuGet/Home/issues/6884)
* NuGet - Signing - Timestamp URL can not be https:// ? - [#6871](https://github.com/NuGet/Home/issues/6871)
* Don't NullRef in NuSpec packing scenario, also improve options - [#6866](https://github.com/NuGet/Home/issues/6866)
* Memory is invalid while updating signer info when adding timestamp to countersignature - [#6840](https://github.com/NuGet/Home/issues/6840)
* Signing:  remove CTL exceptions - [#6794](https://github.com/NuGet/Home/issues/6794)
* Signing:  contentUrl MUST be HTTPS - [#6777](https://github.com/NuGet/Home/issues/6777)
* Signing:  SignedPackageVerifierSettings.VSClientDefaultPolicy is unused - [#6601](https://github.com/NuGet/Home/issues/6601)


#### Pack
* restore and build should not be needed when using dotnet.exe to pack nuspec - [#6866](https://github.com/NuGet/Home/issues/6866)
* Allow empty replacement tokens in NuspecProperties  - [#6722](https://github.com/NuGet/Home/issues/6722)
* PackTask throws NullReferenceException when NuspecProperties is specified - [#4649](https://github.com/NuGet/Home/issues/4649)

#### Accessibility
* [Accessibility] String ‘Prerelease’ under package button is covered by its package description in PM UI - [#4504](https://github.com/NuGet/Home/issues/4504)
* [Accessibility] Package source drop down and settings button truncated when selecting ‘Microsoft Visual Studio Offline Packages’ in PM UI - [#4502](https://github.com/NuGet/Home/issues/4502)

#### Powershell Management Console (PMC)
* `Update-Package` ignores PackageReference version range - [#6775](https://github.com/NuGet/Home/issues/6775)
* `Update-Package -reinstall` solution wide issue - [#3127](https://github.com/NuGet/Home/issues/3127)
* `Update-Package [packagename] -reinstall` reinstalls all packages instead of just the named one - [#737](https://github.com/NuGet/Home/issues/737)
* Can update to unlisted NuGet package from the Package Manager Console - [#4553](https://github.com/NuGet/Home/issues/4553)

#### Misc
* To fix `NuGet update self` NuGet.Commandline nupkg should not be semver2.0 - [#7116](https://github.com/NuGet/Home/issues/7116)
* Improve experiences with NU1107 install failures - [#7107](https://github.com/NuGet/Home/issues/7107)
* The serialization of GetAuthenticationCredentialRequest is incorrect - [#6983](https://github.com/NuGet/Home/issues/6983)
* NuGet Visual Studio AsyncPackage fails to load when initialized off the UI thread - [#6976](https://github.com/NuGet/Home/issues/6976)
* Restore is reporting misleading errors stating project.json is needed - [#6959](https://github.com/NuGet/Home/issues/6959)
* Package manager UI : preview changes, ok button not automatically useable by keyboard - [#6893](https://github.com/NuGet/Home/issues/6893)
* RestoreSources are ignored for project with p2p references - [#6776](https://github.com/NuGet/Home/issues/6776)
* Creating unit test project using .NET Framework template will open older project model with packages.config - [#6736](https://github.com/NuGet/Home/issues/6736)
* allow project reference to override package dependency - [#6536](https://github.com/NuGet/Home/issues/6536)
* Expose NoDefaultExcludes in MSBuild task - [#6450](https://github.com/NuGet/Home/issues/6450)
* Status message for "Clear All NuGet Cache(s)" can be hidden on window resize - [#5938](https://github.com/NuGet/Home/issues/5938)


[List of all issues fixed in this release](https://github.com/NuGet/Home/issues?q=is%3Aissue+is%3Aclosed+milestone%3A%224.8")
