---
# required metadata

title: NuGet 2.2.1 Release Notes | Microsoft Docs
author: karann
ms.author: karann
manager: ghogen
ms.date: 11/11/2016
ms.topic: article
ms.prod: nuget
#ms.service:
ms.technology: nuget
ms.assetid: 39ceaeb3-2d33-4b1c-b195-eba36c6cbf9a

# optional metadata

#description: release notes 2.2.1
#keywords: release notes 2.2.1
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
# NuGet 2.2.1 Release Notes

[NuGet 2.2 Release Notes](../release-notes/nuget-2.2.md) | [NuGet 2.5 Release Notes](../release-notes/nuget-2.5.md)

NuGet 2.2.1 was released on February 15, 2013.  The VS Extension version number is 2.2.40116.9051.

## Localization Refresh
When NuGet shipped as part of Visual Studio 2012, it was fully localized into English + 13 other languages.  Since then, NuGet 2.1 and 2.2 have shipped but the localization had not been refreshed.  The NuGet 2.2.1 release refreshes our localization.

NuGet's UI and PowerShell Console are localized into the following languages:

1. Chinese (Simplified)
1. Chinese (Traditional)
1. Czech
1. English
1. French
1. German
1. Italian
1. Japanese
1. Korean
1. Polish
1. Portuguese (Brazil)
1. Russian
1. Spanish
1. Turkish

## Visual Studio Templates Support Multiple Preinstalled Package Repositories
If you produce Visual Studio templates, you can use NuGet to [preinstall packages](../visual-studio-extensibility/visual-studio-templates.md) as part of the template.  Until now, this feature had a limitation that all of the packages needed to come from the same source.  With NuGet 2.2.1 though, you can have packages installed from multiple repositories (within the template, a VSIX, or a folder on disk defined in the registry).

The main scenario for this feature is custom ASP.NET project templates.  The built-in ASP.NET templates use preinstalled packages, pulling packages from local disk.  You can now create a custom ASP.NET project template that uses the existing packages installed by ASP.NET but add extra NuGet packages into your template.

## Bug Fixes
NuGet 2.2.1 includes a few targeted bug fixes. For a list of work items fixed in NuGet 2.2.1, please view the [NuGet Issue Tracker for this release](http://nuget.codeplex.com/workitem/list/advanced?keyword=&status=Closed&type=All&priority=All&release=NuGet%202.2.1&assignedTo=All&component=All&sortField=LastUpdatedDate&sortDirection=Descending&page=0).


## Known Issues

If you are extending ASP.NET project templates, all preinstalled package repositories must use the same value for the `isPreunzipped` attribute.
