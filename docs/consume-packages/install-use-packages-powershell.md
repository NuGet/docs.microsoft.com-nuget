---
title: Manage NuGet Packages with the Visual Studio Package Manager Console
description: Find out how to use PowerShell commands in the Visual Studio Package Manager Console so you can use and manage NuGet packages in your project or solution.
author: JonDouglas
ms.author: jodou
ms.date: 03/11/2026
ms.topic: how-to
f1_keywords: 
  - "vs.nuget.packagemanager.console"

# customer intent: As a developer, I want to find out how to use PowerShell commands in the Visual Studio Package Manager Console so that I can use and manage NuGet packages in my project or solution.
---

# Manage packages with the Visual Studio Package Manager Console (PowerShell)

The Package Manager Console in Visual Studio uses PowerShell commands to interact with NuGet packages. You can use the console when there's no way to do an operation through the [Package Manager UI](install-use-packages-visual-studio.md). You can also use [dotnet command-line interface (CLI)](../reference/dotnet-commands.md) or [NuGet CLI](#use-the-nugetexe-cli-in-the-console) commands in the console.

This article describes how to find, install, update, and uninstall NuGet packages by using PowerShell commands in the Package Manager Console. For the complete Package Manager Console PowerShell command reference, see [PowerShell reference](../reference/powershell-reference.md).

> [!IMPORTANT]
> The PowerShell commands and arguments in this article are specific to the Visual Studio Package Manager Console. These commands differ from the [PackageManagement module commands](/powershell/module/packagemanagement) you can use in a general PowerShell environment. Each environment has commands that aren't available in the other, and commands with the same name might differ in their specific arguments.

## Console availability

Starting in Visual Studio 2017, NuGet and the NuGet Package Manager install automatically when you create any .NET-related workloads in Visual Studio. You can also install the Package Manager by using the Visual Studio Installer. In the installer, select **Individual components** > **Code tools** > **NuGet package manager**.

You can also search for the NuGet Package Manager extension in Visual Studio under the **Tools** > **Extensions and Updates** or **Extensions** menu. If you're unable to use the extensions installer in Visual Studio, you can download the extension directly from [https://www.nuget.org/downloads](https://www.nuget.org/downloads).

The Package Manager Console is built into the Package Manager for Visual Studio on Windows. Visual Studio Code doesn't include the console, but you can use the [C# Dev Kit](https://marketplace.visualstudio.com/items?itemName=ms-dotnettools.csdevkit) extension to manage NuGet packages. With this extension, you can run commands in the C# Dev Kit solution explorer or in the Visual Studio Code Command Palette. For more information, see [NuGet in Visual Studio Code](https://code.visualstudio.com/docs/csharp/package-management).

## Quickly find and install a package

To use the Package Manager Console to quickly find and install a package, take the following steps:

1. Open your project or solution in Visual Studio, and select **Tools** > **NuGet Package Manager** > **Package Manager Console** to open the Package Manager Console window.

1. In the console, run the `Find-Package` command with a keyword to find the package you want to install. For example, to find packages that provide error logging modules and handlers (ELMAH), run the following command to search for the keyword `elmah`. If you already know the package name you want, skip this step.

    ```powershell
    Find-Package elmah
    ```

1. After you find the package, run the `Install-Package` command with the package ID to install the package. For example, to install the `Elmah.MVC` package, run the following command:

    ```powershell
    Install-Package Elmah.MVC
    ```

