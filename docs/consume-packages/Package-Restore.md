---
title: NuGet Package Restore
description: An overview of how NuGet restores packages upon which a project depends, including how to disable restore and constrain versions.
author: karann-msft
ms.author: karann
manager: unnir
ms.date: 03/16/2018
ms.topic: conceptual
---

# Package Restore

To promote a cleaner development environment and to reduce repository size, NuGet **Package Restore** installs all a project's dependencies as listed in either the project file or `packages.config`. Visual Studio can restore packages automatically when a project is built. The `dotnet build` and `dotnet run` commands (.NET Core 2.0+) also perform an automatic restore. You can also restore packages at any time through Visual Studio, `nuget restore`, `dotnet restore`, and xbuild on Mono.

Package restore makes sure that all a project's dependencies are available without storing those packages in source control. See [Packages and Source Control](../consume-packages/packages-and-source-control.md) on how to configure your repository to exclude package binaries.

## Package restore overview

Restoring packages first installs the direct dependencies of a project as needed, then installs any dependencies of those packages throughout the entire dependency graph.

If a package is not already installed, NuGet first attempts to retrieve it from the [cache](../consume-packages/managing-the-global-packages-and-cache-folders.md). If the package is not in the cache, NuGet then attempts to download the package from all enabled sources (see [Configuring NuGet behavior](Configuring-NuGet-Behavior.md); sources also appear in the  **Tools > Options > NuGet Package Manager > Package Sources** list in Visual Studio). During restore, NuGet ignores the order of package sources, using the package from whichever source is first to respond to requests.

> [!Note]
> NuGet does not indicate a failure to restore a package until all the sources have been checked. At that time, NuGet reports a failure for only the last source in the list. The error implies that the package wasn't present on *any* of the other sources, even though errors are not shown for each of those sources individually.

Package restore is triggered in the following ways:

- **dotnet CLI**: use the [dotnet restore](/dotnet/core/tools/dotnet-restore?tabs=netcore2x) command, which restores packages listed in the project file (see [PackageReference](../consume-packages/package-references-in-project-files.md)). With .NET Core 2.0 and later, restore is done automatically with `dotnet build` and `dotnet run`.

