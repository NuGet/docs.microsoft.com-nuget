---
title: Manage NuGet packages with the Visual Studio Package Manager Console
description: See how to work with NuGet packages by using PowerShell commands in the Visual Studio Package Manager Console.
author: JonDouglas
ms.author: jodou
ms.date: 08/17/2020
ms.topic: conceptual
f1_keywords: 
  - "vs.nuget.packagemanager.console"
---

# Manage packages with the Visual Studio Package Manager Console (PowerShell)

The Package Manager Console in Visual Studio uses PowerShell commands to interact with NuGet packages. You can use the console when there's no way to do an operation through the [Package Manager UI](install-use-packages-visual-studio.md). You can also use [dotnet CLI](../reference/dotnet-commands.md) or [NuGet CLI](#use-the-nugetexe-cli-in-the-console) commands in the console.

This article describes how to find, install, update, and uninstall NuGet packages with PowerShell commands in the Package Manager Console. For the complete Package Manager Console PowerShell command reference, see [PowerShell reference](../reference/powershell-reference.md).

> [!IMPORTANT]
> The PowerShell commands and arguments in this article are specific to the Visual Studio Package Manager Console. These commands differ from the [PackageManagement module commands](/powershell/module/packagemanagement) you can use in a general PowerShell environment. Each environment has commands that aren't available in the other, and commands with the same name might differ in their specific arguments.

## Console availability

Starting in Visual Studio 2017, NuGet and the NuGet Package Manager install automatically when you create any .NET-related workloads in Visual Studio. You can also install the Package Manager by selecting **Individual components** > **Code tools** > **NuGet package manager** in the Visual Studio Installer. 

You can also search for the NuGet Package Manager extension under the **Tools** > **Extensions and Updates** or **Extensions** menus. If you're unable to use the extensions installer in Visual Studio, you can download the extension directly from [https://dist.nuget.org/index.html](https://dist.nuget.org/index.html).

The Package Manager Console is built into the Package Manager for Visual Studio on Windows. Visual Studio Code and Visual Studio for Mac don't include the console. Visual Studio for Mac has a UI for managing NuGet packages, and the equivalent console commands are available through the [NuGet CLI](../reference/nuget-exe-CLI-reference.md). For more information, see [Install and manage NuGet packages in Visual Studio for Mac](/visualstudio/mac/nuget-walkthrough).

## Quickly find and install a package

To use the Package Manager Console to quickly find and install a package:

1. Open your project or solution in Visual Studio, and select **Tools** > **NuGet Package Manager** > **Package Manager Console** to open the Package Manager Console window.

1. In the console, enter `Find-Package` with a keyword to find the package you want to install. For example, to find packages that contain the keyword `elmah`, run the following command. If you already know the package name you want, skip this step.

    ```powershell
    Find-Package elmah
    ```

1. Use the `Install-Package` command to install the package. For example, to install the `Elmah.MVC` package, enter:

    ```powershell
    Install-Package Elmah.MVC
    ```