For more details about these commands, see the [Find a package](#find-a-package) and [Install a package](#install-a-package) sections.

> [!TIP]
> Many console operations depend on having a solution with a known path name open in Visual Studio. If you have an unsaved solution or no solution, an error message appears about not having an open or saved solution. To correct the error, create and save a solution, or save an unsaved solution.

## Use console controls

To open the Package Manager Console in Visual Studio, go to the top menu, and then select **Tools** > **NuGet Package Manager** > **Package Manager Console**. The console is a Visual Studio window that you can arrange and position as you like. For more information, see [Customize window layouts and personalize tabs](/visualstudio/ide/customizing-window-layouts-in-visual-studio).

By default, console commands operate against the specific package source and project shown in the controls at the top of the Package Manager Console window:

:::image type="content" source="media/package-manager-console-package-source-default-project.png" alt-text="Screenshot of the Package Manager Console window. At the top, the Package source and Default project lists are highlighted.":::

Selecting a different package source or project changes the defaults for subsequent commands. To override these settings for single commands without changing the defaults, most console commands support `-Source` and `-ProjectName` options.

To manage package sources, select the gear icon, which opens the **Tools** > **Options** > **NuGet Package Manager** > **Package Sources** dialog. To clear the console's contents, select **Clear Console** :::image type="icon" source="media/package-manager-console-clear-console-icon.png":::, next to the **Default project** list.

:::image type="content" source="media/package-manager-console-settings-clear-console.png" alt-text="Screenshot of the Package Manager Console window. Near the top, two icons are highlighted. One shows a gear. The other shows output lines and a red X.":::

To interrupt a long-running command, select **Stop command execution** :::image type="icon" source="media/package-manager-console-stop-command-execution-icon.png":::, next to the **Clear Console** icon. For example, running `Get-Package -ListAvailable -PageSize 500` lists the top 500 available packages on the default source, such as nuget.org, which can take several minutes.

:::image type="content" source="media/package-manager-console-stop-command-execution.png" alt-text="Screenshot of the Package Manager Console window. Near the top, the Stop command execution icon, which looks like a red square, is highlighted." lightbox="media/package-manager-console-stop-command-execution.png":::

## Find a package

To find a package in the default source, use [Find-Package](../reference/ps-reference/ps-ref-find-package.md). The following code blocks show you how to use parameters to refine your search:

- Find and list packages that contain a certain keyword:

  ```powershell
  Find-Package <keyword>
  ```

- Find and list packages with an ID that begins with a certain string:

  ```powershell
  Find-Package <string> -StartWith
  ```

- Show the first 100 packages that contain a certain keyword:

  ```powershell
  Find-Package <keyword> -First 100
  ```

  By default, `Find-Package` returns a list of 20 packages. As in the preceding example, you can use `-First` to specify a different number of packages.

- List all versions of a certain package:

  ```powershell
  Find-Package <package-name> -AllVersions -ExactMatch
  ```

## Install a package

To install a package into the default project, use `Install-Package <package-name>`. The [Install-Package](../reference/ps-reference/ps-ref-install-package.md) console command takes the following actions:

- Does the steps in [What happens when a NuGet package is installed](../concepts/package-installation-process.md).
- Displays applicable license terms in the console window with implied agreement. If you don't agree to the terms, you should uninstall the package.
- Adds a reference to the package in the project file and in **Solution Explorer** under the **Dependencies** or **References** node. You must save the project to propagate the changes to the project file.

By default, `Install-Package` adds the package to the default project that the console window specifies. To add the package to a project that isn't the default project, use the `-ProjectName` option. For example, to add the `Elmah.MVC` package to the non-default `UtilitiesLib` project, run the following command:

```powershell
Install-Package Elmah.MVC -ProjectName UtilitiesLib
```

## Uninstall a package

To uninstall a package from the default project, use `Uninstall-Package <package-name>`. If you need to find the package name, use [Get-Package](../reference/ps-reference/ps-ref-get-package.md) to list all packages installed in the default project.

[Uninstall-Package](../reference/ps-reference/ps-ref-uninstall-package.md) takes the following actions:

- Removes references to the package from the project and any management formats. References no longer appear in **Solution Explorer**. You might need to rebuild the project to remove the reference in the *bin* folder.
- Reverses any changes that installing the package made to *app.config* or *web.config*.
- Removes previously installed dependencies if no remaining packages use those dependencies.

The following code blocks show you how to use the command in various scenarios:

- Uninstall a package and all its unused dependencies:

  ```powershell
  Uninstall-Package <package-name> -RemoveDependencies
  ```

- Uninstall a package even if other packages depend on it:

  ```powershell
  Uninstall-Package <package-name> -Force
  ```

## Update a package

To update packages, use [Update-Package](../reference/ps-reference/ps-ref-update-package.md). You can also use [Get-Package](../reference/ps-reference/ps-ref-get-package.md) to list available updates for installed packages. The following code blocks show you how to use parameters to modify the update scope:

- Check whether newer versions are available for any packages that are installed in the solution:

  ```powershell
  Get-Package -updates
  ```

- Update a specific package:

  ```powershell
  Update-Package <package-name>
  ```

- Update all packages in a project:

  ```powershell
  Update-Package -ProjectName <project-name>
  ```

- Update all packages in a solution:

  ```powershell
  Update-Package
  ```

<a name="use-the-nugetexe-cli-in-the-console"></a>
## Use the NuGet CLI in the console

You can also do most console operations by using the [NuGet CLI](../reference/nuget-exe-cli-reference.md). However, the PowerShell console commands operate within the context of the Visual Studio saved project and solution, and often do more than their equivalent NuGet CLI commands. For example, installing a package through `Install-Package` adds a reference to the project file, but the NuGet CLI command doesn't. For this reason, developers who work in Visual Studio typically prefer to use the console commands rather than the NuGet CLI.

To use NuGet CLI commands in the Package Manager Console, install the [NuGet.CommandLine](https://www.nuget.org/packages/NuGet.CommandLine) package.

```powershell
Install-Package NuGet.CommandLine
```

The preceding command installs the latest version of the NuGet CLI. To install a specific version, use the `-Version` option. For example, to install Version 4.4.1, use the following command:

```powershell
Install-Package NuGet.CommandLine -Version 4.4.1
```

After you install the `NuGet.CommandLine` package, you can run all NuGet CLI commands through the Package Manager Console.

## Extend the Package Manager Console

Some packages extend the Package Manager Console by adding commands. For example, the `Microsoft.EntityFrameworkCore.Tools` package adds commands like the following ones:

- `Add-Migration`: Generates a migration file for creating or updating database tables
- `Update-Database`: Creates or updates a database according to the latest migration

:::image type="content" source="media/extend-package-manager-console.png" alt-text="Screenshot of the Package Manager Console window. Output from the Add-Migration and Update-Database commands shows a database being created." lightbox="media/extend-package-manager-console.png":::

## Set up a NuGet PowerShell profile

You can create a PowerShell profile to make your commonly used commands available in all PowerShell contexts, so you don't lose your PowerShell settings between sessions. NuGet supports a NuGet-specific profile, usually at *%UserProfile%\Documents\WindowsPowerShell\NuGet_profile.ps1*.

To find your user profile location, enter `$profile` in the console:

```powershell
$profile
C:\Users\<user>\Documents\WindowsPowerShell\NuGet_profile.ps1
```

To determine whether a profile exists at that location, enter `test-path $profile`. If the command returns `False`, you need to create the profile with the specified name at that location. For more information, see [Windows PowerShell Profiles](/previous-versions//bb613488(v=vs.85)).

## Related content

- [Install and manage NuGet packages with the dotnet CLI](install-use-packages-dotnet-cli.md)
- [Manage NuGet packages with the NuGet CLI](install-use-packages-nuget-cli.md)
- [Install and manage packages in Visual Studio using the NuGet Package Manager](install-use-packages-visual-studio.md)
