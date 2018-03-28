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

NuGet 4.x.x. that ships with Visual Studio 2017 Version 15.7 Preview 3 adds support for migrating a project from packages.config to [package references (PackageReference) in project files](../consume-packages/Package-References-in-Project-Files.md).

## Who should migrate?

* Visual Studio 2017 - The PackageReference feature is only supported in Visual Studio 2017. Projects that use PackageReference cannot be opened with Visual Studio 2015 or older versions.

* C++ and ASP.NET projects - The option to migrate is currently not available for C++ and ASP.NET project types.

* Known package compatibility issues - Some packages may not be fully incompatible with PackageReference based projects. The [package compatibility issues](#package-compatibility-issues) section talks about this in more detail.

* Leverage the advantages of PackageReference
  * **Manage all project dependencies in one place** - Just like project to project references or assembly references, NuGet package references, using the `PackageReference` node, can be managed directly within project files (as opposed to a separate `packages.config` file).
  * **Uncluttered view of top-level dependencies** - Unlike packages.config, PackageReference only lists the NuGet packages you care about i.e. the packages you explicitly installed to the project . It does not clutter the list of installed packages (NuGet package manager UI) or the list of references (project file) with the dependencies of the NuGet package itself. 
  * **Performance improvements** - Solution-local packages folders are no longer used. Packages are resolved against the user’s cache at `%userdata%\.nuget`, rather than a solution specific packages folder. This makes PackageReference perform faster and consume less disk space by using a shared folder of packages on your workstation.
  * **Fine control over dependencies and content flow** - Using the existing features of MSBuild allows you to [conditionally reference a NuGet package](../consume-packages/Package-References-in-Project-Files.md#adding-a-packagereference-condition) and choose package references per target framework, configuration, platform, or other pivots.

## Steps to migrate

> [!Note]
> Visual Studio creates a backup of the project when the migration is initiated which allows you to [roll back to packages.config](#Steps-to-rollback-to-packages-config). 

1. Open a solution containing a `packages.config` based project

2. Right click on the `References` node or the `packages.config` file and select `Migrate packages.config to PackageReference...`

3. The migrator analyzes the NuGet package references and attempts to categorize them into `Top-level dependencies` i.e. NuGet packages that you explicitly installed v/s `Transitive dependencies` i.e. packages that were installed because one of the top-level NuGet package depends on it.

> [!Note]
> PackageRefence format supports transitive package restore and dynamically resolves dependencies. This means transitive dependecies do not need to be installed explicitly.

4. Optional - you may choose to treat a NuGet package classified as a transitive dependency, to be treated as a top-level dependency by checking the `Top-Level` check box against the package.

5. Review the package compatibiltiy issues. The next section talks in detail about the types of issues and how they might impact your project.

6. Click `OK` to begin the migration. At the end of the migration, a report is generated that provides a path to the backup, list of packages installed (Top-level dependencies), packages referenced as transitive dependencies, and a list of compatibility issues that were identified at the start of migration. This report is also saved to the backup folder.

7. Validate that the solution builds and runs. [Found an issue?]()

## Steps to rollback to packages.config

The project file and the packages.config are backedup to `<solution_root>\MigrationBackup\<unique_guid>\<project_name>\`

1. Close the project and copy the project file and packages.config from the backup to the project folder which is usually `<solution_root>\<project_name>\`

2. Open the project and bring up the package mananger console from Tools > NuGet Package Manager > Package Manager Console

3. Run the following command from the package manager console
   ```cli
   update-package -reinstall
   ```

## Package compatibility issues

Some aspects that were supported in packages.config are not supported in PackageReference. The migrator analyzes and detects such issues.

### "install.ps1" script will be ignored when the package is installed after the migration

| | |
| --- | --- |
| **Issue** | PackageReference does not support install.ps1 scripts because they can potentially do bad things **TBD** |
| **Potential impact** | The project might not build correctly |
| **Workaround** | **TBD** |

### "content" assets will not be available when the package is installed after the migration

| | |
| --- | --- |
| **Issue** | PackageReference support contentFiles but not content **TBD** |
| **Potential impact** | The project might not build correctly |
| **Workaround** | **TBD** |
 
### XDT transforms will not be applied when the package is installed after the upgrade

| | |
| --- | --- |
| **Issue** | PackageReference does not support XDT transforms **TBD** |
| **Potential impact** | The project might not build correctly |
| **Workaround** | **TBD** |
 
### "lib\foo.dll" will be ignored when the package is installed after the migration

| | |
| --- | --- |
| **Issue** | PackageReference does not support .dll files at the root of the lib folder **TBD** |
| **Potential impact** | The project might not build correctly |
| **Workaround** | **TBD** |


## Found an issue? Report it!

If you run into a problem with the migration experience, please [file an issue on the NuGet GitHub repository](https://github.com/NuGet/Home/issues/).
