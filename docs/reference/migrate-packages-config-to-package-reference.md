---
title: Migrate from package.config to PackageReference (package references in project files) | Microsoft Docs
author: karann-msft
ms.author: karann-msft
manager: unniravindranathan
ms.date: 03/27/2018
ms.topic: article
ms.prod: nuget
ms.technology: null
description: Details on how to migrate a project from package.config to PackageReference in project files as supported by NuGet 4.0+ and VS2017 and .NET Core 2.0
keywords: NuGet migrator, migrate, package references, project files, PackageReference, packages.config, VS2017, Visual Studio 2017, NuGet 4, .NET Core 2.0
ms.reviewer:
- karann-msft
- unniravindranathan
ms.workload: 
 - "dotnet"
 - "aspnet"
---

# Migrate from packages.config to PackageReference

NuGet 4.x.x. that ships with Visual Studio 2017 Version 15.7 Preview 3 adds support for migrating a project from [packages.config](./packages-config.md) to [package references (PackageReference) in project files](../consume-packages/Package-References-in-Project-Files.md).

## Why migrate?

* **Manage all project dependencies in one place** - Just like project to project references or assembly references, NuGet package references, using the `PackageReference` node, can be managed directly within project files (as opposed to a separate `packages.config` file).
  * **Uncluttered view of top-level dependencies** - Unlike packages.config, PackageReference only lists the NuGet packages you care about i.e. the packages you explicitly installed to the project. It does not clutter the list of installed packages (NuGet package manager UI) or the list of references (project file) with the dependencies of the NuGet package itself. 
  * **Performance improvements** - Solution-local packages folders are no longer used. Packages are resolved against the user’s cache at `%userdata%\.nuget`, rather than a solution specific packages folder. This makes PackageReference perform faster and consume less disk space by using a shared folder of packages on your workstation.
  * **Fine control over dependencies and content flow** - Using the existing features of MSBuild allows you to [conditionally reference a NuGet package](../consume-packages/Package-References-in-Project-Files.md#adding-a-packagereference-condition) and choose package references per target framework, configuration, platform, or other pivots.
  
We are working on further improving PackageReference. [Take a look at this GitHub issue for more details](TBD).

> [!Note]
> * NuGet PackageReference support was added with Visual Studio 2017 and projects that use PackageReference are incomatible with Visual Studio 2015 and older.
> * The option to migrate is currently not available for C++ and ASP.NET project types.
> * Some packages may not be fully compatible with PackageReference based projects. The [package compatibility issues](#package-compatibility-issues) section talks about this in more detail.


## How to migrate?

> [!Note]
> Visual Studio creates a backup of the project when the migration is initiated which allows you to [roll back to packages.config](#steps-to-rollback-to-packagesconfig). 

1. Open a solution containing a `packages.config` based project

2. Right click on the `References` node or the `packages.config` file and select `Migrate packages.config to PackageReference...`

3. The migrator analyzes the NuGet package references and attempts to categorize them into `Top-level dependencies` i.e. NuGet packages that you explicitly installed v/s `Transitive dependencies` i.e. packages that were installed because one of the top-level NuGet package depends on it.

> [!Note]
> PackageReference format supports transitive package restore and dynamically resolves dependencies. This means transitive dependencies do not need to be installed explicitly.

4. Optional - you may choose to treat a NuGet package classified as a transitive dependency, to be treated as a top-level dependency by checking the `Top-Level` check box against the package.

5. Review the package compatibility issues. The next section talks in detail about the types of issues and how they might impact your project.

6. Click `OK` to begin the migration. At the end of the migration, a report is generated that provides a path to the backup, list of packages installed (Top-level dependencies), packages referenced as transitive dependencies, and a list of compatibility issues that were identified at the start of migration. This report is also saved to the backup folder.

7. Validate that the solution builds and runs. [Found an issue?](#found-an-issue-report-it)

## How to rollback to packages.config?

The project file and the packages.config are backed up to `<solution_root>\MigrationBackup\<unique_guid>\<project_name>\`

1. Close the project and copy the project file and packages.config from the backup to the project folder which is usually `<solution_root>\<project_name>\`

2. Open the project and bring up the package manager console from Tools > NuGet Package Manager > Package Manager Console

3. Run the following command from the package manager console
   ```cli
   update-package -reinstall
   ```

## Package compatibility issues

Some aspects that were supported in packages.config are not supported in PackageReference. The migrator analyzes and detects such issues. Any package that has one or more of the following issues may not behave as expected after the migration.

### "install.ps1" script will be ignored when the package is installed after the migration

| | |
| --- | --- |
| **Description** | With PackageReference, install.ps1 and uninstall.ps1 powershell scripts are not executed while installing or uninstalling a package. |
| **Potential impact** | Packages that depend on these scripts to configure some behavior in the destination project might not work as expected. |

### "content" assets will not be available when the package is installed after the migration

| | |
| --- | --- |
| **Description** | “content” assets are not supported with PackageReference and are ignored while installing the package. PackageReference adds support for contentFiles to have better transitive support and shared content.  |
| **Potential impact** | “content” assets are not copied into the project and project code that depends on the presence of those assets will require refactoring.  |
 
### XDT transforms will not be applied when the package is installed after the upgrade

| | |
| --- | --- |
| **Description** | XDT transforms are not supported with PackageReference and .xdt files are ignored while installing or uninstalling a package.   |
| **Potential impact** | XDT transforms will not be applied to any project XML files, most commonly, Web.config.(un)install.xdt, which means the project's web.config file will not be updated when the package is installed or uninstalled. |
 
### "lib\foo.dll" will be ignored when the package is installed after the migration

| | |
| --- | --- |
| **Description** | With PackageReference, assemblies present at the root of lib folder without a target framework specific sub-folder are ignored. NuGet looks for a sub-folder matching the TargetFrameworkMoniker (TFM) corresponding to the project’s target framework and installs the matching assemblies into the project. |
| **Potential impact** | Packages that do not have a sub-folder matching the TargetFrameworkMoniker (TFM) corresponding to the project’s target framework may not behave as expected after the transition or fail installation during the migration |


## Found an issue? Report it!

If you run into a problem with the migration experience, please [file an issue on the NuGet GitHub repository](https://github.com/NuGet/Home/issues/).
