---
title: Migrating from package.config to PackageReference formats | Microsoft Docs
author: karann-msft
ms.author: karann
manager: unniravindranathan
ms.date: 03/27/2018
ms.topic: article
ms.prod: nuget
ms.technology: null
description: Details on how to migrate a project from the package.config management format to PackageReference as supported by NuGet 4.0+ and VS2017 and .NET Core 2.0
keywords: NuGet migrator, migrate, package references, project files, PackageReference, packages.config, VS2017, Visual Studio 2017, NuGet 4, .NET Core 2.0
ms.reviewer:
- karann
- unnir
ms.workload: 
 - "dotnet"
 - "aspnet"
---

# Migrate from packages.config to PackageReference

NuGet 4.x.x., which ships with Visual Studio 2017 Version 15.7 Preview 3 and later, supports for migrating a project from using the [packages.config](./packages-config.md) management format to the [PackageReference](../consume-packages/Package-References-in-Project-Files.md) format.

## Benefits of using PackageReference

* **Manage all project dependencies in one place**: Just like project to project references and assembly references, NuGet package references (using the `PackageReference` node) are managed directly within project files rather than using a separate packages.config file.
* **Uncluttered view of top-level dependencies**: Unlike packages.config, PackageReference lists only those NuGet packages you directly installed in the project. As a result, the NuGet Package Manager UI and the project file aren't cluttered with down-level dependencies.
* **Performance improvements**: When using PackageReference, packages are maintained in the *global-packages* folder (as described on [Managing the global packages and cache folders](../consume-packages/managing-the-global-packages-and-cache-folders.md) rather than in a `packages` folder within the solution. As a result, PackageReference performs faster and consumes less disk space.
* **Fine control over dependencies and content flow**: Using the existing features of MSBuild allows you to [conditionally reference a NuGet package](../consume-packages/Package-References-in-Project-Files.md#adding-a-packagereference-condition) and choose package references per target framework, configuration, platform, or other pivots.
* **PackageReference is under active development**: See [PackageReference issues on GitHub](TBD). packages.config is no longer under active development.

### Reasons to not migrate

* NuGet PackageReference is not available in Visual Studio 2015 and earlier. Migrated projects can be opened only in Visual Studio 2017.
* Migration is not currently available for C++ and ASP.NET project.
* Some packages may not be fully compatible with PackageReference. For more information, see [package compatibility issues](#package-compatibility-issues).

## Migration steps

> [!Note]
> Before migration begins, Visual Studio creates a backup of the project to allow you to [roll back to packages.config](#how-to-roll-back-to-packagesconfig) if necessary.

1. Open a solution containing project using `packages.config`.

1. In **Solution Explorer**, right-click on the **References** node or the `packages.config` file and select **Migrate packages.config to PackageReference...**.

1. The migrator analyzes the project's NuGet package references and attempts to categorize them into **Top-level dependencies** (NuGet packages that you installed directory) and **Transitive dependencies** (packages that were installed as dependencies of top-level packages).

   > [!Note]
   > PackageReference supports transitive package restore and resolves dependencies dynamically, meaning that transitive dependencies need not be installed explicitly.

1. (Optional) You may choose to treat a NuGet package classified as a transitive dependency, to be treated as a top-level dependency by selecting the **Top-Level** option for the package.

1. Review any [package compatibility issues](#package-compatibility-issues).

1. Select **OK** to begin the migration.

1. At the end of the migration, Visual Studio provides a report with a path to the backup, the list of installed packages (top-level dependencies), a list of packages referenced as transitive dependencies, and a list of compatibility issues identified at the start of migration. The report is saved to the backup folder.

1. Validate that the solution builds and runs. If you encounter problems, [file an issue on GitHub](https://github.com/NuGet/Home/issues/).

## How to roll back to packages.config

1. Close the migrated project.

1. Copy the project file and `packages.config` from the backup to the project folder, typically `<solution_root>\MigrationBackup\<unique_guid>\<project_name>\`.

1. Open the project.

1. Open the Package Manager Console using the **Tools > NuGet Package Manager > Package Manager Console** menu command.

1. Run the following command in the Console:

   ```ps
   update-package -reinstall
   ```

## Package compatibility issues

Some aspects that were supported in packages.config are not supported in PackageReference. The migrator analyzes and detects such issues. Any package that has one or more of the following issues may not behave as expected after the migration.

### "install.ps1" scripts are ignored when the package is installed after the migration

| | |
| --- | --- |
| **Description** | With PackageReference, install.ps1 and uninstall.ps1 PowerShell scripts are not executed while installing or uninstalling a package. |
| **Potential impact** | Packages that depend on these scripts to configure some behavior in the destination project might not work as expected. |

### "content" assets are not available when the package is installed after the migration

| | |
| --- | --- |
| **Description** | Assets in a package's `content` folder are not supported with PackageReference and are ignored. PackageReference adds support for `contentFiles` to have better transitive support and shared content.  |
| **Potential impact** | Assets in `content` are not copied into the project and project code that depends on the presence of those assets requires refactoring.  |

### XDT transforms are not applied when the package is installed after the upgrade

| | |
| --- | --- |
| **Description** | XDT transforms are not supported with PackageReference and `.xdt` files are ignored when installing or uninstalling a package.   |
| **Potential impact** | XDT transforms are not applied to any project XML files, most commonly, `web.config.install.xdt` and `web.config.uninstall.xdt`, which means the project's` web.config` file is not updated when the package is installed or uninstalled. |

### Assemblies in the lib root are ignored when the package is installed after the migration

| | |
| --- | --- |
| **Description** | With PackageReference, assemblies present at the root of `lib` folder without a target framework specific sub-folder are ignored. NuGet looks for a sub-folder matching the target framework moniker (TFM) corresponding to the project’s target framework and installs the matching assemblies into the project. |
| **Potential impact** | Packages that do not have a sub-folder matching the target framework moniker (TFM) corresponding to the project’s target framework may not behave as expected after the transition or fail installation during the migration |

## Found an issue? Report it!

If you run into a problem with the migration experience, please [file an issue on the NuGet GitHub repository](https://github.com/NuGet/Home/issues/).
