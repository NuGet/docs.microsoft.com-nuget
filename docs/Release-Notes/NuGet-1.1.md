# NuGet 1.0 and 1.1 Release Notes

[NuGet 1.2 Release Notes](nuget-1.2)

NuGet 1.0 was released on January 13, 2011.  NuGet 1.1 was released on February 12, 2011.

## Overview

This document contains the release notes for the various releases of NuGet 1.0 grouped according to major preview release.

NuGet includes the following components:

* *NuGet.Tools.vsix* - which consists of:
**Add Library Package Dialog** - Dialog within Visual Studio used to browse and install packages.
**Package Manager Console** - Powershell based console within Visual Studio.
* *NuGet Command Line Tool* - Tool used to create (and eventually publish) packages.

The NuGet Tools Visual Studio Extension (*NuGet.Tools.vsix*) requires:

* Visual Studio 2010 or Visual Web Developer 2010 Express.

The NuGet Command Line Tool requires:

* .NET Framework Version 4 

## Installation

To use this <a title="Download NuGet" href="http://nuget.codeplex.com/releases/view/52018">latest release</a>:

* First uninstall your older build. You'll need to run VS as administrator to do this. 
* Remove all the existing feeds that you have. 
* Add a new feed pointing to <a href="http://go.microsoft.com/fwlink/?LinkId=206669">http://go.microsoft.com/fwlink/?LinkId=206669</a>
    
## NuGet 1.1

The list of issues fixed in this release <a title="NuGet 1.1 issues" href="http://nuget.codeplex.com/workitem/list/advanced?keyword=&amp;status=All&amp;type=All&amp;priority=All&amp;release=NuGet%201.1&amp;assignedTo=All&amp;component=All&amp;sortField=LastUpdatedDate&amp;sortDirection=Descending&amp;page=0">can be found here</a>.

## NuGet 1.0 RTM

One issue was fixed for RTM since the RC.

* <a href="http://nuget.codeplex.com/workitem/474">Issue 474: Removing Packages Affects All Project In Solution</a>
    
## Release Candidate

The following are the changes made in this Release Candidate since CTP 2. Visit the Issue Tracker to see the full list of bugs.

