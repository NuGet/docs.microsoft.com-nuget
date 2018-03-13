---
title: Troubleshooting NuGet Package Restore in Visual Studio | Microsoft Docs
author: kraigb
ms.author: kraigb
manager: ghogen
ms.date: 03/13/2018
ms.topic: article
ms.prod: nuget
ms.technology: null
description: A description of common NuGet restore errors in Visual Studio and how to troubleshoot them.
keywords: NuGet package restore, restoring packages, troubleshooting, troubleshoot
ms.reviewer:
- karann-msft
- unniravindranathan
---

# Troubleshooting package restore errors

This article focuses on common errors when restoring packages and steps to resolve them. For complete details on restoring packages, see [Package restore](../consume-packages/package-restore.md#enabling-and-disabling-package-restore).

If the instructions here do not work for you, [please file an issue on GitHub](https://github.com/NuGet/docs.microsoft.com-nuget/issues) so that we can examine your scenario more carefully. Do not use the "Is this page helpful?" control shown on this page because it doesn't give us the ability to contact you for more information.

<a name="missing"></a>

## This project references NuGet package(s) that are missing on this computer

Complete error message:

```output
This project references NuGet package(s) that are missing on this computer.
Use NuGet Package Restore to download them. The missing file is {name}.
```

This error occurs you attempt to build a project that contains references to one or more NuGet packages, but those packages are not presently cached in the project. (Packages are cached in a `packages` folder at the solution root if the project uses `packages.config`, or in the `obj/project.assets.json` file if the project uses the PackageReference format.)

This situation commonly occurs when you obtain the project's source code from source control or another download. Packages are typically omitted from source control or downloads because they can be restored from package feeds like nuget.org (see [Packages and source control](Packages-and-Source-Control.md)). Including them would otherwise bloat the repository or create unnecessarily large .zip files.

Use one of the following methods to restore the packages:

- For .NET Core projects, run `dotnet restore` or `dotnet build` (which automatically runs restore).
- In Visual Studio, right-click the solution and select **Restore NuGet packages**, or right-click the project and select **Build**. If package restore isn't enabled, however, you'll see the error described in the [consent](#consent) section below.
- On the command line, run `nuget restore` (except for projects created with `dotnet`, in which case use `dotnet restore`).
- On the command line with projects using the PackageReference format, run `msbuild /t:restore`.

After a successful restore, you should see either a `packages` folder (when using `packages.config`) or the `obj/project.assets.json` file (when using PackageReference). The project should now build successfully. If not, [file an issue on GitHub](https://github.com/NuGet/docs.microsoft.com-nuget/issues) so we can follow up with you.

<a name="assets"></a>

## Assets file project.assets.json not found

Complete error message:

```output
Assets file '<path>\project.assets.json' not found. Run a NuGet package restore to generate this file.
```

This error occurs for the same reasons as explained in the [previous section](#missing), and has the same remedies. For example, running `msbuild` on a .NET Core project that's been obtained from source control won't automatically restore packages. In this case, run `msbuild /t:restore` followed by `msbuild`, or use `dotnet build` (which restores packages automatically).

<a name="consent"></a>

## One or more NuGet packages need to be restored by couldn't be because consent has not been granted

Complete error message:

```output
One or more NuGet packages need to be restored but couldn't be because consent has
not been granted. To give consent, open the Visual Studio Options dialog, click on
the NuGet Package Manager node and check 'Allow NuGet to download missing packages
during build.' You can also give consent by setting the environment variable
'EnableNuGetPackageRestore' to 'true'. Missing packages: {name}
```

This error indicates that package restore is disabled in your NuGet configuration, that is, the applicable `nuget.config` file (typically `%AppData%\NuGet\NuGet.Config` on Windows and `~/.nuget/NuGet/NuGet.Config` on Mac/Linux) contains the following:

```xml
<!-- Package restore is disabled when these settings are false -->
<configuration>
    <packageRestore>
        <add key="enabled" value="False" />
        <add key="automatic" value="False" />
    </packageRestore>
</configuration>
```

To enable package restore and automatic restore on build, set both keys to true:

```xml
<!-- Package restore is enabled -->
<configuration>
    <packageRestore>
        <add key="enabled" value="True" />
        <add key="automatic" value="True" />
    </packageRestore>
</configuration>
```

You can edit these settings directly, or by opening **Tools > Options > NuGet Package Manager** in Visual Studio and setting the options for **Allow NuGet to download missing packages** and **Automatically check for missing packages during build in Visual Studio** (shown below). To enable restore, make sure both options are selected, then try restoring packages again or running a build.

![Enable NuGet package restore in Tool/Options](../consume-packages/media/restore-01-autorestoreoptions.png)

> [!Warning]
> If you edit the `packageRestore` settings directly in `nuget.config`, restart Visual Studio so that the options dialog box shows the current values.

## Other potential conditions

- You may encounter build errors due to missing files, with a message saying to use NuGet restore to download them. However, running a restore might say, "All packages are already installed and there is nothing to restore." In this case, delete the `packages` folder (when using `packages.config`) or the `obj/project.assets.json` file (when using PackageReference) and run restore again.

- When obtaining a project from source control, your project folders may be set to read-only. Change the folder permissions and try restoring packages again.

- You may be using an old version of NuGet. Check [nuget.org/downloads](https://www.nuget.org/downloads) for the latest recommended versions. For Visual Studio 2015, we recommend 3.6.0.

If you encounter other problems, [file an issue on GitHub](https://github.com/NuGet/docs.microsoft.com-nuget/issues) so we can get more details from you.