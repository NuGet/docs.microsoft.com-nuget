---
# required metadata

title: NuGet 2.7.2 Release Notes | Microsoft Docs
author: karann
ms.author: karann
manager: ghogen
ms.date: 11/11/2016
ms.topic: article
ms.prod: nuget
#ms.service:
ms.technology: nuget
ms.assetid: c775d1d7-de26-476c-bf9e-0cf95986a22f

# optional metadata

#description: release notes 2.7.2
#keywords: release notes 2.7.2
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
# NuGet 2.7.2 Release Notes

[NuGet 2.7.1 Release Notes](../release-notes/nuget-2.7.1.md) | [NuGet 2.8 Release Notes](../release-notes/nuget-2.8.md)

NuGet 2.7.2 was released on November 11, 2013.

## Noteworthy Bug Fixes and Features

### License Text
For quite some time, Microsoft has included the NuGet packages for several popular open-source libraries as a part of the default templates for Web application projects in Visual Studio. jQuery is probably the most well-known example of this type of library. Because of the support agreement associated with components that are delivered along with a product, the package's script file contains different license text than the script file found in the same package on the public nuget.org gallery. This difference in text can prevent package updates from proceeding as a result of the different license text blocks causing the script files to have different content hash values (and therefore to be treated as modified within the project).

To mitigate this issue, NuGet 2.7.2 allows the script author to include the license text block within a specially marked section which looks as follows.

    /************** NUGET: BEGIN LICENSE TEXT **************
     * The following code is licensed under the MIT license
     * Additional license information below is informational
     * only.
     ************** NUGET: END LICENSE TEXT ***************/

When updating packages with content files containing this block, NuGet does not factor the contents of the block into the comparison with the version on the NuGet gallery, and can therefore delete and update the content file as though it matches the original copy.

This block is identified by the text "NUGET: BEGIN LICENSE TEXT" and "NUGET: END LICENSE TEXT" occurring anywhere on the beginning and ending lines.  No other formatting requirements exist, allowing this feature to be used in any type of text file regardless of language.

### Add Binding Redirects for non-Framework Assemblies
For assemblies that are part of the .NET Framework, NuGet skips adding binding redirects into the application's configuration file when updating the package. This fix addresses a regression in NuGet 2.7 whereby binding redirects were not being added for some assemblies, even though those assemblies are not considered a part of the .NET Framework. NuGet 2.7.2 restores the previous NuGet 2.5 and 2.6 behavior and adds the binding redirects.

### Installing portable libraries with Xamarin Tools installed
When Xamarin's development tools are installed on a machine, they modify the supported frameworks configuration data to specify compatibility between existing target framework combinations and Xamarin frameworks. With version 2.7.2, NuGet is now aware of these implicit compatibility rules, and therefore makes it easy for developers targeting Xamarin platforms to install portable libraries that are Xamarin-compatible but not explicitly marked as such in the package metadata itself.

### Machine-wide configuration settings honored
When using hierarchical nuget.config files, the repositoryPath key was not being honored for nuget.config files closest to the solution root. In Visual Studio 2013, NuGet installs a custom nuget.config file at %ProgramData%\NuGet\Config\VisualStudio\12.0\Microsoft.VisualStudio.config in order to add the "Microsoft and .NET" package source. As a result, the work-around for using a custom repositoryPath in a solution was to delete the machine-level nuget.config - which also meant removing the "Microsoft and .NET" package source. NuGet 2.7.2 now honors the precedence rules for repositoryPath when using hierarchical nuget.config files.

## All Changes
For a full list of work items fixed in NuGet 2.7.2, please view the [NuGet Issue Tracker for this release](https://nuget.codeplex.com/workitem/list/advanced?keyword=&status=All&type=All&priority=All&release=NuGet%202.7.2&assignedTo=All&component=All&sortField=LastUpdatedDate&sortDirection=Descending&page=0&reasonClosed=Fixed).
