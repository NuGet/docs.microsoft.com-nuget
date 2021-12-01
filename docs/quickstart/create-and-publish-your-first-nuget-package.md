---
title: Create and publish your first NuGet package
description: A tutorial for creating your first NuGet package with a .NET library.
author: JonDouglas
ms.author: jodou
ms.date: 08/16/2019
ms.topic: quickstart
---

# Quickstart: Create and publish your first NuGet package

In this tutorial, you learn how to create a NuGet package with Visual Studio or Visual Studio Code, and then publish it to nuget.org.

In this tutorial, you:

> [!div class="checklist"]
> * Create a class library project.
> * Configure package properties.
> * Create the package.
> * Publish your package to nuget.org.

> [!Note]
> On platforms other than Windows, you can [create packages using dotnet CLI tools](create-and-publish-a-package-using-the-dotnet-cli.md). On macOS, you can also [create packages with Visual Studio for Mac](/xamarin/cross-platform/app-fundamentals/nuget-multiplatform-libraries/existing-library).

## Prerequisites

# [Visual Studio](#tab/visual-studio)

1. [Visual Sutdio 2022](https://www.visualstudio.com/) - you can install it for free!

1. [A free account on nuget.org](../nuget-org/individual-accounts.md#add-a-new-individual-account). Creating a new account sends a confirmation email. You must confirm the account before you can upload a package.

# [Visual Studio Code](#tab/visual-studio-code)

1. [Visual Studio Code](https://code.visualstudio.com/download)

1. [C# for Visual Studio Code](https://marketplace.visualstudio.com/items?itemName=ms-dotnettools.csharp)

1. [.NET 6.0 SDK](https://dotnet.microsoft.com/download/dotnet/6.0)

## Create a class library project

To start, create a .NET class library. This project type comes with all the template files you need.

# [Visual Studio](#tab/visual-studio)

1. Open Visual Studio, and choose **Create a new project** in the Start window.

   ![Screenshot that shows the Create a new project window.](media/create-new-project.png)

1. In the **Create a new project** window, search for *C# Class Library*. Select the C# Class Library template, then selct **Next**.

   ![Screenshot that shows the class library project selection in the Create new project window.](media/select-class-library-project.png)

    > [!NOTE]
    > If you don't see the **Class Library** template, select **Install more tools and features**.
    >
    > ![Screenshot that shows the Install more tools and features link.](media/not-finding-what-looking-for.png)
    >
    > In the Visual Studio Installer, choose the **.NET desktop development** workload, and then select **Modify**.
    >
    > ![Screenshot showing the .NET desktop development workload in the Visual Studio Installer.](media/dot-net-development-workload.png)

1. In the **Configure your new project** window, type or enter *AppLogger* in the **Project name** box, and then select **Next**.

   ![Screenshot that shows naming the project AppLogger in the Configure your new project window.](media/configure-your-new-project-applogger.png)

1. In the **Additional information** window select **Create**. For this tutorial, you can use the default selected framework.

   ![Screenshot that shows .NET 6.0 selected in the Additional information window.](media/csharp-target-framework.png)

1. You can already pack the template code as-is, but let's make it do something interesting! Replace the existing AppLogger code with the following code, so our AppLogger library has a method to write text to the console.

    ```cs
    namespace AppLogger;
    
    public class Logger
    {
        public void Log(string text)
        {
            Console.WriteLine(text);
        }
    }
    ```

# [Visual Studio Code](#tab/visual-studio-code)

1. Create a folder named `AppLogger` where your project will live. 

1. Open Visual Studio Code to your `AppLogger` folder, and open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal) (`Ctrl+`\`).

1. In the terminal, enter `dotnet new classlib`, which uses the name of the current folder for the project.

   This creates your new `AppLogger` project.

1. Open the `Class1.cs` in the left hand explorer to view the code template.

    You can already pack the template code as-is, but let's make it do something interesting! Replace the existing AppLogger code with the following code, so our AppLogger library has a method to write text to the console.

    ```cs
    namespace AppLogger;
    
    public class Logger
    {
        public void Log(string text)
        {
            Console.WriteLine(text);
        }
    }
    ```

## Configure package properties

# [Visual Studio](#tab/visual-studio)

1. Right-click the **AppLogger** project in Solution Explorer and select the **Properties** menu command. Within the project properties window, select the **Package** tab on the left.

   The **Package** tab appears only for SDK-style projects in Visual Studio, typically .NET Standard or .NET Core class library projects; if you are targeting a non-SDK style project (typically .NET Framework), either [migrate the project](../consume-packages/migrate-packages-config-to-package-reference.md) or see [Create and publish a .NET Framework package](create-and-publish-a-package-using-visual-studio-net-framework.md) instead for step-by-step instructions.

    ![NuGet package properties in a Visual Studio project](media/view-package-properties.png)

1. Visual Studio sets default values for some package properties such as **Package ID** and **Package Version**, however you can input your own values. 

    Input your **Package ID** as `[nuget_username].Sample.AppLogger`, and save (CTRL + S). Subsitute your nuget.org username for `[nuget_username]`.

    > [!TIP]
    > Check out our [best practices guide](../create-packages/Package-authoring-best-practices.md) for a detailed walkthrough of all the other important package properties.

## Create the package

1. Right click the project in **Solution Explorer** and select the **Pack** command:

    ![NuGet pack command on the Visual Studio project context menu](media/right-click-pack.png)

1. Visual Studio builds the project and creates the `.nupkg` package file. Examine the **Output** window for details, including the path to the package file.

    ```output
    1>------ Build started: Project: AppLogger, Configuration: Debug Any CPU ------
    1>AppLogger -> C:\Users\UserName\source\repos\AppLogger\AppLogger\bin\Debug\net6.0\AppLogger.dll
    1>Successfully created package 'C:\Users\UserName\source\repos\AppLogger\AppLogger\bin\Debug\nuget_username.Sample.AppLogger.1.0.0.nupkg'.
    ========== Build: 1 succeeded, 0 failed, 0 up-to-date, 0 skipped ==========
    ```

    From our example output, we can see the path to the generated package is `C:\Users\UserName\source\repos\AppLogger\AppLogger\bin\Debug\nuget_username.Sample.AppLogger.1.0.0.nupkg`.

# [Visual Studio Code](#tab/visual-studio-code)

Open the `.csproj` project file and add the following group of properties within the <Project> tag. Change the values in straight brackets `[]` to match your info.

```xml
<!--Package properties-->
<PropertyGroup>
    <PackageId>[nuget_username].Sample.AppLogger</PackageId>
    <Version>1.0.0</Version>
    <Authors>[your_name]</Authors>
</PropertyGroup>
```

> [!TIP]
> Check out our [best practices guide](../create-packages/Package-authoring-best-practices.md) for a detailed walkthrough of all the other important package properties.

## Publish your package to nuget.org

In this walthrough, we'll publish the package using the nuget.org **Upload** page. However, you can also publish package from the command line for more advanced scenarios.

## Related video

# [Visual Studio](#tab/visual-studio)

> [!Video https://channel9.msdn.com/Series/NuGet-101/Create-and-Publish-a-NuGet-Package-with-Visual-Studio-4-of-5/player]

Find more NuGet videos on [Channel 9](https://channel9.msdn.com/Series/NuGet-101) and [YouTube](https://www.youtube.com/playlist?list=PLdo4fOcmZ0oVLvfkFk8O9h6v2Dcdh2bh_).

# [Visual Studio Code](#tab/visual-studio-code)

## Related video

> [!Video https://channel9.msdn.com/Series/NuGet-101/Create-and-Publish-a-NuGet-Package-with-the-NET-CLI-5-of-5/player]

Find more NuGet videos on [Channel 9](https://channel9.msdn.com/Series/NuGet-101) and [YouTube](https://www.youtube.com/playlist?list=PLdo4fOcmZ0oVLvfkFk8O9h6v2Dcdh2bh_).

## Related topics

- [Create a Package](../create-packages/creating-a-package-dotnet-cli.md)
- [Publish a Package](../nuget-org/publish-a-package.md)
- [Pre-release Packages](../create-packages/Prerelease-Packages.md)
- [Support multiple target frameworks](../create-packages/multiple-target-frameworks-project-file.md)
- [Package versioning](../concepts/package-versioning.md)
- [Creating localized packages](../create-packages/creating-localized-packages.md)
- [.NET Standard Library documentation](/dotnet/articles/standard/library)
- [Porting to .NET Core from .NET Framework](/dotnet/articles/core/porting/index)
