---
title: NuGet Package Restore
description: See an overview of how NuGet restores packages a project depends on, including how to disable restore and constrain versions.
author: JonDouglas
ms.author: jodou
ms.date: 10/20/2023
ms.topic: conceptual
---

# Restore packages with NuGet Package Restore

NuGet Package Restore restores all of a project's dependencies that are listed in either a project file or a *packages.config* file. You can restore packages manually with `nuget restore`, `dotnet restore`, `msbuild -t:restore`, or through Visual Studio. The `dotnet build` and `dotnet run` commands automatically restore packages, and you can configure Visual Studio to restore packages automatically when it builds a project.

To promote a cleaner development environment and to reduce repository size, Package Restore makes all of a project's dependencies available without having to store them in source control. To configure your source control repository to exclude package binaries, see [Packages and source control](../consume-packages/packages-and-source-control.md).

## Package Restore behavior

Package Restore tries to install all package dependencies to the state that matches the `<PackageReference>`s in a project file, such as *.csproj*, or `<package>`s in a *packages.config* file. Package Restore first installs the direct dependencies of a project as needed, then installs any dependencies of those packages throughout the entire dependency graph.

If a needed package isn't already installed, NuGet first attempts to retrieve it from the local [global packages or HTTP cache folders](../consume-packages/managing-the-global-packages-and-cache-folders.md). If the package isn't in the local folders, NuGet tries to download it from all sources configured in Visual Studio at **Tools** > **Options** > **NuGet Package Manager** > **Package Sources**.

During restore, NuGet ignores the order of package sources, and uses the package from the first source that responds to requests. If restore fails, NuGet doesn't indicate the failure until after it checks all sources. NuGet then reports a failure for only the last source in the list. The error implies that the package wasn't present on any of the sources, even though it doesn't list the other failures individually.

For more information about NuGet behavior, see [Common NuGet configurations](Configuring-NuGet-Behavior.md).

## Restore packages

If the package references in your project file or *packages.config* file are correct, use your preferred tool to restore packages:

