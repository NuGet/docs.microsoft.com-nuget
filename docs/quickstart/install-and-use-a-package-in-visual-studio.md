---
title: "Quickstart: Install and Use a NuGet Package in Visual Studio (Windows Only)"
description: In this quickstart, find out how to install and use the Newtonsoft.Json NuGet package in a Visual Studio project for Windows.
author: JonDouglas
ms.author: jodou
ms.date: 04/16/2026
ms.topic: quickstart
# customer intent: As a developer, I want to find out how to install and use a NuGet package in a Visual Studio project so that I can take advantage of available code.
---

# Quickstart: Install and use a NuGet package in Visual Studio (Windows only)

In this quickstart, you use Microsoft Visual Studio to install and use a *NuGet package* in a project. A NuGet package contains reusable code that other developers make available to you for use in your projects.

You can install a NuGet package in a Visual Studio project by using the [NuGet Package Manager](../consume-packages/install-use-packages-visual-studio.md), the [Package Manager Console](../consume-packages/install-use-packages-powershell.md), or the [.NET command-line interface (CLI)](install-and-use-a-package-using-the-dotnet-cli.md). This quickstart demonstrates how to use the NuGet Package Manager and the Package Manager Console to install a package. You follow steps for creating a Windows Presentation Foundation (WPF) project that uses the popular `Newtonsoft.Json` package. The same process applies to any other .NET project.

This article is for Windows users only. If you're using Visual Studio for Mac, see [Install and use a package in Visual Studio for Mac](install-and-use-a-package-in-visual-studio-mac.md).

