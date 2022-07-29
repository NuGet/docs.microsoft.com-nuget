---
title: NuGet 4.0 RTM Release Notes
description: Release notes for NuGet 4.0 RTM including known issues, bug fixes, added features, and DCRs.
author: anangaur
ms.author: anangaur
ms.date: 03/03/2017
ms.topic: conceptual
---

# NuGet 4.0 RTM Release Notes

[Visual Studio 2017](https://www.visualstudio.com/news/releasenotes/vs2017-relnotes) comes with NuGet 4.0 which adds support for .NET Core, has a bunch of quality fixes and improves performance. This release also brings several improvements like support for PackageReference, NuGet commands as MSBuild targets, background package restores, and more.

## Known issues

### NuGet restore may fail when you have multiple projects referencing another project in a solution

#### Issue

NuGet restore may not work if, in a solution, you have project references to the same project with different casing or with different relative paths. [NuGet#4574](https://github.com/NuGet/Home/issues/4574)

#### Workaround

Fix the casings or relative paths to be the same for all project references.

### While using Package Manager Console, 'Enter' key may not work

#### Issue

Occasionally, the enter key does not work in the Package Manager Console. If you see this, please check out the progress on the fix, and provide any additional helpful information about your repro steps. [NuGet#4204](https://github.com/NuGet/Home/issues/4204) [NuGet#4570](https://github.com/NuGet/Home/issues/4570)

#### Workaround

Restart Visual Studio and open the PMC before opening the solution. Alternatively, try deleting the `project.lock.json` and restoring again.

### In .NET Core projects, you may end up in infinite restore loop when you use a package containing an assembly with an invalid signature

#### Issue

Occassionally, when you use a package containing an assembly with an invalid signature or when the package version is set with 'DateTime' ticker, it causes package auto-restore to run in infinite loop. [NuGet#4542](https://github.com/NuGet/Home/issues/4542)

#### Workaround

There is no workaround at this time.

### You are unable to view, add, or update DotNetCLITools, using Nuget Package Manager

#### Issue

NuGet Package Manager does not display and does not allow add/update of DotNetCLITools. [NuGet#4256](https://github.com/NuGet/Home/issues/4256)

#### Workaround

DotNetCLIToolReferences must be manually edited in your project file.

### NuGet restore will fail when you set PackageId property for projects

#### Issue

For .NET Core projects, NuGet restore in Visual Studio does not respect PackageId property of projects. [NuGet#4586](https://github.com/NuGet/Home/issues/4586)

#### Workaround

Run restore using the command-line.

### When your project does not have 'obj' folder, package restore may fail

#### Issue

Visual Studio fails to restore PackageReferences when 'obj' folder has been deleted. [NuGet#4528](https://github.com/NuGet/Home/issues/4528)

#### Workaround

Create 'obj' folder manually and the restore should work.

### Manually updating packages using Update-Package in console may fail

#### Issue

Using Update-Package manually in the console only works once for PackageReferences projects that were just converted. [NuGet#4431](https://github.com/NuGet/Home/issues/4431)

#### Workaround

There is no workaround at this time.

### Retargeting target framework version may lead to incomplete Intellisense

#### Issue

Retargeting target framework version may lead to incomplete Intellisense, in Visual Studio. This happens when you are using PackageReferences as the package manager format. [NuGet#4216](https://github.com/NuGet/Home/issues/4216)

#### Workaround

Do a manual restore.

### msbuild /t:restore fails when a project targeting .NET461 references another project targeting .NETStandard

#### Issue

msbuild /t:restore fails when a PackageReferenece based project targeting .NET461 references another PackageReference based project targeting .NETStandard.  [NuGet#4532](https://github.com/NuGet/Home/issues/4532)

#### Workaround

There is no workaround at this time.

## Issues fixed in NuGet 4.0 RTM timeframe

[NuGet 4.0 RC Release Notes](../release-notes/nuget-4.0-RC.md) - Lists all the issues fixed for NuGet 4.0 RC

### Features

- Localize strings in NuGet.Core.sln - [#2041](https://github.com/NuGet/Home/issues/2041)

- Nuget forces to load web application projects in LSL mode - [#4258](https://github.com/NuGet/Home/issues/4258)

- AutoReferenced PackageReference support to block version changes in UI for "sdk installed" packages - [#4044](https://github.com/NuGet/Home/issues/4044)

- Correctly communicate PackageSpec.Version for any project dependencies (PackageRef) - [#3902](https://github.com/NuGet/Home/issues/3902)

- support for removing references into `.csproj` from commandline(s) - [#4101](https://github.com/NuGet/Home/issues/4101)

- Support restore for PackageReference projects (normal and xplat) and Lightweight Solution Load - [#4003](https://github.com/NuGet/Home/issues/4003)

- support for adding references into `.csproj` from commandline(s) - [#3751](https://github.com/NuGet/Home/issues/3751)

- Support NuGet restore for Lightweight Solution Load for `packages.config` or `project.json` - [#3711](https://github.com/NuGet/Home/issues/3711)

- contentFiles support in nuget generated targets file - [#3683](https://github.com/NuGet/Home/issues/3683)

- Establish a Mono CI for nuget.exe validation on Mac using MSBuild - [#3646](https://github.com/NuGet/Home/issues/3646)

- Move NuGet off of v2 NuGet.Core dependencies - [#3645](https://github.com/NuGet/Home/issues/3645)

### Bugs

- NuGet restore in Visual Studio does not respect PackageId property of projects - [#4586](https://github.com/NuGet/Home/issues/4586)

- NuGet ProjectSystemCache error when adding package in vsix package - [#4545](https://github.com/NuGet/Home/issues/4545)

- Pack throws exception if IncludeSource is used in a project with multiple TFMs - [#4536](https://github.com/NuGet/Home/issues/4536)

- VS 2017 RC3 crashes on using update from Solution-wide package management - [#4474](https://github.com/NuGet/Home/issues/4474)

- Cannot uninstall newly installed package  - [#4435](https://github.com/NuGet/Home/issues/4435)

- When migrating to PackageRef, hybrid solutions have strange restore behavior - [#4433](https://github.com/NuGet/Home/issues/4433)

- Building soon after starting NuGet operation (install, update, restore), can cause VS to Hang - [#4420](https://github.com/NuGet/Home/issues/4420)

- UI Hang - Deadlock initializing NuGet.SolutionRestoreManager.RestoreManagerPackage [#4371](https://github.com/NuGet/Home/issues/4371)

- add package command should add version as attribute instead of element - [#4325](https://github.com/NuGet/Home/issues/4325)

- dotnet
  - dotnetcore Restore foo.sln -- fails when configurations in SLN cause duplicate (but diff config) projects in restore graph - [#4316](https://github.com/NuGet/Home/issues/4316)

- Content only packages - [#3668](https://github.com/NuGet/Home/issues/3668)

- By default opt out of package format selector option - [#4468](https://github.com/NuGet/Home/issues/4468)

- Perf: CreateUAP_CSharp_VS.01.1.Create project regressed Duration_TotalElapsedTime by 3,153.570 ms (149.1%). Baseline 26129.02 - [#4452](https://github.com/NuGet/Home/issues/4452)

- Perf: ManagedLangs_CS_DDRIT.0300.Rebuild Solution regressed Duration_TotalElapsedTime by 1.5sec. Baseline 26105 - [#4441](https://github.com/NuGet/Home/issues/4441)

- Nomination fails in multi-TFM projects - [#4419](https://github.com/NuGet/Home/issues/4419)

- Perf: WebForms_DDRIT.1200.Close Solution regressed VM_ImagesInMemory_Total_devenv by 3.000 Count (0.5%). Baseline 26123.04 - [#4408](https://github.com/NuGet/Home/issues/4408)

- vsfeedback - Pack warnings when targeting netcoreapp1.1 - [#4397](https://github.com/NuGet/Home/issues/4397)

- PathTooLongException when trying to add a NuGet package to empty ASP.NET Core web application - [#4391](https://github.com/NuGet/Home/issues/4391)

- Pack runs too often -- dotnet
  - dotnetcore pack fails with There is a circular dependency in the target dependency graph involving target "Pack"  - [#4381](https://github.com/NuGet/Home/issues/4381)

- Pack runs too often -- Generate NuGet package doesn't include all the configurations  - [#4380](https://github.com/NuGet/Home/issues/4380)

- NullReferenceException adding  nuget with packageref in  C++  project - [#4378](https://github.com/NuGet/Home/issues/4378)

- Accessibility : Narrator does not narrate the checkbox to select the projects to install the package to - [#4366](https://github.com/NuGet/Home/issues/4366)

- NuGet VS17 sporadically fails connecting to VSO/VSTS feeds - VS Bug 365798 - [#4365](https://github.com/NuGet/Home/issues/4365)

- contentFiles get output to wrong location if PackagePath specifies path as "contentFiles" - [#4348](https://github.com/NuGet/Home/issues/4348)

- Pack target appends PackageVersion property with VersionSuffix - [#4324](https://github.com/NuGet/Home/issues/4324)

- Specifying package path doesn't work with dotnet pack - [#4321](https://github.com/NuGet/Home/issues/4321)

- NuGet outputs a bunch of warnings about duplicate imports during restore - [#4304](https://github.com/NuGet/Home/issues/4304)

- Choose "NuGet Package Manager Format" dialog looks bad under dark theme - [#4300](https://github.com/NuGet/Home/issues/4300)

- VS crash on build restore - [#4298](https://github.com/NuGet/Home/issues/4298)

- Visual Studio deadlocks if you add TFM in targetframeworks, save, then build. 10% of time - [#4295](https://github.com/NuGet/Home/issues/4295)

- nuget pack does not output success message on packing a project successfully - [#4294](https://github.com/NuGet/Home/issues/4294)

- PackTask fails due to System.IO.Compression 4.1 not being found - [#4290](https://github.com/NuGet/Home/issues/4290)

- Pack runs too often -- PackTask frequently fails with file access conflict - [#4289](https://github.com/NuGet/Home/issues/4289)

- NuGet opens the output window during background restore - [#4274](https://github.com/NuGet/Home/issues/4274)

- Eliminate ServiceProvider as dangerous coding pattern (which can cause hangs) - [#4268](https://github.com/NuGet/Home/issues/4268)

- Perf/UIHang - Improve DownloadTimeoutStream reads - [#4266](https://github.com/NuGet/Home/issues/4266)

- Visual Studio deadlocks if you attempt to close a project before NuGet restore has finished - [#4257](https://github.com/NuGet/Home/issues/4257)

- Issues with PackTask and packing `.nuspec` - [#4250](https://github.com/NuGet/Home/issues/4250)

- [vsfeedback] Cannot resolve nuget packages on new project (needs to restart visual studio) - [#4217](https://github.com/NuGet/Home/issues/4217)

- [vsfeedback] The "Version" drop down that shows available package versions, struggles to stay in-sync with the selected nuGet package...  - [#4198](https://github.com/NuGet/Home/issues/4198)

- Nuget.Client should use CPS JoinableTaskFactory when interacting with CPS to prevent deadlocks - [#4185](https://github.com/NuGet/Home/issues/4185)

- NuGet 3.5.0 not unpacking `.targets` from package - [#4171](https://github.com/NuGet/Home/issues/4171)

- dotnet
  - dotnetcore pack does not support title in `.csproj` - [#4150](https://github.com/NuGet/Home/issues/4150)

- Install-Package results in error dialog in VS2017 RC - [#4127](https://github.com/NuGet/Home/issues/4127)

- Updating a package for .net core project appears to not work, as the UI doesn't get the CPS update from the nominate. - [#4035](https://github.com/NuGet/Home/issues/4035)

- Improve unresolved reference warning - [#3955](https://github.com/NuGet/Home/issues/3955)

- dotnet
  - dotnetcore pack - ProjectReference loses version information - [#3953](https://github.com/NuGet/Home/issues/3953)

- Create UWP app create project & rebuild total elapsed time regressions - [#3873](https://github.com/NuGet/Home/issues/3873)

- Successful restore message is displayed even after error during restore. - [#3799](https://github.com/NuGet/Home/issues/3799)

- re-Publish Nuget.CommandLine 3.4.4 to Nuget.org - [#2931](https://github.com/NuGet/Home/issues/2931)

- On Migrate, projects change from `project.json` to `.csproj` --- restore fails - [#4297](https://github.com/NuGet/Home/issues/4297)

- Restore failing on newly created xunit Test project  - [#4296](https://github.com/NuGet/Home/issues/4296)

- Core projects can hang, lock up UI on open - [#4269](https://github.com/NuGet/Home/issues/4269)

- fix targets file for build tasks - [#4267](https://github.com/NuGet/Home/issues/4267)

- Error list has error after build solution which unload the referenced project - [#4208](https://github.com/NuGet/Home/issues/4208)

- MSB4057: The target "_GenerateRestoreGraphProjectEntry" does not exist in the project. - [#4194](https://github.com/NuGet/Home/issues/4194)

- vsfeedback: nuget manager ui for solution crashes when you select all projects - [#4191](https://github.com/NuGet/Home/issues/4191)

- nuget.exe msbuildpath fails when there is a trailing slash - [#4180](https://github.com/NuGet/Home/issues/4180)

- vsfeedback: NuGet restore give several project reference warnings for LinqToTwitter project - [#4156](https://github.com/NuGet/Home/issues/4156)

- Pack from `.csproj` does not include the minClientVersion attribute  - [#4135](https://github.com/NuGet/Home/issues/4135)

- NuGet.Build.Tasks.Pack.dll shipped delay signed in VS2017 (d15rel 26014.00) - [#4122](https://github.com/NuGet/Home/issues/4122)

- VSFeedback: Restore fails for a VS 2015 project generated with CMake 3.7.1 - [#4114](https://github.com/NuGet/Home/issues/4114)

- VSFeedback: Restore errors can obscure more complete error messages that build could give - [#4113](https://github.com/NuGet/Home/issues/4113)

- [VSFeedback] Error occurred while restoring NuGet packages for website project: Value cannot be null. - [#4092](https://github.com/NuGet/Home/issues/4092)

- Migration Throws "Object reference Exception" in NuGet.PackageManagement.VisualStudio.SolutionRestoreWorker - [#4067](https://github.com/NuGet/Home/issues/4067)

- dotnet
  - dotnetcore pack should pack tools with the versions that the package was built against - [#4063](https://github.com/NuGet/Home/issues/4063)

- New background restore writes milliseconds to status bar when it takes seconds to restore - [#4036](https://github.com/NuGet/Home/issues/4036)

- Typo on failed to resolve all project references - [#4018](https://github.com/NuGet/Home/issues/4018)

- Enable PCM workflows in package reference scenarios - [#4016](https://github.com/NuGet/Home/issues/4016)

- Can not find installed packages in package manager UI - [#4015](https://github.com/NuGet/Home/issues/4015)

- dotnet
  - dotnetcore pack fails when PackagePath is empty - [#3993](https://github.com/NuGet/Home/issues/3993)

- Restore task fails in an multi user scenario - [#3897](https://github.com/NuGet/Home/issues/3897)

- Cannot change Content type when packing using NuGet Pack Task - [#3895](https://github.com/NuGet/Home/issues/3895)

- Default Copy of ContentFiles are incorrect for MsBuild /t:pack - [#3894](https://github.com/NuGet/Home/issues/3894)

- Install package restore double logs the restoring packages message - [#3785](https://github.com/NuGet/Home/issues/3785)

- Remove Guardrails - Restore of "runtimes" section should only apply to the current project - [#3768](https://github.com/NuGet/Home/issues/3768)

- Pack task puts content files in both 'content/' and 'contentFiles/' - [#3718](https://github.com/NuGet/Home/issues/3718)

- dotnet
  - dotnetcore pack3 does extra tag splitting - [#3701](https://github.com/NuGet/Home/issues/3701)

- dotnet
  - dotnetcore pack: packing projects with package references results in duplicate import warning - [#3665](https://github.com/NuGet/Home/issues/3665)

- Restore logging in VS doesn't always show - [#3633](https://github.com/NuGet/Home/issues/3633)

- nuget locals help text still mentioned packages cache - [#3592](https://github.com/NuGet/Home/issues/3592)

- Restore3 couples PackageReferences with TargetFrameworks. - [#3504](https://github.com/NuGet/Home/issues/3504)

- Nuget picks unexpected version of MSBuild in VS "15" Preview 4 dev. command prompt - [#3408](https://github.com/NuGet/Home/issues/3408)

- Write out targets/props files on failed restore - [#3399](https://github.com/NuGet/Home/issues/3399)

- NuGet during restore doesn't respect the same compat shims as MSBuild when running in VS 15 command prompt - [#3387](https://github.com/NuGet/Home/issues/3387)

- Re-enable PackFromProjectWithDevelopmentDependencySet for VS15 - [#3272](https://github.com/NuGet/Home/issues/3272)

- Blend problems with NuGet - [#4043](https://github.com/NuGet/Home/issues/4043)

- Integrate 4.0.0.2067 into CLI and SDK repos to ship with RC2 - [#4029](https://github.com/NuGet/Home/issues/4029)

- VS Hangs when you Create new Core Console App, Close Solution, Open Solution and Close Solution  - [#4008](https://github.com/NuGet/Home/issues/4008)

- Hitting hang opening project against d15prerel.25916.01 - [#3982](https://github.com/NuGet/Home/issues/3982)

- Fix dotnet/nuget.exe locals doc/help message - [#3919](https://github.com/NuGet/Home/issues/3919)

- Inspect PackTask for issues with trailing or leading whitespace - [#3906](https://github.com/NuGet/Home/issues/3906)

- dotnet
  - dotnetcore pack is packing from obj not bin - [#3880](https://github.com/NuGet/Home/issues/3880)

- dotnet
  - dotnetcore pack always seems to set ProjectReference version to 1.0.0 - [#3874](https://github.com/NuGet/Home/issues/3874)

- dotnet
  - dotnetcore pack fails with project references and \<TargetFramework\> - [#3865](https://github.com/NuGet/Home/issues/3865)

- LockRecursionException in ProjectSystemCache.TryGetProjectNameByShortName - [#3861](https://github.com/NuGet/Home/issues/3861)

- Trim whitespace from MSBuild properties - [#3819](https://github.com/NuGet/Home/issues/3819)

- Consolidate the two project events raised on project load - [#3759](https://github.com/NuGet/Home/issues/3759)

- P2P libraries in `project.assets.json` file have incorrect Version - [#3748](https://github.com/NuGet/Home/issues/3748)

- Restore crash due to unresponsive feed and unavailable package - [#3672](https://github.com/NuGet/Home/issues/3672)

- nuget.exe could hang on a large amount of MSBuild error output - [#3572](https://github.com/NuGet/Home/issues/3572)

- Restore-on-build for Blend fails first time, succeeds second time (VS scenario fixed) - [#2121](https://github.com/NuGet/Home/issues/2121)

### DCRs

- migrate vsix from v2 vsix to v3 vsix - [#4196](https://github.com/NuGet/Home/issues/4196)

- NuGet should have a mechanism for getting the path to the lock file in MSBuild - [#3351](https://github.com/NuGet/Home/issues/3351)

- Add build assets to the TFM compatibility check and assets file - [#3296](https://github.com/NuGet/Home/issues/3296)

- Define a new ProjectCapability "Pack" in Pack targets for enabling Package related capabilities - [#4146](https://github.com/NuGet/Home/issues/4146)

- Run Pack as a post build target conditioned on "GeneratePackageOnBuild" MSBuild property - [#4145](https://github.com/NuGet/Home/issues/4145)

- Use NuGet property RestoreProjectStyle to create specific NuGet project - [#4134](https://github.com/NuGet/Home/issues/4134)

- Adapt Restore for Transitive Project References change - [#4076](https://github.com/NuGet/Home/issues/4076)

- Add NuGet properties in target file for non-UWP projects - [#4030](https://github.com/NuGet/Home/issues/4030)

- UWP TargetPlatformVersion support - [#3923](https://github.com/NuGet/Home/issues/3923)

- Communicate project reference metadata to NuGet project system - [#3922](https://github.com/NuGet/Home/issues/3922)

- Add UI for packaging mode - [#3921](https://github.com/NuGet/Home/issues/3921)

- Legacy `.csproj` needs NugetTargetMoniker and RuntimeIdentifiers set in proj/targets - [#3854](https://github.com/NuGet/Home/issues/3854)

- Install package may overlap with auto-restore - [#3836](https://github.com/NuGet/Home/issues/3836)

- Context menu QueryStatus doesn't happen when VSPackage is not loaded - [#3835](https://github.com/NuGet/Home/issues/3835)

- Solution Restore and Build Restore still show dialogs - [#3789](https://github.com/NuGet/Home/issues/3789)

- Isolate VSSDK version in NuGet.Clients solution build - [#3890](https://github.com/NuGet/Home/issues/3890)

## Links to GitHub issues fixed in RTM
[Issues list 1](https://github.com/NuGet/Home/issues?q=is%3Aissue+is%3Aclosed+milestone%3A%224.0%20RTM")  
[Issues list 2](https://github.com/NuGet/Home/issues?q=is%3Aissue+is%3Aclosed+milestone%3A%224.0%20RC4")  
[Issues list 3](https://github.com/NuGet/Home/issues?q=is%3Aissue+is%3Aclosed+milestone%3A%224.0%20RC3")  
[Issues list 4](https://github.com/NuGet/Home/issues?q=is%3Aissue+is%3Aclosed+milestone%3A%224.0%20RC2")  
[Issues list 5](https://github.com/NuGet/Home/issues?q=is%3Aissue+is%3Aclosed+milestone%3A%224.0%20RC")
