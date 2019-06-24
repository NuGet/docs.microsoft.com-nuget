---
title: NuGet Package Restore
description: An overview of how NuGet restores packages a project depends on, including how to disable restore and constrain versions.
author: karann-msft
ms.author: karann
ms.date: 06/24/2019
ms.topic: conceptual
---

# Package Restore

To promote a cleaner development environment and to reduce repository size, NuGet **Package Restore** installs all of a project's dependencies as listed in either the project file or `packages.config`. Visual Studio can restore packages automatically when a project is built. The `dotnet build` and `dotnet run` commands (.NET Core 2.0+) also perform an automatic restore. You can also restore packages at any time through Visual Studio, `nuget restore`, `dotnet restore`, and xbuild on Mono.

Package Restore makes sure that all a project's dependencies are available without storing those packages in source control. See [Packages and source control](../consume-packages/packages-and-source-control.md) on how to configure your repository to exclude package binaries.

## Package Restore overview

Package Restore first installs the direct dependencies of a project as needed, then installs any dependencies of those packages throughout the entire dependency graph.

If a package is not already installed, NuGet first attempts to retrieve it from the [cache](../consume-packages/managing-the-global-packages-and-cache-folders.md). If the package is not in the cache, NuGet then attempts to download the package from all enabled sources (see [Configure NuGet behavior](Configuring-NuGet-Behavior.md); sources also appear in the  **Tools** > **Options** > **NuGet Package Manager** > **Package Sources** list in Visual Studio). During restore, NuGet ignores the order of package sources, using the package from whichever source is first to respond to requests.

> [!Note]
> NuGet does not indicate a failure to restore a package until all the sources have been checked. At that time, NuGet reports a failure for only the last source in the list. The error implies that the package wasn't present on *any* of the other sources, even though errors aren't shown for each of those sources individually.

The following methods can trigger Package Restore:

- **dotnet CLI**: Use the [dotnet restore](/dotnet/core/tools/dotnet-restore?tabs=netcore2x) command, which restores packages listed in the project file (see [PackageReference](../consume-packages/package-references-in-project-files.md)). With .NET Core 2.0 and later, restore is done automatically with `dotnet build` and `dotnet run`.

