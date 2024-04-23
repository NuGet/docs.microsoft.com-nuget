---
title: Migrating from packages.config to PackageReference formats
description: Details on how to migrate a project from the packages.config management format to PackageReference as supported by NuGet 4.0+ and VS2017 and .NET Core 2.0
author: JonDouglas
ms.author: jodou
ms.date: 08/23/2021
ms.topic: conceptual
---

# Migrate from packages.config to PackageReference

Visual Studio 2017 Version 15.7 and later supports migrating a project from the [packages.config](../reference/packages-config.md) management format to the [PackageReference](../consume-packages/Package-References-in-Project-Files.md) format.

## Benefits of using PackageReference

* **Manage all project dependencies in one place**: Just like project to project references and assembly references, NuGet package references (using the `PackageReference` node) are managed directly within project files rather than using a separate packages.config file.
* **Uncluttered view of top-level dependencies**: Unlike packages.config, PackageReference lists only those NuGet packages you directly installed in the project. As a result, the NuGet Package Manager UI and the project file aren't cluttered with down-level dependencies.
* **Performance improvements**: When using PackageReference, packages are maintained in the *global-packages* folder (as described on [Managing the global packages and cache folders](../consume-packages/managing-the-global-packages-and-cache-folders.md) rather than in a `packages` folder within the solution. As a result, PackageReference performs faster and consumes less disk space.
* **Fine control over dependencies and content flow**: Using the existing features of MSBuild allows you to [conditionally reference a NuGet package](../consume-packages/Package-References-in-Project-Files.md#adding-a-packagereference-condition) and choose package references per target framework, configuration, platform, or other pivots.


### Limitations

* NuGet PackageReference is not available in Visual Studio 2015 and earlier. Migrated projects can be opened only in Visual Studio 2017 and later.
* Migration is not currently available for C++ and ASP.NET projects.
* Some packages may not be fully compatible with PackageReference. For more information, see [package compatibility issues](#package-compatibility-issues).

In addition, there are some differences in how PackageReferences work compared to packages.config. For example, [Constraints on upgrade versions](../consume-packages/reinstalling-and-updating-packages.md#constraints-on-upgrade-versions) is not supported by PackageReference, but PackageReference adds support for [Floating Versions](../consume-packages/package-references-in-project-files.md#floating-versions).

### Known Issues

1. The `Migrate packages.config to PackageReference...` option is not available in the right-click context menu 

#### Issue 
 
When a project is first opened, NuGet may not have initialized until a NuGet operation is performed. This causes the migration option to not show up in the right-click context menu on `packages.config` or `References`. 

#### Workaround 

Perform any one of the following NuGet actions: 
* Open the Package Manager UI - Right-click on `References` and select `Manage NuGet Packages...` 
* Open the Package Manager Console - From `Tools > NuGet Package Manager`, select `Package Manager Console` 
* Run NuGet restore - Right-click on the solution node in the Solution Explorer and select `Restore NuGet Packages` 
* Build the project which also triggers NuGet restore 

You should now be able to see the migration option. Note that this option is not supported and will not show up for ASP.NET and C++ project types. 

## Migration steps

> [!Note]
> Before migration begins, Visual Studio creates a backup of the project to allow you to [roll back to packages.config](#how-to-roll-back-to-packagesconfig) if necessary.

1. Open a solution containing project using `packages.config`.

1. In **Solution Explorer**, right-click on the **References** node or the `packages.config` file and select **Migrate packages.config to PackageReference...**.

1. The migrator analyzes the project's NuGet package references and attempts to categorize them into **Top-level dependencies** (NuGet packages that you installed directly) and **Transitive dependencies** (packages that were installed as dependencies of top-level packages).

   > [!Note]
   > PackageReference supports transitive package restore and resolves dependencies dynamically, meaning that transitive dependencies need not be installed explicitly.

1. (Optional) You may choose to treat a NuGet package classified as a transitive dependency as a top-level dependency by selecting the **Top-Level** option for the package. This option is automatically set for packages containing assets that do not flow transitively (those in the `build`, `buildCrossTargeting`, `contentFiles`, or `analyzers` folders) and those marked as a development dependency (`developmentDependency = "true"`).

1. Review any [package compatibility issues](#package-compatibility-issues).

1. Select **OK** to begin the migration.

1. At the end of the migration, Visual Studio provides a report with a path to the backup, the list of installed packages (top-level dependencies), a list of packages referenced as transitive dependencies, and a list of compatibility issues identified at the start of migration. The report is saved to the backup folder.

1. Validate that the solution builds and runs. If you encounter problems, [file an issue on GitHub](https://github.com/NuGet/Home/issues/).

## How to roll back to packages.config

1. Close the migrated project.

1. Copy the project file and `packages.config` from the backup (typically `<solution_root>\MigrationBackup\<unique_guid>\<project_name>\`) to the project folder. Delete the obj folder if it exists in the project root directory.

1. Open the project.

1. Open the Package Manager Console using the **Tools > NuGet Package Manager > Package Manager Console** menu command.

1. Run the following command in the Console:

   ```ps
   update-package -reinstall
   ```

## Create a package after migration

Once the migration is complete, we recommend that you add a reference to the [nuget.build.tasks.pack](https://www.nuget.org/packages/nuget.build.tasks.pack) nuget package, and then use [msbuild -t:pack](../reference/msbuild-targets.md#pack-target) to create the package. Although in some scenarios you could use `dotnet.exe pack` instead of `msbuild -t:pack`, it is not recommended.

## Package compatibility issues

Some aspects that were supported in packages.config are not supported in PackageReference. The migrator analyzes and detects such issues. Any package that has one or more of the following issues may not behave as expected after the migration.

### "install.ps1" scripts are ignored when the package is installed after the migration

* **Description**: With PackageReference, install.ps1 and uninstall.ps1 PowerShell scripts are not executed while installing or uninstalling a package.

* **Potential impact**: Packages that depend on these scripts to configure some behavior in the destination project might not work as expected.

### "content" assets are not available when the package is installed after the migration

* **Description**: Assets in a package's `content` folder are not supported with PackageReference and are ignored. PackageReference adds support for `contentFiles` to have better transitive support and shared content.

* **Potential impact**: Assets in `content` are not copied into the project and project code that depends on the presence of those assets requires refactoring.

### XDT transforms are not applied when the package is installed after the upgrade

* **Description**: XDT transforms are not supported with PackageReference and `.xdt` files are ignored when installing or uninstalling a package.

* **Potential impact**: XDT transforms are not applied to any project XML files, most commonly, `web.config.install.xdt` and `web.config.uninstall.xdt`, which means the project's `web.config` file is not updated when the package is installed or uninstalled.

### Assemblies in the lib root are ignored when the package is installed after the migration

* **Description**: With PackageReference, assemblies present at the root of `lib` folder without a target framework specific sub-folder are ignored. NuGet looks for a sub-folder matching the target framework moniker (TFM) corresponding to the project’s target framework and installs the matching assemblies into the project.

* **Potential impact**: Packages that do not have a sub-folder matching the target framework moniker (TFM) corresponding to the project’s target framework may not behave as expected after the transition or fail installation during the migration.

## Found an issue? Report it!

If you run into a problem with the migration experience, please [file an issue on the NuGet GitHub repository](https://github.com/NuGet/Home/issues/).
