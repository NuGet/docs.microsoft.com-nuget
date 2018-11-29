---
title: Introductory Guide to Using NuGet Packages from within Visual Studio
description: A walkthrough tutorial on the process of installing and using a NuGet package in a Visual Studio project.
author: karann-msft
ms.author: karann
ms.date: 01/23/2018
ms.topic: quickstart
---

# Quickstart: Install and use a package in Visual Studio

NuGet packages contain reusable code that other developers make available to you for use in your projects. See [What is NuGet?](../What-is-NuGet.md) for background. Packages are installed into a Visual Studio project using the Package Manager UI or the Package Manager Console. This article demonstrates the process using the popular [Newtonsoft.Json](https://www.nuget.org/packages/Newtonsoft.Json/) package and a Universal Windows Platform (UWP) project. The same process applies to any other .NET or .NET Core project.

Once installed, refer to the package in code with `using <namespace>` where \<namespace\> is specific to the package you're using. Once the reference is made, you can call the package through its API.

> [!Tip]
> **Start with nuget.org**: Browsing nuget.org is how .NET developers typically find components they can reuse in their own applications. You can search nuget.org directly or find and install packages within Visual Studio as shown in this article.

## Prerequisites

- Visual Studio 2017 with the Universal Windows Platform development workload, or
- Visual Studio 2015 Update 3 with Tools for Universal Windows Apps.

You can install the 2017 Community edition for free from [visualstudio.com](https://www.visualstudio.com/) or use the Professional or Enterprise editions.

## Create a project

NuGet packages can be installed into any .NET project, provided that the package supports the same target framework as the project.

For this walkthrough, use a simple Universal Windows (UWP) app. Create a project in Visual Studio using **File > New Project...** and selecting the **Windows Universal > Blank App (Universal Windows)**. Accept the default values for Target Version and Minimum Version when prompted.

## Add the Newtonsoft.Json NuGet package

To install the package, you can use either the Package Manager UI or the Package Manager Console. When you install a package, NuGet records the dependency in either your project file or a `packages.config` file. For more information, see [Package consumption overview and workflow](../consume-packages/Overview-and-Workflow.md).

### Package Manager UI

1. In Solution Explorer, right-click **References** and choose **Manage NuGet Packages**.

    ![Manage NuGet Packages command for project References](media/QS_Use-02-ManageNuGetPackages.png)

1. Choose "nuget.org" as the **Package source**, select the **Browse** tab, search for **Newtonsoft.Json**, select that package in the list, and select **Install**:

    ![Locating Newtonsoft.Json package](media/QS_Use-03-NewtonsoftJson.png)

1. Accept any license prompts.

1. (Visual Studio 2017) If prompted to select a package management format, select **PackageReference in project file**:

    ![Selecting a package management format](media/QS_Use-03b-SelectFormat.png)

1. If prompted to review changes, select **OK**.

### Package Manager Console

1. Select the **Tools > NuGet Package Manager > Package Manager Console** menu command.

1. Once the console opens, check that the **Default project** drop-down list shows the project into which you want to install the package. If you have a single project in the solution, it is already selected.

    ![Locating Newtonsoft.Json package](media/QS_Use-08-Console1.png)

1. Enter the command `Install-Package Newtonsoft.Json` (see [Install-Package](../tools/ps-ref-install-package.md)). The console window shows output for the command. Errors typically indicate that the package isn't compatible with the project's target framework.

## Use the Newtonsoft.Json API in the app

With the Newtonsoft.Json package in the project, you can call its `JsonConvert.SerializeObject` method to convert an object to a human-readable string.

1. Open `MainPage.xaml` and replace the existing `Grid` element with the following:

    ```xaml
    <Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
        <StackPanel VerticalAlignment="Center">
            <Button Click="Button_Click" Content="Click Me" Margin="10"/>
            <TextBlock Name="TextBlock" Text="TextBlock" Margin="10"/>
        </StackPanel>
    </Grid>
    ```

1. Open the `MainPage.xaml.cs` file (located in Solution Explorer under the `MainPage.xaml` node), and insert the following code inside the `MainPage` constructor:

    ```cs
    public class Account
    {
        public string Name { get; set; }
        public string Email { get; set; }
        public DateTime DOB { get; set; }
    }

    private void Button_Click(object sender, RoutedEventArgs e)
    {
        Account account = new Account
        {
            Name = "John Doe",
            Email = "john@microsoft.com",
            DOB = new DateTime(1980, 2, 20, 0, 0, 0, DateTimeKind.Utc),
        };
        string json = JsonConvert.SerializeObject(account, Formatting.Indented);
        TextBlock.Text = json;
    }
    ```

1. Even though you added the Newtonsoft.Json package to the project, red squiggles appears under `JsonConvert` because you need a `using` statement at the top of the code file:

    ```cs
    using Newtonsoft.json;
    ```

1. Build and run the app by pressing F5 or selecting **Debug > Start Debugging**:

    ![Initial output of the UWP app](media/QS_Use-06-AppStart.png)

1. Select on the button to see the contents of the TextBlock replaced with some JSON text:

    ![Output of the UWP app after selecting the button](media/QS_Use-07-AppEnd.png)

## Related articles

- [Overview and workflow of package consumption](../consume-packages/overview-and-workflow.md)
- [Finding and choosing packages](../consume-packages/finding-and-choosing-packages.md)
- [Ways to install a package](../consume-packages/ways-to-install-a-package.md)
- [Configuring NuGet Behavior](../consume-packages/configuring-nuget-behavior.md)
