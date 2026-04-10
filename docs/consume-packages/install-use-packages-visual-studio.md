---
title: Install and Manage Packages in Visual Studio Using the NuGet Package Manager
description: Find out how to use the NuGet Package Manager UI in Visual Studio to install, uninstall, and update NuGet packages in projects and solutions.
author: JonDouglas
ms.author: jodou
ms.date: 04/10/2026
ms.topic: install-set-up-deploy
f1_keywords: 
  - "vs.nuget.packagemanager.ui"
# customer intent: As a developer, I want to find out how to use the NuGet Package Manager UI in Visual Studio so that I can install, uninstall, and update NuGet packages in projects and solutions.
---

# Install and manage packages in Visual Studio using the NuGet Package Manager

You can use the NuGet Package Manager UI in Microsoft Visual Studio to easily install, uninstall, and update NuGet packages in projects and solutions.

## Prerequisites

- Visual Studio 2026 with any .NET-related workload. You can install the 2026 Community edition for free from [visualstudio.microsoft.com](https://visualstudio.microsoft.com/), or you can use the Professional or Enterprise edition.

- The NuGet Package Manager. Visual Studio 2017 and later versions automatically include the NuGet Package Manager when a .NET-related workload is installed. To install the NuGet Package Manager manually in Visual Studio Installer, select **Individual components** > **Code tools** > **NuGet package manager**.

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

      :::image type="content" source="media/package-manager-package-details.png" alt-text="Screenshot of the NuGet Package Manager. In the Browse tab, a package is selected. The Package Details tab of its details pane lists package data." lightbox="media/package-manager-package-details.png":::

      The **README** tab of the details pane displays the package read-me file if it's provided by the package author.

      :::image type="content" source="media/package-manager-package-readme.png" alt-text="Screenshot of the NuGet Package Manager. In the Browse tab, a package is selected. The README tab of its details pane describes the package." lightbox="media/package-manager-package-readme.png":::

1. In the details pane, next to **Version**, select a version. If you want to include prerelease versions in the **Version** list, go to the top of the **NuGet Package Manager** window. Next to the search box, select **Include prerelease**.

1. To install the NuGet package, select **Install**. You might be prompted to accept license terms or verify the installation.

   Visual Studio installs the package and its dependencies in the project. When installation is complete, the added packages appear on the NuGet Package Manager **Installed** tab. You can also find packages in **Solution Explorer**, in the **Dependencies** > **Packages** node of your project. After you install a package, you can refer to it in the project with a `using` statement.

## Set the package management format

NuGet has two formats in which a project can use packages:

- [PackageReference](Package-References-in-Project-Files.md)
- [packages.config](../reference/packages-config.md)

To set the default format, follow these steps:

1. In Visual Studio, select **Tools** > **Options**.
1. In the side pane, select **NuGet Package Manager**, and then select **General**.
1. In the main window, under **Package Management**, select a format in the **Default package management format** list.

For more information, see [Choose the default package management format](Package-Restore.md#choose-the-default-package-management-format).

## Uninstall a package

To uninstall a NuGet package, follow these steps:

1. Load a project in **Solution Explorer**, select **Project** > **Manage NuGet Packages**, and then go to the **Installed** tab.

1. In the main pane, select the package to uninstall. If needed, use the search box to find the package. Then in the package details pane, select **Uninstall**.

    :::image type="content" source="media/uninstall-package.png" alt-text="Screenshot of the NuGet Package Manager. In the main pane, a package is selected. In its details pane, the Uninstall button is highlighted." lightbox="media/uninstall-package.png":::

## Update a package

To update a NuGet package, follow these steps:

1. Load a project in **Solution Explorer**, and then select **Project** > **Manage NuGet Packages**. For legacy ASP.NET Web Site projects, which manage dependencies through the *bin* folder, go to **Solution Explorer** and select the *bin* folder before you open the NuGet Package Manager UI.

1. Select the **Updates** tab to list packages that have available updates from the source that's selected next to **Package source**. To include prerelease packages in the update list, go to the top of the **NuGet Package Manager** window. Next to the search box, select **Include prerelease**.

1. Select the package to update. In the details pane, next to **Version**, select the desired version, and then select **Update**.

    :::image type="content" source="media/update-package.png" alt-text="Screenshot of the NuGet Package Manager. In the main pane, a package is selected. In its details pane, the version and Update button are highlighted." lightbox="media/update-package.png":::

### Update an implicitly referenced package

For some packages, the **Update** button isn't available, and the following message appears: "Implicitly referenced by an SDK. To update the package, update the SDK to which it belongs."

This message indicates that the package is part of a larger framework or SDK and can't be updated independently. For example, `Microsoft.NETFramework.ReferenceAssemblies` is automatically added when an SDK‑style project targets .NET Framework.

:::image type="content" source="media/package-update-disabled.png" alt-text="Screenshot of a package details pane in the NuGet Package Manager. The Update button appears dimmed. A message about implicit reference is visible." lightbox="media/package-update-disabled.png":::

Such packages are internally marked with `<IsImplicitlyDefined>True</IsImplicitlyDefined>`. These packages are versioned with the SDK or runtime and must be updated by installing a newer .NET SDK, not by using NuGet Package Manager.

To download a new version of a framework, see [Download .NET](https://dotnet.microsoft.com/download/dotnet). For more information, see [.NET application publishing overview](/dotnet/core/deploying).

### Update multiple packages

To update multiple packages to their latest versions, select them in the NuGet package list, and then select **Update**.

### Update from the Installed tab

You can also update an individual package from the **Installed** tab. For this case, you can also select a version and the **Include prerelease** option.

## Manage packages for the solution

Managing packages for a solution is a convenient means to work with multiple projects simultaneously.

1. Select a solution in **Solution Manager**, and then select **Tools** > **NuGet Package Manager** > **Manage NuGet Packages for Solution**.

1. In the **Manage Packages for Solution** window, select the projects to apply an operation to.

    :::image type="content" source="media/manage-packages-for-solution.png" alt-text="Screenshot of the Manage Packages for Solution window. In the package details pane, all projects are selected, and the Install button is available." lightbox="media/manage-packages-for-solution.png":::

### Consolidate tab

Developers typically consider it bad practice to use different versions of the same NuGet package across different projects in the same solution. You can use the **Manage Packages for Solution** window to use a common version for your NuGet packages. To do so, go to the **Consolidate** tab to discover where packages with distinct version numbers are used by different projects in the solution.

:::image type="content" source="media/consolidate-tab.png" alt-text="Screenshot of the Manage Packages for Solution window. The package details pane lists two projects that use different versions of the package." lightbox="media/consolidate-tab.png":::

In this example, the MyClassLibrary project uses `EntityFramework` 6.5.1, but MyConsoleApp uses `EntityFramework` 6.5.0. To consolidate package versions, follow these steps:

1. On the **Consolidate** tab, select the projects to update in the project list.

1. Next to **Version**, select the version to use for all the selected projects.

1. Select **Install**.

   The NuGet Package Manager installs the selected package version in all the selected projects, and the package no longer appears on the **Consolidate** tab.

## Package sources

Visual Studio ignores the order of package sources. Instead, it uses the package from the source that responds first to a request. For more information, see [Restore packages](Package-Restore.md). For information about how to load a package from a specific source, see [Package source mapping](Package-Source-Mapping.md).

1. To change the source from which Visual Studio loads package metadata, go to the top of the **NuGet Package Manager** window or the **Manage Packages for Solution** window. Next to **Package source**, select the source that you want to use.

    :::image type="content" source="media/package-source-selector.png" alt-text="Screenshot of the upper-right corner of the Manage Packages for Solution window. The Package source list is highlighted, and nuget.org is selected.":::

1. To manage your package sources, select the **Settings** icon, or select **Tools** > **Options**.

    :::image type="content" source="media/package-source-settings.png" alt-text="Screenshot of the upper-right corner of the Manage Packages for Solution window. Next to the Package source list, the settings icon is highlighted.":::

1. To manage NuGet package sources, see [NuGet Package Manager options in Visual Studio](nuget-visual-studio-options.md#sources).

## NuGet Package Manager Options control

When you select a package, the NuGet Package Manager displays an expandable **Options** control in the details pane, below the **Version** list. For most project types, only the **Show preview window** checkbox is provided. But for some project types, other options are also available.

:::image type="content" source="media/package-manager-options.png" alt-text="Screenshot of the Options control in a package details pane in the NuGet Package Manager, with options for installing, updating, and uninstalling." lightbox="media/package-manager-options.png":::

The following sections explain the available options.

<!-- This is here because the link in the UI needs this anchor. See https://github.com/NuGet/NuGet.Client/blob/dev/src/NuGet.Clients/PackageManagement.UI/Xamls/OptionsControl.xaml -->
<a name="install-options"></a>

### Install and update options

These options are available only for certain project types:

- **Dependency behavior**: This option specifies the versions of dependent packages that NuGet installs. It has the following settings:

  - **Ignore Dependencies** skips the installation of dependencies, which typically breaks the package being installed.
  - **Lowest** installs the dependency with the minimum version number that meets the requirements of the primary chosen package. This setting is the default one.
  - **Highest Patch** installs the version with the same major and minor version numbers as the selected version, but the highest patch number. For example, if version 1.2.2 is specified, the highest version that starts with 1.2 is installed.
  - **Highest Minor** installs the version with the same major version number as the selected version, but the highest minor number and patch number. If version 1.2.2 is specified, the highest version that starts with 1 is installed.
  - **Highest** installs the highest available version of the package.

- **File conflict action**: This option specifies how NuGet handles packages that already exist in the project or local machine. It has the following settings:

  - **Prompt** instructs NuGet to ask whether to keep or overwrite existing packages.
  - **Ignore All** instructs NuGet to skip overwriting any existing packages.
  - **Overwrite All** instructs NuGet to overwrite any existing packages.

<!-- This is here because the link in the UI needs this anchor. See https://github.com/NuGet/NuGet.Client/blob/dev/src/NuGet.Clients/PackageManagement.UI/Xamls/OptionsControl.xaml -->
<a name="uninstall-options"></a>

### Uninstall options

These options are available only for certain project types:

- **Remove dependencies**: When selected, this option removes any dependent packages if they're not referenced elsewhere in the project.

- **Force uninstall, even if there are dependencies on it**: When selected, this option uninstalls a package even if it's still referenced in the project. This option is typically used in combination with **Remove dependencies** to remove a package and the dependencies it installed. But using this option can lead to broken references in the project. In such a case, you might need to [reinstall those other packages](Reinstalling-and-Updating-Packages.md).

## Related videos

- For videos about using NuGet for package management, see [Channel 9](/shows/dotnet-package-management-with-nuget-for-beginners/) and [YouTube](https://www.youtube.com/playlist?list=PLdo4fOcmZ0oVLvfkFk8O9h6v2Dcdh2bh_).

## Related content

For more information about NuGet, see the following articles:

- [An introduction to NuGet](../what-is-nuget.md)
- [Package consumption workflow](Overview-and-Workflow.md)
- [Find and evaluate NuGet packages for your project](Finding-and-Choosing-Packages.md)
- [`PackageReference` in project files](Package-References-in-Project-Files.md)
- [Quickstart: Install and use a package with the dotnet CLI](../quickstart/install-and-use-a-package-using-the-dotnet-cli.md)
