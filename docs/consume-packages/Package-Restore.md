---
title: NuGet Package Restore
description: An overview of how NuGet restores packages a project depends on, including how to disable restore and constrain versions.
author: karann-msft
ms.author: karann
ms.date: 06/24/2019
ms.topic: conceptual
---

# Package Restore

To promote a cleaner development environment and to reduce repository size, NuGet **Package Restore** installs all of a project's dependencies listed in either the project file or `packages.config`. The .NET Core 2.0+ `dotnet build` and `dotnet run` commands do an automatic package restore. Visual Studio can restore packages automatically when it builds a project, and you can restore packages at any time through Visual Studio, `nuget restore`, `dotnet restore`, and xbuild on Mono.

Package Restore makes sure that all a project's dependencies are available, without having to store them in source control. To configure your source control repository to exclude the package binaries, see [Packages and source control](../consume-packages/packages-and-source-control.md). 

## Package Restore overview

Package Restore first installs the direct dependencies of a project as needed, then installs any dependencies of those packages throughout the entire dependency graph.

If a package isn't already installed, NuGet first attempts to retrieve it from the [cache](../consume-packages/managing-the-global-packages-and-cache-folders.md). If the package isn't in the cache, NuGet tries to download the package from all enabled sources in the list at **Tools** > **Options** > **NuGet Package Manager** > **Package Sources** in Visual Studio. During restore, NuGet ignores the order of package sources, and uses the package from whichever source is first to respond to requests. For more information about how NuGet behaves, see [Configure NuGet behavior](Configuring-NuGet-Behavior.md). 

> [!Note]
> NuGet doesn't indicate a failure to restore a package until all the sources have been checked. At that time, NuGet reports a failure for only the last source in the list. The error implies that the package wasn't present on *any* of the other sources, even though errors aren't shown for each of those sources individually.

You can trigger Package Restore in any of the following ways:

- **dotnet CLI**: Use the [dotnet restore](/dotnet/core/tools/dotnet-restore?tabs=netcore2x) command to restore packages listed in the project file with [PackageReference](../consume-packages/package-references-in-project-files.md). With .NET Core 2.0 and later, restore happens automatically with the `dotnet build` and `dotnet run` commands.  

- **Package Manager**: In Visual Studio on Windows, Package Restore happens automatically when you create a project from a template or build a project, subject to the options in [Enable and disable package restore](#enable-and-disable-package-restore)). In NuGet 4.0+, restore also happens automatically when you make changes to a .NET Core SDK-based project.

    To restore packages manually, right-click the solution in **Solution Explorer** and select **Restore NuGet Packages**. If one or more individual packages still aren't installed properly, **Solution Explorer** shows an error icon. Right-click and select **Manage NuGet Packages**, and use **Package Manager** to uninstall and reinstall the affected packages. For more information, see [Reinstall and update packages](../consume-packages/reinstalling-and-updating-packages.md)

    If you see the error "This project references NuGet package(s) that are missing on this computer," or "One or more NuGet packages need to be restored but couldn't be because consent has not been granted," [enable automatic restore](#enable-and-disable-package-restore). Also see [Package Restore troubleshooting](Package-restore-troubleshooting.md).

- **nuget.exe CLI**: Use the [nuget restore](../tools/cli-ref-restore.md) command to restore packages listed in a project or solution file, or in `packages.config`. 

- **MSBuild**: Use the [msbuild -t:restore](../reference/msbuild-targets.md#restore-target) command to restore packages listed in the project file with PackageReference. This command is available only in NuGet 4.x+ and MSBuild 15.1+, which are included with Visual Studio 2017. Both `nuget restore` and `dotnet restore` use this command for applicable projects.

- **Azure Pipelines**: When you create a build definition in Azure Pipelines, include the NuGet [restore](/azure/devops/pipelines/tasks/package/nuget#restore-nuget-packages) or .NET Core [restore](/azure/devops/pipelines/tasks/build/dotnet-core#restore-nuget-packages) task in the definition before any build tasks. Some build templates include the restore task by default.

- **Azure DevOps Server**: Azure DevOps Server and TFS 2013 and later automatically restore packages during build, if you're using a TFS 2013 or later Team Build template. For earlier TFS versions, you can include a build step to run a command-line restore option, or optionally migrate the build template to a later version. For more information, see [Set up package restore with Team Foundation Build](../consume-packages/team-foundation-build.md).

## Enable and disable package restore

In Visual Studio, you control Package Restore primarily through **Tools** > **Options** > **NuGet Package Manager**:

![Control Package Restore through NuGet Package Manager options](media/Restore-01-AutoRestoreOptions.png)

- **Allow NuGet to download missing packages** controls all forms of package restore by changing the `packageRestore/enabled` setting in the [packageRestore section](../reference/nuget-config-file.md#packagerestore-section) of the `NuGet.Config` file, at `%AppData%\NuGet\` on Windows, or `~/.nuget/NuGet/` on Mac/Linux. This setting also enables the **Restore NuGet Packages** command on the solution's context menu in Visual Studio, .

    ```xml
    <configuration>
        <packageRestore>
            <!-- The 'enabled' key is True when the "Allow NuGet to download missing packages" checkbox is set.
                 Clearing the box sets this to False, disabling command-line, automatic, and MSBuild-integrated restore. -->
            <add key="enabled" value="True" />
        </packageRestore>
    </configuration>
    ```
    
  > [!Note]
  > To globally override the `packageRestore/enabled` setting, set the environment variable **EnableNuGetPackageRestore** with a value of True or False before launching Visual Studio or starting a build.

- **Automatically check for missing packages during build in Visual Studio** controls automatic restore by changing the `packageRestore/automatic` setting in the [packageRestore section](../reference/nuget-config-file.md#packagerestore-section) of the `NuGet.Config` file. When this option is set to True, running a build from Visual Studio automatically restores any missing packages. This setting doesn't affect builds run from the MSBuild command line.

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

To enable or disable Package Restore for all users on a computer, a developer or company can add the configuration settings to the global `nuget.config` file. The global `nuget.config` is in Windows at `%ProgramData%\NuGet\Config`, sometimes under a specific `\{IDE}\{Version}\{SKU}\` Visual Studio folder, or in Mac/Linux at `~/.local/share`. Individual users can then selectively enable restore as needed on a project level. For more details on how NuGet prioritizes multiple config files, see [Configure NuGet behavior](../consume-packages/configuring-nuget-behavior.md#how-settings-are-applied).

> [!Important]
> If you edit the `packageRestore` settings directly in `nuget.config`, restart Visual Studio, so that the **Options** dialog box shows the current values.

## Constrain package versions with restore

When NuGet restores packages through any method, it honors any constraints you specified in `packages.config` or the project file:

- In `packages.config`, you can specify a version range in the `allowedVersion` property of the dependency. See [Constrain upgrade versions](../consume-packages/reinstalling-and-updating-packages.md#constraining-upgrade-versions) for more information. For example:

    ```xml
    <package id="Newtonsoft.json" version="6.0.4" allowedVersions="[6,7)" />
    ```

- In a project file, you can use PackageReference to specify a dependency's range directly. For example:

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

- Use the `-NoCache` option with `nuget restore`, or the `--no-cache` option with `dotnet restore`. These options don't affect restore operations through the Visual Studio Package Manager or console.
- Clear the cache using `nuget locals http-cache -clear` or `dotnet nuget locals http-cache --clear`.
- Temporarily set the NUGET_HTTP_CACHE_PATH environment variable to a different folder.

## Troubleshooting

See [Troubleshoot package restore](package-restore-troubleshooting.md).
