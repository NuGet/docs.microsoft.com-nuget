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

To use this [latest release](http://nuget.codeplex.com/releases/view/52018):

* First uninstall your older build. You need to run VS as administrator to do this.
* Remove all the existing feeds that you have.
* Add a new feed pointing to <https://go.microsoft.com/fwlink/?LinkId=206669>.

## NuGet 1.1

The list of issues fixed in this release [can be found here](https://nuget.codeplex.com/workitem/list/advanced?keyword=&amp;status=All&amp;type=All&amp;priority=All&amp;release=NuGet%201.1&amp;assignedTo=All&amp;component=All&amp;sortField=LastUpdatedDate&amp;sortDirection=Descending&amp;page=0)

## NuGet 1.0 RTM

One issue was fixed for RTM since the RC.

* [Issue 474: Removing Packages Affects All Project In Solution](https://nuget.codeplex.com/workitem/474)

## Release Candidate

The following are the changes made in this Release Candidate since CTP 2. Visit the Issue Tracker to see the full list of bugs.

* [Updating Package from Console does not update dependencies.](https://nuget.codeplex.com/workitem/443)
* [Adding package picks up bin not package reference (CTP1)](https://nuget.codeplex.com/workitem/442)
* [Updating a package leaves broken references](https://nuget.codeplex.com/workitem/440)
* [Get-Package -Updates fails in the dialog, or when the 'All' aggregate source is selected in the console](https://nuget.codeplex.com/workitem/439)
* [Getting package verification errors](https://nuget.codeplex.com/workitem/426)
* [Warn users when a package cannot be installed from the Add Package Dialog](https://nuget.codeplex.com/workitem/425)
* [Get-Package -Updates throws when updating large number of packages](https://nuget.codeplex.com/workitem/424)
* [Improve error handling when nuspec files are authored incorrectly](https://nuget.codeplex.com/workitem/423)
* [Nuget pack ignores specified files](https://nuget.codeplex.com/workitem/422)
* [Removing the second-to-last package source and then clicking "Move Down" crashes VS](https://nuget.codeplex.com/workitem/418)
* [Remove assembly reference while installing packages](https://nuget.codeplex.com/workitem/413)
* [InvalidOperationException when opening Settings dialog](https://nuget.codeplex.com/workitem/411)
* [Access Key for Package Source in Package Manager Console doesn't work](https://nuget.codeplex.com/workitem/410)
* [NuGet VS Settings Dialog Access Keys Give Focus to Wrong Fields](https://nuget.codeplex.com/workitem/409)
* [Package ID intellisense should not query too many items](https://nuget.codeplex.com/workitem/404)
* [Failure adding package to project with a dot character in the Project name](https://nuget.codeplex.com/workitem/403)
* [Issue with specified files in nuspec](https://nuget.codeplex.com/workitem/400)
* [Correct official feed should get registered when using newer build](https://nuget.codeplex.com/workitem/399)
* [Tags should use spaces instead of #](https://nuget.codeplex.com/workitem/397)
* [IPackageMetadata lacks some useful information](https://nuget.codeplex.com/workitem/388)
* [Add Report Abuse Link to the Dialog](https://nuget.codeplex.com/workitem/386)
* [Using App_Data to unzip packages breaks in Visual Studio](https://nuget.codeplex.com/workitem/380)
* [Implement Tags](https://nuget.codeplex.com/workitem/376)
* [PackageBuilder allows empty package with no dependencies to be created](https://nuget.codeplex.com/workitem/373)
* [Add Owners Field for the Package](https://nuget.codeplex.com/workitem/365)
* [Update the VSIX manifest to say NuGet Package Manager rather than VSIX Tools](https://nuget.codeplex.com/workitem/364)
* [Get-Package command throws error when All source is selected](https://nuget.codeplex.com/workitem/359)
* [Allow ordering of package sources in Options dialog](https://nuget.codeplex.com/workitem/356)
* [Update-Package does not remove older version](https://nuget.codeplex.com/workitem/352)
* [Implement Version Range Specification for Dependencies](https://nuget.codeplex.com/workitem/347)
* [Visual Studio crashes when clicking "Add new package"](https://nuget.codeplex.com/workitem/346)
* [Display Downloads and Ratings in the Add Package Dialog](https://nuget.codeplex.com/workitem/345)
* [Changing between package sources in the Dialog doesn't update active source](https://nuget.codeplex.com/workitem/344)
* [Remove Key Binding for Package Manager Console Window](https://nuget.codeplex.com/workitem/339)
* [Install-Package is not recognized as the name of a cmdlet...](https://nuget.codeplex.com/workitem/338)
* [Installing a package from a local feed the dependencies on regular feeds are not resolved](https://nuget.codeplex.com/workitem/332)
* [RemoveDependencies should skip dependencies that are still in use](https://nuget.codeplex.com/workitem/331)
* [If cancelling page navigation, user cannot navigate to a different page while the original page request returns](https://nuget.codeplex.com/workitem/325)
* [Investigate performance of NuPack.Server for serving feeds with large number of packages.](https://nuget.codeplex.com/workitem/324)
* [The second time I filter for a package it uses the "New" package source, instead of the previously selected source.](https://nuget.codeplex.com/workitem/321)
* [Default package source should be selected when selecting the "Online" tab on the dialog.](https://nuget.codeplex.com/workitem/320)
* [List-Package should show installed packages by default](https://nuget.codeplex.com/workitem/309)
* [Assembly Reference HintPaths](https://nuget.codeplex.com/workitem/294)
* [Exception while opening Package Manager Console](https://nuget.codeplex.com/workitem/268)
* [Console intellisense downloads entire feed](https://nuget.codeplex.com/workitem/259)
* ['Default' package source should be renamed to 'Active'](https://nuget.codeplex.com/workitem/258)
* [Package sources UI: pressing OK should add the new source if Name/Source fields are non-empty](https://nuget.codeplex.com/workitem/257)
* [Dialog becomes super slow when the number of installed packages is large](https://nuget.codeplex.com/workitem/243)
* [Support Binding Redirects for Strong Named Assemblies](https://nuget.codeplex.com/workitem/238)
* [Add Package Reference... UI to include drop down for Package source](https://nuget.codeplex.com/workitem/226)
* [NuPack needs to support config transform agnostically of the config file name](https://nuget.codeplex.com/workitem/224)
* [Allows BasePath to be Overriden in NuPack.exe](https://nuget.codeplex.com/workitem/222)
* [Package Source Fallback Behavior](https://nuget.codeplex.com/workitem/204)
* [Crash on GUI](https://nuget.codeplex.com/workitem/201)
* [Add sorting options to Add Package Dialog](https://nuget.codeplex.com/workitem/179)
* [shortcut key to clear the Package Manager Console](https://nuget.codeplex.com/workitem/174)
* [PowerConsole causes NuPack Console to fail](https://nuget.codeplex.com/workitem/166)
* [Console and Add Package Dialog should set user agent in requests](https://nuget.codeplex.com/workitem/141)
* [Set version number of the VSIX and NuPack.exe in the build.](https://nuget.codeplex.com/workitem/134)
* [Hide common PowerShell parameters from -?](https://nuget.codeplex.com/workitem/118)
* [Add -detailed help for console commands](https://nuget.codeplex.com/workitem/110)
* [Add Package Dialog Should Allow Choosing the Current Package Source](https://nuget.codeplex.com/workitem/88)
* [Move NuPack.Core classes into different namespaces](https://nuget.codeplex.com/workitem/50)
* [Add help to cmdlets](https://nuget.codeplex.com/workitem/23)
* [Verify hash from feed after package download](https://nuget.codeplex.com/workitem/18)

## CTP 2

The following are the most significant changes made in CTP 2:

* Switched the package feed from ATOM to an OData service endpoint: If you upgrade to the CTP2 version of NuGet, be sure to add the following URL as a package source: `https://feed.nuget.org/ctp2/odata/v1/`.
* Renamed the Add-Package command to *Install-Package*.
* Updated the `.nuspec` Format. The `.nuspec` format now includes the *iconUrl* field for specifying a 32x32 png icon which will show up in the Add Package Dialog. So be sure to set that to distinguish your package. The `.nuspec` format also includes the new *projectUrl* field which you can use to point to a web page that provides more information about your package.

This build will not work with old `.nupkg` files. If you get null reference exceptions, you're using an old `.nupkg` file and
need to rebuild it with the updated [NuGet command line tool](http://nuget.codeplex.com/releases/52017/download/165468).

The following is a list of features and bugs that were fixed for NuGet CTP 2 (does not include bugs for minor code cleanups etc.).

* [Error unpacking package assemblies when specifiying the TargetFramework for an assembly.](https://nuget.codeplex.com/workitem/10)
* [Make NuPack Console window more discoverable](https://nuget.codeplex.com/workitem/14)
* [ILMerge the nupack.exe release](https://nuget.codeplex.com/workitem/19)
* [Better error/exception handling](https://nuget.codeplex.com/workitem/24)
* [[Nupack.Core]: PackageManager should gracefully handle feed-related errors](https://nuget.codeplex.com/workitem/28)
* [Need a new icon for the console](https://nuget.codeplex.com/workitem/29)
* [Localize strings in the Dialog](https://nuget.codeplex.com/workitem/38)
* [NuPack caches downloaded .nupack files in memory](https://nuget.codeplex.com/workitem/40)
* [NuPack Console: Change the default shortcut for displaying console](https://nuget.codeplex.com/workitem/48)
* [ProjectSystem should support default values for common properties](https://nuget.codeplex.com/workitem/49)
* [Running nupack.exe in a folder with just one nuspec file should use that nuspec](https://nuget.codeplex.com/workitem/52)
* [Project Menu Shows Up Even When No Project/Solution Is Loaded](https://nuget.codeplex.com/workitem/54)
* [build.cmd fails on a clean clone of the codebase](https://nuget.codeplex.com/workitem/56)
* [Updates available feature](https://nuget.codeplex.com/workitem/57)
* [Dialog: Adding a package through the dialog removes the prompt in the console](https://nuget.codeplex.com/workitem/73)
* [Adding a package by clicking 'Install' is often slow, with no visual feedback](https://nuget.codeplex.com/workitem/80)
* [There is no way to discover which of my installed packages have updates.](https://nuget.codeplex.com/workitem/82)
* [There is no way to update an installed package in the dialog.](https://nuget.codeplex.com/workitem/83)
* [There is no way to uninstall an installed package in the dialog](https://nuget.codeplex.com/workitem/84)
* [&ldquo;Add Package Reference&hellip;&rdquo; appears on the context menu of installed references](https://nuget.codeplex.com/workitem/85)
* [After updating a package from the console, it shows both the old version and the new version as installed](https://nuget.codeplex.com/workitem/86)
* [The activity in the console, when using the dialog, disappears after use](https://nuget.codeplex.com/workitem/87)
* [Cleanup command line parsing in nupack.exe](https://nuget.codeplex.com/workitem/89)
* [Add a friendly name to package sources](https://nuget.codeplex.com/workitem/98)
* [Update .nuspec to support including package icons](https://nuget.codeplex.com/workitem/103)
* [Feed UI doesn't allow copying the URL](https://nuget.codeplex.com/workitem/105)
* [Better remove-package error handling.](https://nuget.codeplex.com/workitem/107)
* [Typing in Console Window depends on cursor focus](https://nuget.codeplex.com/workitem/112)
* [Error messages look awful](https://nuget.codeplex.com/workitem/116)
* [The performance of Remove-Package for a package that isn't installed is bad](https://nuget.codeplex.com/workitem/117)
* [Removing a package fails when there are no package sources](https://nuget.codeplex.com/workitem/119)
* [Remove-Package fails when the package source is unavailable](https://nuget.codeplex.com/workitem/120)
* [Add Title to the package metadata and the feed.](https://nuget.codeplex.com/workitem/125)
* [Add the -Source parameter back to Add-Package](https://nuget.codeplex.com/workitem/127)
* [List-Package should have a -Source parameter](https://nuget.codeplex.com/workitem/128)
* [Update NuPack.Server to require NuPack User Agent To Download Package](https://nuget.codeplex.com/workitem/142)
* [License Acceptance Dialog Must List Licenses For All Dependencies That Require Acceptance](https://nuget.codeplex.com/workitem/145)
* [Log an error when a package throws in the feed](https://nuget.codeplex.com/workitem/150)
* [NuPack.exe should not allow an empty &lt;licenseurl&gt; element](https://nuget.codeplex.com/workitem/152)
* [Rename List-Package to Get-Package, Add-Package to Install-Package, and Remove-Package to Uninstall-Package](https://nuget.codeplex.com/workitem/155)
* [Using the Add Package Reference menu item from the Solution Navigator crashes Visual Studio](https://nuget.codeplex.com/workitem/158)
* ["Available package sources" label is missing a colon](https://nuget.codeplex.com/workitem/160)
* [Make .nuspec xml element casing consistently camel cased](https://nuget.codeplex.com/workitem/161)
* [The NuPack VSIX's manifest needs to turn on the 'admin' bit](https://nuget.codeplex.com/workitem/162)
* [If you run List-Package with no feeds, you get null ref error](https://nuget.codeplex.com/workitem/164)
* [nuget.exe: specify destination path](https://nuget.codeplex.com/workitem/171)
* [Powershell Errors Opening Package Management Console on WinXP](https://nuget.codeplex.com/workitem/175)
* [VS Crashes while trying to load package list](https://nuget.codeplex.com/workitem/176)
* [allow meta packages (no files, only dependencies)](https://nuget.codeplex.com/workitem/180)
* [Convert Powershell Script to Powershell 2.0 Module](https://nuget.codeplex.com/workitem/181)
* [PathResolver should discard path portion preceeding wildcard characters when target is specified](https://nuget.codeplex.com/workitem/183)
* [No dependencies](https://nuget.codeplex.com/workitem/186)
* [Error installing Elmah](https://nuget.codeplex.com/workitem/192)
* [Config transforms don't work correctly with &lt;configsections&gt;](https://nuget.codeplex.com/workitem/194)
* [The variable '$global:projectCache' cannot be retrieved because it has not been set](https://nuget.codeplex.com/workitem/203)
* [Add MSBuild task for creating NuPack packages](https://nuget.codeplex.com/workitem/205)
* [list-package needs to support searching/filtering](https://nuget.codeplex.com/workitem/206)
* [Always display a link to license if the package author provides a license URL](https://nuget.codeplex.com/workitem/208)
* [Occasional "Access Denied" exception with Remove-Package](https://nuget.codeplex.com/workitem/213)
* [Unit Tests Failing: InvalidPackageIsExcludedFromFeedItems &amp; CreatingFeedConvertsPackagesToAtomEntries](https://nuget.codeplex.com/workitem/214)
* [Allow for a fallback/default set of files if a specfic framework version cannot be found](https://nuget.codeplex.com/workitem/223)
* [Add Package Reference... UI cannot remove a package](https://nuget.codeplex.com/workitem/225)
* [Add Package Reference crashes studio when one or more project is unloaded](https://nuget.codeplex.com/workitem/228)
* [Config transform does not appear to work on web.debug.config file](https://nuget.codeplex.com/workitem/229)
* [init.ps1 not firing on custom package](https://nuget.codeplex.com/workitem/237)
* [When adding paths to the feedlist, the default button is set to OK, so if I press ENTER it automatically closes](https://nuget.codeplex.com/workitem/240)
* [Attempt to uninstall a dependency will crash VS if attempted 2 times in a row](https://nuget.codeplex.com/workitem/241)
* [Display the Project URL in the Add Package dialog](https://nuget.codeplex.com/workitem/253)
* [Default the Add-Package dialog to Installed Packages](https://nuget.codeplex.com/workitem/254)
* [Change Add Package Dialog menu item.](https://nuget.codeplex.com/workitem/261)
* [Rename namespaces and assemblies](https://nuget.codeplex.com/workitem/274)
* [Rename the NuPack Project to NuGet](https://nuget.codeplex.com/workitem/282)
* [Add the following text under the list of dependencies](https://nuget.codeplex.com/workitem/288)
* [Change the license acceptance text in the License Acceptance Dialog](https://nuget.codeplex.com/workitem/291)
* [Change the text in the License Acceptance Dialog above the list of packages](https://nuget.codeplex.com/workitem/292)
* [OData doesn't work with an fwlink URL](https://nuget.codeplex.com/workitem/304)
* [Package Manager UI: Over aggressive caching of package count used for paging](https://nuget.codeplex.com/workitem/317)
* [NuPack / NuGet -&gt; Package Manager Console error](https://nuget.codeplex.com/workitem/335)
* [Add Package Dialog shows License Acceptance For Already Installed Packaged](https://nuget.codeplex.com/workitem/336)

## CTP 1

The following is a list of features and bugs that were fixed for NuGet CTP 1.

* [Package extension should be renamed to .nupack](https://nuget.codeplex.com/workitem/1)
* [Move package file into folder](https://nuget.codeplex.com/workitem/2)
* [Merge install &amp; Add PS commands](https://nuget.codeplex.com/workitem/3)
* [Create aliases for Verb-Noun cmdlets](https://nuget.codeplex.com/workitem/4)
* [NuPack gets confused when switching solution in VS](https://nuget.codeplex.com/workitem/6)
* [We should hide the 'packages' solution folder by default](https://nuget.codeplex.com/workitem/11)
* [Add support for token replacement in content items.](https://nuget.codeplex.com/workitem/12)
* [NuPack.UI should use the PackageSource API](https://nuget.codeplex.com/workitem/26)
* [[Nupack.Core]: PackageManager marks packages as installed prior to installing them](https://nuget.codeplex.com/workitem/27)
* [Deleting default project from solution still shows the deleted project as default](https://nuget.codeplex.com/workitem/30)
* [New-Package fails with "Cannot add part for the specified URI because it's already in the package."](https://nuget.codeplex.com/workitem/32)
* [Remove "NuPack" strings from Visual Studio GUI](https://nuget.codeplex.com/workitem/35)
* [Add Apache Header To a COPYRIGHT.txt file](https://nuget.codeplex.com/workitem/36)
* [Remove Update-PackageSource Command](https://nuget.codeplex.com/workitem/37)
* [Package Manager unusable when loading profile throws an exception](https://nuget.codeplex.com/workitem/39)
* [init.ps1, install.ps1 and uninstall.ps1 need to receive additional state](https://nuget.codeplex.com/workitem/41)
* [Combine Console and GUI Packages Into One Package](https://nuget.codeplex.com/workitem/42)
* [Xml transform logic doesn't work if applied to XML that isn't at the root](https://nuget.codeplex.com/workitem/43)
* [Manage package sources settings dialog not updating the NuPack console](https://nuget.codeplex.com/workitem/44)
* [NuPack Console UI: Rename 'Package feed' drop-down list to 'Package source'](https://nuget.codeplex.com/workitem/45)
* [NuPack Console Options: Rename 'Repository UI' to be consistent with NuPack Console](https://nuget.codeplex.com/workitem/46)
* [Add-Package fails against a website that was opened from IIS or a URL](https://nuget.codeplex.com/workitem/53)
* [Package Manager Source Doesn't Work With FwLink](https://nuget.codeplex.com/workitem/55)
* [Set the default package source](https://nuget.codeplex.com/workitem/59)
* [When adding package sources in option, when only one source is supplied, assume it's the default.](https://nuget.codeplex.com/workitem/60)
* [The Dialog UI shows fake "recent" packages](https://nuget.codeplex.com/workitem/62)
* [Options: Clicking cancel does not cancel changes](https://nuget.codeplex.com/workitem/63)
* [Add Package Reference Dialog Search should be case insensitive](https://nuget.codeplex.com/workitem/65)
* [Fix company metadata in AssemblyInfo.cs files](https://nuget.codeplex.com/workitem/67)
* [Version number for the VSIX](https://nuget.codeplex.com/workitem/71)
* [Remove-Package: Using -? displays help twice](https://nuget.codeplex.com/workitem/72)
* [Execute install/uninstall packages for project level packages](https://nuget.codeplex.com/workitem/74)
* [Server unable to create feed when one nupack fails validation](https://nuget.codeplex.com/workitem/90)
* [Need to Replace NuPack Icons](https://nuget.codeplex.com/workitem/94)
* [NTLM http proxy does not authenticate to the package feed.](https://nuget.codeplex.com/workitem/96)
* [The dialog doesn't always start centered in the VS window](https://nuget.codeplex.com/workitem/100)
* [Many of the fields in a packages details are not being populated in the dialog](https://nuget.codeplex.com/workitem/102)
* [Dialog UI doesn't show Authors' names](https://nuget.codeplex.com/workitem/108)
* [Why -Version for Remove-Package](https://nuget.codeplex.com/workitem/113)
* [Remove the Recent tab on the Dialog UI](https://nuget.codeplex.com/workitem/115)
* [VS crash when right click on solution folder after opening Dialog UI at least one.](https://nuget.codeplex.com/workitem/126)
* [Change the -Local parameter of List-Package to -Installed](https://nuget.codeplex.com/workitem/129)
* [Rename packages.xml to NuPack.config](https://nuget.codeplex.com/workitem/132)
* [Console forces cursor to the end of line](https://nuget.codeplex.com/workitem/135)
* [Remove-Package intellisense is broken](https://nuget.codeplex.com/workitem/136)
* [Add RequireLicenseAcceptance Flag to .nuspec and Feed](https://nuget.codeplex.com/workitem/137)
* [Add LicenseUrl to .nuspec Format and Package Feed](https://nuget.codeplex.com/workitem/138)
* [Clicking Install For Package That Requires Acceptance Should Show Acceptance Dialog](https://nuget.codeplex.com/workitem/139)
* [Add Disclaimer Text to the Add Package Dialog](https://nuget.codeplex.com/workitem/140)
* [Add Disclaimer When the Package Console is run the first time](https://nuget.codeplex.com/workitem/143)
* [Display Disclaimer After Installing Package In The Console](https://nuget.codeplex.com/workitem/144)
* [Rename the .nupack extension to .nupkg](https://nuget.codeplex.com/workitem/146)