- [Visual Studio](#restore-using-visual-studio)
- [dotnet CLI](#restore-using-the-dotnet-cli)
- [nuget.exe CLI](#restore-using-the-nugetexe-cli)
- [MSBuild](#restore-using-msbuild)
- [Azure Pipelines or Azure DevOps Server](#restore-using-azure-pipelines)

After a successful restore:

- For projects that use `<PackageReference>`, the package is present in the local *global-packages* folder, and the project *obj/project.assets.json* file is recreated.
- For projects that use *packages.config*, the package appears in the project's *packages* folder.
- The project should now build successfully.

If the package references in your project file or your *packages.config* file are incorrect and don't match your desired state, install or update the correct packages instead of using Package Restore.

If you have missing packages or package-related errors after you run Package Restore, such as error icons in Solution Explorer, follow the instructions in [Troubleshooting Package Restore errors](package-restore-troubleshooting.md), or [reinstall or update](../consume-packages/reinstalling-and-updating-packages.md) the packages. In Visual Studio, the Package Manager Console provides several options for reinstalling packages. For more information, see [Use Package-Update](reinstalling-and-updating-packages.md#using-update-package).

<a name="restore-using-visual-studio"></a>
## Restore packages in Visual Studio

In Visual Studio on Windows, you can restore packages automatically or manually. First, configure Package Restore through **Tools** > **Options** > **NuGet Package Manager**.

### Configure Visual Studio Package Restore options

Configure the following Package Restore options at **Tools** > **Options** > **NuGet Package Manager** > **General**.

![Screenshot that shows the NuGet Package Manager options.](media/Restore-01-AutoRestoreOptions.png)

<a name="enable-and-disable-package-restore-in-visual-studio"></a>
#### Allow NuGet to download missing packages

Select **Allow NuGet to download missing packages** to enable package restore and the **Restore NuGet Packages** command. This selection sets the `packageRestore/enabled` setting to `True` in the [packageRestore section](../reference/nuget-config-file.md#packagerestore-section) of the global *NuGet.Config* file, at *%AppData%\\Roaming\\NuGet* on Windows or *~/.nuget/NuGet/* on Mac or Linux.

```xml
<configuration>
    <packageRestore>
        <add key="enabled" value="True" />
    </packageRestore>
</configuration>
```

> [!Note]
> To globally override the `packageRestore/enabled` setting, you can set the environment variable **EnableNuGetPackageRestore** to True or False before you open Visual Studio or start a build.

To enable or disable Package Restore for all users on a computer, you can add the configuration settings to the global *NuGet.Config* file in Windows at *%ProgramData%\NuGet\Config*, sometimes under a specific *\<IDE>\\\<Version>\\\<SKU>* Visual Studio folder, or in Mac/Linux at *~/.local/share*. Individual users can then selectively enable restore as needed on a project level. For more details on how NuGet prioritizes multiple config files, see [Common NuGet configurations](../consume-packages/configuring-nuget-behavior.md#how-settings-are-applied).

> [!Important]
> If you edit the `packageRestore` settings in *NuGet.Config* directly, restart Visual Studio so that the **Options** show the current values.

#### Automatically check for missing packages during build

Select **Automatically check for missing packages during build in Visual Studio** to automatically restore any missing packages when you run a build from Visual Studio. This setting doesn't affect builds run from the MSBuild command line. This selection sets the `packageRestore/automatic` setting to `True` in the `packageRestore` section of the *NuGet.Config* file.

```xml
<configuration>
    <packageRestore>
        <add key="automatic" value="True" />
    </packageRestore>
</configuration>
```

For non-SDK-style projects, you must select **Allow NuGet to download missing packages** as well as **Automatically check for missing packages during build in Visual Studio** in **Options** to enable automatic restore.

<a name="choose-default-package-management-format"></a>
#### Choose the default package management format

NuGet has two package management formats, [PackageReference](Package-References-in-Project-Files.md) and [packages.config](../reference/packages-config.md). Select the format you want to use from the dropdown list under **Package Management**. You can also select whether to allow format selection on first package install.

> [!Note]
> - If a project doesn't support both package management formats, NuGet uses the package management format that's compatible with the project, which might not be the default you set in the options. NuGet then won't prompt for selection on first install, even if you selected that option.
>
> - If you use Package Manager Console to install the first package in a project, NuGet doesn't prompt for format selection, even if that option is selected in **Options**.

<a name="restore-packages-automatically-using-visual-studio"></a>
<a name="restore-packages-manually-using-visual-studio"></a>
### Restore packages manually or automatically

After you enable package restore in **Options**, you can right-click the solution in **Solution Explorer** and select **Restore NuGet Packages** to restore packages anytime.

If you enabled automatic restore in **Options**, Package Restore happens automatically when you create a project from a template or build a project. For NuGet 4.0+, restore also happens automatically when you make changes to a SDK-style project.

For projects that use `<PackageReference>`, you can see the package references in Visual Studio **Solution Explorer** under **Dependencies** > **Packages**. Packages that don't install properly when you manually restore or run a build display error icons in **Solution Explorer**. Right-click the project, select **Manage NuGet Packages**, and use the **NuGet Package Manager** to uninstall and reinstall the affected packages. For more information, see [Reinstall and update packages](../consume-packages/reinstalling-and-updating-packages.md).

If you see the error **This project references NuGet package(s) that are missing on this computer**, or **One or more NuGet packages need to be restored but couldn't be because consent has not been granted**, make sure you enabled automatic restore. For older projects, see [Migrate to automatic package restore](#migrate-to-automatic-package-restore-visual-studio). Also see [Troubleshooting package restore errors](Package-restore-troubleshooting.md).

<a name="restore-using-the-dotnet-cli"></a>
## Restore by using the dotnet CLI

[!INCLUDE [restore-dotnet-cli](includes/restore-dotnet-cli.md)]

> [!IMPORTANT]
> To add a missing package reference to the project file, use [dotnet add package](/dotnet/core/tools/dotnet-add-package), which also runs `restore`.

<a name="restore-using-the-nugetexe-cli"></a>
## Restore by using the NuGet CLI

[!INCLUDE [restore-nuget-exe-cli](includes/restore-nuget-exe-cli.md)]

<a name="restore-using-msbuild"></a>
## Restore by using MSBuild

You can use [msbuild -t:restore](../reference/msbuild-targets.md#restore-target) to restore packages in NuGet 4.x+ and MSBuild 15.1+, which are included with Visual Studio 2017 and higher.

This command restores packages in projects that use [PackageReference](package-references-in-project-files.md) for package references. Starting with MSBuild 16.5+, the command also supports [packages.config](/nuget/reference/packages-config) package references, when used with `-p:RestorePackagesConfig=true`.

To use MSBuild restore:

1. Open a Developer Command Prompt by searching for *developer command prompt* and starting the prompt from the Windows **Start** menu, which configures all the necessary paths for MSBuild.

1. Switch to the project folder, and enter `msbuild -t:restore`.

1. After the restore completes, enter `msbuild` to rebuild the project. Make sure the MSBuild output indicates that the build completed successfully.

> [!Note]
> You can use `msbuild -restore` to run `restore`, reload the project, and build, since build is the default target. For more information, see [Restore and build with one MSBuild command](../reference/msbuild-targets.md#restoring-and-building-with-one-msbuild-command).

<a name="restore-using-azure-pipelines"></a>
<a name="restore-using-azure-devops-server"></a>
## Restore with Azure Pipelines or Azure DevOps Server

When you create a build definition in Azure Pipelines, you can include the [NuGet CLI restore](/azure/devops/pipelines/tasks/package/nuget#restore-nuget-packages) or [dotnet CLI restore](/azure/devops/pipelines/tasks/build/dotnet-core-cli) task in the definition before any build tasks. Some build templates include the restore task by default.

Azure DevOps Server and TFS 2013 and later automatically restore packages during build, if you use a TFS 2013 or later Team Build template. You can also include a build step to run a command-line restore option, or optionally migrate the build template to a later version. For more information, see [Set up package restore with Team Foundation Build](../consume-packages/team-foundation-build.md).

## Constrain package versions

NuGet restore through any method honors any version constraints you specify in *packages.config* or the project file.

- In *packages.config*, you can specify an `allowedVersions` range in the dependency. For more information, see [Constraints on upgrade versions](../consume-packages/reinstalling-and-updating-packages.md#constraints-on-upgrade-versions). For example:

  ```xml
  <package id="Newtonsoft.json" version="6.0.4" allowedVersions="[6,7)" />
  ```

- In a project file, you can specify the version range in the `Version` property of the dependency. For example:

  ```xml
  <PackageReference Include="Newtonsoft.json" Version="[6,7)" />
  ```

In both cases, use the notation described in [Package versioning](../concepts/package-versioning.md).

## Force restore from remote package sources

By default, NuGet restore operations use packages from the local *global-packages* and *http-cache* folders, as described in [Manage the global packages and cache folders](managing-the-global-packages-and-cache-folders.md). To avoid using these local packages, use the following options.

To clear all local caches:

- In Visual Studio, select the **Clear All NuGet Cache(s)** button at **Tools** > **Options** > **NuGet Package Manager** > **General**.
- In the dotnet CLI, use `dotnet nuget locals all --clear`.
- In the NuGet CLI, use `nuget locals all -clear`.

To avoid using the packages in the *global-packages* folder:

- Clear the folder by using `nuget locals global-packages -clear` or `dotnet nuget locals global-packages --clear`.
- Temporarily set the **NUGET_PACKAGES** environment variable to a different folder.
- Create a *NuGet.Config* file that sets `globalPackagesFolder` for `PackageReference`, or `repositoryPath` for *packages.config*, to a different folder. For more information, see [configuration settings](../reference/nuget-config-file.md#config-section).
- For MSBuild only, specify a different folder with the `RestorePackagesPath` property.

To avoid using packages in the HTTP cache:

- Clear the cache by using `nuget locals http-cache -clear` or `dotnet nuget locals http-cache --clear`.
- Temporarily set the **NUGET_HTTP_CACHE_PATH** environment variable to a different folder.
- For `nuget restore`, use the `-NoHttpCache` option, or for `dotnet restore`, use the `--no-http-cache` option. These options don't affect restore operations through the Visual Studio Package Manager or Console.

<a name="migrate-to-automatic-package-restore-visual-studio"></a>
## Migrate to automatic package restore

Earlier versions of NuGet supported an MSBuild-integrated package restore. Projects that use the deprecated MSBuild-integrated package restore should migrate to automatic package restore.

These projects typically contain a *.nuget* folder with three files: *NuGet.config*, *nuget.exe*, and *NuGet.targets*. The *NuGet.targets* file causes NuGet to use the MSBuild-integrated approach, so it must be removed.

To migrate to automatic package restore:

1. Enable automatic package restore.
1. Close Visual Studio.
1. Delete *.nuget/nuget.exe* and *.nuget/NuGet.targets*.
1. For each project file, remove the `<RestorePackages>` element, and remove any references to *NuGet.targets*.

To test automatic package restore:

1. Remove the *packages* folder from the solution.
1. Open the solution in Visual Studio and start a build. Automatic package restore should download and install each dependency package, without adding it to source control.

## Next steps

- [Troubleshoot package restore](Package-restore-troubleshooting.md)
- [Manage the global packages, cache, and temp folders](managing-the-global-packages-and-cache-folders.md)
