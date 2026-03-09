---
title: Find and Evaluate NuGet Packages
description: Find and evaluate publicly available NuGet packages for your project by using advanced nuget.org search filters and syntax.
author: JonDouglas
ms.author: jodou
ms.date: 03/09/2026
ms.topic: how-to
# customer intent: As a developer, I want to become familiar with NuGet search syntax and filters and the information on NuGet package pages so that I can use packages in my project.
---

# Find and evaluate NuGet packages for your project

When you start a .NET project, or identify a functional need in your app or service, you can often install existing NuGet packages to save the time and trouble of [creating your own packages](../create-packages/overview-and-workflow.md). Existing packages can come from the [nuget.org](https://www.nuget.org/packages) public collection, or from private sources that your organization or another party provides.

## Find packages

You can find packages directly at [https://nuget.org/packages](https://www.nuget.org/packages), or from the [Visual Studio Package Manager UI](install-use-packages-visual-studio.md) or [Package Manager Console](install-use-packages-powershell.md) with nuget.org as a source. All packages from nuget.org are routinely scanned for viruses.

The [nuget.org/packages](https://www.nuget.org/packages) page lists NuGet packages, with the most popular packages across all .NET projects listed first. Some of these packages might be useful for your projects.

:::image type="content" source="media/nuget-package-list.png" alt-text="Screenshot of the nuget.org/packages site, which lists packages and information about updates, downloads, and supported frameworks for each package." lightbox="media/nuget-package-list.png":::

To search for a package, enter the package name or search terms in the search box at the top of the page. You can use [advanced search syntax](#search-syntax) to filter your search.

### Advanced filtering and sorting

At nuget.org/packages, you can refine your search results by using advanced filtering and sorting options.

:::image type="content" source="media/nuget-package-filtering-sorting.png" alt-text="Screenshot of nuget.org/packages with the filter options and sorting list highlighted. Filter options like the framework and package type are visible." lightbox="media/nuget-package-filtering-sorting.png":::

Use the **Frameworks** filters to show packages that target specific .NET frameworks. For more information, see [Target frameworks in SDK-style projects](/dotnet/standard/frameworks).

- If you select one .NET framework generation, the filter limits the search results to packages that are compatible with any of the individual target frameworks within that generation. For example, selecting **.NET** returns packages that are compatible with any of the modern .NET frameworks, including .NET 5.0 through .NET 10.0.

  :::image type="content" source="media/nuget-framework-filter.png" alt-text="Screenshot that shows the framework filters on nuget.org. Under the expanded .NET framework, frameworks from .NET 5.0 to .NET 10.0 are listed.":::

- If you use the arrows to expand a framework generation, the panel lists individual target framework monikers (TFMs) that you can filter your results by. For example, selecting **net5.0** returns packages that are compatible with the .NET 5.0 framework.
- By default, packages are filtered by their expanded list of computed compatible frameworks. If you want to filter packages purely by the asset frameworks they explicitly target, clear the **Include compatible frameworks** checkbox.
- Combining multiple framework filters shows you search results that match all your selected filters. In other words, the page lists packages that fall in the intersection of your selections. For example:
  - Selecting **netcoreapp3.1** and **net45** shows you packages that target **both** .NET Core 3.1 and .NET Framework 4.5.
  - Selecting the **.NET Core** framework generation and **net45** returns packages that target .NET Framework 4.5 and at least one of the .NET Core TFMs (.NET Core 1.0 through .NET Core 3.1).

  Alternatively, you can search for packages that match **any one** of your framework filters. To do so, go to the top of the filter options. Next to **Framework Filter Mode**, select **Any**.
  - Selecting **netcoreapp3.1** and **net5.0** then shows you packages that target **either** .NET Core 3.1 or .NET 5.0.
  - Selecting **netcoreapp3.1** and the **.NET** framework generation returns packages that target .NET Core 3.1 or any of the .NET TFMs (.NET 5.0 through .NET 10.0).
- For more information about how to evaluate a package's supported frameworks and its compatibility with your project, see [Determine supported frameworks](#determine-supported-frameworks), later in this article.

Use the **Package type** filter to list packages of a specific type:

- **All types** is the default value and shows all packages regardless of type.
- **Dependency** filters to regular NuGet packages that you can install in your project.
- **.NET tool** filters to [.NET tools](/dotnet/core/tools/global-tools) packages that contain console applications.
- **Template** filters to [.NET templates](/dotnet/core/install/templates) that you can use to create new projects with the [dotnet new](/dotnet/core/tools/dotnet-new) command.
- **MCP Server** filters to packages that implement [Model Context Protocol (MCP) servers](../concepts/nuget-mcp.md), which you can use to connect AI applications to your data and systems.

By default, NuGet lists all versions of packages, including prerelease and beta versions. In the **Options** section, clear the **Include prerelease** checkbox to list only stable, released package versions.

To apply changes, select **Apply**. To go back to the default values, select **Reset**.

Use the **Sort by** list in the upper-right corner of the page to sort the package list by several criteria:

- **Relevance** is the default value, and sorts results according to an internal scoring algorithm.
- **Downloads** sorts the search results by the total number of downloads, in descending order.
- **Recently updated** sorts the search results by the creation date of the latest package version, in descending chronological order.

### Search syntax

Package search queries that you run at nuget.org, from the NuGet command-line interface (CLI), and from within Visual Studio all use the same syntax. Other package sources, like Azure Artifacts or GitHub Package Repository, might use different syntax or might not support advanced filtering.

- You can search the package `id`, `packageid`, `version`, `title`, `tags`, `author`, `description`, `summary`, or `owner` properties by using the syntax `<property>:<term>`.

- Searches apply to keywords and descriptions, and are case-insensitive. For example, the following strings all search the `id` property for the string `nuget.core`:

  `id:NuGet.Core`<br/>  `ID:nuget.core`<br/>  `Id:NUGET.CORE`

- Searches on the `id` property match substrings, but `packageid` and `owner` use exact, case-insensitive matches. For example:

  `PackageId:jquery` searches for the exact package ID `jquery`.<br/>  `Id:jquery` searches for all package IDs that contain the string `jquery`.

- You can search for multiple values or properties at the same time. For example:

  `id:jquery id:ui` searches for multiple terms in the `id` property.<br/>  `id:jquery tags:validation` searches for multiple properties.

- Searches ignore unsupported properties, so `invalid:jquery ui` is the same as searching for `ui`, and `invalid:jquery` returns all packages.

### Determine supported frameworks

NuGet installs a package into a project only if the package's supported .NET frameworks include the project's target frameworks. If the package isn't compatible, NuGet issues an error.

There are several ways to determine the frameworks that a package supports:

- Check the search page for a package's supported frameworks, which appear as badges below the package ID. These badges show the lowest supported framework versions from the .NET, .NET Core, .NET Standard, and .NET Framework generations. The package is compatible with any framework version that's greater than or equal to the badge version shown.

  Dark blue badges represent explicitly targeted frameworks. Light blue badges represent computed compatible frameworks.

  Selecting a badge redirects you to the package's details page on nuget.org. On that page, the **Frameworks** tab displays the full list of supported frameworks.

  :::image type="content" source="media/nuget-supported-framework-badge.png" alt-text="Screenshot of nuget.org/packages. Under two package names, the rows of framework badges are highlighted, and a tooltip about compatibility is visible." lightbox="media/nuget-supported-framework-badge.png":::

- Check the package's page at nuget.org for supported frameworks. They appear below the package ID and on the **Frameworks** tab, but not all packages show supported frameworks.

  :::image type="content" source="media/supported-frameworks.png" alt-text="Screenshot of a nuget.org package page. Under the name, the framework badges are highlighted. The Frameworks tab lists numerous .NET versions." lightbox="media/supported-frameworks.png":::

- Download the package manually from the package page by going to the **About** section and selecting **Download package**. Change the file extension of the downloaded package from *.nupkg* to *.zip*, open the *.zip* folder, and examine its *lib* folder. There are subfolders for each supported framework, each named with a TFM. For more information, see [Target frameworks](../reference/target-frameworks.md). If there aren't any subfolders under *lib* and there's only a single DLL, try to install the package to discover its compatibility.

- Try to install the package into a project by using [Install-Package](../reference/ps-reference/ps-ref-install-package.md) in the Visual Studio Package Manager Console. If the package is incompatible, the console output shows the package's supported frameworks.

### Prerelease packages

Many package authors provide preview and beta releases as they continue to improve and seek feedback on latest revisions. By default, nuget.org shows prerelease packages in its package list and search results.

To list and search for only stable releases:

- At nuget.org, go to the advanced search panel and clear the **Include prerelease** checkbox.
- In the Visual Studio NuGet Package Manager UI, clear the **Include prerelease** checkbox next to the search box.

The Visual Studio Package Manager Console, NuGet CLI, and dotnet CLI tools don't include prerelease versions by default. To include prerelease versions:

- In the Package Manager Console, use the `-IncludePrerelease` switch with the `Find-Package`, `Get-Package`, `Install-Package`, `Sync-Package`, and `Update-Package` commands. For more information, see [PowerShell reference](../reference/powershell-reference.md).

- For the NuGet CLI, use the `-prerelease` switch with the `install`, `update`, `list`, `search`, and `mirror` commands. For more information, see [NuGet CLI reference](../reference/nuget-exe-cli-reference.md).

- For the dotnet CLI, specify a prerelease version with the `-v` argument. For more information, see [dotnet package add](/dotnet/core/tools/dotnet-package-add).

<a name="native-cpp-packages"></a>
### Native C++ packages

Visual Studio C++ projects can use native C++ NuGet packages. Installing these packages enables the **Manage NuGet Packages** context-menu command, exposes a `native` target framework, and provides Microsoft Build Engine (MSBuild) integration.

To find native packages on nuget.org/packages, search by using `tag:native`. Such packages typically provide *.targets* and *.props* files, which NuGet imports automatically when adding the packages.

## Evaluate packages

The best way to evaluate a package's usefulness is to try it out. You take a dependency on a package when you use it, so you must make sure it's robust and reliable. However, installing a package and directly testing it is time-consuming. You can find out a lot about a package's quality by using the information on the package's page at nuget.org/packages.

- The **Prefix Reserved** check mark means the package owners have applied for and been granted a [reserved package ID prefix](../nuget-org/id-prefix-reservation.md). On the package list, you can find this check mark next to the package ID. On the package page, it's under the ID. To meet the [ID prefix reservation criteria](../nuget-org/id-prefix-reservation.md#id-prefix-reservation-criteria), package owners must clearly identify themselves and their packages.

  :::image type="content" source="media/prefix-reserved.png" alt-text="Screenshot of a package page at nuget.org. Under the package name is a blue check mark labeled Prefix Reserved.":::
  
- The **Downloads** section in the package page's side column shows **Total**, **Current version**, and **Per day average** downloads. Large numbers indicate that the package has proven itself among many developers.

  :::image type="content" source="media/package-downloads.png" alt-text="Screenshot of a package page Downloads section that shows total downloads, downloads of the current version, and per-day average downloads.":::
  
  Next to **Downloads**, select **Full stats** to go to a page that shows package downloads for the past six weeks by version number. Versions that more developers are using are typically better choices.

- The **Used By** tab on the package page shows the top five most popular nuget.org packages and GitHub repositories that depend on this package. Packages and repositories that depend on this package are called *dependents*. Dependent packages and repositories can be considered as endorsing this package, because they choose to trust and depend on it.

  :::image type="content" source="media/package-repository-dependencies.png" alt-text="Screenshot of a package page. The Used By tab lists NuGet packages and GitHub repositories and their download counts. The highest is in the billions." lightbox="media/package-repository-dependencies.png":::
  
  The *latest stable version* of a dependent package must depend on any version of this package. This definition ensures that listed dependent packages are an up-to-date reflection of package authors' decisions to trust and depend on the package. The dependents list doesn't show prerelease dependents, because they're not considered wholehearted endorsements yet. The following examples show which packages can be listed as dependents:

  | Dependent package version | Dependent package listed as a dependent? |
  |-|-|
  | v1.0.0<br>v1.1.0 (latest stable) depends on this package<br>v1.2.0-preview | True, the latest stable version depends on this package |
  | v1.0.0 depends on this package<br>v1.1.0 (latest stable)<br>v1.2.0-preview | False, the latest stable version doesn't depend on this package |
  | v1.0.0 depends on this package<br>v1.1.0 (latest stable)<br>v1.2.0-preview depends on this package | False, the latest stable version doesn't depend on this package |

  The number of stars for a GitHub repository indicates its popularity with GitHub users. For more information about the GitHub star and repository ranking system, see [About stars](https://docs.github.com/enterprise-cloud@latest/get-started/exploring-projects-on-github/saving-repositories-with-stars#about-stars).

  > [!Note]
  > The **Used By** section is automatically generated periodically, without human review, and solely for informational purposes.

- The **Versions** tab on the package page shows the **Versions**, **Downloads**, **Last Updated** dates, and serious vulnerabilities of package versions. The version you install shouldn't have any high-severity vulnerabilities. A well-maintained package has recent updates and a long version history. Neglected packages have few and long-ago updates.

  :::image type="content" source="media/versions-tab.png" alt-text="Screenshot of a package page. The Versions tab lists the name, downloads, and date of each version. Some versions have a warning icon." lightbox="media/versions-tab.png":::

In the side column of the package page, the **About** and **Owners** sections have other informative links:

:::row:::
   :::column span="":::
      :::image type="content" source="media/right-column.png" alt-text="Screenshot of the side column of a package page, with links for the owners, project website, source repository, and license highlighted.":::
   :::column-end:::
   :::column span="2":::
      - Select **Project website**, if available, to find out what support options the author provides. A project with a dedicated site is generally well supported.

      - Select **Source repository** to go to the Git source code repository for the package. Many authors maintain their packages in open-source repositories, so users can directly contribute bug fixes and feature improvements. The package's contribution history is a good indicator of how many developers are actively involved.

      - Select **\<license-type> license** to get information about the package's license. If a package doesn't specify license terms, contact the package owner.

      - Select any of the package owners under **Owners** to get information about other packages they've published. Owners with multiple packages are more likely to continue supporting their work. Next to **Owners**, select **Contact owners** to reach out directly to the package developers.
   :::column-end:::
:::row-end:::  

## Retrieve license information

Some NuGet clients and NuGet feeds can't display licensing information. To maintain backward compatibility in such cases, the license URL points here, to this document.

If selecting the license URL for a package brings you to this page, it implies the package contains a license file. Also, one or both of the following statements is true:

- You're connected to a feed that doesn't know how to interpret and display the license information to the client.
- You're using a client that doesn't know how to interpret and read the license information the feed provides.

To read the information in the license file inside the package:

1. Manually download the package, and unzip its contents to a folder.
1. Open the *.nuspec* file at the root of the folder.
1. Examine the `<license>` tag, such as `<license type="file">license\license.txt</license>`, to identify the license file and subfolder. In this case, the license file is named *license.txt*. It's in a subfolder called *license*.
1. Go to the specified subfolder and open the license file.

For information about the MSBuild equivalent to setting the license in the *.nuspec* file, see [Packing a license expression or a license file](../reference/msbuild-targets.md#packing-a-license-expression-or-a-license-file).

## Related content

- [Ways to install a NuGet package](overview-and-workflow.md#ways-to-install-a-nuget-package)
- [Install and manage packages in Visual Studio using the NuGet Package Manager](install-use-packages-visual-studio.md)
- [Manage packages with the Visual Studio Package Manager Console (PowerShell)](install-use-packages-powershell.md)
- [Install and manage NuGet packages with the dotnet CLI](install-use-packages-dotnet-cli.md)
