---
title: NuGet 1.0 and 1.1 Release Notes
description: Release notes for NuGet 1.1 including known issues, bug fixes, added features, and DCRs.
author: JonDouglas
ms.author: jodou
ms.date: 11/11/2016
ms.topic: conceptual
---

# NuGet 1.0 and 1.1 Release Notes

[NuGet 1.2 Release Notes](../release-notes/nuget-1.2.md)

NuGet 1.0 was released on January 13, 2011.  NuGet 1.1 was released on February 12, 2011.

## Overview

This document contains the release notes for the various releases of NuGet 1.0 grouped according to major preview release.

NuGet includes the following components:

* *NuGet.Tools.vsix* * which consists of:
  * **Add Library Package Dialog** * Dialog within Visual Studio used to browse and install packages.
  * **Package Manager Console** * Powershell based console within Visual Studio.
* *NuGet Command Line Tool* * Tool used to create (and eventually publish) packages.

The NuGet Tools Visual Studio Extension (*NuGet.Tools.vsix*) requires:

* Visual Studio 2010 or Visual Web Developer 2010 Express.

The NuGet Command Line Tool requires:

* .NET Framework Version 4

## Installation

To use this ```[latest release](http://nuget.codeplex.com/releases/view/52018)```:

* First uninstall your older build. You need to run VS as administrator to do this.
* Remove all the existing feeds that you have.
* Add a new feed pointing to <https://go.microsoft.com/fwlink/?LinkId=206669>.

## NuGet 1.1

The list of issues fixed in this release ```[can be found here](http://nuget.codeplex.com/workitem/list/advanced?keyword=&amp;status=All&amp;type=All&amp;priority=All&amp;release=NuGet%201.1&amp;assignedTo=All&amp;component=All&amp;sortField=LastUpdatedDate&amp;sortDirection=Descending&amp;page=0)```

## NuGet 1.0 RTM

One issue was fixed for RTM since the RC.

* ```[Issue 474: Removing Packages Affects All Project In Solution](http://nuget.codeplex.com/workitem/474)```

## Release Candidate

The following are the changes made in this Release Candidate since CTP 2. Visit the Issue Tracker to see the full list of bugs.

