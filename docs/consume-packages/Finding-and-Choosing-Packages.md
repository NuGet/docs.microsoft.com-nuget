---
title: Finding and Choosing NuGet Packages
description: An overview of how to find and choose the best NuGet packages for a project including details on the NuGet search syntax.
author: karann-msft
ms.author: karann
ms.date: 06/04/2018
ms.topic: conceptual
---

# Finding and evaluating NuGet packages for your project

When starting any .NET project, or whenever you identify a functional need for your app or service, you can save yourself lots of time and trouble by using existing NuGet packages that fulfill that need. These packages can come from the public collection on [nuget.org](http://www.nuget.org/packages/), or a private source that's provided by your organization or another third party.

## Finding packages

When you visit nuget.org or open the Package Manager UI in Visual Studio, you see a list of packages sorted by total downloads. This immediately shows you the most widely-used packages across the millions of .NET projects. There's a good chance, then, that at least some of the packages listed on the first few pages will be useful in your projects.

![Default view of nuget.org/packages showing the most popular packages](media/Finding-01-Popularity.png)

Notice the **Include prerelease** option on the upper right of the page. When selected, nuget.org shows all versions of packages including beta and other early releases. To show only stable released, clear the option.

For specific needs, searching by tags (within the Visual Studio Package Manager or on a portal like nuget.org) is the most common means of discovering a suitable package. For example, searching on "json" lists all NuGet packages that are tagged with that keyword and thus have some relationship to the JSON data format.

![Search results for 'json' on nuget.org](media/Finding-02-SearchResults.png)

You can also search using the package ID, if you know it. See [Search Syntax](#search-syntax) below.

At this time, search results are sorted only by relevance, so you generally want to look through at least the first few pages of results for packages that suit your needs, or refine your search terms to be more specific.

### Does the package support my project's target framework?

NuGet installs a package into a project only if that package's supported frameworks include the project's target framework. If the package is not compatible, NuGet issues an error.

Some packages list their supported frameworks directly in the nuget.org gallery, but because such data is not required, many packages do not include that list. At present there is no means to search nuget.org for packages that support a specific target framework (the feature is under consideration, see [NuGet Issue 2936](https://github.com/NuGet/NuGetGallery/issues/2936)).

Fortunately, you can determine supported frameworks through two other means:

1. Attempt to install a package into a project using the [`Install-Package`](../tools/ps-ref-install-package.md) command in the NuGet Package Manager Console. If the package is incompatible, this command shows you the package's supported frameworks.

1. Download the package from its page on nuget.org using the **Manual download** link under **Info**. Change the extension from `.nupkg` to `.zip`, and open the file to examine the content of its `lib` folder. There you see subfolders for each of the supported frameworks, where each subfolder is named with a target framework moniker (TFM; see [Target Frameworks](../reference/target-frameworks.md)). If you see no subfolders under `lib` and only a single DLL, then you must attempt to install the package in your project to discover its compatibility.

## Pre-release packages

Many package authors make preview and beta releases available as they continue to make improvements and seek feedback on their latest revisions.

By default, nuget.org shows pre-release packages in search results. To search only stable releases, clear the **Include prerelease** option on the upper right of the page

![Include prerelease checkbox on nuget.org](media/Finding-06-include-prerelease.png)

In Visual Studio, and when using the NuGet and dotnet CLI tools, NuGet does not include pre-release versions by default. To change this behavior, do the following steps:

- **Package Manager UI in Visual Studio**: In the **Manage NuGet Packages** UI, set the **Include prerelease** box. Setting or clearing this box refreshes the Package Manager UI and the list of available versions you can install.

    ![The Include prerelease checkbox in Visual Studio](media/Prerelease_02-CheckPrerelease.png)

- **Package Manager Console**: Use the `-IncludePrerelease` switch with the `Find-Package`, `Get-Package`, `Install-Package`, `Sync-Package`, and `Update-Package` commands. Refer to the [PowerShell Reference](../tools/powershell-reference.md).

- **nuget.exe CLI**: Use the `-prerelease` switch with the `install`, `update`, `delete`, and `mirror` commands. Refer to the [NuGet CLI reference](../tools/nuget-exe-cli-reference.md)

- **dotnet.exe CLI**: Specify the exact pre-release version using the `-v` argument. Refer to the [dotnet add package reference](/dotnet/core/tools/dotnet-add-package).

<a name="native-cpp-packages"></a>

### Native C++ packages

NuGet supports native C++ packages can that can be used in C++ projects in Visual Studio. This enables the **Manage NuGet Packages** context-menu command for projects, introduces a `native` target framework, and provides MSBuild integration.

To find native packages on [nuget.org](https://www.nuget.org/packages), search using `tag:native`. Such packages typically provide `.targets` and `.props` files, which NuGet imports automatically when the package is added to a project.

## Evaluating packages

The best way to evaluate the usefulness of a package is to download it and try it out in your code (all packages on nuget.org are routinely scanned for viruses, by the way). After all, every highly popular package got started with only a few developers using it, and you might be one of the early adopters!

At the same time, using a NuGet package means taking a dependency on it, so you want to make sure it's robust and reliable. Because installing and directly testing a package is time-consuming, you can also learn a lot about a package's quality by using the information on a package's listing page:

- *Downloads statistics*: on the package page on nuget.org, the **Statistics** section shows total downloads, downloads of the most recent version, and average downloads per day. Larger numbers indicate that many other developers have taken a dependency on the package, which means that it has proven itself.

    ![Download statistics on a package's listing page](media/Finding-03-Downloads.png)

- *Version history*: on the package page, look under **Info** for the date of the most recent update and examine the **Version History**. A well-maintained package has recent updates and a rich version history. Neglected packages have few updates and often haven't been updated in some time.

    ![Version history on a package's listing page](media/Finding-04-VersionHistory.png)

- *Recent installs*: on the package page under **Statistics**, select **View full stats**. The full stats page shows the package installs over the last six weeks by version number. A package that other developers are actively using is typically a better choice than one that's not.

- *Support*: on the package page under **Info**, select **Project Site** (if available) to see what support options the author provides. A project with a dedicated site is generally better supported.

- *Developer history*: on the package page under **Owners**, select an owner to see what other packages they've published. Those with multiple packages are more likely to continue supporting their work in the future.

- *Open source contributions*: many packages are maintained in open-source repositories, making it possible for developers depending on them to directly contribute bug fixes and feature improvements. The contribution history of any given package is also a good indicator of how many developers are actively involved.

- *Interview the owners*: new developers can certainly be equally committed to producing great packages for you to use, and it's good to give them a chance to bring something new to the NuGet ecosystem. With this in mind, reach out directly to the package developers through the **Contact Owners** option under **Info** on the listing page. Chances are, they'll be happy to work with you to serve your needs!

- *Reserved Package ID Prefixes*: many package owners have applied for and have been granted a [reserved package ID prefix](../reference/id-prefix-reservation.md). When you see the visual checkmark next to a package ID on [nuget.org](https://www.nuget.org/), or in Visual Studio, that means that the package owner has met our [criteria](../reference/id-prefix-reservation.md#id-prefix-reservation-criteria) for ID prefix reservation. This means the package owner is being clear on identifying themselves and their package.

> [!Note]
> Always be mindful of a package's license terms, which you can see by selecting **License Info** on a package's listing page on nuget.org. If a package does not specify license terms, contact the package owner directly using the **Contact owners** link on the package page. Microsoft does not license any intellectual property to you from third party package providers and is not responsible for information provided by third parties.

## License URL deprecation
As we transition from [licenseUrl](../reference/nuspec#licenseurl) to [license](../reference/nuspec#license), some NuGet clients and NuGet feeds may not yet have the ability to surface licensing information in some cases. To maintain backward compatibility, the license URL points to this document which talks about how to retrieve the license information in such cases.

If clicking on the license URL for a package brought you to this page, it implies the package contains a license file and
* You are connected to a feed that does not yet know how to interpret and surface the new license information to the client
**OR**
* You are using a client that does not yet know how to interpret and read the new license information that is potentially provided by the feed
**OR**
* A combination of both

Here is how you could read the information contained in the license file inside the package:
1. Download the NuGet package, and unzip its contents to a folder.
1. Open the `.nuspec` file which would be at the root of that folder.
1. It should have a tag like `<license type="file">license\license.txt</license>`. This implies the license file is named `license.txt` and it is inside a folder called `license` which would also be at the root of that folder.
1. Navigate to the `license` folder and open the `license.txt` file.


## Search Syntax

NuGet package search works the same on nuget.org, from the NuGet CLI, and within the NuGet Package Manager extension in Visual Studio. In general, search is applied to keywords as well as package descriptions.

- **Keywords**: Search looks for relevant packages that contain any of the provided keywords. Example: `modern UI`. To search for packages that contain all of the provided keywords, use "+" between the terms, such as `modern+UI`.
- **Phrases**: Entering terms within quotation marks looks for exact case-insensitive matches to those terms. Example: `"modern UI" package`
- **Filtering**: You can apply a search term to a specific property by using the syntax `<property>:<term>` where `<property>` (case-insensitive) can be `id`, `packageid`, `version`, `title`, `tags`, `author`, `description`, `summary`, and `owner`. Terms can be contained in quotes if needed, and you can search for multiple properties at the same time. Also, searches on the `id` property are substring matches, whereas `packageid` uses an exact match. Examples:

    ```
    id:NuGet.Core                # Match any part of the id property
    Id:"Nuget.Core"
    ID:jQuery
    title:jquery                 # Searches title as shown on the package listing
    PackageId:jquery             # Match the package id exactly
    id:jquery id:ui              # Search for multiple terms in the id
    id:jquery tags:validation    # Search multiple properties
    id:"jquery.ui"               # Phrase search
    invalid:jquery ui            # Unsupported properties are ignored, so this
                                 # is the same as searching on jquery ui
    ```
