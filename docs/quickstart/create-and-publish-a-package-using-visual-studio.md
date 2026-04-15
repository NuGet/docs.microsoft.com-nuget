---
title: "Quickstart: Create and Publish a NuGet Package Using Visual Studio (Windows Only)"
description: In this quickstart, find out how to use Visual Studio to create a basic NuGet package and then use the command line to publish the package to nuget.org.
author: JonDouglas
ms.author: jodou
ms.date: 04/15/2026
ms.topic: quickstart
# customer intent: As a developer, I want to find out how to create and publish a NuGet package so that I can make my code available to other developers.
---

# Quickstart: Create and publish a NuGet package using Visual Studio (Windows only)

In this quickstart, you use Microsoft Visual Studio to create a NuGet package from a .NET class library. Then you publish the package to nuget.org by using a command-line interface (CLI) tool.

This quickstart is for Windows users only. If you're using a Mac, use the [.NET CLI](create-and-publish-a-package-using-the-dotnet-cli.md).

## Prerequisites

- Visual Studio 2026 with a .NET-related workload.

  You can install the 2026 Community edition for free from [visualstudio.microsoft.com](https://visualstudio.microsoft.com/), or you can use the Professional or Enterprise edition.

  Visual Studio 2017 and later automatically include NuGet capabilities when you install a .NET-related workload.

- The .NET CLI.

  For Visual Studio 2017 and later, the .NET CLI is automatically installed with any .NET-related workload. You can also install the [.NET SDK](https://dotnet.microsoft.com/download) to get the .NET CLI. The .NET CLI is required for .NET projects that use the [SDK-style format](../resources/check-project-format.md) (and an `SDK` attribute). The default .NET class library template in Visual Studio 2017 and later uses the `SDK` attribute.

  > [!IMPORTANT]
  > If you're working with a non-SDK-style project, follow the procedures in [Create and publish a package using Visual Studio (.NET Framework, Windows)](create-and-publish-a-package-using-visual-studio-net-framework.md) instead to create and publish the package. For this article, the .NET CLI is recommended. Although you can publish any NuGet package by using the NuGet CLI, some of the steps in this article are specific to SDK-style projects and the .NET CLI. The NuGet CLI is used for [non-SDK-style projects](../resources/check-project-format.md) (typically .NET Framework projects).

- A [free account on nuget.org](../nuget-org/individual-accounts.md#add-a-new-individual-account). You must register and confirm the account before you can upload a NuGet package.

- The NuGet CLI. You can install it by downloading it from [nuget.org](https://dist.nuget.org/win-x86-commandline/latest/nuget.exe). Add the *nuget.exe* file to a suitable folder, and add that folder to your `PATH` environment variable.

## Create a class library project

You can use an existing .NET class library project for the code you want to package, or you can create one by taking the following steps:

1. In Visual Studio, select **File** > **New** > **Project/Solution**.

1. In the **Create a new project** window, go to the search box and enter **class library**.

1. In the resulting list of project templates, select the **Class Library** template that meets the following criteria:
   - Has the description *A project for creating a class library that targets .NET or .NET Standard*
   - Has a **C\#** tag

   Select **Next**.

1. In the **Configure your new project** window, for **Project name**, enter **AppLogger**, and then select **Next**.

1. In the **Additional information** window, select an appropriate value for **Framework**, and then select **Create**.

   If you're unsure which framework to select, the latest is a good choice, and can be easily changed later. For information about which framework to use, see [When to target `netx.0` vs. `netstandard`](/dotnet/standard/net-standard#when-to-target-netx0-vs-netstandard).

1. To check whether the project is created properly, select **Build** > **Build Solution**. Then check the *Debug* folder for the DLL. If your project is configured to build a release configuration, check the *Release* folder instead.

1. (Optional) For this quickstart, you don't need to write any additional code for the NuGet package, because the template class library is sufficient to create a package. However, if you want to add some functional code to the package, include the following code:

   ```csharp
   namespace AppLogger
   {
       public class Logger
       {
           public void Log(string text)
           {
               Console.WriteLine(text);
           }
       }
   }
   ```

## Configure package properties

After you create your project, you can configure the NuGet package properties by following these steps:

1. In **Solution Explorer**, select your project, and then select **Project** > **\<project-name\> Properties**, where \<project-name\> is the name of your project.

1. Expand the **Package** node, and then select **General**.

   The **Package** node appears only for SDK-style projects in Visual Studio. If you're targeting a non-SDK style project (typically .NET Framework projects), either [migrate the project](../consume-packages/migrate-packages-config-to-package-reference.md), or see [Create and publish a package using Visual Studio (.NET Framework, Windows)](create-and-publish-a-package-using-visual-studio-net-framework.md) for step-by-step instructions.

   :::image type="content" source="media/qs-create-vs-project-properties.png" alt-text="Screenshot of a project properties window in Visual Studio. General properties for a NuGet package are visible, such as the package ID and title." lightbox="media/qs-create-vs-project-properties.png":::

1. For **Package ID**, give your package a unique ID.

   > [!IMPORTANT]
   > You must give the package an identifier that's unique across the host that you use, such as nuget.org. Otherwise, an error occurs. For this quickstart, we recommend including *Sample* or *Test* in the ID, because the publishing step makes the package publicly visible. For more information about selecting an ID, see [Best practices for the package identifier](../create-packages/Creating-a-Package.md#best-practices-for-the-package-identifier).

1. Fill out any other desired properties. For packages built for public consumption, pay special attention to the **Tags** property, because tags help others find your package and understand what it does.

   All the properties go into the *.nuspec* manifest that Visual Studio creates for the project. For a table that shows how Microsoft Build (MSBuild) properties in SDK-style projects map to *.nuspec* file properties, see [pack target](../reference/msbuild-targets.md#pack-target). For a description of *.nuspec* file properties, see [.nuspec reference](../reference/nuspec.md).

1. (Optional) To see the properties directly in the *AppLogger.csproj* project file, select **Project** > **Edit Project File**.

   The *AppLogger.csproj* file opens in a new tab.

   This option is available for projects that use the SDK-style attribute.

## Run the pack command

To create a NuGet package from your project, follow these steps:

1. Select **Build** > **Configuration Manager**, and then set the **Active solution configuration** value to **Release**.

1. In **Solution Explorer**, right-click the **AppLogger** project, and then select **Pack**.

   Visual Studio builds the project and creates the *.nupkg* file.

1. Examine the **Output** window for detailed information, including the path to the package file. In this example, the built assembly is in the *bin\Release\net8.0* folder, which is appropriate for a .NET 8.0 target:

   ```output
   1>------ Build started: Project: AppLogger, Configuration: Release Any CPU ------
   1>  AppLogger -> d:\proj\AppLogger\AppLogger\bin\Release\net8.0\AppLogger.dll
   1>  Successfully created package 'd:\proj\AppLogger\AppLogger\bin\Release\Contoso.App.Logger.Test.1.0.0.nupkg'.
   ========== Build: 1 succeeded, 0 failed, 0 up-to-date, 0 skipped ==========
   ```

If the **Pack** command is missing from the menu, your project probably isn't an SDK-style project. Take one of the following steps:

- [Migrate the project](../consume-packages/migrate-packages-config-to-package-reference.md) from the `packages.config` format to the `PackageReference` format so that you can use the .NET CLI.
- Follow the instructions in [Create and publish a package using Visual Studio (.NET Framework, Windows)](create-and-publish-a-package-using-visual-studio-net-framework.md) to use the NuGet CLI to create and publish a NuGet package from your project.

### (Optional) Generate package on build

You can configure Visual Studio to automatically generate the NuGet package when you build the project:

1. Select your project in **Solution Explorer**, and then select **Project** > **\<project-name\> Properties**, where \<project-name\> is the name of your project (**AppLogger** in this case).

1. Expand the **Package** node, select **General**, and then select **Generate NuGet package on build**.

   :::image type="content" source="media/qs-create-vs-generate-on-build.png" alt-text="Screenshot of a project properties window in Visual Studio. In the General properties section, the Generate NuGet package on build option is selected." lightbox="media/qs-create-vs-generate-on-build.png":::

> [!NOTE]
> When you select this option, the extra time that's needed to generate the package increases the overall build time for your project.

### (Optional) Pack with MSBuild

As an alternative to using the **Pack** menu command, you can use the `msbuild -t:pack` command to build a NuGet package from your project. NuGet 4.x+ and MSBuild 15.1+ support a `pack` target when your project contains the necessary package data.

1. With your project open in **Solution Explorer**, open a command prompt window by selecting **Tools** > **Command Line** > **Developer Command Prompt**.

   The command prompt window opens in your project directory.

1. Run the following command: `msbuild -t:pack`.

For more information, see [Create a NuGet package using MSBuild](../create-packages/creating-a-package-msbuild.md).

## Publish the package

After you create a *.nupkg* file, take the steps in the following sections to publish it to nuget.org. You can use the .NET CLI or the NuGet CLI for publishing. You also use an API key that you acquire from nuget.org.

[!INCLUDE [publish-notes](includes/publish-notes.md)]

### Acquire your API key

Before you publish your NuGet package, create an API key:

[!INCLUDE [publish-api-key](includes/publish-api-key-with-link.md)]

### Publish by using the .NET CLI or NuGet CLI

You can use the .NET CLI or the NuGet CLI to push a package to the server and publish it. Go to the tab for the tool that you want to use.

#### [.NET CLI](#tab/netcore-cli)

The .NET CLI (`dotnet.exe`) is the recommended alternative to the NuGet CLI.

[!INCLUDE [publish-dotnet](includes/publish-dotnet.md)]

#### [NuGet CLI](#tab/nuget)

For publishing, you can use the NuGet CLI (`nuget.exe`) as an alternative to the .NET CLI:

1. Open a command prompt window, and then go to the folder that contains the *.nupkg* file.

1. Run the following command. Replace `<package-file>` with the name of your *.nupkg* file, and replace `<API-key>` with your API key. The package file has the format *\<package-ID\>.version.nupkg*, for example, *Contoso.App.Logger.Test.1.0.0.nupkg*.

   ```nuget
   nuget push <package-file> <API-key> -Source https://api.nuget.org/v3/index.json
   ```

   The output shows the results of the publishing process:

   ```output
   Pushing <package-file> to 'https://www.nuget.org/api/v2/package'...
     PUT https://www.nuget.org/api/v2/package/
     Created https://www.nuget.org/api/v2/package/ 950ms
   Your package was pushed.
   ```

For more information, see [push command (NuGet CLI)](../reference/cli-reference/cli-ref-push.md).

---

### Errors during publishing

[!INCLUDE [publish-errors](includes/publish-errors.md)]

### Manage the published package

[!INCLUDE [publish-manage](includes/publish-manage.md)]

## Add a read-me or another file

You can include a read-me file or other files in your package.

### Add a read-me file

To add a read-me file to the package, take the following steps:

1. In **Solution Explorer**, select your project, and then select **Project** > **\<project-name\> Properties**, where \<project-name\> is the name of your project.

1. Expand the **Package** node, select **General**, and then scroll to the **README** section.

1. Select **Browse**, and then select your read-me file.

   Visual Studio adds information about your read-me file to the project file:
   - A `PackageReadmeFile` child element is added to the `PropertyGroup` element.
   - A `None` child element is added to the `ItemGroup` element.

   ```xml
   <Project Sdk="Microsoft.NET.Sdk">
       <PropertyGroup>
           ...
           <PackageReadmeFile>read-me.md</PackageReadmeFile>
           ...
       </PropertyGroup>
   
       <ItemGroup>
           ...
           <None Update="read-me.md">
             <Pack>True</Pack>
             <PackagePath>\</PackagePath>
           </None>
           ...
       </ItemGroup>
   </Project>
   ```

In the preceding example, the property specifies a file named *read-me.md* in the project root. After you build, pack, and publish your package, nuget.org displays the contents of the read-me file on the package page. Visual Studio also displays the contents of that file in the Package Manager UI.

For example, the following screenshot shows the read-me file for the `HtmlAgilityPack` package:

:::image type="content" source="media/package-read-me-file.png" alt-text="Screenshot of the Visual Studio Package Manager UI that shows a package details pane. The README tab describes HTML parsing abilities of the package." lightbox="media/package-read-me-file.png" lightbox="media/package-read-me-file.png":::

### Add other files

To add other files to a package, open the project file by selecting **Project** > **Edit Project File**. Then add a `Content` child element to the `ItemGroup` element:

```xml
<ItemGroup>
  <Content Include="other-content.md">
    <Pack>true</Pack>
    <PackagePath>\</PackagePath>
  </Content>
</ItemGroup>
```

For more information, see [Including content in a package](../reference/msbuild-targets.md#including-content-in-a-package).

## Related videos

For videos about using NuGet for package management, see [.NET Package Management with NuGet for Beginners](/shows/dotnet-package-management-with-nuget-for-beginners) and [NuGet for Beginners](https://www.youtube.com/playlist?list=PLdo4fOcmZ0oVLvfkFk8O9h6v2Dcdh2bh_).

## Related content

To find out how to create a NuGet package with the Visual Studio .NET Framework, see [Create a package using the nuget.exe CLI](../create-packages/creating-a-package.md).

For more information about NuGet, see the following articles:

- [Create a NuGet package with the dotnet CLI](../create-packages/creating-a-package-dotnet-cli.md)
- [Publish NuGet packages](../nuget-org/publish-a-package.md)
- [Building pre-release packages](../create-packages/Prerelease-Packages.md)
- [Support multiple .NET frameworks in your project file](../create-packages/multiple-target-frameworks-project-file.md)
- [Package versioning](../concepts/package-versioning.md)
- [Creating localized NuGet packages](../create-packages/Creating-Localized-Packages.md)
- [Overview of upgrading .NET apps](/dotnet/core/porting/)