* ```[Updating Package from Console does not update dependencies.](http://nuget.codeplex.com/workitem/443)```
* ```[Adding package picks up bin not package reference (CTP1)](http://nuget.codeplex.com/workitem/442)```
* ```[Updating a package leaves broken references](http://nuget.codeplex.com/workitem/440)```
* ```[Get-Package -Updates fails in the dialog, or when the 'All' aggregate source is selected in the console](http://nuget.codeplex.com/workitem/439)```
* ```[Getting package verification errors](http://nuget.codeplex.com/workitem/426)```
* ```[Warn users when a package cannot be installed from the Add Package Dialog](http://nuget.codeplex.com/workitem/425)```
* ```[Get-Package -Updates throws when updating large number of packages](http://nuget.codeplex.com/workitem/424)```
* ```[Improve error handling when nuspec files are authored incorrectly](http://nuget.codeplex.com/workitem/423)```
* ```[Nuget pack ignores specified files](http://nuget.codeplex.com/workitem/422)```
* ```[Removing the second-to-last package source and then clicking "Move Down" crashes VS](http://nuget.codeplex.com/workitem/418)```
* ```[Remove assembly reference while installing packages](http://nuget.codeplex.com/workitem/413)```
* ```[InvalidOperationException when opening Settings dialog](http://nuget.codeplex.com/workitem/411)```
* ```[Access Key for Package Source in Package Manager Console doesn't work](http://nuget.codeplex.com/workitem/410)```
* ```[NuGet VS Settings Dialog Access Keys Give Focus to Wrong Fields](http://nuget.codeplex.com/workitem/409)```
* ```[Package ID intellisense should not query too many items](http://nuget.codeplex.com/workitem/404)```
* ```[Failure adding package to project with a dot character in the Project name](http://nuget.codeplex.com/workitem/403)```
* ```[Issue with specified files in nuspec](http://nuget.codeplex.com/workitem/400)```
* ```[Correct official feed should get registered when using newer build](http://nuget.codeplex.com/workitem/399)```
* ```[Tags should use spaces instead of #](http://nuget.codeplex.com/workitem/397)```
* ```[IPackageMetadata lacks some useful information](http://nuget.codeplex.com/workitem/388)```
* ```[Add Report Abuse Link to the Dialog](http://nuget.codeplex.com/workitem/386)```
* ```[Using App_Data to unzip packages breaks in Visual Studio](http://nuget.codeplex.com/workitem/380)```
* ```[Implement Tags](http://nuget.codeplex.com/workitem/376)```
* ```[PackageBuilder allows empty package with no dependencies to be created](http://nuget.codeplex.com/workitem/373)```
* ```[Add Owners Field for the Package](http://nuget.codeplex.com/workitem/365)```
* ```[Update the VSIX manifest to say NuGet Package Manager rather than VSIX Tools](http://nuget.codeplex.com/workitem/364)```
* ```[Get-Package command throws error when All source is selected](http://nuget.codeplex.com/workitem/359)```
* ```[Allow ordering of package sources in Options dialog](http://nuget.codeplex.com/workitem/356)```
* ```[Update-Package does not remove older version](http://nuget.codeplex.com/workitem/352)```
* ```[Implement Version Range Specification for Dependencies](http://nuget.codeplex.com/workitem/347)```
* ```[Visual Studio crashes when clicking "Add new package"](http://nuget.codeplex.com/workitem/346)```
* ```[Display Downloads and Ratings in the Add Package Dialog](http://nuget.codeplex.com/workitem/345)```
* ```[Changing between package sources in the Dialog doesn't update active source](http://nuget.codeplex.com/workitem/344)```
* ```[Remove Key Binding for Package Manager Console Window](http://nuget.codeplex.com/workitem/339)```
* ```[Install-Package is not recognized as the name of a cmdlet...](http://nuget.codeplex.com/workitem/338)```
* ```[Installing a package from a local feed the dependencies on regular feeds are not resolved](http://nuget.codeplex.com/workitem/332)```
* ```[RemoveDependencies should skip dependencies that are still in use](http://nuget.codeplex.com/workitem/331)```
* ```[If cancelling page navigation, user cannot navigate to a different page while the original page request returns](http://nuget.codeplex.com/workitem/325)```
* ```[Investigate performance of NuPack.Server for serving feeds with large number of packages.](http://nuget.codeplex.com/workitem/324)```
* ```[The second time I filter for a package it uses the "New" package source, instead of the previously selected source.](http://nuget.codeplex.com/workitem/321)```
* ```[Default package source should be selected when selecting the "Online" tab on the dialog.](http://nuget.codeplex.com/workitem/320)```
* ```[List-Package should show installed packages by default](http://nuget.codeplex.com/workitem/309)```
* ```[Assembly Reference HintPaths](http://nuget.codeplex.com/workitem/294)```
* ```[Exception while opening Package Manager Console](http://nuget.codeplex.com/workitem/268)```
* ```[Console intellisense downloads entire feed](http://nuget.codeplex.com/workitem/259)```
* ```['Default' package source should be renamed to 'Active'](http://nuget.codeplex.com/workitem/258)```
* ```[Package sources UI: pressing OK should add the new source if Name/Source fields are non-empty](http://nuget.codeplex.com/workitem/257)```
* ```[Dialog becomes super slow when the number of installed packages is large](http://nuget.codeplex.com/workitem/243)```
* ```[Support Binding Redirects for Strong Named Assemblies](http://nuget.codeplex.com/workitem/238)```
* ```[Add Package Reference... UI to include drop down for Package source](http://nuget.codeplex.com/workitem/226)```
* ```[NuPack needs to support config transform agnostically of the config file name](http://nuget.codeplex.com/workitem/224)```
* ```[Allows BasePath to be Overriden in NuPack.exe](http://nuget.codeplex.com/workitem/222)```
* ```[Package Source Fallback Behavior](http://nuget.codeplex.com/workitem/204)```
* ```[Crash on GUI](http://nuget.codeplex.com/workitem/201)```
* ```[Add sorting options to Add Package Dialog](http://nuget.codeplex.com/workitem/179)```
* ```[shortcut key to clear the Package Manager Console](http://nuget.codeplex.com/workitem/174)```
* ```[PowerConsole causes NuPack Console to fail](http://nuget.codeplex.com/workitem/166)```
* ```[Console and Add Package Dialog should set user agent in requests](http://nuget.codeplex.com/workitem/141)```
* ```[Set version number of the VSIX and NuPack.exe in the build.](http://nuget.codeplex.com/workitem/134)```
* ```[Hide common PowerShell parameters from -?](http://nuget.codeplex.com/workitem/118)```
* ```[Add -detailed help for console commands](http://nuget.codeplex.com/workitem/110)```
* ```[Add Package Dialog Should Allow Choosing the Current Package Source](http://nuget.codeplex.com/workitem/88)```
* ```[Move NuPack.Core classes into different namespaces](http://nuget.codeplex.com/workitem/50)```
* ```[Add help to cmdlets](http://nuget.codeplex.com/workitem/23)```
* ```[Verify hash from feed after package download](http://nuget.codeplex.com/workitem/18)```