- **Package Manager UI (Visual Studio on Windows)**: Packages are restored automatically when creating a project from a template and when building a project (subject to the option described in [Enabling and disabling package restore](#enabling-and-disabling-package-restore)). In NuGet 4.0+, restore also happens automatically when changes are made to a .NET Core SDK-based project.

    To restore manually, right-click the solution in Solution Explorer and select **Restore NuGet Packages**. If one or more individual packages are still not installed properly (meaning that Solution Explorer shows an error icon), then use the Package Manager UI to uninstall and reinstall the affected packages. See [Reinstalling and updating packages](../consume-packages/reinstalling-and-updating-packages.md)

    If you see the error "This project references NuGet package(s) that are missing on this computer" or "One or more NuGet packages need to be restored but couldn't be because consent has not been granted," turn on automatic restore by following the instructions under [Enabling and disabling package restore](#enabling-and-disabling-package-restore). Also see [Package restore troubleshooting](Package-restore-troubleshooting.md).

- **NuGet CLI**: use the [nuget restore](../tools/cli-ref-restore.md) command, which restores packages listed in the project file or in `packages.config`. You can also specify a solution file.

- **MSBuild**: use the [msbuild /t:restore](../reference/msbuild-targets.md#restore-target) command, which restores packages packages listed in the project file (PackageReference only). Available only in NuGet 4.x+ and MSBuild 15.1+, which are included with Visual Studio 2017. `nuget restore` and `dotnet restore` both use this command for applicable projects.

- **Visual Studio Team Services**: When creating a build definition on Team Services, include the [NuGet restore](/vsts/build-release/tasks/package/nuget#restore-nuget-packages) or [.NET Core Restore](/vsts/build-release/tasks/build/dotnet-core#restore-nuget-packages) task in the definition before any build task. This task is included by default in a number of build templates.

- **Team Foundation Server**: TFS 2013 and later automatically restores packages during build, provided that you're using a Team Build Template for TFS 2013 or later. For earlier version of TFS, you can include a build step to invoke one of the command-line restore options above. You can optionally migrate the build template to TFS 2013. For more information, see the [Walkthrough of package restore with Team Foundation Build](../consume-packages/team-foundation-build.md).

## Enabling and disabling package restore

Package restore is primarily enabled through **Tools > Options > NuGet Package Manager** in Visual Studio:

![Controlling package restore behaviors through NuGet Package Manager options](media/Restore-01-AutoRestoreOptions.png)

- **Allow NuGet to download missing packages**: controls all forms of package restore by changing the `packageRestore/enabled` setting in the `NuGet.Config` file as shown below (`%AppData%\NuGet\NuGet.Config` on Windows, `~/.nuget/NuGet/NuGet.Config` on Mac/Linux). In Visual Studio, this setting allows the **Restore NuGet Packages** command on the solution's context menu to work.

    ```xml
    <configuration>
        <packageRestore>
            <!-- The 'enabled' key is True when the "Allow NuGet to download missing packages" checkbox is set.
                 Clearing the box sets this to False, disabling command-line, automatic, and MSBuild-Integrated restore. -->
            <add key="enabled" value="True" />
        </packageRestore>
    </configuration>
    ```
    <br/>
    > [!Note]
    >  The `packageRestore/enabled` setting can be overridden globally by setting an environment variable called **EnableNuGetPackageRestore** with a value of TRUE or FALSE before launching Visual Studio or starting a build.

- **Automatically check for missing packages during build in Visual Studio**: controls automatic restore by changing the `packageRestore/automatic` setting in the `NuGet.Config` file as shown below (`%AppData%\NuGet\NuGet.Config` on Windows, `~/.nuget/NuGet/NuGet.Config` on Mac/Linux). When this option is set, running a build from Visual Studio automatically restores any missing packages. The option does not affect builds run from the command line using MSBuild.

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

In some cases, a developer or company might want to enable or disable package restore for all users on a computer. To do this, add the same settings above to the global NuGet configuration file located in `%ProgramData%\NuGet\Config` (Windows, potentially under a specific `\{IDE}\{Version}\{SKU}\` folder for Visual Studio) or `~/.local/share` (Mac/Linux). Individual users can then selectively enable restore as needed on a project level. See [Configuring NuGet behavior](../consume-packages/configuring-nuget-behavior.md#how-settings-are-applied) for exact details on how NuGet prioritizes multiple config files.

> [!Important]
> If you edit the `packageRestore` settings directly in `nuget.config`, restart Visual Studio so that the options dialog box shows the current values.

## Constraining package versions with restore

When restoring packages through any method, NuGet honors any constraints specified in `packages.config` or the project file:

- `packages.config`: Specify a version range in the `allowedVersion` property of the dependency. See [Reinstalling and updating packages](../consume-packages/reinstalling-and-updating-packages.md#constraining-upgrade-versions). For example:

    ```xml
    <package id="Newtonsoft.json" version="6.0.4" allowedVersions="[6,7)" />
    ```

- Project file (PackageReference): Specify a version range directly with the dependency's version number. For example:

    ```xml
    <PackageReference Include="Newtonsoft.json" Version="[6, 7)" />
    ```

In all cases, use the notation described in [Package versioning](../reference/package-versioning.md).

## Forcing restore from package sources

By default, NuGet restore operations use packages from the *global-packages* and *http-cache* folders, which are described on [Managing the global packages and cache folders](managing-the-global-packages-and-cache-folders.md).

To avoid using the *global-packages* folder, do one of the following:

- Clear the folder using `nuget locals global-packages -clear` or `dotnet nuget locals global-packages --clear`
- Temporarily change the location of the *global-packages* folder before the restore operation using one of the following methods:
  - Set the NUGET_PACKAGES environment variable to a different folder.
  - Create a `NuGet.Config` file that sets `globalPackagesFolder` (if using PackageReference) or `repositoryPath` (if using `packages.config`) to a different folder (see [configuration settings](../reference/nuget-config-file.md#config-section)
  - MSBuild only: specify a different folder with the `RestorePackagesPath` property.

To avoid using the cache for HTTP sources, do one of the following:

- Use the `-NoCache` option with `nuget restore` or the `--no-cache` option with `dotnet restore`. These options do not affect restore operations through the Visual Studio Package Manager UI or Console.
- Clear the cache using `nuget locals http-cache -clear` or `dotnet nuget locals http-cache --clear`.
- Temporarily set of the NUGET_HTTP_CACHE_PATH environment variable to a different folder.

## Troubleshooting

See [Troubleshooting package restore](package-restore-troubleshooting.md).