- **Package Manager in Visual Studio for Windows**: Packages Restore happens automatically when you create a project from a template or build a project, subject to the options in [Enable and disable package restore](#enable-and-disable-package-restore)). In NuGet 4.0+, restore also happens automatically when you make changes to a .NET Core SDK-based project.

    To restore manually, right-click the solution in **Solution Explorer** and select **Restore NuGet Packages**. If one or more individual packages still aren't installed properly (meaning that **Solution Explorer** shows an error icon), use the **Package Manager** user interface to uninstall and reinstall the affected packages. For more information, see [Reinstall and update packages](../consume-packages/reinstalling-and-updating-packages.md)

    If you see the error "This project references NuGet package(s) that are missing on this computer," or "One or more NuGet packages need to be restored but couldn't be because consent has not been granted," turn on automatic restore by following the instructions under [Enable and disable package restore](#enable-and-disable-package-restore). Also see [Package restore troubleshooting](Package-restore-troubleshooting.md).

- **NuGet CLI**: use the [nuget restore](../tools/cli-ref-restore.md) command, which restores packages listed in the project file or in `packages.config`. You can also specify a solution file.

- **MSBuild**: use the [msbuild -t:restore](../reference/msbuild-targets.md#restore-target) command, which restores packages packages listed in the project file (PackageReference only). This command is available only in NuGet 4.x+ and MSBuild 15.1+, which are included with Visual Studio 2017. `nuget restore` and `dotnet restore` both use this command for applicable projects.

- **Azure Pipelines**: When you create a build definition in Azure Pipelines, include the [NuGet restore](/azure/devops/pipelines/tasks/package/nuget#restore-nuget-packages) or [.NET Core Restore](/azure/devops/pipelines/tasks/build/dotnet-core#restore-nuget-packages) task in the definition before any build task. Many build templates include this restore task by default.

- **Azure DevOps Server**: Azure DevOps Server and TFS 2013 and later automatically restore packages during build, provided that you're using a Team Build Template for TFS 2013 or later. For earlier version of TFS, you can include a build step to invoke one of the command-line restore options above. You can optionally migrate the build template to TFS 2013. For more information, see [Set up package restore with Team Foundation Build](../consume-packages/team-foundation-build.md).

## Enable and disable package restore

You enable Package Restore primarily through **Tools** > **Options** > **NuGet Package Manager** in Visual Studio:

![Control package restore behaviors through NuGet Package Manager options](media/Restore-01-AutoRestoreOptions.png)

- **Allow NuGet to download missing packages** controls all forms of package restore by changing the `packageRestore/enabled` setting in the `NuGet.Config` file as shown below (`%AppData%\NuGet\NuGet.Config` on Windows, `~/.nuget/NuGet/NuGet.Config` on Mac/Linux). In Visual Studio, this setting enables the **Restore NuGet Packages** command on the solution's context menu.

    ```xml
    <configuration>
        <packageRestore>
            <!-- The 'enabled' key is True when the "Allow NuGet to download missing packages" checkbox is set.
                 Clearing the box sets this to False, disabling command-line, automatic, and MSBuild-Integrated restore. -->
            <add key="enabled" value="True" />
        </packageRestore>
    </configuration>
    ```

> [!Note]
> To override the `packageRestore/enabled` setting globally, set the environment variable **EnableNuGetPackageRestore** with a value of TRUE or FALSE before launching Visual Studio or starting a build.

- **Automatically check for missing packages during build in Visual Studio** controls automatic restore by changing the `packageRestore/automatic` setting in the `NuGet.Config` file as shown below (`%AppData%\NuGet\NuGet.Config` on Windows, `~/.nuget/NuGet/NuGet.Config` on Mac/Linux). When this option is set, running a build from Visual Studio automatically restores any missing packages. The option does not affect builds run from the command line using MSBuild.

    ```xml
    ...
    <configuration>
        <packageRestore>
            <!-- The 'automatic' key is set to True when the "Automatically check for missing packages during
                 build in Visual Studio" checkbox is set. Clearing the box sets this to False and disables
                 automatic restore. -->
            <add key="automatic" value="True" />
        </packageRestore>
    </configuration>
    ```

For reference, see the [NuGet config file - packageRestore section](../reference/nuget-config-file.md#packagerestore-section).

In some cases, a developer or company might want to enable or disable Package Restore for all users on a computer. To do this, add the same settings as above to the global NuGet configuration file located in `%ProgramData%\NuGet\Config` (Windows, potentially under a specific `\{IDE}\{Version}\{SKU}\` folder for Visual Studio) or `~/.local/share` (Mac/Linux). Individual users can then selectively enable restore as needed on a project level. See [Configure NuGet behavior](../consume-packages/configuring-nuget-behavior.md#how-settings-are-applied) for exact details on how NuGet prioritizes multiple config files.

> [!Important]
> If you edit the `packageRestore` settings directly in `nuget.config`, restart Visual Studio so that the **Options** dialog box shows the current values.

## Constrain package versions with restore

When NuGet restores packages through any method, it honors any constraints you specified in `packages.config` or the project file:

- `packages.config`: You can specify a version range in the `allowedVersion` property of the dependency. See [Constrain upgrade versions](../consume-packages/reinstalling-and-updating-packages.md#constraining-upgrade-versions) for more information. For example:

    ```xml
    <package id="Newtonsoft.json" version="6.0.4" allowedVersions="[6,7)" />
    ```

- Project file (PackageReference): You can specify a version range directly with the dependency's version number. For example:

    ```xml
    <PackageReference Include="Newtonsoft.json" Version="[6, 7)" />
    ```

In all cases, use the notation described in [Package versioning](../reference/package-versioning.md).

## Force restore from package sources

By default, NuGet restore operations use packages from the *global-packages* and *http-cache* folders, which are described in [Manage the global packages and cache folders](managing-the-global-packages-and-cache-folders.md).

To avoid using the *global-packages* folder, do one of the following:

- Clear the folder using `nuget locals global-packages -clear` or `dotnet nuget locals global-packages --clear`.
- Temporarily change the location of the *global-packages* folder before the restore operation, using one of the following methods:
  - Set the NUGET_PACKAGES environment variable to a different folder.
  - Create a `NuGet.Config` file that sets `globalPackagesFolder` (if using PackageReference) or `repositoryPath` (if using `packages.config`) to a different folder. For more information, see [configuration settings](../reference/nuget-config-file.md#config-section).
  - MSBuild only: Specify a different folder with the `RestorePackagesPath` property.

To avoid using the cache for HTTP sources, do one of the following:

- Use the `-NoCache` option with `nuget restore` or the `--no-cache` option with `dotnet restore`. These options don't affect restore operations through the Visual Studio Package Manager or console.
- Clear the cache using `nuget locals http-cache -clear` or `dotnet nuget locals http-cache --clear`.
- Temporarily set the NUGET_HTTP_CACHE_PATH environment variable to a different folder.

## Troubleshooting

See [Troubleshoot package restore](package-restore-troubleshooting.md).