## CTP 2

The following are the most significant changes made in CTP 2:

* Switched the package feed from ATOM to an OData service endpoint: If you upgrade to the CTP2 version of NuGet, be sure to add the following URL as a package source: `https://feed.nuget.org/ctp2/odata/v1/`.
* Renamed the Add-Package command to *Install-Package*.
* Updated the `.nuspec` Format. The `.nuspec` format now includes the *iconUrl* field for specifying a 32x32 png icon which will show up in the Add Package Dialog. So be sure to set that to distinguish your package. The `.nuspec` format also includes the new *projectUrl* field which you can use to point to a web page that provides more information about your package.

This build will not work with old `.nupkg` files. If you get null reference exceptions, you're using an old `.nupkg` file and
need to rebuild it with the updated ```[NuGet command line tool](http://nuget.codeplex.com/releases/52017/download/165468)```.

The following is a list of features and bugs that were fixed for NuGet CTP 2 (does not include bugs for minor code cleanups etc.).

* ```[Error unpacking package assemblies when specifiying the TargetFramework for an assembly.](http://nuget.codeplex.com/workitem/10)```
* ```[Make NuPack Console window more discoverable](http://nuget.codeplex.com/workitem/14)```
* ```[ILMerge the nupack.exe release](http://nuget.codeplex.com/workitem/19)```
* ```[Better error/exception handling](http://nuget.codeplex.com/workitem/24)```
* ```[[Nupack.Core]: PackageManager should gracefully handle feed-related errors](http://nuget.codeplex.com/workitem/28)```
* ```[Need a new icon for the console](http://nuget.codeplex.com/workitem/29)```
* ```[Localize strings in the Dialog](http://nuget.codeplex.com/workitem/38)```
* ```[NuPack caches downloaded .nupack files in memory](http://nuget.codeplex.com/workitem/40)```
* ```[NuPack Console: Change the default shortcut for displaying console](http://nuget.codeplex.com/workitem/48)```
* ```[ProjectSystem should support default values for common properties](http://nuget.codeplex.com/workitem/49)```
* ```[Running nupack.exe in a folder with just one nuspec file should use that nuspec](http://nuget.codeplex.com/workitem/52)```
* ```[Project Menu Shows Up Even When No Project/Solution Is Loaded](http://nuget.codeplex.com/workitem/54)```
* ```[build.cmd fails on a clean clone of the codebase](http://nuget.codeplex.com/workitem/56)```
* ```[Updates available feature](http://nuget.codeplex.com/workitem/57)```
* ```[Dialog: Adding a package through the dialog removes the prompt in the console](http://nuget.codeplex.com/workitem/73)```
* ```[Adding a package by clicking 'Install' is often slow, with no visual feedback](http://nuget.codeplex.com/workitem/80)```
* ```[There is no way to discover which of my installed packages have updates.](http://nuget.codeplex.com/workitem/82)```
* ```[There is no way to update an installed package in the dialog.](http://nuget.codeplex.com/workitem/83)```
* ```[There is no way to uninstall an installed package in the dialog](http://nuget.codeplex.com/workitem/84)```
* ```[&ldquo;Add Package Reference&hellip;&rdquo; appears on the context menu of installed references](http://nuget.codeplex.com/workitem/85)```
* ```[After updating a package from the console, it shows both the old version and the new version as installed](http://nuget.codeplex.com/workitem/86)```
* ```[The activity in the console, when using the dialog, disappears after use](http://nuget.codeplex.com/workitem/87)```
* ```[Cleanup command line parsing in nupack.exe](http://nuget.codeplex.com/workitem/89)```
* ```[Add a friendly name to package sources](http://nuget.codeplex.com/workitem/98)```
* ```[Update .nuspec to support including package icons](http://nuget.codeplex.com/workitem/103)```
* ```[Feed UI doesn't allow copying the URL](http://nuget.codeplex.com/workitem/105)```
* ```[Better remove-package error handling.](http://nuget.codeplex.com/workitem/107)```
* ```[Typing in Console Window depends on cursor focus](http://nuget.codeplex.com/workitem/112)```
* ```[Error messages look awful](http://nuget.codeplex.com/workitem/116)```
* ```[The performance of Remove-Package for a package that isn't installed is bad](http://nuget.codeplex.com/workitem/117)```
* ```[Removing a package fails when there are no package sources](http://nuget.codeplex.com/workitem/119)```
* ```[Remove-Package fails when the package source is unavailable](http://nuget.codeplex.com/workitem/120)```
* ```[Add Title to the package metadata and the feed.](http://nuget.codeplex.com/workitem/125)```
* ```[Add the -Source parameter back to Add-Package](http://nuget.codeplex.com/workitem/127)```
* ```[List-Package should have a -Source parameter](http://nuget.codeplex.com/workitem/128)```
* ```[Update NuPack.Server to require NuPack User Agent To Download Package](http://nuget.codeplex.com/workitem/142)```
* ```[License Acceptance Dialog Must List Licenses For All Dependencies That Require Acceptance](http://nuget.codeplex.com/workitem/145)```
* ```[Log an error when a package throws in the feed](http://nuget.codeplex.com/workitem/150)```
* ```[NuPack.exe should not allow an empty &lt;licenseurl&gt; element](http://nuget.codeplex.com/workitem/152)```
* ```[Rename List-Package to Get-Package, Add-Package to Install-Package, and Remove-Package to Uninstall-Package](http://nuget.codeplex.com/workitem/155)```
* ```[Using the Add Package Reference menu item from the Solution Navigator crashes Visual Studio](http://nuget.codeplex.com/workitem/158)```
* ```["Available package sources" label is missing a colon](http://nuget.codeplex.com/workitem/160)```
* ```[Make .nuspec xml element casing consistently camel cased](http://nuget.codeplex.com/workitem/161)```
* ```[The NuPack VSIX's manifest needs to turn on the 'admin' bit](http://nuget.codeplex.com/workitem/162)```
* ```[If you run List-Package with no feeds, you get null ref error](http://nuget.codeplex.com/workitem/164)```
* ```[nuget.exe: specify destination path](http://nuget.codeplex.com/workitem/171)```
* ```[Powershell Errors Opening Package Management Console on WinXP](http://nuget.codeplex.com/workitem/175)```
* ```[VS Crashes while trying to load package list](http://nuget.codeplex.com/workitem/176)```
* ```[allow meta packages (no files, only dependencies)](http://nuget.codeplex.com/workitem/180)```
* ```[Convert Powershell Script to Powershell 2.0 Module](http://nuget.codeplex.com/workitem/181)```
* ```[PathResolver should discard path portion preceeding wildcard characters when target is specified](http://nuget.codeplex.com/workitem/183)```
* ```[No dependencies](http://nuget.codeplex.com/workitem/186)```
* ```[Error installing Elmah](http://nuget.codeplex.com/workitem/192)```
* ```[Config transforms don't work correctly with &lt;configsections&gt;](http://nuget.codeplex.com/workitem/194)```
* ```[The variable '$global:projectCache' cannot be retrieved because it has not been set](http://nuget.codeplex.com/workitem/203)```
* ```[Add MSBuild task for creating NuPack packages](http://nuget.codeplex.com/workitem/205)```
* ```[list-package needs to support searching/filtering](http://nuget.codeplex.com/workitem/206)```
* ```[Always display a link to license if the package author provides a license URL](http://nuget.codeplex.com/workitem/208)```
* ```[Occasional "Access Denied" exception with Remove-Package](http://nuget.codeplex.com/workitem/213)```
* ```[Unit Tests Failing: InvalidPackageIsExcludedFromFeedItems &amp; CreatingFeedConvertsPackagesToAtomEntries](http://nuget.codeplex.com/workitem/214)```
* ```[Allow for a fallback/default set of files if a specfic framework version cannot be found](http://nuget.codeplex.com/workitem/223)```
* ```[Add Package Reference... UI cannot remove a package](http://nuget.codeplex.com/workitem/225)```
* ```[Add Package Reference crashes studio when one or more project is unloaded](http://nuget.codeplex.com/workitem/228)```
* ```[Config transform does not appear to work on web.debug.config file](http://nuget.codeplex.com/workitem/229)```
* ```[init.ps1 not firing on custom package](http://nuget.codeplex.com/workitem/237)```
* ```[When adding paths to the feedlist, the default button is set to OK, so if I press ENTER it automatically closes](http://nuget.codeplex.com/workitem/240)```
* ```[Attempt to uninstall a dependency will crash VS if attempted 2 times in a row](http://nuget.codeplex.com/workitem/241)```
* ```[Display the Project URL in the Add Package dialog](http://nuget.codeplex.com/workitem/253)```
* ```[Default the Add-Package dialog to Installed Packages](http://nuget.codeplex.com/workitem/254)```
* ```[Change Add Package Dialog menu item.](http://nuget.codeplex.com/workitem/261)```
* ```[Rename namespaces and assemblies](http://nuget.codeplex.com/workitem/274)```
* ```[Rename the NuPack Project to NuGet](http://nuget.codeplex.com/workitem/282)```
* ```[Add the following text under the list of dependencies](http://nuget.codeplex.com/workitem/288)```
* ```[Change the license acceptance text in the License Acceptance Dialog](http://nuget.codeplex.com/workitem/291)```
* ```[Change the text in the License Acceptance Dialog above the list of packages](http://nuget.codeplex.com/workitem/292)```
* ```[OData doesn't work with an fwlink URL](http://nuget.codeplex.com/workitem/304)```
* ```[Package Manager UI: Over aggressive caching of package count used for paging](http://nuget.codeplex.com/workitem/317)```
* ```[NuPack / NuGet -&gt; Package Manager Console error](http://nuget.codeplex.com/workitem/335)```
* ```[Add Package Dialog shows License Acceptance For Already Installed Packaged](http://nuget.codeplex.com/workitem/336)```

