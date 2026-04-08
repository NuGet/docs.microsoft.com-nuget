---
title: Install and manage packages in Visual Studio using the NuGet Package Manager
description: Learn how to use the NuGet Package Manager UI in Visual Studio for working with NuGet packages.
author: JonDouglas
ms.author: jodou
ms.date: 03/03/2025
ms.topic: install-set-up-deploy
f1_keywords: 
  - "vs.nuget.packagemanager.ui"
---

# Install and manage packages in Visual Studio using the NuGet Package Manager

You can use the NuGet Package Manager UI in Microsoft Visual Studio for Windows to easily install, uninstall, and update NuGet packages in projects and solutions.

## Prerequisites

- Install Visual Studio 2026 for Windows with any .NET-related workload.

  You can install the 2026 Community edition for free from [visualstudio.microsoft.com](https://visualstudio.microsoft.com/), or you can use the Professional or Enterprise edition.

  Visual Studio 2017 and later versions automatically include NuGet Package Manager when a .NET-related workload is installed. To install the NuGet Package Manager individually in Visual Studio Installer, select **Individual components** > **Code tools** > **NuGet package manager**.

  For Visual Studio 2015, if you're missing the NuGet Package Manager, select **Tools** > **Extensions and Updates**, and then search for the **NuGet Package Manager** extension. If you can't use the extensions installer in Visual Studio, download the extension directly from [https://dist.nuget.org/index.html](https://dist.nuget.org/index.html).

- [Register for a free account on nuget.org](../nuget-org/individual-accounts.md#add-a-new-individual-account) if you don't have one already. You must register and confirm the account before you can upload a NuGet package.

## Find and install a package

To find and install a NuGet package by using Visual Studio, follow these steps:

1. Load a project in **Solution Explorer**, and then select **Project** > **Manage NuGet Packages**.

   The **NuGet Package Manager** window opens.

1. Go to the **Browse** tab to display packages by popularity from the currently selected source. For information about sources, see [Package sources](#package-sources).

    - To search for a specific package, use the search box in the upper-left corner of the tab.
    - Abbreviated information might be shown beside each package ID to help you identify the correct package. This information varies based on the selected package sources. Examples include the package download count, the authors, or a link to the owner's profile.

    > [!Note]
    > In Visual Studio 17.11 and later, package owners are shown as profile links when supported by the selected package source.
    > Package ownership is defined by the package source. For more information, see [Manage package owners on nuget.org](../nuget-org/publish-a-package.md#manage-package-owners-on-nugetorg).
    >
    > In Visual Studio 17.10 and earlier, the package `author` metadata is shown, which appears as plain text.
    > For more information, see [Authors package metadata](../create-packages/package-authoring-best-practices.md#authors).

    - Select a package to open its details pane. In the details pane, the **Package Details** tab displays package metadata, information about the owners, authors, and license, and other information. The details pane also provides a way for you to select a version to install.

      :::image type="content" source="media/package-manager-package-details.png" alt-text="Screenshot of the NuGet Package Manager. In the Browse tab, a package is selected. The Package Details tab of its details pane lists information." lightbox="media/package-manager-package-details.png":::

      The **README** tab of the details pane displays the package read-me file if it's provided by the package author.

      :::image type="content" source="media/package-manager-package-readme.png" alt-text="Screenshot of the NuGet Package Manager. In the Browse tab, a package is selected. The README tab of its details pane describes the package." lightbox="media/package-manager-package-readme.png":::

1. In the details pane, next to **Version**, select a version. If you want to include prerelease versions in the **Version** list, go to the top of the **NuGet Package Manager** window. Next to the search box, select **Include prerelease**.

1. To install the NuGet package, select **Install**. You might be prompted to accept license terms or verify the installation.

   Visual Studio installs the package and its dependencies in the project. When installation is complete, the added packages appear on the NuGet Package Manager **Installed** tab. You can also find packages in in **Solution Explorer**, in the **Dependencies** > **Packages** node of your project. After you install a package, you can refer to it in the project with a `using` statement.

1. (Optional) NuGet has two formats in which a project can use packages: [PackageReference](package-references-in-project-files.md) and [packages.config](../reference/packages-config.md). To set the default format, select **Tools** > **Options**, and then expand **NuGet Package Manager**. In the **General** section, under **Package Management**, select a format in the **Default package management format** list. For more information, see [Choose default package management format](package-restore.md#choose-default-package-management-format).

## Uninstall a package

To uninstall a NuGet package, follow these steps:

1. Load a project in **Solution Explorer**, select **Project** > **Manage NuGet Packages**, and then go to the **Installed** tab.

1. In the main pane, select the package to uninstall. If needed, use the search box to find the package. Then in the package details pane, select **Uninstall**.

    ![Screenshot showing the NuGet Package Manager with a package selected and its Uninstall button highlighted.](media/uninstall-package.png)

## Update a package

To update a NuGet package, follow these steps:

1. Load a project in **Solution Explorer**, and then select **Project** > **Manage NuGet Packages**. For website projects, select the **Bin** folder first.

1. Select the **Updates** tab to see packages that have available updates from the selected **Package source**. Select **Include prerelease** to include prerelease packages in the update list.

1. Select the package to update. On the right pane, select the desired **Version** from the dropdown list, and then select **Update**.

    ![Screenshot showing the NuGet Package Manager with a package selected and its Update button highlighted.](media/update-package.png)

1. For some packages, the **Update** button is disabled and the following message appears: *Implicitly referenced by an SDK. To update the package, update the SDK to which it belongs.* This message indicates that the package is part of a larger framework or SDK and can't be updated independently. Such packages are internally marked with `<IsImplicitlyDefined>True</IsImplicitlyDefined>`. For example, `Microsoft.NETCore.App` is part of the .NET Core SDK, and the package version is different than the version of the runtime framework used by the application. To download a new version of the .NET Core, [update your .NET Core installation](https://aka.ms/dotnet-download). For more information, see [.NET Core metapackages and versioning](/dotnet/core/packages). This scenario applies to the following commonly used packages:
   - Microsoft.AspNetCore.All
   - Microsoft.AspNetCore.App
   - Microsoft.NETCore.App
   - NETStandard.Library

    ![Screenshot showing a NuGet package with the Update button disabled.](media/package-update-disabled.png)

1. To update multiple packages to their latest versions, choose them in the NuGet package list, and then select **Update**.

1. You can also update an individual package from the **Installed** tab. For this case, you can also select a **Version** and the **Include prerelease** option.

## Manage packages for the solution

Managing packages for a solution is a convenient means to work with multiple projects simultaneously:

1. Select a solution in **Solution Manager**, and then select **Tools** > **NuGet Package Manager** > **Manage NuGet Packages for Solution**.

1. In the **Manage NuGet Packages for Solution** window, select the projects that are affected by the operations.

    ![Screenshot showing the Manage Packages for Solution window with multiple projects selected.](media/manage-packages-for-solution.png)

### Consolidate tab

Developers typically consider it bad practice to use different versions of the same NuGet package across different projects in the same solution. Visual Studio allows you to use a common version for your NuGet packages. To do so, use the **Consolidate** tab of the **NuGet Package Manager** window to discover where packages with distinct version numbers are used by different projects in the solution.

![Screenshot showing the Manage Packages for Solution window with the Consolidate tab selected.](media/consolidate-tab.png)

In this example, the ClassLibrary1 project is using EntityFramework 6.2.0, whereas ConsoleApp1 is using EntityFramework 6.1.0. To consolidate package versions, follow these steps:

1. From the **Consolidate** tab, select the projects to update in the project list.

1. Select the version to use for all these projects in the **Version** list.

1. Select **Install**.

   The NuGet Package Manager installs the selected package version into all the selected projects, after which the package no longer appears on the **Consolidate** tab.

## Package sources

Visual Studio ignores the order of package sources, and uses the package from whichever source is the first to respond to a request. For more information, see [Restore packages](package-restore.md). For information about how to load a package from a specific source, see [Package source mapping](package-source-mapping.md).

1. To change the source from which Visual Studio loads package metadata, select a source from the **Package source** selector.

   ![Screenshot showing the Package source selector highlighted.](media/package-source-selector.png)

1. To manage your package sources, select the **Settings** icon or select **Tools** > **Options**.

    ![Screenshot showing the Package source settings icon highlighted.](media/package-source-settings.png)

1. To manage NuGet package sources, see [NuGet Options in Visual Studio](nuget-visual-studio-options.md#sources).

## NuGet Package Manager Options control

When you select a package, the NuGet Package Manager displays an expandable **Options** control below the **Version** selector. For most project types, only the **Show preview window** option is provided.

![Screenshot showing the NuGet Package manager Options control expanded.](media/package-manager-options.png)

The following sections explain the available options.

<!-- This is here because the link in the UI needs this anchor. See https://github.com/NuGet/NuGet.Client/blob/dev/src/NuGet.Clients/PackageManagement.UI/Xamls/OptionsControl.xaml -->
<a name="install-options"></a>

### Install and update options

These options are available only for certain project types:

- **Dependency behavior**: This option configures how NuGet decides which versions of dependent packages to install. It has the following settings:

  - **Ignore dependencies** skips the installation of dependencies, which typically breaks the package being installed.
  - **Lowest** [Default] installs the dependency with the minimal version number that meets the requirements of the primary chosen package.
  - **Highest Patch** installs the version with the same major and minor version numbers, but the highest patch number. For example, if version 1.2.2 is specified then the highest version that starts with 1.2 will be installed
  - **Highest Minor** installs the version with the same major version number but the highest minor number and patch number. If version 1.2.2 is specified, then the highest version that starts with 1 will be installed
  - **Highest** installs the highest available version of the package.

- **File conflict action**: This option specifies how NuGet should handle packages that already exist in the project or local machine. It has the following settings:

  - **Prompt** instructs NuGet to ask whether to keep or overwrite existing packages.
  - **Ignore All** instructs NuGet to skip overwriting any existing packages.
  - **Overwrite All** instructs NuGet to overwrite any existing packages.

<!-- This is here because the link in the UI needs this anchor. See https://github.com/NuGet/NuGet.Client/blob/dev/src/NuGet.Clients/PackageManagement.UI/Xamls/OptionsControl.xaml -->
<a name="uninstall-options"></a>

### Uninstall options

These options are available only for certain project types:

- **Remove dependencies**: When selected, removes any dependent packages if they're not referenced elsewhere in the project.

- **Force uninstall even if there are dependencies on it**: When selected, uninstalls a package even if it's still being referenced in the project. This option is typically used in combination with **Remove dependencies** to remove a package and whatever dependencies it installed. Using this option may, however, lead to broken references in the project. In such a case, you might need to [reinstall those other packages](../consume-packages/reinstalling-and-updating-packages.md).

## Related video

- Find NuGet videos on [Channel 9](/shows/nuget-101/) and [YouTube](https://www.youtube.com/playlist?list=PLdo4fOcmZ0oVLvfkFk8O9h6v2Dcdh2bh_).

## See also

For more information about NuGet, see the following articles:

- [What is NuGet?](../what-is-nuget.md)
- [Package consumption workflow](overview-and-workflow.md)
- [Find and choose packages](finding-and-choosing-packages.md)
- [Package references in project files](package-references-in-project-files.md)
- [Install and use a package using the .NET CLI](../quickstart/install-and-use-a-package-using-the-dotnet-cli.md).