> [!TIP]
> To find a NuGet package, start with [nuget.org](https://www.nuget.org/). Browsing nuget.org is how .NET developers typically find components they can reuse in their own applications. You can do a search of nuget.org directly or find and install packages within Visual Studio as shown in this article. For more information, see [Find and evaluate NuGet packages for your project](../consume-packages/Finding-and-Choosing-Packages.md).

## Prerequisites

- Install Visual Studio 2026 with the .NET desktop development workload.

  You can install the 2026 Community edition for free from [visualstudio.microsoft.com](https://visualstudio.microsoft.com/), or you can use the Professional or Enterprise edition.

## Create a project

You can install a NuGet package into any .NET project if that package supports the same target framework as the project. For this quickstart, you create a WPF Application project.

Follow these steps:

1. In Visual Studio, select **File** > **New** > **Project/Solution**.

1. In the **Create a new project** window, go to the search box and enter **wpf**. In the resulting list of project templates, select the **WPF Application** template that has **C#** and **Windows** tags, and then select **Next**.

1. In the **Configure your new project** window, optionally update the **Project name** and **Solution name** values, and then select **Next**.

1. In the **Additional information** window, under **Framework**, select **.NET 10.0** (or the latest version), and then select **Create**.

   Visual Studio creates the project, and it appears in [Solution Explorer](/visualstudio/ide/use-solution-explorer).

## Add the Newtonsoft.Json NuGet package

To install a NuGet package in this quickstart, you can use either the NuGet Package Manager or the Package Manager Console. Depending on your project format, the installation of a NuGet package records the dependency in either your project file or a *packages.config* file. For more information, see [Package consumption workflow](../consume-packages/Overview-and-Workflow.md).

### NuGet Package Manager

To use the [NuGet Package Manager](../consume-packages/install-use-packages-visual-studio.md) to install the `Newtonsoft.Json` package in Visual Studio, follow these steps:

1. Select **Project** > **Manage NuGet Packages**.

1. On the **NuGet Package Manager** page, next to **Package source**, select **nuget.org**.

1. Go to the **Browse** tab and search for **Newtonsoft.Json**. In the list, select **Newtonsoft.Json**, and then select **Install**.

   :::image type="content" source="media/qs-use-install-package.png" alt-text="Screenshot of the NuGet Package Manager. The Newtonsoft.Json package is selected. Its details pane displays package data and has an Install button." lightbox="media/qs-use-install-package.png":::

1. If you're prompted to verify the installation, select **Apply**.

### Package Manager Console

Alternatively, to use the [Package Manager Console](../consume-packages/install-use-packages-powershell.md) in Visual Studio to install the `Newtonsoft.Json` package, follow these steps:

1. In Visual Studio, select **Tools** > **NuGet Package Manager** > **Package Manager Console**.

1. At the top of the **Package Manager Console** window, verify that the **Default project** list shows the project in which you want to install the package. If you have a single project in the solution, it's preselected.

   :::image type="content" source="media/qs-use-package-manager-console.png" alt-text="Screenshot of the Package Manage Console window, which contains information about the version and licensing. The Default project list is highlighted." lightbox="media/qs-use-package-manager-console.png":::

1. At the console prompt, enter the command `Install-Package Newtonsoft.Json`. For more information about this command, see [Install-Package](../reference/ps-reference/ps-ref-install-package.md).

   The console window shows the output for the command. Errors typically indicate that the package isn't compatible with the project's target framework.

## Use the Newtonsoft.Json API in the app

After you install a NuGet package, you can make a reference to it in your code by using the `using <namespace>` statement, where \<namespace\> is the name of the package you're using. After you make a reference, you can call the package through its API.

With the `Newtonsoft.Json` package in the project, you can call its `JsonConvert.SerializeObject` method. To use this method to convert an object to a human-readable string, take these steps:

1. In **Solution Explorer**, open *MainWindow.xaml* and replace the existing `<Grid>` element with the following code:

   ```xaml
   <Grid Background="White">
       <StackPanel VerticalAlignment="Center">
           <Button Click="Button_Click" Width="100px" HorizontalAlignment="Center" Content="Click Me" Margin="10"/>
           <TextBlock Name="TextBlock" HorizontalAlignment="Center" Text="TextBlock" Margin="10"/>
       </StackPanel>
   </Grid>
   ```

1. In **Solution Explorer**, expand the **MainWindow.xaml** node, and then open the *MainWindow.xaml.cs* file. Insert the following code inside the `MainWindow` class, after the constructor:

   ```csharp
   public class Account
   {
       public string ID { get; set; }
       public decimal Balance { get; set; }
       public DateTime Created { get; set; }
   }

   private void Button_Click(object sender, RoutedEventArgs e)
   {
       Account account = new Account
       {
           ID = "A1bC2dE3fH4iJ5kL6mN7oP8qR9sT0u",
           Balance = 4389.21m,
           Created = new DateTime(2026, 4, 16, 0, 0, 0, DateTimeKind.Utc),
       };
       string json = JsonConvert.SerializeObject(account, Newtonsoft.Json.Formatting.Indented);
       TextBlock.Text = json;
   }
   ```

1. If *MainWindow.xaml.cs* doesn't contain the following line, add it to the beginning of the file:

   ```csharp
   using Newtonsoft.Json;
   ```

   Without this line, Visual Studio marks the `JsonConvert` object with a red, squiggly line to indicate an error.

1. To build and run the app, select **F5**, or select **Debug** > **Start Debugging**.

   The following window appears:

   :::image type="content" source="media/qs-use-wpf-app-start.png" alt-text="Screenshot of the MainWindow window that the app creates in Visual Studio. The window contains a Click Me button and the term TextBlock." lightbox="media/qs-use-wpf-app-start.png":::

1. Select **Click Me**. The app updates the window by replacing the `TextBlock` object with JSON text.

   :::image type="content" source="media/qs-use-wpf-app-end.png" alt-text="Screenshot of the MainWindow window in Visual Studio, which contains a Click Me button and JSON code that lists ID, Balance, and Created values." lightbox="media/qs-use-wpf-app-end.png":::

## Related videos

For videos about using NuGet for package management, see [.NET Package Management with NuGet for Beginners](/shows/dotnet-package-management-with-nuget-for-beginners/) and [NuGet for Beginners](https://www.youtube.com/playlist?list=PLdo4fOcmZ0oVLvfkFk8O9h6v2Dcdh2bh_).

## Related content

To find out more about installing and managing NuGet packages, see the following articles:

- [Install and manage packages in Visual Studio using the NuGet Package Manager](../consume-packages/install-use-packages-visual-studio.md)
- [Manage packages with the Visual Studio Package Manager Console (PowerShell)](../consume-packages/install-use-packages-powershell.md)

For more information about NuGet, see the following articles:

- [An introduction to NuGet](../what-is-nuget.md)
- [Package consumption workflow](../consume-packages/Overview-and-Workflow.md)
- [Find and evaluate NuGet packages for your project](../consume-packages/Finding-and-Choosing-Packages.md)
- [`PackageReference` in project files](../consume-packages/Package-References-in-Project-Files.md)
- [Install and use a package with the dotnet CLI](install-and-use-a-package-using-the-dotnet-cli.md)
- [Newtonsoft.Json package](https://www.nuget.org/packages/newtonsoft.json)
