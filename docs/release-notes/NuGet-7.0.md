---
title: NuGet 7.0 Release Notes
description: Release notes for NuGet 7.0 including new features, bug fixes, and DCRs.
author: donnie-msft
ms.date: 10/29/2025
ms.topic: conceptual
---

# NuGet 7.0 Release Notes

NuGet distribution vehicles:

| NuGet version | Available in Visual Studio version | Available in .NET SDK(s) |
|:---|:---|:---|
| [**7.0.0**](https://nuget.org/downloads) | [Visual Studio 2026 version 18.0.0](https://visualstudio.microsoft.com/downloads/) | [10.0.100](https://dotnet.microsoft.com/download/dotnet/10.0)<sup>1</sup> |

<sup>1</sup> Installed with Visual Studio 2026 with any .NET workload


## Summary: What's New in 7.0.0

* Details pane reflects Vulnerability Data from Audit Sources - [#14554](https://github.com/NuGet/Home/issues/14554)

* When adding an http source in the options dialog, have the user click on a checkbox to more explicitly agree to AllowInsecureConnections being added - [#14377](https://github.com/NuGet/Home/issues/14377)

* Error needed in Package Sources settings for HTTP source without AllowInsecureConnections - [#14367](https://github.com/NuGet/Home/issues/14367)

* Implement programmatic validation API for Unified Settings - [#14359](https://github.com/NuGet/Home/issues/14359)

* Use Regex UI validation on NuGet options pages - [#14358](https://github.com/NuGet/Home/issues/14358)

* Create Unified Settings page for &quot;Package Source Mapping&quot; - [#14234](https://github.com/NuGet/Home/issues/14234)

* Create Unified Settings page for &quot;Package sources&quot; - [#14233](https://github.com/NuGet/Home/issues/14233)

* NuGet AuditSources support in the Package Manager UI - [#13954](https://github.com/NuGet/Home/issues/13954)

* dotnet update package --vulnerable (Audit fix) - [#13372](https://github.com/NuGet/Home/issues/13372)

* [Feature]: dotnet list [project | solution] package does not work with solution filters - [#11789](https://github.com/NuGet/Home/issues/11789)

### Breaking changes

### Issues fixed in this release

* Build NuGet.exe with il-repack instead of ilmerge - [#14497](https://github.com/NuGet/Home/issues/14497)

* Don&#39;t use reflection based deserialization in NuGet.Protocol - [#14470](https://github.com/NuGet/Home/issues/14470)

* Convert Search Control to Fluent UI - [#14469](https://github.com/NuGet/Home/issues/14469)

* Make NuGet.Client&#39;s Build.ps1 more friendly to GitHub Copilot in VSCode - [#14453](https://github.com/NuGet/Home/issues/14453)

* unused NuGet VS Extensibility APIs removed - [#14403](https://github.com/NuGet/Home/issues/14403)

* Remove all unused APIs marked as obsolete in NuGet.Frameworks, NuGet.Protocol, NuGet.Commands &amp; NuGet.PackageManagement - [#14395](https://github.com/NuGet/Home/issues/14395)

* Remove all unused APIs marked as obsolete in NuGet.Frameworks, NuGet.Protocol, NuGet.Commands &amp; NuGet.PackageManagement - [#14394](https://github.com/NuGet/Home/issues/14394)

* Remove obsolete APIs from NuGet.Common, NuGet.Configuration, NuGet.LibraryModel, NuGet.Packaging and NuGet.ProjectModel - [#14393](https://github.com/NuGet/Home/issues/14393)

* dotnet nuget verify should output package content hash - [#14384](https://github.com/NuGet/Home/issues/14384)

* Generate identical [Content_Types].xml on repeated builds - [#14357](https://github.com/NuGet/Home/issues/14357)

* Consider not writing NuGetToolVersion to generated MSBuild props files on Restore - [#14355](https://github.com/NuGet/Home/issues/14355)

* Package pruning is enabled for all projects targeting .NET 10 including multi-targetes ones. - [#14345](https://github.com/NuGet/Home/issues/14345)

* Remove NUGET_EXPERIMENTAL_USE_NJ_FOR_FILE_PARSING - [#14257](https://github.com/NuGet/Home/issues/14257)

* Pruning should prune and not warn for a direct reference in a multi-targeting project that does not prune on all TargetFrameworks - [#14196](https://github.com/NuGet/Home/issues/14196)

* JsonSerializerIsReflectionDisabled on update to Nuget.Protocols 6.13.1 in apps with JsonSerializerIsReflectionEnabledByDefault set to false - [#14111](https://github.com/NuGet/Home/issues/14111)

* Enable packing legacy PackageReference projects without the need for a package (ie merge NuGet.Build.Tasks.Pack into NuGet.Build.Tasks) - [#14046](https://github.com/NuGet/Home/issues/14046)

* Enable CanShowDialog for .NET core Authentication Plugins - [#14010](https://github.com/NuGet/Home/issues/14010)

* [DCR] Raise an error for SHA-1 fingerprints usage in NuGet.exe sign, mssign commands - [#13962](https://github.com/NuGet/Home/issues/13962)

* Promote NU3043 warning to error in .NET 10 - [#13814](https://github.com/NuGet/Home/issues/13814)

* Show an error when a non https source is used in a resource in a service index - [#13364](https://github.com/NuGet/Home/issues/13364)

* Remove project.json pack - [#7931](https://github.com/NuGet/Home/issues/7931)

* Remove project.json support - [#7199](https://github.com/NuGet/Home/issues/7199)

* Clean up Package Spec redudant APIs - [#6231](https://github.com/NuGet/Home/issues/6231)

* &quot;dotnet package update&quot; modifies wrong project file (csproj) - [#14585](https://github.com/NuGet/Home/issues/14585)

* VS crashes when the only project in the solution is a project.json project - [#14553](https://github.com/NuGet/Home/issues/14553)

* [Localization] The table title ‘Package Source Mapping’ in the ‘Options-&gt;NuGet Package Manager-&gt;Package Source Mapping’ page was not localized - [#14550](https://github.com/NuGet/Home/issues/14550)

* Remove RestoreTargetGraph.Name as it&#39;s redundant withg restoreTargetGraph.TargetGraphName being the widely used version - [#14529](https://github.com/NuGet/Home/issues/14529)

* Remove RestoreArgs.LockFileVersion as it&#39;s functionality unused - [#14524](https://github.com/NuGet/Home/issues/14524)

* pack legacy csproj: include pack targets and tasks in VS build tools - [#14520](https://github.com/NuGet/Home/issues/14520)

* Change _RestorePackagePruningDefault  to RestorePackagePruningDefault - [#14511](https://github.com/NuGet/Home/issues/14511)

* Remove NUGET_BULK_RESTORE_COORDINATION and NUGET_SOLUTION_CACHE_INITIALIZATION fallbacks - [#14502](https://github.com/NuGet/Home/issues/14502)

* [Bug Bash] The “Options-&gt;NuGet Package Manager-&gt;Package Sources” page is disabled after checking or unchecking the checkbox “Enabled” of any one of the package sources having duplicated sources - [#14499](https://github.com/NuGet/Home/issues/14499)

* Use Fluent TextBox for Project PM UI Installed Version - [#14466](https://github.com/NuGet/Home/issues/14466)

* Dead Code: ActionsAndVersions View - [#14464](https://github.com/NuGet/Home/issues/14464)

* Have to manually select pre-populated text on Add Package Source dialog - [#14450](https://github.com/NuGet/Home/issues/14450)

* Remove PackageSpec.Dependencies - [#14446](https://github.com/NuGet/Home/issues/14446)

* review exception handling - [#14440](https://github.com/NuGet/Home/issues/14440)

* Improve perf by avoiding redundant dictionary lookups - [#14432](https://github.com/NuGet/Home/issues/14432)

* Move pruning enabled frameworks to the NuGet.targets - [#14424](https://github.com/NuGet/Home/issues/14424)

* Add package ID validation during restore - [#14407](https://github.com/NuGet/Home/issues/14407)

* Decommission Legacy VS Options NuGet Settings - [#14398](https://github.com/NuGet/Home/issues/14398)

* LockFileLibrary does not need to be mutable - [#14385](https://github.com/NuGet/Home/issues/14385)

* VS should not delete Package Source attributes when Name is updated - [#14370](https://github.com/NuGet/Home/issues/14370)

* NuGet Restore fails if SQL Server Management Studio 21 is installed - [#14349](https://github.com/NuGet/Home/issues/14349)

* dotnet package update should support --verbosity - [#14319](https://github.com/NuGet/Home/issues/14319)

* dotnet package update should support CPM and VersionOverride - [#14318](https://github.com/NuGet/Home/issues/14318)

* dotnet package update should support multiple packages - [#14308](https://github.com/NuGet/Home/issues/14308)

* dotnet package update should support package source mapping - [#14307](https://github.com/NuGet/Home/issues/14307)

* dotnet package update to a specific version - [#14306](https://github.com/NuGet/Home/issues/14306)

* dotnet package update initial version - [#14305](https://github.com/NuGet/Home/issues/14305)

* Package pruning doesn&#39;t work with lock files - [#14272](https://github.com/NuGet/Home/issues/14272)

* Block and remove code for unused restore implementations such as `Standalone`. - [#14184](https://github.com/NuGet/Home/issues/14184)

* Remove `DotnetToolReference` restore - [#14183](https://github.com/NuGet/Home/issues/14183)

* Warning rollout for PrunePackageReference - [#14126](https://github.com/NuGet/Home/issues/14126)

* [Bug Bash][Unstable] An error “Attempted to divide by zero.” occurs when executing command “dotnet list [ProjectPath] package --vulnerable” - [#14122](https://github.com/NuGet/Home/issues/14122)

* Missing audit warnings from &quot;nuget install&quot; when nuget.org is not a package source - [#14096](https://github.com/NuGet/Home/issues/14096)

* Improve NU1004 when pruning is used with locked mode - [#14075](https://github.com/NuGet/Home/issues/14075)

* It&#39;s not possible to push to HTTP sources specified via command line - [#14047](https://github.com/NuGet/Home/issues/14047)

* New dependency resolver does not properly handle floating prerelease versions - [#13833](https://github.com/NuGet/Home/issues/13833)

* Reenable new algorithm resolution with lock files - [#13800](https://github.com/NuGet/Home/issues/13800)

* [Bug Bash] Pressing the page-down button on the keyboard when focusing on ‘Version’ drop-down box with Tab key makes the box empty - [#13605](https://github.com/NuGet/Home/issues/13605)

* [Bug Bash] [Unstable] The first removing of a source mapping from the ‘Package Source Mappings’ list in ‘Package Source Mapping’ dialog doesn’t work   - [#13520](https://github.com/NuGet/Home/issues/13520)

* dotnet restore/Visual Studio conflicting with .esproj + Nx project.json - [#13512](https://github.com/NuGet/Home/issues/13512)

* [Bug Bash] The offline package source cannot be enabled after disabling it from the ‘Machine-wide package sources’ source list previously in the ‘Options-&gt;NuGet Package Manager-&gt;Package Sources’ window - [#13434](https://github.com/NuGet/Home/issues/13434)

* [Bug Bash] The “source” column of the “Add New Package Source Mapping” dialog doesn’t have the minimum width set which makes it can be dragged out of sight - [#13355](https://github.com/NuGet/Home/issues/13355)

* [Bug Bash] The first reopening of Options dialog should bring back the default package source &quot;Microsoft Visual Studio Offline Packages&quot; in &quot;package sources&quot; list when all the sources were deleted previously - [#13278](https://github.com/NuGet/Home/issues/13278)

* [Bug Bash] Newly added package source mapping shouldn’t be case-sensitive in “Option-&gt;NuGet Package Manager-&gt;Package Source Mapping” window - [#13210](https://github.com/NuGet/Home/issues/13210)

* [Bug Bash] New added package source mapping will lost after switching back to the &quot;Package Source Mapping&quot; tab from other tab in “Option-&gt;NuGet Package Manager” window  - [#13150](https://github.com/NuGet/Home/issues/13150)

* [Bug Bash] The “Remove” button should be disable when no package source mapping is selected in the “Package Source Mappings” list  - [#13115](https://github.com/NuGet/Home/issues/13115)

* Stop ilmerging pack - [#13079](https://github.com/NuGet/Home/issues/13079)

* [Bug Bash] There will be a duplicated package source when modifying the name of source “Microsoft Visual Studio Offline Packages” - [#13057](https://github.com/NuGet/Home/issues/13057)

* [Bug Bash] There is a tiny gap in the version drop-down list of PM UI - [#11990](https://github.com/NuGet/Home/issues/11990)

* [Bug Bash] The dropdown list of PM UI doesn’t distinguish the background color between selected-item and hover-on item - [#10977](https://github.com/NuGet/Home/issues/10977)

* [CSY] Duplicated hotkeys show in “Options-&gt;NuGet Package Manager-&gt;Package Sources” dialog - [#7822](https://github.com/NuGet/Home/issues/7822)

* VS NuGet PMUI - Machine-wide package sources area should be vertically resize-able as well - [#7560](https://github.com/NuGet/Home/issues/7560)

[List of commits in this release](https://github.com/NuGet/NuGet.Client/compare/6.15.0.86...7.0.0.289)

### Community contributions

Thank you to all the contributors who helped make this NuGet release awesome!

* [SimonCropp](https://github.com/NuGet/NuGet.Client/pull/6720)
  * [6720](https://github.com/NuGet/NuGet.Client/pull/6720) remove redundant default constructors
  * [6610](https://github.com/NuGet/NuGet.Client/pull/6610) add "does not have a min version" to messages in GetNupkgInfo
  * [6581](https://github.com/NuGet/NuGet.Client/pull/6581) remove redundant dictionary lookups in MarkTransitiveOrigin
  * [6596](https://github.com/NuGet/NuGet.Client/pull/6596) enable nullability in PackageItemViewModelTests
  * [6634](https://github.com/NuGet/NuGet.Client/pull/6634) remove IFrameworkTargetable
  * [6616](https://github.com/NuGet/NuGet.Client/pull/6616) avoid redundant version parsing in PluginFindPackageByIdResource
  * [6595](https://github.com/NuGet/NuGet.Client/pull/6595) enable nullability in ReadmePreviewViewModelTests
  * [6587](https://github.com/NuGet/NuGet.Client/pull/6587) remove redundant dictionary lookup in ProcessUnrankedEntries
  * [6598](https://github.com/NuGet/NuGet.Client/pull/6598) enable nullability in EmbeddedResourcesCapabilityTests
  * [6577](https://github.com/NuGet/NuGet.Client/pull/6577) use fields instead of private properties
  * [6622](https://github.com/NuGet/NuGet.Client/pull/6622) remove un-used list in GetPackagesToBeReinstalled
  * [6589](https://github.com/NuGet/NuGet.Client/pull/6589) redundant null check for projectManagerService.GetMetadataAsync return value
  * [6605](https://github.com/NuGet/NuGet.Client/pull/6605) enable nullable in VSRestoreSettingsUtilityTests
  * [6575](https://github.com/NuGet/NuGet.Client/pull/6575) remove redundant dictionary lookup in CredentialServiceAdapter.GetCredentials
  * [6601](https://github.com/NuGet/NuGet.Client/pull/6601) enable nullabe in NuGetInstallCommandTest
  * [6593](https://github.com/NuGet/NuGet.Client/pull/6593) enable nullable in IProjectContextInfoExtensionsTests
  * [6614](https://github.com/NuGet/NuGet.Client/pull/6614) remove redundant lists in DependencyGraphFileRequestProvider
  * [6625](https://github.com/NuGet/NuGet.Client/pull/6625) remove un-used variables in PackageExtractor
  * [6609](https://github.com/NuGet/NuGet.Client/pull/6609) fix nullability in UnresolvedMessages
  * [6608](https://github.com/NuGet/NuGet.Client/pull/6608) remove un-used contextForGather
  * [6628](https://github.com/NuGet/NuGet.Client/pull/6628) remove redundant enumeration in CredentialsItem
  * [6617](https://github.com/NuGet/NuGet.Client/pull/6617) remove toolItems  list instance in MSBuildRestoreUtility
  * [6599](https://github.com/NuGet/NuGet.Client/pull/6599) fix parameter nullability in Constructor_SetReportAbuseUrl_Initialize…
  * [6604](https://github.com/NuGet/NuGet.Client/pull/6604) enable nullable in LegacyPackageReferenceProjectTests
  * [6631](https://github.com/NuGet/NuGet.Client/pull/6631) remove DependencyGraphSpecRequestProvider.CollectReferences
  * [6635](https://github.com/NuGet/NuGet.Client/pull/6635) remove EnvDteProjectExtensions.PathComparer
  * [6623](https://github.com/NuGet/NuGet.Client/pull/6623) remove redundant GetManifestResourceNames in ManifestSchemaUtility
  * [6619](https://github.com/NuGet/NuGet.Client/pull/6619) remove unused packageID variable 
  * [6620](https://github.com/NuGet/NuGet.Client/pull/6620) remove un-used variables in PackageManagerControl
  * [6640](https://github.com/NuGet/NuGet.Client/pull/6640) remove redundant exception handling
  * [6637](https://github.com/NuGet/NuGet.Client/pull/6637) remove PackageManagerControl.AddMigratorBar
  * [6629](https://github.com/NuGet/NuGet.Client/pull/6629) remove un-used HashSet instance in ResolverMetadataClient
  * [6602](https://github.com/NuGet/NuGet.Client/pull/6602) enable nullable in MSBuildUtilityTest
  * [6641](https://github.com/NuGet/NuGet.Client/pull/6641) remove redundant null condition in UpdateCommand.ExecuteCommandAsync
  * [6683](https://github.com/NuGet/NuGet.Client/pull/6683) remove redundant string alloc in GetTempFilePath
  * [6626](https://github.com/NuGet/NuGet.Client/pull/6626) remove un-used variables in SignedPackageArchiveIOUtility
  * [6574](https://github.com/NuGet/NuGet.Client/pull/6574) remove redundant dictionary lookup in CreatePackageSourceMappingDictionary
  * [6594](https://github.com/NuGet/NuGet.Client/pull/6594) enable nullability in InfiniteScrollListTests
  * [6682](https://github.com/NuGet/NuGet.Client/pull/6682) Use async delay in SafeReadAsync
  * [6600](https://github.com/NuGet/NuGet.Client/pull/6600) enable nullable in NuGetUpdateCommandTests
  * [6606](https://github.com/NuGet/NuGet.Client/pull/6606) enable nullable in VSNominationUtilitiesTests
  * [6613](https://github.com/NuGet/NuGet.Client/pull/6613) remove redundant type variable in GetExternalProject
  * [6611](https://github.com/NuGet/NuGet.Client/pull/6611) remove un-used solutionService instances
  * [6643](https://github.com/NuGet/NuGet.Client/pull/6643) remove MsBuildUtility.GetMsBuildPathInPathVar
  * [6632](https://github.com/NuGet/NuGet.Client/pull/6632) remove PackageSpecFactory.GetTargetFrameworkStrings
  * [6573](https://github.com/NuGet/NuGet.Client/pull/6573) avoid some allocation in ProjectFactory.ProcessDependencies
  * [6588](https://github.com/NuGet/NuGet.Client/pull/6588) remove dictionary lookups from GetPreviewResultsAsync
  * [6597](https://github.com/NuGet/NuGet.Client/pull/6597) fix nullability in PackageModelCreationTestHelper
  * [6612](https://github.com/NuGet/NuGet.Client/pull/6612) remove un-used list instances in NuGetPackageManager
  * [6591](https://github.com/NuGet/NuGet.Client/pull/6591) enable nullable in PackageSourceValidatorTests
  * [6603](https://github.com/NuGet/NuGet.Client/pull/6603) fix nullability in IVsProjectBuildProperties
  * [6636](https://github.com/NuGet/NuGet.Client/pull/6636) remove _project* fields from PackageReferenceProject
  * [6630](https://github.com/NuGet/NuGet.Client/pull/6630) remove un-used identity instance in LocalV3FindPackageByIdResource
  * [6618](https://github.com/NuGet/NuGet.Client/pull/6618) remove LoggerAdapter instance in NuGetPackageManager
  * [6621](https://github.com/NuGet/NuGet.Client/pull/6621) remove un-used projectsByUniqueName in SolutionUpToDateChecker
  * [6624](https://github.com/NuGet/NuGet.Client/pull/6624) remove un-used variables in PackageBuilder
  * [6633](https://github.com/NuGet/NuGet.Client/pull/6633) remove JsonPackageSpecReader DelimitedStringSeparators  and VersionSeparators
  * [6642](https://github.com/NuGet/NuGet.Client/pull/6642) use cast instead of as and null check in CommandLineParser.AssignValue
  * [6615](https://github.com/NuGet/NuGet.Client/pull/6615) remove redundant list in GetPluginAsync
  * [6627](https://github.com/NuGet/NuGet.Client/pull/6627) remove un-used GetDirectoryName in MisplacedAssemblyOutsideLibRule
  * [6578](https://github.com/NuGet/NuGet.Client/pull/6578) remove redundant dictionary lookup in PrunePackageTree.PruneDowngrades
  * [6576](https://github.com/NuGet/NuGet.Client/pull/6576) remove redundant dictionary lookup in RecommenderPackageFeed
* [baronfel](https://github.com/NuGet/NuGet.Client/pull/6554)
  * [6554](https://github.com/NuGet/NuGet.Client/pull/6554) Fix NuGet->SDK Codeflow
  * [6514](https://github.com/NuGet/NuGet.Client/pull/6514) Pin and stabilize the `NuGetToolVersion` property in the generated NuGet props files during restore.
* [omajid](https://github.com/NuGet/NuGet.Client/pull/6500)
  * [6500](https://github.com/NuGet/NuGet.Client/pull/6500) Ensure stable order of entries in Content_Types.xml
  * [6507](https://github.com/NuGet/NuGet.Client/pull/6507) Support building on Linux when full signing is not available
* [AlexDelepine](https://github.com/NuGet/NuGet.Client/pull/6793)
  * [6793](https://github.com/NuGet/NuGet.Client/pull/6793) Update Ngen Priorities for VS
* [hickford](https://github.com/NuGet/NuGet.Client/pull/6475)
  * [6475](https://github.com/NuGet/NuGet.Client/pull/6475) Populate audit sources consistently
* [nohwnd](https://github.com/NuGet/NuGet.Client/pull/6735)
  * [6735](https://github.com/NuGet/NuGet.Client/pull/6735) Disable loading profile in utility powershell.exe calls
* [mmitche](https://github.com/NuGet/NuGet.Client/pull/6539)
  * [6539](https://github.com/NuGet/NuGet.Client/pull/6539) Move NuGet to xliff-tasks
* [dkurepa](https://github.com/NuGet/NuGet.Client/pull/6644)
  * [6644](https://github.com/NuGet/NuGet.Client/pull/6644) Add Version.Details.props
* [bdukes](https://github.com/NuGet/NuGet.Client/pull/6530)
  * [6530](https://github.com/NuGet/NuGet.Client/pull/6530) Fix `nuget.exe` restore finding MSBuild from SSMS instead of Visual Studio
* [ToddGrun](https://github.com/NuGet/NuGet.Client/pull/6519)
  * [6519](https://github.com/NuGet/NuGet.Client/pull/6519) Make LockFileLibrary immutable for performance and sanity reasons
**TODO: Issues that could not be categorized. Make sure the issue has the correct milestone (if required) or an appropriate Type label - Nones:**

* review un-used Members - [#14435](https://github.com/NuGet/Home/issues/14435)

**TODO: Issues that could not be categorized. Make sure the issue has the correct milestone (if required) or an appropriate Type label - StillOpens:**

* Investigate and fix use of obsolete API VSPROPID_DeferredSaveSolution in VSSolutionManager - [#14423](https://github.com/NuGet/Home/issues/14423)

* Remove Sha512HashFunction  - [#8450](https://github.com/NuGet/Home/issues/8450)

* Deprecate and remove the old nuget.exe credential plugin model - [#7586](https://github.com/NuGet/Home/issues/7586)

