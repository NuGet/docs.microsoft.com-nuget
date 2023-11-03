---
title: Reinstall and update NuGet packages in Visual Studio
description: Learn how to reinstall and update NuGet packages to address broken package references and broken projects in Visual Studio.
author: JonDouglas
ms.author: jodou
ms.date: 11/03/2023
ms.topic: conceptual
---

# Reinstall and update NuGet packages in Visual Studio

Sometimes package references can break within a Visual Studio project. Uninstalling and reinstalling the same version of the package often restores the references to working order. Updating a package, which installs an updated version, can also resolve the issue. This article describes how to reinstall and update NuGet packages to address broken package references and broken projects.

> [!NOTE]
> The guidance in this article applies only to projects that use the [packages.config](../reference/packages-config.md) management format. For [PackageReference](../consume-packages/Package-References-in-Project-Files.md) projects, a restore operation automatically fixes broken references.

## Common scenarios

Here are some common scenarios where you might encounter broken package references in your Visual Studio project.

| Scenario | Description | Resolution |
|---|---|---|
| **Broken references after package restore** | You open your Visual Studio project and restore NuGet packages, but broken package references remain. | To fix the references, try reinstalling each package separately. |
| **Broken project due to deleted files** | Deleted (missing) package files cause your project to break. NuGet doesn't prevent deleting items that you add from packages. It can be easy to inadvertently modify contents installed from a package and break your project. | To restore your project, try reinstalling the affected packages. |
| **Broken project after package update** | A package update breaks your project. Companion updates to a dependency package generally cause this type of failure. | To restore the state of the dependency to its previous working order, try reinstalling the specific dependent package. |
| **Broken references after project retarget or upgrade** | A project retarget or upgrade process causes broken package references. After you retarget your project, NuGet shows a build error. Build warnings list packages that might need to be reinstalled. Or, after you upgrade your project, NuGet shows an error in the project upgrade log. The log file lists packages that might need to be reinstalled. | To resolve issues due to a change in target framework, try reinstalling one or more packages. |
| **Package changes under development** | Package authors often need to reinstall the same version of a package they're currently developing to test their changes. | The NuGet Package Manager Console in Visual Studio provides flexible options for updating and reinstalling packages. To reinstall a package under development, you can use the `Update-Package -reinstall` command. |

## Implementation options

You have several choices for how to update and reinstall NuGet packages. Common methods include NuGet Package Manager UI options, the NuGet Package Manager Console, and the NuGet (Command Line Interface) CLI. 

### Package Manager UI

In addition to the Console interface, the Package Manager UI also provides menu options to install, update, and uninstall packages.

- To update a package, open the **Updates** tab, choose one or more packages, then select **Update**.

- To reinstall a package, first uninstall the package and then install it again. Open the **Installed** tab, choose a package and record its name, then select **Uninstall**. Switch to the **Browse** tab and search for the package name, choose the package, then select **Install**.

### Package Manager Console

You can access the Package Manager Console under **Tools** > **NuGet Package Manager** > **Package Manager Console**.

- To update a package, the Package Manager Console provides the `Update-Package` command. 

- To reinstall a package, you can use the same command with the `-reinstall` parameter. This approach is the easiest option, if it's compatible with your configuration.