## CTP 1

The following is a list of features and bugs that were fixed for NuGet CTP 1.

* ```[Package extension should be renamed to .nupack](http://nuget.codeplex.com/workitem/1)```
* ```[Move package file into folder](http://nuget.codeplex.com/workitem/2)```
* ```[Merge install &amp; Add PS commands](http://nuget.codeplex.com/workitem/3)```
* ```[Create aliases for Verb-Noun cmdlets](http://nuget.codeplex.com/workitem/4)```
* ```[NuPack gets confused when switching solution in VS](http://nuget.codeplex.com/workitem/6)```
* ```[We should hide the 'packages' solution folder by default](http://nuget.codeplex.com/workitem/11)```
* ```[Add support for token replacement in content items.](http://nuget.codeplex.com/workitem/12)```
* ```[NuPack.UI should use the PackageSource API](http://nuget.codeplex.com/workitem/26)```
* ```[[Nupack.Core]: PackageManager marks packages as installed prior to installing them](http://nuget.codeplex.com/workitem/27)```
* ```[Deleting default project from solution still shows the deleted project as default](http://nuget.codeplex.com/workitem/30)```
* ```[New-Package fails with "Cannot add part for the specified URI because it's already in the package."](http://nuget.codeplex.com/workitem/32)```
* ```[Remove "NuPack" strings from Visual Studio GUI](http://nuget.codeplex.com/workitem/35)```
* ```[Add Apache Header To a COPYRIGHT.txt file](http://nuget.codeplex.com/workitem/36)```
* ```[Remove Update-PackageSource Command](http://nuget.codeplex.com/workitem/37)```
* ```[Package Manager unusable when loading profile throws an exception](http://nuget.codeplex.com/workitem/39)```
* ```[init.ps1, install.ps1 and uninstall.ps1 need to receive additional state](http://nuget.codeplex.com/workitem/41)```
* ```[Combine Console and GUI Packages Into One Package](http://nuget.codeplex.com/workitem/42)```
* ```[Xml transform logic doesn't work if applied to XML that isn't at the root](http://nuget.codeplex.com/workitem/43)```
* ```[Manage package sources settings dialog not updating the NuPack console](http://nuget.codeplex.com/workitem/44)```
* ```[NuPack Console UI: Rename 'Package feed' drop-down list to 'Package source'](http://nuget.codeplex.com/workitem/45)```
* ```[NuPack Console Options: Rename 'Repository UI' to be consistent with NuPack Console](http://nuget.codeplex.com/workitem/46)```
* ```[Add-Package fails against a website that was opened from IIS or a URL](http://nuget.codeplex.com/workitem/53)```
* ```[Package Manager Source Doesn't Work With FwLink](http://nuget.codeplex.com/workitem/55)```
* ```[Set the default package source](http://nuget.codeplex.com/workitem/59)```
* ```[When adding package sources in option, when only one source is supplied, assume it's the default.](http://nuget.codeplex.com/workitem/60)```
* ```[The Dialog UI shows fake "recent" packages](http://nuget.codeplex.com/workitem/62)```
* ```[Options: Clicking cancel does not cancel changes](http://nuget.codeplex.com/workitem/63)```
* ```[Add Package Reference Dialog Search should be case insensitive](http://nuget.codeplex.com/workitem/65)```
* ```[Fix company metadata in AssemblyInfo.cs files](http://nuget.codeplex.com/workitem/67)```
* ```[Version number for the VSIX](http://nuget.codeplex.com/workitem/71)```
* ```[Remove-Package: Using -? displays help twice](http://nuget.codeplex.com/workitem/72)```
* ```[Execute install/uninstall packages for project level packages](http://nuget.codeplex.com/workitem/74)```
* ```[Server unable to create feed when one nupack fails validation](http://nuget.codeplex.com/workitem/90)```
* ```[Need to Replace NuPack Icons](http://nuget.codeplex.com/workitem/94)```
* ```[NTLM http proxy does not authenticate to the package feed.](http://nuget.codeplex.com/workitem/96)```
* ```[The dialog doesn't always start centered in the VS window](http://nuget.codeplex.com/workitem/100)```
* ```[Many of the fields in a packages details are not being populated in the dialog](http://nuget.codeplex.com/workitem/102)```
* ```[Dialog UI doesn't show Authors' names](http://nuget.codeplex.com/workitem/108)```
* ```[Why -Version for Remove-Package](http://nuget.codeplex.com/workitem/113)```
* ```[Remove the Recent tab on the Dialog UI](http://nuget.codeplex.com/workitem/115)```
* ```[VS crash when right click on solution folder after opening Dialog UI at least one.](http://nuget.codeplex.com/workitem/126)```
* ```[Change the -Local parameter of List-Package to -Installed](http://nuget.codeplex.com/workitem/129)```
* ```[Rename packages.xml to NuPack.config](http://nuget.codeplex.com/workitem/132)```
* ```[Console forces cursor to the end of line](http://nuget.codeplex.com/workitem/135)```
* ```[Remove-Package intellisense is broken](http://nuget.codeplex.com/workitem/136)```
* ```[Add RequireLicenseAcceptance Flag to .nuspec and Feed](http://nuget.codeplex.com/workitem/137)```
* ```[Add LicenseUrl to .nuspec Format and Package Feed](http://nuget.codeplex.com/workitem/138)```
* ```[Clicking Install For Package That Requires Acceptance Should Show Acceptance Dialog](http://nuget.codeplex.com/workitem/139)```
* ```[Add Disclaimer Text to the Add Package Dialog](http://nuget.codeplex.com/workitem/140)```
* ```[Add Disclaimer When the Package Console is run the first time](http://nuget.codeplex.com/workitem/143)```
* ```[Display Disclaimer After Installing Package In The Console](http://nuget.codeplex.com/workitem/144)```
* ```[Rename the .nupack extension to .nupkg](http://nuget.codeplex.com/workitem/146)```