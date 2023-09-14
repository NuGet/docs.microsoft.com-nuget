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

To use this latest release:

* First uninstall your older build. You need to run VS as administrator to do this.
* Remove all the existing feeds that you have.
* Add a new feed pointing to <https://go.microsoft.com/fwlink/?LinkId=206669>.

## NuGet 1.1

The list of issues fixed in this release can be found here

## NuGet 1.0 RTM

One issue was fixed for RTM since the RC.

* Issue 474: Removing Packages Affects All Project In Solution

## Release Candidate

The following are the changes made in this Release Candidate since CTP 2. Visit the Issue Tracker to see the full list of bugs.

* Updating Package from Console does not update dependencies.
* Adding package picks up bin not package reference (CTP1)](http://nuget.codeplex.com/workitem/442)
* Updating a package leaves broken references
* Get-Package -Updates fails in the dialog, or when the 'All' aggregate source is selected in the console
* Getting package verification errors
* Warn users when a package cannot be installed from the Add Package Dialog
* Get-Package -Updates throws when updating large number of packages
* Improve error handling when nuspec files are authored incorrectly
* Nuget pack ignores specified files
* Removing the second-to-last package source and then clicking "Move Down" crashes VS
* Remove assembly reference while installing packages
* InvalidOperationException when opening Settings dialog
* Access Key for Package Source in Package Manager Console doesn't work
* NuGet VS Settings Dialog Access Keys Give Focus to Wrong Fields
* Package ID intellisense should not query too many items
* Failure adding package to project with a dot character in the Project name
* Issue with specified files in nuspec
* Correct official feed should get registered when using newer build
* Tags should use spaces instead of #
* IPackageMetadata lacks some useful information
* Add Report Abuse Link to the Dialog
* Using App_Data to unzip packages breaks in Visual Studio
* Implement Tags
* PackageBuilder allows empty package with no dependencies to be created
* Add Owners Field for the Package
* Update the VSIX manifest to say NuGet Package Manager rather than VSIX Tools
* Get-Package command throws error when All source is selected
* Allow ordering of package sources in Options dialog
* Update-Package does not remove older version
* Implement Version Range Specification for Dependencies
* Visual Studio crashes when clicking "Add new package"
* Display Downloads and Ratings in the Add Package Dialog
* Changing between package sources in the Dialog doesn't update active source
* Remove Key Binding for Package Manager Console Window
* Install-Package is not recognized as the name of a cmdlet...
* Installing a package from a local feed the dependencies on regular feeds are not resolved
* RemoveDependencies should skip dependencies that are still in use
* If cancelling page navigation, user cannot navigate to a different page while the original page request returns
* Investigate performance of NuPack.Server for serving feeds with large number of packages.
* The second time I filter for a package it uses the "New" package source, instead of the previously selected source.
* Default package source should be selected when selecting the "Online" tab on the dialog.
* List-Package should show installed packages by default
* Assembly Reference HintPaths
* Exception while opening Package Manager Console
* Console intellisense downloads entire feed
* 'Default' package source should be renamed to 'Active'
* Package sources UI: pressing OK should add the new source if Name/Source fields are non-empty
* Dialog becomes super slow when the number of installed packages is large
* Support Binding Redirects for Strong Named Assemblies
* Add Package Reference... UI to include drop down for Package source
* NuPack needs to support config transform agnostically of the config file name
* Allows BasePath to be Overriden in NuPack.exe
* Package Source Fallback Behavior
* Crash on GUI
* Add sorting options to Add Package Dialog
* shortcut key to clear the Package Manager Console
* PowerConsole causes NuPack Console to fail
* Console and Add Package Dialog should set user agent in requests
* Set version number of the VSIX and NuPack.exe in the build.
* Hide common PowerShell parameters from -?
* Add -detailed help for console commands
* Add Package Dialog Should Allow Choosing the Current Package Source
* Move NuPack.Core classes into different namespaces
* Add help to cmdlets
* Verify hash from feed after package download

## CTP 2

The following are the most significant changes made in CTP 2:

* Switched the package feed from ATOM to an OData service endpoint: If you upgrade to the CTP2 version of NuGet, be sure to add the following URL as a package source: `https://feed.nuget.org/ctp2/odata/v1/`.
* Renamed the Add-Package command to *Install-Package*.
* Updated the `.nuspec` Format. The `.nuspec` format now includes the *iconUrl* field for specifying a 32x32 png icon which will show up in the Add Package Dialog. So be sure to set that to distinguish your package. The `.nuspec` format also includes the new *projectUrl* field which you can use to point to a web page that provides more information about your package.

This build will not work with old `.nupkg` files. If you get null reference exceptions, you're using an old `.nupkg` file and
need to rebuild it with the updated NuGet command line tool.

The following is a list of features and bugs that were fixed for NuGet CTP 2 (does not include bugs for minor code cleanups etc.).

* Error unpacking package assemblies when specifiying the TargetFramework for an assembly.
* Make NuPack Console window more discoverable
* ILMerge the nupack.exe release
* Better error/exception handling
* [Nupack.Core
* Need a new icon for the console
* Localize strings in the Dialog
* NuPack caches downloaded .nupack files in memory
* NuPack Console: Change the default shortcut for displaying console
* ProjectSystem should support default values for common properties
* Running nupack.exe in a folder with just one nuspec file should use that nuspec
* Project Menu Shows Up Even When No Project/Solution Is Loaded
* build.cmd fails on a clean clone of the codebase
* Updates available feature
* Dialog: Adding a package through the dialog removes the prompt in the console
* Adding a package by clicking 'Install' is often slow, with no visual feedback
* There is no way to discover which of my installed packages have updates.
* There is no way to update an installed package in the dialog.
* There is no way to uninstall an installed package in the dialog
* &ldquo;Add Package Reference&hellip;&rdquo; appears on the context menu of installed references
* After updating a package from the console, it shows both the old version and the new version as installed
* The activity in the console, when using the dialog, disappears after use
* Cleanup command line parsing in nupack.exe
* Add a friendly name to package sources
* Update .nuspec to support including package icons
* Feed UI doesn't allow copying the URL
* Better remove-package error handling.
* Typing in Console Window depends on cursor focus
* Error messages look awful
* The performance of Remove-Package for a package that isn't installed is bad
* Removing a package fails when there are no package sources
* Remove-Package fails when the package source is unavailable
* Add Title to the package metadata and the feed.
* Add the -Source parameter back to Add-Package
* List-Package should have a -Source parameter
* Update NuPack.Server to require NuPack User Agent To Download Package
* License Acceptance Dialog Must List Licenses For All Dependencies That Require Acceptance
* Log an error when a package throws in the feed
* NuPack.exe should not allow an empty &lt;licenseurl&gt; element
* Rename List-Package to Get-Package, Add-Package to Install-Package, and Remove-Package to Uninstall-Package
* Using the Add Package Reference menu item from the Solution Navigator crashes Visual Studio
* "Available package sources" label is missing a colon
* Make .nuspec xml element casing consistently camel cased
* The NuPack VSIX's manifest needs to turn on the 'admin' bit
* If you run List-Package with no feeds, you get null ref error
* nuget.exe: specify destination path
* Powershell Errors Opening Package Management Console on WinXP
* VS Crashes while trying to load package list
* allow meta packages (no files, only dependencies)](http://nuget.codeplex.com/workitem/180)
* Convert Powershell Script to Powershell 2.0 Module
* PathResolver should discard path portion preceeding wildcard characters when target is specified
* No dependencies
* Error installing Elmah
* Config transforms don't work correctly with &lt;configsections&gt;
* The variable '$global:projectCache' cannot be retrieved because it has not been set
* Add MSBuild task for creating NuPack packages
* list-package needs to support searching/filtering
* Always display a link to license if the package author provides a license URL
* Occasional "Access Denied" exception with Remove-Package
* Unit Tests Failing: InvalidPackageIsExcludedFromFeedItems &amp; CreatingFeedConvertsPackagesToAtomEntries
* Allow for a fallback/default set of files if a specfic framework version cannot be found
* Add Package Reference... UI cannot remove a package
* Add Package Reference crashes studio when one or more project is unloaded
* Config transform does not appear to work on web.debug.config file
* init.ps1 not firing on custom package
* When adding paths to the feedlist, the default button is set to OK, so if I press ENTER it automatically closes
* Attempt to uninstall a dependency will crash VS if attempted 2 times in a row
* Display the Project URL in the Add Package dialog
* Default the Add-Package dialog to Installed Packages
* Change Add Package Dialog menu item.
* Rename namespaces and assemblies
* Rename the NuPack Project to NuGet
* Add the following text under the list of dependencies
* Change the license acceptance text in the License Acceptance Dialog
* Change the text in the License Acceptance Dialog above the list of packages
* OData doesn't work with an fwlink URL
* Package Manager UI: Over aggressive caching of package count used for paging
* NuPack / NuGet -&gt; Package Manager Console error
* Add Package Dialog shows License Acceptance For Already Installed Packaged

## CTP 1

The following is a list of features and bugs that were fixed for NuGet CTP 1.

* Package extension should be renamed to .nupack
* Move package file into folder
* Merge install &amp; Add PS commands
* Create aliases for Verb-Noun cmdlets
* NuPack gets confused when switching solution in VS
* We should hide the 'packages' solution folder by default
* Add support for token replacement in content items.
* NuPack.UI should use the PackageSource API
* [Nupack.Core
* Deleting default project from solution still shows the deleted project as default
* New-Package fails with "Cannot add part for the specified URI because it's already in the package."
* Remove "NuPack" strings from Visual Studio GUI
* Add Apache Header To a COPYRIGHT.txt file
* Remove Update-PackageSource Command
* Package Manager unusable when loading profile throws an exception
* init.ps1, install.ps1 and uninstall.ps1 need to receive additional state
* Combine Console and GUI Packages Into One Package
* Xml transform logic doesn't work if applied to XML that isn't at the root
* Manage package sources settings dialog not updating the NuPack console
* NuPack Console UI: Rename 'Package feed' drop-down list to 'Package source'
* NuPack Console Options: Rename 'Repository UI' to be consistent with NuPack Console
* Add-Package fails against a website that was opened from IIS or a URL
* Package Manager Source Doesn't Work With FwLink
* Set the default package source
* When adding package sources in option, when only one source is supplied, assume it's the default.
* The Dialog UI shows fake "recent" packages
* Options: Clicking cancel does not cancel changes
* Add Package Reference Dialog Search should be case insensitive
* Fix company metadata in AssemblyInfo.cs files
* Version number for the VSIX
* Remove-Package: Using -? displays help twice
* Execute install/uninstall packages for project level packages
* Server unable to create feed when one nupack fails validation
* Need to Replace NuPack Icons
* NTLM http proxy does not authenticate to the package feed.
* The dialog doesn't always start centered in the VS window
* Many of the fields in a packages details are not being populated in the dialog
* Dialog UI doesn't show Authors' names
* Why -Version for Remove-Package
* Remove the Recent tab on the Dialog UI
* VS crash when right click on solution folder after opening Dialog UI at least one.
* Change the -Local parameter of List-Package to -Installed
* Rename packages.xml to NuPack.config
* Console forces cursor to the end of line
* Remove-Package intellisense is broken
* Add RequireLicenseAcceptance Flag to .nuspec and Feed
* Add LicenseUrl to .nuspec Format and Package Feed
* Clicking Install For Package That Requires Acceptance Should Show Acceptance Dialog
* Add Disclaimer Text to the Add Package Dialog
* Add Disclaimer When the Package Console is run the first time
* Display Disclaimer After Installing Package In The Console
* Rename the .nupack extension to .nupkg