* <a href="http://nuget.codeplex.com/workitem/443">Updating Package from Console does not update dependencies.</a>
* <a href="http://nuget.codeplex.com/workitem/442">Adding package picks up bin not package reference (CTP1)</a>
* <a href="http://nuget.codeplex.com/workitem/440">Updating a package leaves broken references</a>
* <a href="http://nuget.codeplex.com/workitem/439">Get-Package -Updates fails in the dialog, or when the 'All' aggregate source is selected in the console</a>
* <a href="http://nuget.codeplex.com/workitem/426">Getting package verification errors</a>
* <a href="http://nuget.codeplex.com/workitem/425">Warn users when a package cannot be installed from the Add Package Dialog</a>
* <a href="http://nuget.codeplex.com/workitem/424">Get-Package -Updates throws when updating large number of packages</a>
* <a href="http://nuget.codeplex.com/workitem/423">Improve error handling when nuspec files are authored incorrectly</a>
* <a href="http://nuget.codeplex.com/workitem/422">Nuget pack ignores specified files</a>
* <a href="http://nuget.codeplex.com/workitem/418">Removing the second-to-last package source and then clicking "Move Down" crashes VS</a>
* <a href="http://nuget.codeplex.com/workitem/413">Remove assembly reference while installing packages</a>
* <a href="http://nuget.codeplex.com/workitem/411">InvalidOperationException when opening Settings dialog</a>
* <a href="http://nuget.codeplex.com/workitem/410">Access Key for Package Source in Package Manager Console doesn't work</a>
* <a href="http://nuget.codeplex.com/workitem/409">NuGet VS Settings Dialog Access Keys Give Focus to Wrong Fields</a>
* <a href="http://nuget.codeplex.com/workitem/404">Package ID intellisense should not query too many items</a>
* <a href="http://nuget.codeplex.com/workitem/403">Failure adding package to project with a dot character in the Project name</a>
* <a href="http://nuget.codeplex.com/workitem/400">Issue with specified files in nuspec</a>
* <a href="http://nuget.codeplex.com/workitem/399">Correct official feed should get registered when using newer build</a>
* <a href="http://nuget.codeplex.com/workitem/397">Tags should use spaces instead of #</a>
* <a href="http://nuget.codeplex.com/workitem/388">IPackageMetadata lacks some useful information</a>
* <a href="http://nuget.codeplex.com/workitem/386">Add Report Abuse Link to the Dialog</a>
* <a href="http://nuget.codeplex.com/workitem/380">Using App_Data to unzip packages breaks in Visual Studio</a>
* <a href="http://nuget.codeplex.com/workitem/376">Implement Tags</a>
* <a href="http://nuget.codeplex.com/workitem/373">PackageBuilder allows empty package with no dependencies to be created</a>
* <a href="http://nuget.codeplex.com/workitem/365">Add Owners Field for the Package</a>
* <a href="http://nuget.codeplex.com/workitem/364">Update the VSIX manifest to say NuGet Package Manager rather than VSIX Tools</a>
* <a href="http://nuget.codeplex.com/workitem/359">Get-Package command throws error when All source is selected</a>
* <a href="http://nuget.codeplex.com/workitem/356">Allow ordering of package sources in Options dialog</a>
* <a href="http://nuget.codeplex.com/workitem/352">Update-Package does not remove older version</a>
* <a href="http://nuget.codeplex.com/workitem/347">Implement Version Range Specification for Dependencies</a>
* <a href="http://nuget.codeplex.com/workitem/346">Visual Studio crashes when clicking "Add new package"</a>
* <a href="http://nuget.codeplex.com/workitem/345">Display Downloads and Ratings in the Add Package Dialog</a>
* <a href="http://nuget.codeplex.com/workitem/344">Changing between package sources in the Dialog doesn't update active source</a>
* <a href="http://nuget.codeplex.com/workitem/339">Remove Key Binding for Package Manager Console Window</a>
* <a href="http://nuget.codeplex.com/workitem/338">Install-Package is not recognized as the name of a cmdlet...</a>
* <a href="http://nuget.codeplex.com/workitem/332">Installing a package from a local feed the dependencies on regular feeds are not resolved</a>
* <a href="http://nuget.codeplex.com/workitem/331">RemoveDependencies should skip dependencies that are still in use</a>
* <a href="http://nuget.codeplex.com/workitem/325">If cancelling page navigation, user cannot navigate to a different page while the original page request returns</a>
* <a href="http://nuget.codeplex.com/workitem/324">Investigate performance of NuPack.Server for serving feeds with large number of packages.</a>
* <a href="http://nuget.codeplex.com/workitem/321">The second time I filter for a package it uses the "New" package source, instead of the previously selected source.</a>
* <a href="http://nuget.codeplex.com/workitem/320">Default package source should be selected when selecting the "Online" tab on the dialog.</a>
* <a href="http://nuget.codeplex.com/workitem/309">List-Package should show installed packages by default</a>
* <a href="http://nuget.codeplex.com/workitem/294">Assembly Reference HintPaths</a>
* <a href="http://nuget.codeplex.com/workitem/268">Exception while opening Package Manager Console</a>
* <a href="http://nuget.codeplex.com/workitem/259">Console intellisense downloads entire feed</a>
* <a href="http://nuget.codeplex.com/workitem/258">'Default' package source should be renamed to 'Active'</a>
* <a href="http://nuget.codeplex.com/workitem/257">Package sources UI: pressing OK should add the new source if Name/Source fields are non-empty</a>
* <a href="http://nuget.codeplex.com/workitem/243">Dialog becomes super slow when the number of installed packages is large</a>
* <a href="http://nuget.codeplex.com/workitem/238">Support Binding Redirects for Strong Named Assemblies</a>
* <a href="http://nuget.codeplex.com/workitem/226">Add Package Reference... UI to include drop down for Package source</a>
* <a href="http://nuget.codeplex.com/workitem/224">NuPack needs to support config transform agnostically of the config file name</a>
* <a href="http://nuget.codeplex.com/workitem/222">Allows BasePath to be Overriden in NuPack.exe</a>
* <a href="http://nuget.codeplex.com/workitem/204">Package Source Fallback Behavior</a>
* <a href="http://nuget.codeplex.com/workitem/201">Crash on GUI</a>
* <a href="http://nuget.codeplex.com/workitem/179">Add sorting options to Add Package Dialog</a>
* <a href="http://nuget.codeplex.com/workitem/174">shortcut key to clear the Package Manager Console</a>
* <a href="http://nuget.codeplex.com/workitem/166">PowerConsole causes NuPack Console to fail</a>
* <a href="http://nuget.codeplex.com/workitem/141">Console and Add Package Dialog should set user agent in requests</a>
* <a href="http://nuget.codeplex.com/workitem/134">Set version number of the VSIX and NuPack.exe in the build.</a>
* <a href="http://nuget.codeplex.com/workitem/118">Hide common PowerShell parameters from -?</a>
* <a href="http://nuget.codeplex.com/workitem/110">Add -detailed help for console commands</a>
* <a href="http://nuget.codeplex.com/workitem/88">Add Package Dialog Should Allow Choosing the Current Package Source</a>
* <a href="http://nuget.codeplex.com/workitem/50">Move NuPack.Core classes into different namespaces</a>
* <a href="http://nuget.codeplex.com/workitem/23">Add help to cmdlets</a>
* <a href="http://nuget.codeplex.com/workitem/18">Verify hash from feed after package download</a>

