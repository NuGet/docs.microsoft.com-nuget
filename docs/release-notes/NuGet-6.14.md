---
title: NuGet 6.14 Release Notes
description: Release notes for NuGet 6.14 including new features, bug fixes, and DCRs.
author: zivkan
ms.date: 5/6/2025
ms.topic: conceptual
---

# NuGet 6.14 Release Notes

NuGet distribution vehicles:

| NuGet version | Available in Visual Studio version | Available in .NET SDK(s) |
|:---|:---|:---|
| [**6.14.0**](https://nuget.org/downloads) | [Visual Studio 2022 version 17.14.1](https://visualstudio.microsoft.com/downloads/) | [9.0.300](https://dotnet.microsoft.com/download/dotnet/9.0)<sup>1</sup> |

<sup>1</sup> Installed with Visual Studio 2022 with any .NET workload

## Summary: What's New in 6.14.0

* Event tracing in new dependency resolver - [#14134](https://github.com/NuGet/Home/issues/14134)

* Support for new slnx format in static graph-based restore - [#14086](https://github.com/NuGet/Home/issues/14086)

* Add Net90 to FrameworkConstants.CommonFrameworks - [#14067](https://github.com/NuGet/Home/issues/14067)

* NuGet needs to onboard to Unified Settings and create General page - [#14040](https://github.com/NuGet/Home/issues/14040)

* Support for new `slnx` solution format - [#14034](https://github.com/NuGet/Home/issues/14034)

* dotnet-nuget-push: does not support --config-file included in examples - [#4879](https://github.com/NuGet/Home/issues/4879)

### Issues fixed in this release

* Don&#39;t show usage help when dotnet cli command throws unhandled exception - [#14200](https://github.com/NuGet/Home/issues/14200)

* Multiple callers check for NuGet entries before calling Error List `BringToFrontIfSettingsPermitAsync` - [#14163](https://github.com/NuGet/Home/issues/14163)

* NuGetAudit should report transitive packages with vulnerabilities when targeting .NET 10 or higher - [#14161](https://github.com/NuGet/Home/issues/14161)

* Update README preview to render with same font size as Visual Studio - [#14112](https://github.com/NuGet/Home/issues/14112)

* NU5100 (AssemblyOutsideLibWarning): Why build is allowed but buildTransitive is not? - [#14080](https://github.com/NuGet/Home/issues/14080)

* Can&#39;t copy the text from NuGet package manager gold bar - [#14074](https://github.com/NuGet/Home/issues/14074)

* `dotnet list package --vulerable` should support auditSources - [#13767](https://github.com/NuGet/Home/issues/13767)

* [DCR]: Focus shifts from Output window to Error List after every package operation, even with no error or warnings - [#11728](https://github.com/NuGet/Home/issues/11728)

* NuGet 6.13.2.1 does no longer support prerelease versions - [#14219](https://github.com/NuGet/Home/issues/14219)

* Badly specified framework leads to an uninformative error - [#14216](https://github.com/NuGet/Home/issues/14216)

* Restore should fail more quickly when using http sources - [#14210](https://github.com/NuGet/Home/issues/14210)

* README spins indefinitely if the Readme URI does not result in a readme - [#14201](https://github.com/NuGet/Home/issues/14201)

* Error in Visual Studio if Path contains directory you do not have permission to view - [#14192](https://github.com/NuGet/Home/issues/14192)

* list package doesn&#39;t with a solution argument in 9.0.201 - [#14177](https://github.com/NuGet/Home/issues/14177)

* Focus shifts from Output window to Error List after Clear NuGet Locals command - [#14157](https://github.com/NuGet/Home/issues/14157)

* NuGet adding a bunch of generally unuseful information to VS activity log - [#14153](https://github.com/NuGet/Home/issues/14153)

* NuGet authentication plug-in discovery fails when environment variable has trailing semicolon - [#14144](https://github.com/NuGet/Home/issues/14144)

* nuget.exe fails to find Microsoft.VisualStudio.SolutionPersistence.dll - [#14136](https://github.com/NuGet/Home/issues/14136)

* NuGet restore writes dgspec too frequently - [#14135](https://github.com/NuGet/Home/issues/14135)

* dotnet list package does not display resolved versions when AuditSources are used - [#14116](https://github.com/NuGet/Home/issues/14116)

* The REAMD tab always shows “Loading README” for the latest version of the package in the detail panel of PM UI - [#14098](https://github.com/NuGet/Home/issues/14098)

* [Bug Bash] The second time clicking ‘Installed’ tab for a remote source which doesn’t allow for downloading a README shows the README tab - [#14097](https://github.com/NuGet/Home/issues/14097)

* HttpFileSystemBasedFindPackageByIdResource.ConsumeFlatContainerIndexAsync allocates significantly more than necessary - [#14095](https://github.com/NuGet/Home/issues/14095)

* UnresolvedMessages.GetMessageAsync is allocating more heavily than necessary - [#14094](https://github.com/NuGet/Home/issues/14094)

* New dependency resolver does not properly detect a cycle with a transitive dependency with the same name as the root project - [#14052](https://github.com/NuGet/Home/issues/14052)

* dotnet nuget why does not give an error if only a project path was specified - [#14030](https://github.com/NuGet/Home/issues/14030)

* Cleanup ServiceProviderExtensions, remove GetFreeThreadedServiceAsync - [#14007](https://github.com/NuGet/Home/issues/14007)

* Value cannot be null. Parameter name: versionRange when opening the PM UI - [#13933](https://github.com/NuGet/Home/issues/13933)

* Reenable new algorithm resolution with lock files - [#13800](https://github.com/NuGet/Home/issues/13800)

* Report the path when unable to read corrupted .nupkg.metadata  - [#13763](https://github.com/NuGet/Home/issues/13763)

* [Bug Bash] The vulnerability InfoBar disappears in the Solution Explorer window after restoring packages for .NET SDK based project - [#13318](https://github.com/NuGet/Home/issues/13318)

* Use System.Text.Json to read  the cache file in CacheFileFormat - [#13059](https://github.com/NuGet/Home/issues/13059)

* [Bug]: dotnet nuget push symbols not working as expected - [#11871](https://github.com/NuGet/Home/issues/11871)

* nuget.exe restore fails when MSBuildPath ends with a slash - [#8634](https://github.com/NuGet/Home/issues/8634)

* nuget.exe -msbuildpath c:\foo\msbuild.exe gives bad error experience - [#4195](https://github.com/NuGet/Home/issues/4195)

* Address comments in `Implement Support for NuGet Authentication Plugins as .NET Tools` PR - [#13975](https://github.com/NuGet/Home/issues/13975)

[List of commits in this release](https://github.com/NuGet/NuGet.Client/compare/6.13.2.1...6.14.0.116)

### Community contributions

Thank you to all the contributors who helped make this NuGet release awesome!

* [ViktorHofer](https://github.com/NuGet/NuGet.Client/pull/6309)
  * [6309](https://github.com/NuGet/NuGet.Client/pull/6309) Define MicrosoftVisualStudioSolutionPersistenceVersion property
  * [6292](https://github.com/NuGet/NuGet.Client/pull/6292) Upgrade ProtectedData version when building source-only
  * [6270](https://github.com/NuGet/NuGet.Client/pull/6270) Update dependencies and remove unused runtime dependencies
  * [6261](https://github.com/NuGet/NuGet.Client/pull/6261) React to NuGet package pruning warnings
* [jkoritzinsky](https://github.com/NuGet/NuGet.Client/pull/6306)
  * [6306](https://github.com/NuGet/NuGet.Client/pull/6306) Don't publish RID-agnostic nuget packages when we only want RID-specific artifacts
  * [6233](https://github.com/NuGet/NuGet.Client/pull/6233) Use the documented Artifact extension point to add artifacts
* [ToddGrun](https://github.com/NuGet/NuGet.Client/pull/6294)
  * [6294](https://github.com/NuGet/NuGet.Client/pull/6294) Reduce activity log output from VsSolutionRestoreService.NominateProjectAsync
  * [6264](https://github.com/NuGet/NuGet.Client/pull/6264) Reduce allocations under UnresolvedMessages.GetMessageAsync
* [AlexDelepine](https://github.com/NuGet/NuGet.Client/pull/6338)
  * [6338](https://github.com/NuGet/NuGet.Client/pull/6338) Update ngen Priorities for VS
* [mmitche](https://github.com/NuGet/NuGet.Client/pull/6305)
  * [6305](https://github.com/NuGet/NuGet.Client/pull/6305) Set build number to arcade build revision for VMR builds
* [premun](https://github.com/NuGet/NuGet.Client/pull/6251)
  * [6251](https://github.com/NuGet/NuGet.Client/pull/6251) Remove extra spaces in Publishing.props
* [baronfel](https://github.com/NuGet/NuGet.Client/pull/6219)
  * [6219](https://github.com/NuGet/NuGet.Client/pull/6219) Use new serializer library to parse solution files
