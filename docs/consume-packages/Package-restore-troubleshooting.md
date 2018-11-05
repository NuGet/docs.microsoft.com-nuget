---
title: Troubleshooting NuGet Package Restore in Visual Studio
description: A description of common NuGet restore errors in Visual Studio and how to troubleshoot them.
author: karann-msft
ms.author: karann
ms.date: 05/25/2018
ms.topic: conceptual
---

# Troubleshooting package restore errors

This article focuses on common errors when restoring packages and steps to resolve them. For complete details on restoring packages, see [Package restore](../consume-packages/package-restore.md#enabling-and-disabling-package-restore).

If the instructions here do not work for you, [please file an issue on GitHub](https://github.com/NuGet/docs.microsoft.com-nuget/issues) so that we can examine your scenario more carefully. Do not use the "Is this page helpful?" control that may appear on this page because it doesn't give us the ability to contact you for more information.

## Quick solution for Visual Studio users

If you're using Visual Studio, first enable package restore as follows. Otherwise continue to the sections that follow.

1. Select the **Tools > NuGet Package Manager > Package Manager Settings** menu command.
1. Set both options under **Package Restore**.
1. Select **OK**.
1. Build your project again.

![Enable NuGet package restore in Tool/Options](../consume-packages/media/restore-01-autorestoreoptions.png)

These settings can also be changed in your `NuGet.config` file; see the [consent](#consent) section.

<a name="missing"></a>

## This project references NuGet package(s) that are missing on this computer

Complete error message:

```output
This project references NuGet package(s) that are missing on this computer.
Use NuGet Package Restore to download them. The missing file is {name}.
```

This error occurs when you attempt to build a project that contains references to one or more NuGet packages, but those packages are not presently installed on the computer or in the project.

- When using the PackageReference management format, the error means that the package is not installed in the *global-packages* folder as described on [Managing the global packages and cache folders](managing-the-global-packages-and-cache-folders.md).
- When using `packages.config`, the error means that the package is not installed in the `packages` folder at the solution root.

This situation commonly occurs when you obtain the project's source code from source control or another download. Packages are typically omitted from source control or downloads because they can be restored from package feeds like nuget.org (see [Packages and source control](Packages-and-Source-Control.md)). Including them would otherwise bloat the repository or create unnecessarily large .zip files.

The error can also happen if your project file contains absolute paths to package locations, and you move the project.

Use one of the following methods to restore the packages:

- If you've moved the project file, edit the file directly to update the package references.
- In Visual Studio, enable package restore by selecting the **Tools > NuGet Package Manager > Package Manager Settings** menu command, setting both options under **Package Restore**, and selecting **OK**. Then build the solution again.
- For .NET Core projects, run `dotnet restore` or `dotnet build` (which automatically runs restore).
- On the command line, run `nuget restore` (except for projects created with `dotnet`, in which case use `dotnet restore`).
- On the command line with projects using the PackageReference format, run `msbuild /t:restore`.

After a successful restore, the package should be present in the *global-packages* folder. For projects using PackageReference, a restore should recreate the `obj/project.assets.json` file; for projects using `packages.config`, the package should appear in the project's `packages` folder. The project should now build successfully. If not, [file an issue on GitHub](https://github.com/NuGet/docs.microsoft.com-nuget/issues) so we can follow up with you.

<a name="assets"></a>

## Assets file project.assets.json not found

Complete error message:

```output
Assets file '<path>\project.assets.json' not found. Run a NuGet package restore to generate this file.
```

The `project.assets.json` file maintains a project's dependency graph when using the PackageReference management format, which is used to make sure that all necessary packages are installed on the computer. Because this file is generated dynamically through package restore, it's typically not added to source control. As a result, this error occurs when building a project with a tool such as `msbuild` that does not automatically restore packages.

In this case, run `msbuild /t:restore` followed by `msbuild`, or use `dotnet build` (which restores packages automatically). You can also use any of the package restore methods in the [previous section](#missing).

<a name="consent"></a>

## One or more NuGet packages need to be restored but couldn't be because consent has not been granted

Complete error message:

```output
One or more NuGet packages need to be restored but couldn't be because consent has
not been granted. To give consent, open the Visual Studio Options dialog, click on
the NuGet Package Manager node and check 'Allow NuGet to download missing packages
during build.' You can also give consent by setting the environment variable
'EnableNuGetPackageRestore' to 'true'. Missing packages: {name}
```

This error indicates that package restore is disabled in your NuGet configuration.

You can change the applicable settings in Visual Studio as described earlier under [Quick solution for Visual Studio users](#quick-solution-for-visual-studio-users).

You can also edit these settings directly in the applicable `nuget.config` file (typically `%AppData%\NuGet\NuGet.Config` on Windows and `~/.nuget/NuGet/NuGet.Config` on Mac/Linux). Make sure the `enabled` and `automatic` keys under `packageRestore` are set to True:

```xml
<!-- Package restore is enabled -->
<configuration>
    <packageRestore>
        <add key="enabled" value="True" />
        <add key="automatic" value="True" />
    </packageRestore>
</configuration>
```

> [!Important]
> If you edit the `packageRestore` settings directly in `nuget.config`, restart Visual Studio so that the options dialog box shows the current values.

## Other potential conditions

- You may encounter build errors due to missing files, with a message saying to use NuGet restore to download them. However, running a restore might say, "All packages are already installed and there is nothing to restore." In this case, delete the `packages` folder (when using `packages.config`) or the `obj/project.assets.json` file (when using PackageReference) and run restore again. If the error still persists, use `nuget locals all -clear` or `dotnet locals all --clear` from the command line to clear the *global-packages* and cache folders as described on [Managing the global packages and cache folders](managing-the-global-packages-and-cache-folders.md).

- When obtaining a project from source control, your project folders may be set to read-only. Change the folder permissions and try restoring packages again.

- You may be using an old version of NuGet. Check [nuget.org/downloads](https://www.nuget.org/downloads) for the latest recommended versions. For Visual Studio 2015, we recommend 3.6.0.

If you encounter other problems, [file an issue on GitHub](https://github.com/NuGet/docs.microsoft.com-nuget/issues) so we can get more details from you.