## CTP 2

The following are the most significant changes made in CTP 2:

* Switched the package feed from ATOM to an OData service endpoint: If you upgrade to the CTP2 version of NuGet, be sure to add the following URL as a package source: <a title="http://go.microsoft.com/fwlink/?LinkID=204820" href="http://go.microsoft.com/fwlink/?LinkID=204820">http://go.microsoft.com/fwlink/?LinkID=204820</a>.
* Renamed the Add-Package command to *Install-Package*.
* Updated the NuSpec Format. The NuSpec format now includes the *iconUrl* field for specifying a 32x32 png icon 
which will show up in the Add Package Dialog. So be sure to set that to distinguish your package.
The NuSpec format also includes the new *projectUrl* field which you can use to point to a web page that 
provides more information about your package.

This build will not work with old nupkg files. If you get null reference exceptions, you're using an old nupkg file and 
need to rebuild it with the updated 
<a title="NuGet Command Line Tool" href="http://nuget.codeplex.com/releases/52017/download/165468">NuGet command line tool</a>.

The following is a list of features and bugs that were fixed for NuGet CTP 2 (does not include bugs for minor code cleanups etc.).

* <a href="http://nuget.codeplex.com/workitem/10">Error unpacking package assemblies when specifiying the TargetFramework for an assembly.</a>
* <a href="http://nuget.codeplex.com/workitem/14">Make NuPack Console window more discoverable</a>
* <a href="http://nuget.codeplex.com/workitem/19">ILMerge the nupack.exe release</a>
* <a href="http://nuget.codeplex.com/workitem/24">Better error/exception handling</a>
* <a href="http://nuget.codeplex.com/workitem/28">[Nupack.Core]: PackageManager should gracefully handle feed-related errors</a>
* <a href="http://nuget.codeplex.com/workitem/29">Need a new icon for the console</a>
* <a href="http://nuget.codeplex.com/workitem/38">Localize strings in the Dialog</a>
* <a href="http://nuget.codeplex.com/workitem/40">NuPack caches downloaded .nupack files in memory</a>
* <a href="http://nuget.codeplex.com/workitem/48">NuPack Console: Change the default shortcut for displaying console</a>
* <a href="http://nuget.codeplex.com/workitem/49">ProjectSystem should support default values for common properties</a>
* <a href="http://nuget.codeplex.com/workitem/52">Running nupack.exe in a folder with just one nuspec file should use that nuspec</a>
* <a href="http://nuget.codeplex.com/workitem/54">Project Menu Shows Up Even When No Project/Solution Is Loaded</a>
* <a href="http://nuget.codeplex.com/workitem/56">build.cmd fails on a clean clone of the codebase</a>
* <a href="http://nuget.codeplex.com/workitem/57">Updates available feature</a>
* <a href="http://nuget.codeplex.com/workitem/73">Dialog: Adding a package through the dialog removes the prompt in the console</a>
* <a href="http://nuget.codeplex.com/workitem/80">Adding a package by clicking 'Install' is often slow, with no visual feedback</a>
* <a href="http://nuget.codeplex.com/workitem/82">There is no way to discover which of my installed packages have updates.</a>
* <a href="http://nuget.codeplex.com/workitem/83">There is no way to update an installed package in the dialog.</a>
* <a href="http://nuget.codeplex.com/workitem/84">There is no way to uninstall an installed package in the dialog</a>
* <a href="http://nuget.codeplex.com/workitem/85">&ldquo;Add Package Reference&hellip;&rdquo; appears on the context menu of installed references</a>
* <a href="http://nuget.codeplex.com/workitem/86">After updating a package from the console, it shows both the old version and the new version as installed</a>
* <a href="http://nuget.codeplex.com/workitem/87">The activity in the console, when using the dialog, disappears after use</a>
* <a href="http://nuget.codeplex.com/workitem/89">Cleanup command line parsing in nupack.exe</a>
* <a href="http://nuget.codeplex.com/workitem/98">Add a friendly name to package sources</a>
* <a href="http://nuget.codeplex.com/workitem/103">Update NuSpec to support including package icons</a>
* <a href="http://nuget.codeplex.com/workitem/105">Feed UI doesn't allow copying the URL</a>
* <a href="http://nuget.codeplex.com/workitem/107">Better remove-package error handling.</a>
* <a href="http://nuget.codeplex.com/workitem/112">Typing in Console Window depends on cursor focus</a>
* <a href="http://nuget.codeplex.com/workitem/116">Error messages look awful</a>
* <a href="http://nuget.codeplex.com/workitem/117">The performance of Remove-Package for a package that isn't installed is bad</a>
* <a href="http://nuget.codeplex.com/workitem/119">Removing a package fails when there are no package sources</a>
* <a href="http://nuget.codeplex.com/workitem/120">Remove-Package fails when the package source is unavailable</a>
* <a href="http://nuget.codeplex.com/workitem/125">Add Title to the package metadata and the feed.</a>
* <a href="http://nuget.codeplex.com/workitem/127">Add the -Source parameter back to Add-Package</a>
* <a href="http://nuget.codeplex.com/workitem/128">List-Package should have a -Source parameter</a>
* <a href="http://nuget.codeplex.com/workitem/142">Update NuPack.Server to require NuPack User Agent To Download Package</a>
* <a href="http://nuget.codeplex.com/workitem/145">License Acceptance Dialog Must List Licenses For All Dependencies That Require Acceptance</a>
* <a href="http://nuget.codeplex.com/workitem/150">Log an error when a package throws in the feed</a>
* <a href="http://nuget.codeplex.com/workitem/152">NuPack.exe should not allow an empty &lt;licenseurl&gt;element</a>
* <a href="http://nuget.codeplex.com/workitem/155">Rename List-Package to Get-Package, Add-Package to Install-Package, and Remove-Package to Uninstall-Package</a>
* <a href="http://nuget.codeplex.com/workitem/158">Using the Add Package Reference menu item from the Solution Navigator crashes Visual Studio</a>
* <a href="http://nuget.codeplex.com/workitem/160">"Available package sources" label is missing a colon</a>
* <a href="http://nuget.codeplex.com/workitem/161">Make Nuspec xml element casing consistently camel cased</a>
* <a href="http://nuget.codeplex.com/workitem/162">The NuPack VSIX's manifest needs to turn on the 'admin' bit</a>
* <a href="http://nuget.codeplex.com/workitem/164">If you run List-Package with no feeds, you get null ref error</a>
* <a href="http://nuget.codeplex.com/workitem/171">NuGet.exe: specify destination path</a>
* <a href="http://nuget.codeplex.com/workitem/175">Powershell Errors Opening Package Management Console on WinXP</a>
* <a href="http://nuget.codeplex.com/workitem/176">VS Crashes while trying to load package list</a>
* <a href="http://nuget.codeplex.com/workitem/180">allow meta packages (no files, only dependencies)</a>
* <a href="http://nuget.codeplex.com/workitem/181">Convert Powershell Script to Powershell 2.0 Module</a>
* <a href="http://nuget.codeplex.com/workitem/183">PathResolver should discard path portion preceeding wildcard characters when target is specified</a>
* <a href="http://nuget.codeplex.com/workitem/186">No dependencies</a>
* <a href="http://nuget.codeplex.com/workitem/192">Error installing Elmah</a>
* <a href="http://nuget.codeplex.com/workitem/194">Config transforms don't work correctly with &lt;configsections&gt;</a>
* <a href="http://nuget.codeplex.com/workitem/203">The variable '$global:projectCache' cannot be retrieved because it has not been set</a>
* <a href="http://nuget.codeplex.com/workitem/205">Add MSBuild task for creating NuPack packages</a>
* <a href="http://nuget.codeplex.com/workitem/206">list-package needs to support searching/filtering</a>
* <a href="http://nuget.codeplex.com/workitem/208">Always display a link to license if the package author provides a license URL</a>
* <a href="http://nuget.codeplex.com/workitem/213">Occasional "Access Denied" exception with Remove-Package</a>
* <a href="http://nuget.codeplex.com/workitem/214">Unit Tests Failing: InvalidPackageIsExcludedFromFeedItems &amp; CreatingFeedConvertsPackagesToAtomEntries</a>
* <a href="http://nuget.codeplex.com/workitem/223">Allow for a fallback/default set of files if a specfic framework version cannot be found</a>
* <a href="http://nuget.codeplex.com/workitem/225">Add Package Reference... UI cannot remove a package</a>
* <a href="http://nuget.codeplex.com/workitem/228">Add Package Reference crashes studio when one or more project is unloaded</a>
* <a href="http://nuget.codeplex.com/workitem/229">Config transform does not appear to work on web.debug.config file</a>
* <a href="http://nuget.codeplex.com/workitem/237">init.ps1 not firing on custom package</a>
* <a href="http://nuget.codeplex.com/workitem/240">When adding paths to the feedlist, the default button is set to OK, so if I press ENTER it automatically closes</a>
* <a href="http://nuget.codeplex.com/workitem/241">Attempt to uninstall a dependency will crash VS if attempted 2 times in a row</a>
* <a href="http://nuget.codeplex.com/workitem/253">Display the Project URL in the Add Package dialog</a>
* <a href="http://nuget.codeplex.com/workitem/254">Default the Add-Package dialog to Installed Packages</a>
* <a href="http://nuget.codeplex.com/workitem/261">Change Add Package Dialog menu item.</a>
* <a href="http://nuget.codeplex.com/workitem/274">Rename namespaces and assemblies</a>
* <a href="http://nuget.codeplex.com/workitem/282">Rename the NuPack Project to NuGet</a>
* <a href="http://nuget.codeplex.com/workitem/288">Add the following text under the list of dependencies</a>
* <a href="http://nuget.codeplex.com/workitem/291">Change the license acceptance text in the License Acceptance Dialog</a>
* <a href="http://nuget.codeplex.com/workitem/292">Change the text in the License Acceptance Dialog above the list of packages</a>
* <a href="http://nuget.codeplex.com/workitem/304">OData doesn't work with an fwlink URL</a>
* <a href="http://nuget.codeplex.com/workitem/317">Package Manager UI: Over aggressive caching of package count used for paging</a>
* <a href="http://nuget.codeplex.com/workitem/335">NuPack / NuGet -&gt; Package Manager Console error</a>
* <a href="http://nuget.codeplex.com/workitem/336">Add Package Dialog shows License Acceptance For Already Installed Packaged</a>
    