For more details about these commands, see the [Find a package](#find-a-package) and [Install a package](#install-a-package) sections.

> [!Tip]
> Many console operations depend on having a solution with a known path name open in Visual Studio. If you have an unsaved solution, or no solution, you see the error **Solution is not opened or not saved. Please ensure you have an open and saved solution.** To correct the error, create and save a solution, or save an unsaved solution.

## Console controls

To open the Package Manager Console in Visual Studio, select **Tools** > **NuGet Package Manager** > **Package Manager Console** from the top menu. The console is a Visual Studio window that you can arrange and position as you like. For more information, see [Customize window layouts in Visual Studio](/visualstudio/ide/customizing-window-layouts-in-visual-studio).

By default, console commands operate against the specific package source and project shown in the controls at the top of the window:

:::image type="content" source="media/PackageManagerConsoleControls1.png" alt-text="Screenshot that shows the Package Manager Console controls for package source and project.":::

Selecting a different package source or project changes the defaults for subsequent commands. To override these settings for single commands without changing the defaults, most console commands support `-Source` and `-ProjectName` options.

To manage package sources, select the gear icon, which opens the **Tools** > **Options** > **NuGet Package Manager** > **Package Sources** dialog box. The control next to the project selector clears the console's contents.

:::image type="content" source="media/PackageManagerConsoleControls2.png" alt-text="Screenshot that shows the Package Manager Console settings and clear controls.":::

The button on the far right interrupts a long-running command. For example, running `Get-Package -ListAvailable -PageSize 500` lists the top 500 available packages on the default source, such as nuget.org, which could take several minutes.

:::image type="content" source="media/PackageManagerConsoleControls3.png" alt-text="Screenshot that shows the Package Manager Console stop control.":::

## Find a package

To find a package in the default source, use [Find-Package](../reference/ps-reference/ps-ref-find-package.md).

- To find and list packages that contain certain keywords:

  ```powershell
  Find-Package <keyword1>
  Find-Package <keyword2>
  ```

- To find and list packages whose name begins with a string:

  ```powershell
  Find-Package <string> -StartWith
  ```

- By default, `Get-Package` returns a list of 20 packages. Use `-First` to show more packages. For example, to show the first 100 packages, use:

  ```powershell
  Find-Package <keyword> -First 100
  ```

- To list all versions of a certain package:

  ```powershell
  Find-Package <PackageName> -AllVersions -ExactMatch
  ```

## Install a package

To install a package into the default project, use `Install-Package <PackageName>`. The [Install-Package](../reference/ps-reference/ps-ref-install-package.md) console command takes the following actions:

- Does the steps in [What happens when a NuGet package is installed](../concepts/package-installation-process.md).
- Displays applicable license terms in the console window with implied agreement. If you don't agree to the terms, you should uninstall the package.
- Adds a reference to the package in the project file and in **Solution Explorer** under the **References** node. You must save the project before you can see the changes in the project file.

By default, `Install-Package` adds the package to the default project the console window specifies. To add the package to a project that isn't the default, use the `-ProjectName` option. For example, to add the `Elmah.MVC` package to the non-default `UtilitiesLib` project, run the following command:

```powershell
Install-Package Elmah.MVC -ProjectName UtilitiesLib
```

## Uninstall a package

To uninstall a package from the default project, use `Uninstall-Package <PackageName>`. If you need to find the package name, use [Get-Package](../reference/ps-reference/ps-ref-get-package.md) to see all packages installed in the default project.

[Uninstall-Package](../reference/ps-reference/ps-ref-uninstall-package.md) takes the following actions:

- Removes references to the package from the project and any management formats. References no longer appear in **Solution Explorer**. You might need to rebuild the project to remove the reference in the *bin* folder.
- Reverses any changes that installing the package made to *app.config* or *web.config*.
- Removes previously-installed dependencies if no remaining packages use those dependencies.

To uninstall a package and all its unused dependencies, run:

```powershell
Uninstall-Package <PackageName> -RemoveDependencies
```

To uninstall a package even if other packages depend on it, run:

```powershell
Uninstall-Package <PackageName> -Force
```

## Update a package

To update a package, use [Get-Package](../reference/ps-reference/ps-ref-get-package.md) and [Update-Package](../reference/ps-reference/ps-ref-update-package.md). You can run the following commands:

- To check if there are newer versions available for any installed packages:

  ```powershell
  Get-Package -updates
  ```

- To update a specific package:

  ```powershell
  Update-Package <PackageName>
  ```

- To update all packages in a project:

  ```powershell
  Update-Package -ProjectName <ProjectName>
  ```

- To update all packages in the solution:

  ```powershell
  Update-Package
  ```

<a name="use-the-nugetexe-cli-in-the-console"></a>
## Use the NuGet CLI in the console

You can also do all operations that are available in the console with the [NuGet CLI](../reference/nuget-exe-cli-reference.md). However, console commands operate within the context of Visual Studio saved project and solution, and often do more than their equivalent NuGet CLI commands. For example, installing a package through the console adds a reference to the project file, but the NuGet CLI command doesn't. For this reason, developers working in Visual Studio typically prefer to use the console commands rather than the NuGet CLI.

To use NuGet CLI commands in the Package Manager Console, install the [NuGet.CommandLine](https://www.nuget.org/packages/NuGet.CommandLine) package.

```powershell
Install-Package NuGet.CommandLine
```

The preceding command installs the latest version of the NuGet CLI. To install a specific version, use the `-Version` option. For example, to install Version 4.4.1, enter:

```powershell
Install-Package NuGet.CommandLine -Version 4.4.1
```

After you install the `NuGet.CommandLine` package, you can run all NuGet CLI commands through the Package Manager Console.

## Extend the Package Manager Console

Some packages install new commands for the console. For example, `MvcScaffolding` creates commands like `Scaffold`, which generates ASP.NET MVC controllers and views:

:::image type="content" source="media/PackageManagerConsoleInstall.png" alt-text="Screenshot that shows NuGet CLI commands available after installing the NuGet.CommandLine package.":::

## Set up a NuGet PowerShell profile

You can create a PowerShell profile to make your commonly-used commands available in all PowerShell contexts, so you don't lose your PowerShell settings between sessions. NuGet supports a NuGet-specific profile, usually at *%UserProfile%\Documents\WindowsPowerShell\NuGet_profile.ps1*.

To find your user profile location, enter `$profile` in the console:

```powershell
$profile
C:\Users\<user>\Documents\WindowsPowerShell\NuGet_profile.ps1
```

To determine whether a profile exists at that location, enter `test-path $profile`. If the command returns `False`, you need to create the profile with the specified name at that location. For more information, see [Windows PowerShell Profiles](/previous-versions//bb613488(v=vs.85)).

## Next steps

- [Install and manage NuGet packages with the dotnet CLI](install-use-packages-dotnet-cli.md)
- [Manage packages using the nuget.exe CLI](install-use-packages-nuget-cli.md)
- [Install and manage packages in Visual Studio using the NuGet Package Manager](install-use-packages-visual-studio.md)
