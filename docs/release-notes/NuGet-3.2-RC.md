---
title: NuGet 3.2 RC Release Notes
description: Release notes for NuGet 3.2 RC including known issues, bug fixes, added features, and DCRs.
author: JonDouglas
ms.author: jodou
ms.date: 11/11/2016
ms.topic: release-notes
---

# NuGet 3.2 RC Release Notes

[NuGet 3.1.1 Release Notes](../release-notes/nuget-3.1.1.md) | [NuGet 3.2 Release Notes](../release-notes/nuget-3.2.md)

NuGet 3.2 release candidate was released September 2, 2015 as a collection of improvements and fixes for the 3.1.1 release.  Also, these are the first releases that are published first to the new dist.nuget.org repository.

## New Features

* Projects that live in the same folder can now have different `project.json` files in that folder specific to each project.  For each project, name the `project.json` file `{ProjectName}.project.json` and NuGet will properly reference and use that content for each project appropriately.  This supports a new feature  [1102](https://github.com/NuGet/Home/issues/1102)
* `NuGet.Config` now supports a globalPackagesFolder as a relative path - [1062](https://github.com/NuGet/Home/issues/1062)

## Command-line updates

This is the first version of the nuget.exe client that supports the NuGet v3 servers and restoring packages for projects managed with a `project.json` file.

#### There were a number of authenticated feed issues that were addressed in this release to improve interactions with the client.

* Install / restore interactions only submit credentials for the initial request to the authenticated feed - [1300](https://github.com/NuGet/Home/issues/1300), [456](https://github.com/NuGet/Home/issues/456)
* Push command does not resolve credentials from configuration - [1248](https://github.com/NuGet/Home/issues/1248)
* User agent and headers are now submitted to NuGet repositories to help with statistics tracking - [929](https://github.com/NuGet/Home/issues/929)

#### We made a number of improvements to better handle network failures while attempting to work with a remote NuGet repository:

* Improved error messages when unable to connect to remote feeds - [1238](https://github.com/NuGet/Home/issues/1238)
* Corrected NuGet restore command to properly return a 1 when an error condition occurs - [1186](https://github.com/NuGet/Home/issues/1186)
* Now retrying network connections every 200ms for a maximum of 5 attempts in the case of HTTP 5xx failures - [1120](https://github.com/NuGet/Home/issues/1120)
* Improved handling of server redirect responses during a push command - [1051](https://github.com/NuGet/Home/issues/1051)
* `nuget install -source` now supports either URL or repository name from Nuget.Config as an argument - [1046](https://github.com/NuGet/Home/issues/1046)
* Missing packages that were not located on a repository during a restore are now reported as errors instead of warnings [1038](https://github.com/NuGet/Home/issues/1038)
* Corrected multipartwebrequest handling of \r\n for Unix/Linux scenarios - [776](https://github.com/NuGet/Home/issues/776)

#### There are a number of fixes to issues with various commands:

* Push command no longer does a GET before a PUT against a package source - [1237](https://github.com/NuGet/Home/issues/1237)
* List command no longer repeats version numbers - [1185](https://github.com/NuGet/Home/issues/1185)
* Pack with the -build argument now properly supports C# 6.0 - [1107](https://github.com/NuGet/Home/issues/1107)
* Corrected issues attempting to pack an F# project built with Visual Studio 2015 - [1048](https://github.com/NuGet/Home/issues/1048)
* Restore now no-ops when packages already exist - [1040](https://github.com/NuGet/Home/issues/1040)
* Improved error messages when `packages.config` file is malformed - [1034](https://github.com/NuGet/Home/issues/1034)
* Corrected restore command with `-SolutionDirectory` switch to work with relative paths - [992](https://github.com/NuGet/Home/issues/992)
* Improved Updated command to support solution-wide update - [924](https://github.com/NuGet/Home/issues/924)

A complete list of issues addressed in this release can be found in the NuGet GitHub [Command-Line milestone](https://github.com/nuget/home/issues?utf8=%E2%9C%93&q=is%3Aissue+milestone%3A3.2.0-commandline+is%3Aclosed+-label%3AClosedAs%3ADuplicate).

## Visual Studio extension updates

### New Features in Visual Studio

* A new context menu item was added to the Solution Explorer on the solution node that allows packages to be restored without building the solution ([1274](https://github.com/NuGet/Home/issues/1274)).

![New 'Restore Packages' Context Menu Item](./media/NuGet-3.2/newContextMenu.png)

### Updates and Fixes in Visual Studio

#### The fixes for authenticated feeds were rolled up and addressed in the extension as well.  The following authentication items were also addressed in the extension:

* Now correctly treating NuGet v3 authenticated feeds properly, instead of as v2 authenticated feeds - [1216](https://github.com/NuGet/Home/issues/1216)
* Corrected request for authentication credentials in projects using `project.json` and communicating with v2 feeds - [1082](https://github.com/NuGet/Home/issues/1082)

#### Network connectivity had affected the user interface in Visual Studio, and we addressed this with the following fixes:

* Improved the maintenance of the local cache of package versions - [1096](https://github.com/NuGet/Home/issues/1096)
* Changed the failure behavior when connecting to a v3 feed to no longer attempt to treat it as a v2 feed - [1253](https://github.com/NuGet/Home/issues/1253)
* Now preventing install failures when installing a package with multiple package sources - [1183](https://github.com/NuGet/Home/issues/1183)

We improved handling of interactions with build operations:

* Now continuing to build projects if restoring packages for a single project fails - [1169](https://github.com/NuGet/Home/issues/1169)
* Installing a package into a project that is depended on by another project in the solution forces a solution rebuild - [981](https://github.com/NuGet/Home/issues/981)
* Corrected failed package installs to properly rollback changes to a project - [1265](https://github.com/NuGet/Home/issues/1265)
* Corrected inadvertent removal of the `developmentDependency` attribute on a package in `packages.config` - [1263](https://github.com/NuGet/Home/issues/1263)
* Calls to `install.ps1` now have a proper `$package.AssemblyReferences` object passed - [1245](https://github.com/NuGet/Home/issues/1245)
* No longer preventing uninstalls of packages in UWP projects while the project is in a bad state - [1128](https://github.com/NuGet/Home/issues/1128)
* Solutions containing a mix of `packages.config` and `project.json` projects are now properly built without requiring a second build operation - [1122](https://github.com/NuGet/Home/issues/1122)
* Properly locating app.config files if they are linked or located in a different folder - [1111](https://github.com/NuGet/Home/issues/1111), [894](https://github.com/NuGet/Home/issues/894)
* UWP projects can now install unlisted packages - [1109](https://github.com/NuGet/Home/issues/1109)
* Package restore is now allowed while a solution is not in a saved state - [1081](https://github.com/NuGet/Home/issues/1081)


Handling updates to configuration files were corrected:

* No longer removing a targets file delivered from a package on subsequent builds of a `project.json` managed project - [1288](https://github.com/NuGet/Home/issues/1288)
* No longer modifying Nuget.Config files during ASP.NET 5 solution build - [1201](https://github.com/NuGet/Home/issues/1201)
* No longer changing allowed versions constraint during package update - [1130](https://github.com/NuGet/Home/issues/1130)
* Lock files now remain locked during build - [1127](https://github.com/NuGet/Home/issues/1127)
* Now modifying `packages.config` and not rewriting it during updates - [585](https://github.com/NuGet/Home/issues/585)


Interactions with TFS source control are improved:

* No longer failing installs for packages that are bound to TFS - [1164](https://github.com/NuGet/Home/issues/1164), [980](https://github.com/NuGet/Home/issues/980)
* Corrected NuGet user interface to allow TFS 2013 integration - [1071](https://github.com/NuGet/Home/issues/1071)
* Corrected references to packages restored to properly come from a packages folder - [1004](https://github.com/NuGet/Home/issues/1004)

Finally, we also improved these items:

* Verbosity of log messages reduced for `project.json` managed projects - [1163](https://github.com/NuGet/Home/issues/1163)
* Now properly displaying the installed version of a package in the user interface - [1061](https://github.com/NuGet/Home/issues/1061)


A complete list of issues addressed for the Visual Studio extension can be found in the NuGet GitHub [3.2 milestone](https://github.com/nuget/home/issues?q=is%3Aissue+is%3Aclosed+-label%3AClosedAs%3ADuplicate+milestone%3A3.2)

## Known Issues

We continue to track issues on our GitHub issues list which can be found at: [https://github.com/nuget/home/issues](https://github.com/nuget/home/issues)
