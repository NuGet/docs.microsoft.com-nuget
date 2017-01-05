--- 
# required metadata 
 
title: NuGet 2.2 Release Notes | Microsoft Docs 
author: harikmenon
ms.author: harikm 
manager: ghogen 
ms.date: 11/11/2016 
ms.topic: article 
ms.prod: nuget 
#ms.service: 
ms.technology: nuget 
ms.assetid: 25389d8c-e7db-4920-ab5e-cff20cebee7e 
 
# optional metadata 
 
#description: release notes 2.2
#keywords: release notes 2.2
#ROBOTS: 
#audience: 
#ms.devlang: 
ms.reviewer:  
- karann 
- harikm 
#ms.suite:  
#ms.tgt_pltfrm: 
#ms.custom: 
 
--- 
# NuGet 2.2 Release Notes

[NuGet 2.1 Release Notes](../release-notes/nuget-2.1.md) | [NuGet 2.2.1 Release Notes](../release-notes/nuget-2.2.1.md)

NuGet 2.2 was released on December 12, 2012.

## Visual Studio Quick Launch
One of the new features that was added in Visual Studio 2012 was the [quick launch dialog](http://msdn.microsoft.com/library/hh417697.aspx). NuGet 2.2 extends this dialog, allowing it to initialize the package manager dialog with the search terms entered in the quick launch. For example, entering 'jquery' in quick launch now includes an option in the results to search NuGet packages matching 'jquery'.

![NuGet in Visual Studio Quick Launch](./media/quick-launch.png)

Selecting this option will launch the standard NuGet package manager search experience for the term 'jquery'.

![Pre-populated NuGet Package Manager Dialog](./media/pkg-mgr-search-from-quick-launch.png)

## Specify Entire Folder for Package Contents
NuGet 2.2 now allows you to specify an entire directory in the `<file>` element of the .nuspec file to include all of the contents of that directory. For example, the following will cause all scripts in the package's scripts folder to be added to the contents\scripts folder when the package is installed into a project.

**`<file src="scripts\" target="content\scripts"/>`**

**Update 6/24/16: Empty folders in the "content" folder are ignored when installing the package.**

## Known Issues

### Package installation fails for F# projects when using the package manager console
When attempting to install a NuGet package into an F# project using the package manager console, an InvalidOperationException is thrown. We are actively working with the F# team to resolve the issue, but in the meantime, the workaround is to install NuGet packages into F# projects via NuGet's package manager dialog rather than the console. [More information is available on CodePlex](http://nuget.codeplex.com/workitem/2873).


## Bug Fixes
NuGet 2.2 includes many bug fixes. For a full list of work items fixed in NuGet 2.2, please view the [NuGet Issue Tracker for this release](http://nuget.codeplex.com/workitem/list/advanced?keyword=&status=Closed&type=All&priority=All&release=NuGet%202.2&assignedTo=All&component=All&sortField=LastUpdatedDate&sortDirection=Descending&page=0).
