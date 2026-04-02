---
title: Reinstall and Update NuGet Packages in Visual Studio
description: Find out how to reinstall and update NuGet packages to address broken package references and broken projects in Visual Studio.
author: JonDouglas
ms.author: jodou
ms.date: 04/02/2026
ms.topic: how-to
# customer intent: As a developer, I want to find out how to reinstall and update NuGet packages so that I can address broken package references and broken projects in Visual Studio.
---

# Reinstall and update NuGet packages in Visual Studio

Sometimes package references can break within a Visual Studio project. Uninstalling and reinstalling the same version of the package often restores the references to working order. Updating a package, which installs an updated version, can also resolve the issue. This article describes how to reinstall and update NuGet packages to address broken package references and broken projects.

> [!NOTE]
> The guidance in this article applies only to projects that use the [`packages.config`](../reference/packages-config.md) management format. For [`PackageReference`](Package-References-in-Project-Files.md) projects, a restore operation automatically fixes broken references.

## Common scenarios

Here are some common scenarios where you might encounter broken package references in your Visual Studio project.

| Scenario | Description | Resolution |
|---|---|---|
| **Broken references after package restore** | You open your Visual Studio project and restore NuGet packages, but broken package references remain. | To fix the references, try reinstalling each package separately. |
| **Broken project due to deleted files** | Deleted (missing) package files cause your project to break. NuGet doesn't prevent deleting items that you add from packages. It can be easy to inadvertently modify contents installed from a package and break your project. | To restore your project, try reinstalling the affected packages. |
| **Broken project after package update** | A package update breaks your project. Companion updates to a dependency package generally cause this type of failure. | To restore the state of the dependency to its previous working order, try reinstalling the specific dependent package. |
| **Broken references after project retarget or upgrade** | A project retarget or upgrade process causes broken package references. After you retarget your project, NuGet shows a build error. Build warnings list packages that might need to be reinstalled. Or, after you upgrade your project, NuGet shows an error in the project upgrade log. The log file lists packages that might need to be reinstalled. | To resolve issues due to a change in target framework, try reinstalling one or more packages. |
| **Package changes under development** | Package authors often need to reinstall the same version of a package they're currently developing to test their changes. | To reinstall a package under development, use the `Update-Package -reinstall` command in the NuGet Package Manager Console in Visual Studio. The console provides flexible options for updating and reinstalling packages. |

## Implementation options

You have several choices for how to update and reinstall NuGet packages. Common methods include NuGet Package Manager UI options, the NuGet Package Manager Console, and the NuGet command-line interface (CLI).

### Package Manager UI

In addition to the Package Manager Console, the Package Manager UI also provides menu options to install, update, and uninstall packages.

- To update a package in the UI, go to the **Updates** tab, select one or more packages, and then select **Update**.

- To reinstall a package, first uninstall the package and then install it again:
  1. Go to the **Installed** tab, select a package and record its name, and then select **Uninstall**.
  1. Go to the **Browse** tab and search for the package name, select the package, and then select **Install**.

### Package Manager Console

You can access the Package Manager Console under **Tools** > **NuGet Package Manager** > **Package Manager Console**.

- To update a package, the Package Manager Console provides the `Update-Package` command. 

- To reinstall a package, you can use the `Update-Package` command with the `-reinstall` parameter. This approach is the easiest option, if it's compatible with your configuration.

