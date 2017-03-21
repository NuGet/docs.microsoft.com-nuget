---
# required metadata

title: NuGet Package Restore | Microsoft Docs
author: kraigb
ms.author: kraigb
manager: ghogen
ms.date: 1/9/2017
ms.topic: article
ms.prod: nuget
#ms.service:
ms.technology: nuget
ms.assetid: a7bf21da-86ae-4c2d-8750-04ff53f41967

# optional metadata

description: A description of how NuGet restores packages upon which a project depends, including how to disable restore and constrain versions.
keywords: NuGet package restore, NuGet package installation, installing package, restoring packages, dependency versions, disabling automatic restore, constraining package versions
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

# Installing and Reinstalling Packages with Package Restore

To promote a cleaner development environment and to reduce repository size, NuGet **Package Restore** installs all referenced packages before a project is built. This widely-used feature ensures that all dependencies are available in a project without requiring those packages to be stored in source control (see [Packages and Source Control](../consume-packages/packages-and-source-control.md) on how to configure your repository to exclude package binaries).

In this topic:

- [Enabling and disabling package restore](#enabling-and-disabling-package-restore)
- [Constraining package versions with restore](#constraining-package-versions-with-restore)
- [Command-line restore](#command-line-restore), for all versions of NuGet
- [Automatic restore in Visual Studio](#automatic-restore-in-visual-studio), for NuGet 2.7 and later.
- [MSBuild-integrated restore in Visual Studio](#msbuild-integrated-restore), for NuGet 2.6 and earlier.
- [Package restore with Team Foundation Build](#package-restore-with-team-foundation-build)

To check your NuGet version, simply run `nuget.exe` on the command line and look at the first line of output.

It's best to avoid mixing the automatic and MSBuild-integrated restore methods for a single project. We instead recommend [migrating to automatic restore](#migrating-to-automatic-restore).

For additional details on package restore on build servers, see [Package restore with TFBuild](../consume-packages/team-foundation-build.md).

> [!Note]
> Projects configured for package restore also work with xbuild on Mono.

## Enabling and disabling package restore

Automatic restore and command-line restore is enabled by default with NuGet 2.7 and later. MSBuild-integrated restore and command-line restore is **not** enabled by default for NuGet 2.6 and earlier and must be enabled manually.

Package restore is primarily enabled through **Tools > Options > [NuGet] Package Manager > General** in Visual Studio:

![Controlling package restore behaviors through NuGet Package Manager options](media/Restore-01-AutoRestoreOptions.png)

- **Allow NuGet to download missing packages**: enables all forms of package restore by changing the `packageRestore/enabled` setting in the `%AppData%\NuGet\NuGet.config` file as shown below. (For NuGet 2.6 or earlier, this setting can also be used in a project-specific `.nuget\nuget.config` file.)

    ```xml
    ...
    <configuration>
        <packageRestore>
            <!-- Disables command-line, automatic, and MSBuild-Integrated restore -->
        <add key="enabled" value="False" />
        </packageRestore>
    </configuration>
    ```


> [!Note]
>  The `packageRestore/enabled` setting can be overridden globally by setting an environment variable called **EnableNuGetPackageRestore** with a value of TRUE or FALSE before launching Visual Studio or starting a build.


- **Automatically check for missing packages during build in Visual Studio**: enables automatic restore for NuGet 2.7 and later by changing the `packageRestore/automatic` setting in the `%AppData%\NuGet\NuGet.config` file as shown below.
            
    ```xml
    ...
    <configuration>
        <packageRestore>
        <!-- Disables automatic restore in Visual Studio -->
        <add key="automatic" value="False" />
        </packageRestore>
    </configuration>
    ```

For reference, see the [NuGet config file - packageRestore section](../Schema/nuget-config-file.md#packagerestore-section).

MSBuild-integrated restore with NuGet 2.6 and earlier is typically enabled by right-clicking a solution in Visual Studio and selecting **Enable NuGet Package Restore**. This sets up the necessary files and folders for this option to work, as explained under [MSBuild-integrated restore in Visual Studio](#msbuild-integrated-restore).

In some cases, a developer or company might want to enable or disable package restore on a machine by default for all users. This can be done by adding the same settings above to the global NuGet configuration file located in `%ProgramData%\NuGet\Config[\{IDE}[\{Version}[\{SKU}]]]`. Individual users can then selectively enable restore as needed on a project level. See [Configuring NuGet Behavior](../consume-packages/configuring-nuget-behavior.md#how-settings-are-applied) for exact details on how NuGet prioritizes multiple config files.

## Constraining package versions with restore

When NuGet restores packages through any method, it will honor any constraints specified in either `packages.config` or `project.json`:

- `packages.config`: Specify a version range in the `allowedVersion` property of the dependency. See [Reinstalling and Updating Packages](../consume-packages/reinstalling-and-updating-packages.md#constraining-upgrade-versions). For example:

    ```xml
    <package id="Newtonsoft.json" version="6.0.4" allowedVersions="[6,7)" />
    ```

- `project.json`: Specify a version range directly with the dependency's version number. For example:

    ```json
    "newtonsoft.json": "[6, 7)"
    ```

In both cases, use the notation described in [Dependency versions](../create-packages/dependency-versions.md).


## Command-line restore

For NuGet 2.6 and earlier, you use the [Install](../tools/nuget-exe-cli-reference.md#install) command and point to the `packages.config` file that lists all the dependencies.

For NuGet 2.7 and above, use the [Restore](../tools/nuget-exe-cli-reference.md#restore) command to restore all packages in a solution (using either `packages.config` in NuGet 2.x and later or `project.json` in NuGet 3.x and later). For a given project folder such as `c:\proj\app`, the common variations below each restore the packages:

```bash
c:\proj\app\> nuget restore
c:\proj\app\> nuget.exe restore app.sln
c:\proj\> nuget restore app
```

## Automatic restore in Visual Studio

With NuGet 2.7 and later, Visual Studio automatically restores missing packages by default at the beginning of a build. This behavior can be changed by unchecking **Tools > Options > [NuGet] Package Manager > General > Automatically check for missing packages during build in Visual Studio**.

Automatic restore is also ignored if a `.nuget\NuGet.targets` file exists in a project, indicating that the project is configured for MSBuild-integrated restore. This can cause some errors as described below in [Automatic restore errors](#automatic-restore-errors). To update a project, see [Migrating to automatic restore](#migrating-to-automatic-restore).

When enabled, automatic restore works as follows:

1. A `.nuget` folder is created in the solution containing a `nuget.config` file that contains only a single setting for `disableSourceControlIntegration` (as described in [Packages and source control](../consume-packages/packages-and-source-control.md) for Team Foundation Version Control).
2. When a build begins, Visual Studio instructs NuGet to restore packages.
3. NuGet recursively looks for all `packages.config` files in the solution (NuGet 2.x), or looks for `project.json` (NuGet 3.x).
4. For each packages listed in the configuration files, NuGet checks if it exists in the solution's `packages` folder.
5. If the package is not found, NuGet attempts to retrieve the package from its cache first (see [Managing the NuGet cache](../consume-packages/managing-the-nuget-cache.md). If the package is not in the cache, NuGet downloads the package from the enabled sources as listed in **Tools > Options > [NuGet] Package Manager > Package Sources**, in the order that the sources appear.
6. If the download is successful, NuGet caches it, and then installs the package into the `packages` folder; otherwise NuGet fails and the build fails.

During this process, developers see a progress dialog with the option to cancel package restore.


### Automatic restore errors

If you're using NuGet 2.7 or later and have a solution that is still configured for MSBuild-integrated restore, you may have an older version of `nuget.exe` in the solution's `.nuget` folder. This will cause builds to fail with an error stating that you have not given consent to restore packages.

To correct these errors, do one of the following:

1. [Migrate the project to automatic restore](#migrating-to-automatic-restore).
1. Update `nuget.exe` in the `.nuget` folder** as follows

    ```bash
    cd .nuget
    nuget update -self
    ```

1. Reset consent in your `%AppData%\NuGet\NuGet.config` file by going to **Tools > Options > NuGet Package Manager > General** in Visual Studio, uncheck and re-check both **Package Restore** options, and click OK. This re-saves `NuGet.config` with the proper consent settings for NuGet 2.6 and earlier.


## MSBuild-integrated restore

> [!Note]    
> Although MS-Build integrated restore still works with NuGet 2.7 and later, we  recommended that you [migrate to automatic restore](#migrating-to-automatic-restore) instead because it's simpler and more robust.

As noted before, MSBuild-integrated restore with NuGet 2.6 and earlier is typically enabled by right-clicking a solution in Visual Studio and selecting **Enable NuGet Package Restore**. This has the following effects:

- NuGet creates a `.nuget` folder in the solution containing `nuget.exe`, `nuget.config`, and `nuget.targets` files.
- NuGet updates all projects in the solution to include a `<RestorePackages>true</RestorePackages>` flag and to import `nuget.targets`. These cause MSBuild to invoke `nuget.exe` to restore packages before a build provided that the **Tools > Options > NuGet Package Manager > General > Allow NuGet to download missing packages** option is checked.

Again, this option is **not** checked by default for NuGet 2.6 and earlier and must be manually set for package restore to work. For a custom build `.proj`, a pre build `<Exec>` action must also be added manually to restore packages.

With MSBuild-integrated restore, packages that are downloaded by Visual Studio will be automatically added to Team Foundation Version Control by default when the project is connected to a repository. To change this behavior, see [Omitting packages with Team Foundation Version Control](../consume-packages/packages-and-source-control.md#omitting-packages-with-team-foundation-version-control).


### Migrating to automatic restore

Although the MSBuild-integrated restore approach works, it has several drawbacks:

- It requires additional files within the solution folder.
- It requires importing a `.targets` file into all projects in the solution, which this can introduce issues when projects are shared among multiple solutions.
- Projects will fail to load if `nuget.targets` cannot be found.
- Projects won't build successfully if any of the restored NuGet packages extend MSBuild through a targets/props file import.
- Packages are automatically added to Team Foundation Version Control, when in use, unless specifically disabled.
- It is not compatible with ASP.NET web site projects created in Visual Studio.

To avoid all these issues, it's recommended to migrate any project using MSBuild-integrated restore to use the automatic restore capabilities of NuGet 2.7 and above.

The process is as follows:

1. Close Visual Studio to avoid file potential file locks and conflicts.
1. If using TFS:
    a. Remove `nuget.exe` and `nuget.targets` from the solution's `.nuget` folder and remove those files from the solution workspace.
    a. Retain `nuget.config` with the `disableSourceControlIntegration` setting as explained in [Omitting packages with Team Foundation Version Control](../consume-packages/packages-and-source-control.md#omitting-packages-with-team-foundation-version-control).
1. If not using TFS:
    a. Remove the `.nuget` folder from the solution and the solution workspace.
1. Edit each project file in the solution, remove the `&lt;RestorePackages&gt;` element, and remove any references to the `nuget.targets` file. Those settings generally appear as follows:

    ```xml
    <RestorePackages>true</RestorePackages>
    ...
    <Import Project="$(SolutionDir)\.nuget\nuget.targets" />
    ...
    <Target Name="EnsureNuGetPackageBuildImports" BeforeTargets="PrepareForBuild">
        <PropertyGroup>
            <ErrorText>This project references NuGet package(s) that are missing on this computer. Enable NuGet Package Restore to download them.  For more information, see http://go.microsoft.com/fwlink/?LinkID=322105. The missing file is {0}.</ErrorText>
        </PropertyGroup>
        <Error Condition="!Exists('$(SolutionDir)\.nuget\NuGet.targets')" Text="$([System.String]::Format('$(ErrorText)', '$(SolutionDir)\.nuget\NuGet.targets'))" />
    </Target>
    ```

> [!Tip]    
> Owen Johnson has created a [PowerShell migration script](https://github.com/owen2/AutomaticPackageRestoreMigrationScript) that can work in many cases, but is used at your own risk. Be sure to commit your project to source control or make a backup before using it.</div>

To test the migration, do the following:

1. Remove the `packages` folder from the solution.
1. Open the solution in Visual Studio and start a build.
1. Automatic restore should download and install each dependency package, without adding them to source control.


## Package Restore with Team Foundation Build

Package restore is commonly used when building projects on build servers, as with Team Foundation Server (TFS) and Visual Studio Team Services (as well as other build systems, which are not covered here).

### Visual Studio Team Services

When creating a build definition on Team Services, simply include the Restore NuGet Packages task in the definition before any build task. This task is included by default in a number of build templates.

![NuGet package restore task in a Visual Studio Team Service build definition](media/Restore-02-VSTSBuild.png)

### Team Foundation Server

With TFS 2013 and later, packages are automatically restored by default during build, provided that you're using a Team Build Template for Team Foundation Server 2013 or later.

If you're using a previous version of build templates (such as in a project that's been migrated from earlier versions of TFS), you'll need to also migrate those build templates to TFS 2013. This essentially means recreating the custom parts of the Build Templates using the appropriate template for your source control (TFVC or Git).

For earlier version of TFS, you can simply include a build step to invoke [command-line restore](#commandline-restore) as described earlier.

For more details see then [Walkthrough of Package Restore with Team Foundation Build](../consume-packages/team-foundation-build.md).

### Command-line restore wrapped in MSBuild

With existing build servers and MSBuild-based projects, you can also choose to wrap the command line in a regular MSBuild project. This minimizes changes on the server and also provides built-in support for aggregated logging and error reporting. This differs from the usual MSBuild-integrated restore as it runs before any other build process happens, instead of as part of those other processes.