For more information, see the [Update-Package command](#update-package-command) and [Package reinstall considerations](#package-reinstall-considerations) sections.

### NuGet CLI

The NuGet CLI, `nuget.exe`, is the command-line utility for Windows that provides all NuGet capabilities. 

- To update an installed package, run the `nuget update` command.

- To reinstall all NuGet packages, delete the package folder and then run the `nuget install` command.

- To reinstall a single package, delete the package folder and then run the `nuget install <id>` command, where the `<id>` argument is the ID of the specific package.

> [!NOTE]
> For the **dotnet CLI**, the equivalent procedure isn't required. When you run the `dotnet restore` command, the dotnet CLI uses NuGet to determine dependencies and download any necessary NuGet packages. For more information, see [Restore NuGet packages with the dotnet CLI](package-restore.md#restore-using-the-dotnet-cli).

## Constraints on upgrade versions

By default, reinstalling or updating a package _always_ installs the latest version available from the package source. However, projects that use the `packages.config` management format can specifically limit the allowed NuGet package version range.

Suppose your application works only with version 1.x of a package, but not with version 2.0 or later, due to a major change in the package API. To ensure your application works as expected, you want to constrain NuGet package upgrades to versions 1.x only. The limitation helps to prevent accidental updates that might break your application.

To set a constraint, open the `packages.config` file in a text editor. Locate the dependency that you want to limit, and add the `allowedVersions` attribute with your desired version range.

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

The [Update-Package command](../reference/ps-reference/ps-ref-update-package.md) in the Package Manager Console is the easiest way to reinstall a package and address broken references. However, this approach isn't usable in all scenarios. You can use the command to update an installed package, but not to do an initial install. If you try to update or reinstall a package that isn't already installed in the configuration, the command returns an error. Be sure to review the [Package reinstall considerations](#package-reinstall-considerations) section before working with the command.

Updating packages in a project or solution by using [PackageReference](../Consume-Packages/Package-References-in-Project-Files.md) always updates to the latest version of the package (excluding prerelease packages). Projects that use the `packages.config` management format can limit upgrade versions as described in [Constraints on upgrade versions](#constraints-on-upgrade-versions).

The following sections provide examples for working with the command.

### Reinstall package options

Here's a basic usage of the command to do a reinstall. To identify a specific NuGet package, you can use the optional `-Id` parameter.

```powershell
# Reinstall the package named <package_name>
Update-Package -Id <package_name> –reinstall
```

Using the `Update-Package` command is easier than removing a package and then trying to locate the same package in the NuGet gallery with the same version. 

### Update package options

The same command without the `-reinstall` parameter updates a package to a newer version, if applicable. The command returns an error if the specified package isn't already installed in a project.

```powershell
# Update the package named <package_name>
Update-Package <package_name>
```

### Project and solution options

By default, the `Update-Package` command affects all projects in a solution. To limit the action to a specific project, use the `-ProjectName` parameter. Provide the name of the project as it appears in Visual Studio Solution Explorer.

The following command reinstalls a NuGet package for a specific project in your solution. The name of the specific NuGet package to reinstall is provided in the `<package_name>` parameter.

```powershell
# Reinstall the package named <package_name> in MyProject only
Update-Package <package_name> -ProjectName MyProject -reinstall
```

If you want to reinstall all packages in your project, use the `-ProjectName` parameter by don't specify any particular package. You can follow this same approach to update the packages in your project, as shown in this example:

```powershell
# Update all packages in MyProject only
Update-Package -ProjectName MyProject
```

To update all packages in a solution, just use the `Update-Package` command by itself with no other arguments or parameters.

> [!IMPORTANT]
> Be sure to use the following form of the command carefully. The command process can take considerable time to perform all of the updates.

```powershell
# Update all packages in all projects in the solution
Update-Package 
```

## Package reinstall considerations

If you intend to use the `Update-Package` command to reinstall packages, review the following considerations to ensure compatibility with your configuration scenario.

- Packages and their dependencies might not support a retargeted project target framework.
- When the `requireReinstallation` attribute is set to `true`, Visual Studio issues build warnings for affected packages.
- Reinstall of a package and version constraints can introduce dependency version compatibility issues. 
- Reinstall of a specific package can cause dependent packages to break.

### Package doesn't support project target framework

If you retarget your project target framework, one or more packages might not support the new target configuration.

Usually, reinstalling a package with the `Update-Package –reinstall <package_name>` command works. A package installed against an old target framework is uninstalled and the same package is installed against the new target framework of the project.

In some cases, a package might not support the new target framework. Here are some of the issues you might encounter:

- If a package supports portable class libraries (PCLs), and you retarget the project to a combination of platforms no longer supported by the package, references to the package can be missing after reinstalling.

- This issue can surface for packages you use directly or for packages installed as dependencies. Any package that you use directly might support the new target framework while their dependencies don't.

- If reinstalling packages after retargeting your application results in build or runtime errors, you might need to revert your target framework or search for alternative packages that properly support your new target framework.

### requireReinstallation attribute set to true

After you retarget your project target framework or upgrade NuGet packages, NuGet might add the `requireReinstallation` attribute to the `packages.config` file for your project. If NuGet detects affected packages during the retargeting or upgrading process, it adds a `requireReinstallation="true"` attribute to the `packages.config` file for all affected package references. As a result, each subsequent build of your project in Visual Studio raises build warnings for those packages. The warnings are presented as a reminder to reinstall the affected package.

### Package dependency version incompatibility

The `Update-Package –reinstall` command reinstalls the **same** version of an installed package and the **latest** version of any dependencies. To address version incompatibility issues, you can set version range constraints to control the configuration. NuGet adheres to the constraints and updates the package dependencies to newer versions only as required to fix an issue.

- If your constraint settings cause a dependency to revert to an earlier version during package reinstall, you can address the issue with the `Update-Package <dependency_name>` command. This command reinstalls the specified dependency without affecting the dependent package.

- You can also use the `Update-Package –reinstall <packageName> -ignoreDependencies` command. This option reinstalls the same version of the original package, but it doesn't reinstall dependencies. Use this approach when updating package dependencies might result in a broken configuration state.

### Broken dependent package

When you reinstall a specific package, any installed packages that depend on the reinstalled package aren't updated. The versions of these other installed packages remain the same. As a result, reinstalling a dependency can break a dependent package.

## Related articles

- Review the [packages.config](../reference/packages-config.md) management format.
- Implement [PackageReference](../consume-packages/Package-References-in-Project-Files.md) in project files.
- Use the [Update-Package command](../reference/ps-reference/ps-ref-update-package.md) in the NuGet Package Manager Console in Visual Studio.
- Explore [package versioning](../concepts/package-versioning.md#version-ranges) notation formats.
