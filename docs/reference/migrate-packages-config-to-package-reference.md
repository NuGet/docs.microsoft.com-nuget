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

Visual Studio 2017 Version 15.7 Preview 3 adds support for migrating a project from packages.config to [package references (PackageReference) in project files](../consume-packages/Package-References-in-Project-Files.md).

> Note
> This option is currently not available for ASP.NET and C++ projects.

## Advantages of PackageReference

* **Manage all project dependencies in one place** - Just like project to project references or assembly references, NuGet package references, using the `PackageReference` node, can be managed directly within project files (as opposed to a separate `packages.config` file).
* **Uncluttered view of top-level dependencies** - Unlike packages.config, PackageReference only lists the NuGet packages you care about i.e. the packages you explicitly installed to the project . It does not clutter the list of installed packages (NuGet package manager UI) or the list of references (project file) with the dependencies of the NuGet package itself. 
* **Performance improvements** - Solution-local packages folders are no longer used. Packages are resolved against the user’s cache at `%userdata%\.nuget`, rather than a solution specific packages folder. This makes PackageReference perform faster and consume less disk space by using a shared folder of packages on your workstation.
* **Fine control over dependencies and content flow** - Using the existing features of MSBuild allows you to [conditionally reference a NuGet package](../consume-packages/Package-References-in-Project-Files.md#adding-a-packagereference-condition) and choose package references per target framework, configuration, platform, or other pivots.

## Steps to migrate

> Note
> The migrator saves a backup of the project file and the packages.config when the migration is initiated should you 

1. Open a solution containing a package.config based project

2. Right click on the `References` node or the `packages.config` file and select `Migrate packages.config to PackageReference...`

3. The migrator analyzes the NuGet package references and attempts to categorize them into `Top-level dependencies` i.e. the NuGet packages that you explicitly installed v/s `Transitive dependencies` i.e. packages that were installed because one of your top-level NuGet package depends on it.

> Note 
> PackageRefence format supports transitive package restore and dynamically resolves dependencies. This means transitive dependecies do not need to be installed explicitly.

4. Optional - you may choose to treat a NuGet package classified as a transitive dependency, to be treated as a top-level dependency by checking the check box against the package under the `Top-Level` column.

5. Review the package compatibiltiy issues. The next section talks in detail about the types of issues and how they might impact your project.


## Package compatibility issues

## Rolling back to packages.config

As part 
