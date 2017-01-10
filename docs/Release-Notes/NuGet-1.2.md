---
# required metadata

title: NuGet 1.2 Release Notes | Microsoft Docs
author: karann-msft
ms.author: karann
manager: ghogen
ms.date: 11/11/2016
ms.topic: article
ms.prod: nuget
#ms.service:
ms.technology: nuget
ms.assetid: 48f23141-b2ad-4cdf-8d81-7bb6b9419aa6

# optional metadata

#description: release notes 1.2
#keywords: release notes 1.2
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


# NuGet 1.2 Release Notes

[NuGet 1.0 and 1.1 Release Notes](../release-notes/nuget-1.1.md) | [NuGet 1.3 Release Notes](../release-notes/nuget-1.3.md)

NuGet 1.2 was released on March 30, 2011.

## New Features

### Framework Profile Support

From the start, NuGet supported having libraries target different frameworks. But now packages may contain assemblies
that target specific profiles such as the Windows Phone profile. To target a specific profile of a framework, append
a dash followed by the profile abbreviation. For example, to target SilverLight running on a Windows Phone (aka Windows
Phone 7), you can put an assembly in the sl3-wp folder as demonstrated in the following screenshot.

![Framework Profile Folder Layout](./media/framework-profile-support.png)

You might ask why we didn’t just choose to use “wp7” as the moniker. In part, we’re anticipating that Windows Phone 7
might run a newer version of Silverlight in the future, in which case you may need to be more specific about which
framework profile you’re targetting.

### Automatically Add Binding Redirects

When installing a package with strong named assemblies, NuGet can now detect cases where the project requires binding
redirects to be added to the configuration file in order for the project to compile and add them automatically. Part
3 of David Ebbo’s blog post series on NuGet Versioning entitled “[Unification via Binding Redirects](http://blog.davidebbo.com/2011/01/nuget-versioning-part-3-unification-via.html)”
covers the purpose of this feature in more details.

<a name="framework-assembly-refs"></a>

### Specifying Framework Assembly References (GAC)

In some cases, a package may depend on an assembly that’s in the .NET Framework. Strictly speaking, it’s not always
necessary that the consumer of your package reference the framework assembly. But in some cases, it is important,
such as when the developer needs to code against types in that assembly in order to use your package. The new
`frameworkAssemblies` element, a child element of the metadata element, allows you to specify a set of
`frameworkAssembly` elements pointing to a Framework assembly in the GAC. Note the emphasis on Framework assembly.
These assemblies are not included in your package as they are assumed to be on every machine  as part of the .NET
Framework. The following table lists attributes of the `frameworkAssembly` element.


|Attribute |Description|
|----------------|-----------|
|**assemblyName**|*Required*. Name of the assembly such as `System.Net`.|
|**targetFramework**|*Optional*. Allows specifying a framework and profile name (or alias) that this framework assembly applies to such as "net40" or "sl4". Uses the same format described in [Supporting Multiple Target Frameworks](../create-packages/supporting-multiple-target-frameworks.md).|

    ```xml
    <frameworkAssemblies>
      <frameworkAssembly assemblyName="System.ComponentModel.DataAnnotations" targetFramework="net40" />
      <frameworkAssembly assemblyName="System.ServiceModel" targetFramework="net40" />
    </frameworkAssemblies>
    ```

### NuGet.exe now is able to store API Key credentials

When using the NuGet.exe command line tool, you can now use the SetApiKey command to store your API key. That way,
you won’t need to specify it every time you push a package. For more details on saving your API key with NuGet.exe,
[read the documentation on publishing a package](../create-packages/publish-a-package.md).

### Package Explorer
Package Explorer has been updated to support NuGet 1.2. For more information, check out the
[Package Explorer release notes](http://nuget.codeplex.com/wikipage?title=New%20features%20in%20NuGet%20Package%20Explorer%201.0).

## Other features/fixes

The previous list were the most noticeable of the many features we implemented and bugs we fixed. All in all, we
implemented/fixed [59 work items](http://nuget.codeplex.com/workitem/list/advanced?keyword=&status=All&type=All&priority=All&release=NuGet%201.2&assignedTo=All&component=All&sortField=Votes&sortDirection=Descending&page=0)
in this release.

## Known Issues

* **1.2 Package incompatibility**: Packages built with the latest version of the command line tool, NuGet.exe (> 1.2)
will not work with older versions of the NuGet VS Add-in (such as 1.1). If you run into an error message stating
something about incompatible schema, you are running into this error. Please update NuGet to the latest version.
* **NuGet.Server incompatibility**: If you’re hosting an internal NuGet feed using the NuGet.Server project, you’ll
need to update that project with the latest version of NuGet.Server.
* **Signature Mismatch Error**: If you run into an error during an upgrade with a message about a Signature Mismatch,
you'll need to uninstall NuGet first and then install it. This is listed in our [Known Issues page](../release-notes/Known-Issues.md)
which provides more details. The issue only affects those running Visual Studio 2010 SP1 and have a version of NuGet
1.0 installed that was incorrectly signed. This version was only made available from the CodePlex website for a brief
period so this issue shouldn't affect too many people.