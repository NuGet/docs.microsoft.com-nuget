---
# required metadata

title: NuGet 3.3 Release Notes | Microsoft Docs
author: karann
ms.author: karann
manager: ghogen
ms.date: 11/11/2016
ms.topic: article
ms.prod: nuget
#ms.service:
ms.technology: nuget
ms.assetid: 4110a36a-cffe-4038-8da4-e841bce6e94b

# optional metadata

#description: release notes 3.3
#keywords: release notes 3.3
#ROBOTS:
#audience:
#ms.devlang:
ms.reviewer:
- karann
- unnir
#ms.suite:
#ms.tgt_pltfrm:
#ms.custom:

---

# NuGet 3.3 Release Notes

[NuGet 3.2.1 Release Notes](../release-notes/nuget-3.2.1.md) | [NuGet 3.4-RC Release Notes](../release-notes/nuget-3.4-RC.md)

NuGet 3.3 was released November 30, 2015 with a significant number of user interface updates and command-line features as well as a collection of useful fixes to the NuGet clients.

## New Features

* Credential Providers have been introduced that allow NuGet command-line clients to be able to work seamlessly with an authenticated feed. [Instructions on how to install the Visual Studio Team Services credential provider ](../API/NuGet.exe-Credential-Providers.md) and configure the NuGet clients to use it are available on NuGet Docs.

## New User Interface Features

* Separate Browse, Installed, and Updates Available tabs
* Updates Available badge indicating the number of packages with available updates
* Package badges in the package list to indicate if the package is installed or has an update available
* Download count and author added to the package list
* Highest available version number and currently installed version number on the package list
* Action buttons to allow quick install, update, and uninstall from the package list
* Clearer action buttons on the package detail panel
* Package update date on the package detail panel
* Consolidate panel in Solution view
* Sortable grid of projects and installed version numbers on the solution view

## New Command-line Features

In this version we introduced the `add` and `init` commands to initialize folder-based repositories as described in the [nuget.exe reference](../tools/nuget.exe-cli-reference.md). Repositories that are constructed and maintained with this folder structure will [deliver significant performance benefits](http://blog.nuget.org/20150922/Accelerate-Package-Source.html) as outlined on our blog.

## ContentFiles

Content is now supported in project.json managed projects through the new `contentFiles` folder and nuspec `contentFiles` element notation.  This content can be more directly specified by the package author for interactions with project systems.  More information about how to configure contentFiles in a NuSpec document can be found in the [NuSpec Reference](../schema/nuspec.md).

## NuGet Locals Cache Management

The NuGet command-line has been updated to include information about how to manage the local caches on a workstation.  More information about the locals command is available in the [NuGet command-line reference](../tools/nuget.exe-cli-reference.md#locals).

## Fixed Issues

**Notable Issues**

* NuGet command-line restored support for restoring packages with a solution file on Mono - [1543](https://github.com/NuGet/Home/issues/1543)

The complete list of issues that were addressed in the 3.3 release can be found on GitHub under the [3.3 milestone](https://github.com/NuGet/Home/issues?q=is%3Aissue+milestone%3A3.3.0+is%3Aclosed).

The list of issues fixed in the 3.3 command-line release are recorded in the [3.3 Command-Line milestone](https://github.com/NuGet/Home/issues?q=is%3Aissue+is%3Aclosed+milestone%3A3.3.0-commandline).

## Known Issues

We continue to track issues on our GitHub issues list which can be found at: [http://github.com/nuget/home/issues](http://github.com/nuget/home/issues)