For more information, see the [Update-Package command](#update-package-command) and [Reinstall package considerations](#reinstall-package-considerations) sections.

### NuGet CLI

The NuGet CLI, `nuget.exe`, is the command-line utility for Windows that provides all NuGet capabilities. 

- To update an installed package, run the following command. For `<package-configuration-file-path>`, use the path to your *packages.config* file.

  `nuget update <package-configuration-file-path>`

- To reinstall all NuGet packages, delete the *packages* folder and then run the following command.
  - For `<package-configuration-file-path>`, use the path to your *packages.config* file.
  - For `<packages-folder-path>`, use the path to the *packages* folder.

  `nuget install <package-configuration-file-path> -OutputDirectory <packages-folder-path>`

- To reinstall a single package, go to the *packages* folder and delete the subfolder for that package. Then run the following command.
  - For `<ID>`, use the ID of the package you want to reinstall.
  - For `<packages-folder-path>`, use the path to the *packages* folder.

  `nuget install <ID> -OutputDirectory <packages-folder-path>`

  If you don't use the most recent version of the package, run the following command instead, to specify the version that you use:

  `nuget install <ID> -Version <version> -OutputDirectory <packages-folder-path>`

> [!NOTE]
> This article doesn't provide an equivalent procedure for the **dotnet CLI**, because when you run the `dotnet restore` command, the dotnet CLI uses NuGet to determine dependencies and download any necessary NuGet packages. For more information, see [Restore NuGet packages with the dotnet CLI](package-restore.md#restore-using-the-dotnet-cli).

## Constraints on upgrade versions

By default, reinstalling or updating a package *always* installs the latest version available from the package source. However, projects that use the `packages.config` management format can specifically limit the allowed NuGet package version range.

Suppose your application works only with version 1.x of a package, but not with version 2.0 or later, due to a major change in the package API. To help ensure your application works as expected, you want to constrain NuGet package upgrades to versions 1.x only. The limitation helps to prevent accidental updates that might break your application.

To set a constraint, open the *packages.config* file in a text editor. Locate the dependency that you want to limit, and add the `allowedVersions` attribute with your desired version range.

The following example shows how to constrain updates to version 1.x by setting the `allowedVersions` attribute to `[1,2)`:

```xml
<?xml version="1.0" encoding="utf-8"?>
<packages>
    <package id="ExamplePackage" version="1.1.0" allowedVersions="[1,2)" />

    <!-- ... -->
</packages>
```

In all cases, use the notation described in [Package versioning](../concepts/package-versioning.md#version-ranges).

## Update-Package command

The [Update-Package command](../reference/ps-reference/ps-ref-update-package.md) in the Package Manager Console is the easiest way to reinstall a package and address broken references. However, this approach isn't usable in all scenarios. You can use the command to update an installed package, but not to do an initial install. If you try to update or reinstall a package that isn't already installed in the configuration, the command returns an error. Before working with the command, review the [Reinstall package considerations](#reinstall-package-considerations) section.

For [`PackageReference`](Package-References-in-Project-Files.md) projects and solutions, packages are always updated to the latest version (excluding prerelease packages). In contrast, projects that use the `packages.config` management format can limit upgrade versions as described in [Constraints on upgrade versions](#constraints-on-upgrade-versions).

The following sections provide examples for working with the command.

### Reinstall package options

For a basic reinstallation, use the following command. To identify a specific NuGet package, you can use the optional `-Id` parameter.

```powershell
# Reinstall the package named <package-name>.
Update-Package -Id <package-name> -reinstall
```

Using the `Update-Package` command is easier than removing a package and then trying to locate the package and the correct version in the NuGet gallery.

### Update package options

The same command without the `-reinstall` parameter updates a package to a newer version, if applicable. The command returns an error if the specified package isn't already installed in a project.

```powershell
# Update the package named <package-name>.
Update-Package <package-name>
```

### Project and solution options

By default, the `Update-Package` command affects all projects in a solution. To limit the action to a specific project, use the `-ProjectName` parameter. Provide the name of the project as it appears in Visual Studio Solution Explorer.

The following command reinstalls a NuGet package for a specific project in your solution. Use the `<package-name>` parameter to specify the name of the NuGet package to reinstall.

```powershell
# Reinstall the package named <package-name> in MyProject only.
Update-Package <package-name> -ProjectName MyProject -reinstall
```

If you want to reinstall all packages in your project, use the `-ProjectName` parameter but don't specify any particular package. You can follow this same approach to update the packages in your project, as shown in this example:

```powershell
# Update all packages in MyProject only.
Update-Package -ProjectName MyProject
```

To update all packages in a solution, use the `Update-Package` command by itself with no other arguments or parameters.

> [!IMPORTANT]
> Be sure to use the following form of the command carefully. The command process can take considerable time to perform all the updates.

```powershell
# Update all packages in all projects in the solution.
Update-Package 
```

## Reinstall package considerations

If you intend to use the `Update-Package` command to reinstall packages, review the following considerations to ensure compatibility with your configuration scenario.

- Packages and their dependencies might not support a retargeted project target framework.
- When the `requireReinstallation` attribute is set to `true`, Visual Studio issues build warnings for affected packages.
- Reinstalling a package and version constraints can introduce dependency version compatibility issues.
- Reinstalling a specific package can cause dependent packages to break.

### Package doesn't support project target framework

If you retarget your project target framework, one or more packages might not support the new target configuration.

Usually, you can reinstall a package by using the `Update-Package -reinstall <package-name>` command. A package installed against an old target framework is uninstalled and the same package is installed against the new target framework of the project.

In some cases, a package might not support the new target framework. Here are some of the issues you might encounter:

- If a package supports portable class libraries (PCLs), and you retarget the project to a combination of platforms no longer supported by the package, references to the package can be missing after reinstalling.

- Missing references can occur in packages you use directly or in packages that are installed as dependencies. Any package that you use directly might support the new target framework even if its dependencies don't.

- Reinstalling packages after retargeting your application can result in build or runtime errors. In this case, you might need to revert your target framework or search for alternative packages that properly support your new target framework.

### The requireReinstallation attribute set to true

After you retarget your project target framework or upgrade NuGet packages, NuGet might add the `requireReinstallation` attribute to the *packages.config* file for your project. If NuGet detects affected packages during the retargeting or upgrading process, it adds a `requireReinstallation="true"` attribute to the *packages.config* file for all affected package references. As a result, each subsequent build of your project in Visual Studio raises build warnings for those packages. The warnings are presented as a reminder to reinstall the affected package.

### Package dependency version incompatibility

The `Update-Package -reinstall` command reinstalls the **same** version of an installed package and the **latest** version of any dependencies. To address version incompatibility issues, you can set version range constraints to control the configuration. NuGet adheres to the constraints and updates the package dependencies to newer versions only as required to fix an issue.

- If your constraint settings cause a dependency to revert to an earlier version during a package reinstallation, you can address the issue by using the `Update-Package <dependency-name>` command. This command reinstalls the specified dependency without affecting the dependent package.

- You can also use the `Update-Package -reinstall <package-name> -ignoreDependencies` command. This option reinstalls the same version of the original package, but it doesn't reinstall dependencies. Using this approach when updating package dependencies might result in a broken configuration state.

### Broken dependent package

When you reinstall a specific package, any installed packages that depend on the reinstalled package aren't updated. The versions of these other installed packages remain the same. As a result, reinstalling a dependency can break a dependent package.

## Related content

- Review the [`packages.config`](../reference/packages-config.md) management format.
- Implement [`PackageReference`](Package-References-in-Project-Files.md) in project files.
- Use the [Update-Package command](../reference/ps-reference/ps-ref-update-package.md) in the NuGet Package Manager Console in Visual Studio.
- Explore [package versioning](../concepts/package-versioning.md#version-ranges) notation formats.