## CTP 1
The following is a list of features and bugs that were fixed for NuGet CTP 1.

* <a href="http://nuget.codeplex.com/workitem/1">Package extension should be renamed to .nupack </a>
* <a href="http://nuget.codeplex.com/workitem/2">Move package file into folder </a>
* <a href="http://nuget.codeplex.com/workitem/3">Merge install &amp; Add PS commands </a>
* <a href="http://nuget.codeplex.com/workitem/4">Create aliases for Verb-Noun cmdlets </a>
* <a href="http://nuget.codeplex.com/workitem/6">NuPack gets confused when switching solution in VS </a>
* <a href="http://nuget.codeplex.com/workitem/11">We should hide the 'packages' solution folder by default </a>
* <a href="http://nuget.codeplex.com/workitem/12">Add support for token replacement in content items. </a>
* <a href="http://nuget.codeplex.com/workitem/26">NuPack.UI should use the PackageSource API </a>
* <a href="http://nuget.codeplex.com/workitem/27">[Nupack.Core]: PackageManager marks packages as installed prior to installing them </a>
* <a href="http://nuget.codeplex.com/workitem/30">Deleting default project from solution still shows the deleted project as default </a>
* <a href="http://nuget.codeplex.com/workitem/32">New-Package fails with "Cannot add part for the specified URI because it is already in the package." </a>
* <a href="http://nuget.codeplex.com/workitem/35">Remove "NuPack" strings from Visual Studio GUI </a>
* <a href="http://nuget.codeplex.com/workitem/36">Add Apache Header To a COPYRIGHT.txt file </a>
* <a href="http://nuget.codeplex.com/workitem/37">Remove Update-PackageSource Command </a>
* <a href="http://nuget.codeplex.com/workitem/39">Package Manager unusable when loading profile throws an exception </a>
* <a href="http://nuget.codeplex.com/workitem/41">init.ps1, install.ps1 and uninstall.ps1 need to receive additional state </a>
* <a href="http://nuget.codeplex.com/workitem/42">Combine Console and GUI Packages Into One Package </a>
* <a href="http://nuget.codeplex.com/workitem/43">Xml transform logic doesn't work if applied to xml that isn't at the root </a>
* <a href="http://nuget.codeplex.com/workitem/44">Manage package sources settings dialog not updating the NuPack console </a>
* <a href="http://nuget.codeplex.com/workitem/45">NuPack Console UI: Rename 'Package feed' drop-down list to 'Package source' </a>
* <a href="http://nuget.codeplex.com/workitem/46">NuPack Console Options: Rename 'Repository UI' to be consistent with NuPack Console </a>
* <a href="http://nuget.codeplex.com/workitem/53">Add-Package fails against a website that was opened from IIS or a URL </a>
* <a href="http://nuget.codeplex.com/workitem/55">Package Manager Source Doesn't Work With FwLink </a>
* <a href="http://nuget.codeplex.com/workitem/59">Set the default package source </a>
* <a href="http://nuget.codeplex.com/workitem/60">When adding package sources in option, when only one source is supplied, assume it is the default. </a>
* <a href="http://nuget.codeplex.com/workitem/62">The Dialog UI shows fake "recent" packages </a>
* <a href="http://nuget.codeplex.com/workitem/63">Options: Clicking cancel does not cancel changes </a>
* <a href="http://nuget.codeplex.com/workitem/65">Add Package Reference Dialog Search should be case insensitive </a>
* <a href="http://nuget.codeplex.com/workitem/67">Fix company metadata in AssemblyInfo.cs files </a>
* <a href="http://nuget.codeplex.com/workitem/71">Version number for the VSIX </a>
* <a href="http://nuget.codeplex.com/workitem/72">Remove-Package: Using -? displays help twice </a>
* <a href="http://nuget.codeplex.com/workitem/74">Execute install/uninstall packages for project level packages </a>
* <a href="http://nuget.codeplex.com/workitem/90">Server unable to create feed when one nupack fails validation </a>
* <a href="http://nuget.codeplex.com/workitem/94">Need to Replace NuPack Icons </a>
* <a href="http://nuget.codeplex.com/workitem/96">NTLM http proxy does not authenticate to the package feed. </a>
* <a href="http://nuget.codeplex.com/workitem/100">The dialog doesn't always start centered in the VS window </a>
* <a href="http://nuget.codeplex.com/workitem/102">Many of the fields in a packages details are not being populated in the dialog </a>
* <a href="http://nuget.codeplex.com/workitem/108">Dialog UI doesn't show Authors' names </a>
* <a href="http://nuget.codeplex.com/workitem/113">Why -Version for Remove-Package </a>
* <a href="http://nuget.codeplex.com/workitem/115">Remove the Recent tab on the Dialog UI </a>
* <a href="http://nuget.codeplex.com/workitem/126">VS crash when right click on solution folder after opening Dialog UI at least one. </a>
* <a href="http://nuget.codeplex.com/workitem/129">Change the -Local parameter of List-Package to -Installed </a>
* <a href="http://nuget.codeplex.com/workitem/132">Rename packages.xml to NuPack.config </a>
* <a href="http://nuget.codeplex.com/workitem/135">Console forces cursor to the end of line </a>
* <a href="http://nuget.codeplex.com/workitem/136">Remove-Package intellisense is broken </a>
* <a href="http://nuget.codeplex.com/workitem/137">Add RequireLicenseAcceptance Flag to NuSpec and Feed </a>
* <a href="http://nuget.codeplex.com/workitem/138">Add LicenseUrl to NuSpec Format and Package Feed </a>
* <a href="http://nuget.codeplex.com/workitem/139">Clicking Install For Package That Requires Acceptance Should Show Acceptance Dialog </a>
* <a href="http://nuget.codeplex.com/workitem/140">Add Disclaimer Text to the Add Package Dialog </a>
* <a href="http://nuget.codeplex.com/workitem/143">Add Disclaimer When the Package Console is run the first time </a>
* <a href="http://nuget.codeplex.com/workitem/144">Display Disclaimer After Installing Package In The Console </a>
* <a href="http://nuget.codeplex.com/workitem/146">Rename the .nupack extension to .nupkg </a>
    