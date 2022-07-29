---
title: Reinstalling and Updating NuGet Packages
description: Details on when it's necessary to reinstall and update packages, as with broken package references in Visual Studio.
author: JonDouglas
ms.author: jodou
ms.date: 12/07/2017
ms.topic: conceptual
---

# How to reinstall and update packages

> [!NOTE]
> The following section applies to [packages.config](../reference/packages-config.md) based projects only. [PackageReference](../consume-packages/Package-References-in-Project-Files.md) projects automatically fix broken references when restore is run.

There are a number of situations, described below under [When to Reinstall a Package](#when-to-reinstall-a-package), where references to a package might get broken within a Visual Studio project. In these cases, uninstalling and then reinstalling the same version of the package will restore those references to working order. Updating a package simply means installing an updated version, which often restores a package to working order.

In Visual Studio, the Package Manager Console provides many flexible options for updating and reinstalling packages.

Updating and reinstalling packages is accomplished as follows:

| Method | Update | Reinstall |
| --- | --- | --- |
| Package Manager console (described in [Using Update-Package](#using-update-package)) | `Update-Package` command | `Update-Package -reinstall` command |
| Package Manager UI | On the **Updates** tab, select one or more packages and select **Update** | On the **Installed** tab, select a package, record its name, then select **Uninstall**. Switch to the **Browse** tab, search for the package name, select it, then select **Install**). |
| nuget.exe CLI | `nuget update` command | For all packages, delete the package folder, then run `nuget install`. For a single package, delete the package folder and use `nuget install <id>` to reinstall the same one. |

> [!NOTE]
> For the dotnet CLI, the equivalent procedure is not required. In a similar scenario, you can [restore packages with the dotnet CLI](package-restore.md#restore-using-the-dotnet-cli).

In this article:

- [When to Reinstall a Package](#when-to-reinstall-a-package)
- [Constraining upgrade versions](#constraining-upgrade-versions)

## When to Reinstall a Package

1. **Broken references after package restore**: If you've opened a project and restored NuGet packages, but still see broken references, try reinstalling each of those packages.
1. **Project is broken due to deleted files**: NuGet does not prevent you from removing items added from packages, so it's easy to inadvertently modify contents installed from a package and break your project. To restore the project, reinstall the affected packages.
1. **Package update broke the project**: If an update to a package breaks a project, the failure is generally caused by a dependency package which may have also been updated. To restore the state of the dependency, reinstall that specific package.
1. **Project retargeting or upgrade**: This can be useful when a project has been retargeted or upgraded and if the package requires reinstallation due to the change in target framework. NuGet shows a build error in such cases immediately after project retargeting, and subsequent build warnings let you know that the package may need to be reinstalled. For project upgrade, NuGet shows an error in the Project Upgrade Log.
1. **Reinstalling a package during its development**: Package authors often need to reinstall the same version of package they're developing to test the behavior. The `Install-Package` command does not provide an option to force a reinstall, so use `Update-Package -reinstall` instead.

## Constraining upgrade versions

By default, reinstalling or updating a package *always* installs the latest version available from the package source.

In projects using the `packages.config` management format, however, you can specifically constrain the version range. For example, if you know that your application works only with version 1.x of a package but not 2.0 and above, perhaps due to a major change in the package API, then you'd want to constrain upgrades to 1.x versions. This prevents accidental updates that would break the application.

To set a constraint, open `packages.config` in a text editor, locate the dependency in question, and add the `allowedVersions` attribute with a version range. For example, to constrain updates to version 1.x, set `allowedVersions` to `[1,2)`:

```xml
<?xml version="1.0" encoding="utf-8"?>
<packages>
    <package id="ExamplePackage" version="1.1.0" allowedVersions="[1,2)" />

    <!-- ... -->
</packages>
```

In all cases, use the notation described in [Package versioning](../concepts/package-versioning.md#version-ranges).

## Using Update-Package

Being mindful of the [Considerations](#considerations) described below, you can easily reinstall any package using the [Update-Package command](../reference/ps-reference/ps-ref-update-package.md) in the Visual Studio Package Manager Console (**Tools** > **NuGet Package Manager** > **Package Manager Console**).

```ps
Update-Package -Id <package_name> –reinstall
```

Using this command is much easier than removing a package and then trying to locate the same package in the NuGet gallery with the same version. Note that the `-Id` switch is optional.

The same command without `-reinstall` updates a package to a newer version, if applicable. The command gives an error if the package in question is not already installed in a project; that is, `Update-Package` does not install packages directly.

```ps
Update-Package <package_name>
```

By default, `Update-Package` affects all projects in a solution. To limit the action to a specific project, use the `-ProjectName` switch, using the name of the project as it appears in Solution Explorer:

```ps
# Reinstall the package in just MyProject
Update-Package <package_name> -ProjectName MyProject -reinstall
```

To *update* all packages in a project (or reinstall using `-reinstall`), use `-ProjectName` without specifying any particular package:

```ps
Update-Package -ProjectName MyProject
```

To update all packages in a solution, just use `Update-Package` by itself with no other arguments or switches. Use this form carefully, because it can take considerable time to perform all the updates:

```ps
# Updates all packages in all projects in the solution
Update-Package 
```

Updating packages in a project or solution using [PackageReference](../Consume-Packages/Package-References-in-Project-Files.md) always updates to the latest version of the package (excluding pre-release packages). Projects that use `packages.config` can, if desired, limit update versions as described below in [Constraining upgrade versions](#constraining-upgrade-versions).

For full details on the command, see the [Update-Package](../reference/ps-reference/ps-ref-update-package.md) reference.

### Considerations

The following may be affected when reinstalling a package:

1. **Reinstalling packages according to project target framework retargeting**
    - In a simple case, just reinstalling a package using `Update-Package –reinstall <package_name>` works. A package that is installed against an old target framework gets uninstalled and the same package gets installed against the current target framework of the project.
    - In some cases, there may be a package that does not support the new target framework.
        - If a package supports portable class libraries (PCLs) and the project is retargeted to a combination of platforms no longer supported by the package, references to the package will be missing after reinstalling.
        - This can surface for packages you're using directly or for packages installed as dependencies. It's possible for the package you're using directly to support the new target framework while its dependency does not.
        - If reinstalling packages after retargeting your application results in build or runtime errors, you may need to revert your target framework or search for alternative packages that properly support your new target framework.

1. **requireReinstallation attribute added in packages.config after project retargeting or upgrade**
    - If NuGet detects that packages were affected by retargeting or upgrading a project, it adds a `requireReinstallation="true"` attribute in  `packages.config` to all affected package references. Because of this, each subsequent build in Visual Studio raises build warnings for those packages so you can remember to reinstall them.

1. **Reinstalling packages with dependencies**
    - `Update-Package –reinstall` reinstalls the same version of the original package, but installs the latest version of dependencies unless specific version constraints are provided. This allows you to update only the dependencies as required to fix an issue. However, if this rolls a dependency back to an earlier version, you can use `Update-Package <dependency_name>` to reinstall that one dependency without affecting the dependent package.
    - `Update-Package –reinstall <packageName> -ignoreDependencies` reinstalls the same version of the original package but does not reinstall dependencies. Use this when updating package dependencies might result in a broken state

1. **Reinstalling packages when dependent versions are involved**
    - As explained above, reinstalling a package does not change versions of any other installed packages that depend on it. It's possible, then, that reinstalling a dependency could break the dependent package.
