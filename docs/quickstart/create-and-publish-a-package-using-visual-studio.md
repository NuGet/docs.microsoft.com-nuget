---
title: "Quickstart: Create and publish a NuGet package using Visual Studio (Windows only)"
description: A quickstart that shows how to create and publish a .NET NuGet package using Visual Studio for Windows.
author: JonDouglas
ms.author: jodou
ms.date: 08/29/2022
ms.topic: quickstart
---

# Quickstart: Create and publish a NuGet package using Visual Studio (Windows only)

With Microsoft Visual Studio, you can create a NuGet package from a .NET class library, and then publish it to nuget.org using a CLI tool.

The quickstart is for Windows users only. If you're using Visual Studio for Mac, see [Create a NuGet package from existing library projects](/xamarin/cross-platform/app-fundamentals/nuget-multiplatform-libraries/existing-library) or use the [.NET CLI](create-and-publish-a-package-using-the-dotnet-cli.md).

## Prerequisites

- Install Visual Studio 2022 for Windows with a .NET Core-related workload.

  You can install the 2022 Community edition for free from [visualstudio.microsoft.com](https://visualstudio.microsoft.com/), or use the Professional or Enterprise edition.

  Visual Studio 2017 and later automatically includes NuGet capabilities when you install a .NET-related workload.

- Install the .NET CLI, if it's not already installed.

   For Visual Studio 2017 and later, the .NET CLI is automatically installed with any .NET Core-related workload. Otherwise, install the [.NET Core SDK](https://www.microsoft.com/net/download/) to get the .NET CLI. The .NET CLI is required for .NET projects that use the [SDK-style format](../resources/check-project-format.md) (SDK attribute). The default .NET class library template in Visual Studio 2017 and later uses the SDK attribute.

   > [!IMPORTANT]
   > If you're working with a non-SDK-style project, follow the procedures in [Create and publish a .NET Framework package (Visual Studio)](create-and-publish-a-package-using-visual-studio-net-framework.md) instead to create and publish the package. For this article, the .NET CLI is recommended. Although you can publish any NuGet package using the NuGet CLI, some of the steps in this article are specific to SDK-style projects and the .NET CLI. The NuGet CLI is used for [non-SDK-style projects](../resources/check-project-format.md) (typically .NET Framework).

- [Register for a free account on nuget.org](../nuget-org/individual-accounts.md#add-a-new-individual-account) if you don't have one already. You must register and confirm the account before you can upload a NuGet package.

- Install the NuGet CLI by downloading it from [nuget.org](https://dist.nuget.org/win-x86-commandline/latest/nuget.exe). Add the *nuget.exe* file to a suitable folder, and add that folder to your PATH environment variable.

## Create a class library project

You can use an existing .NET Class Library project for the code you want to package, or create one as follows:

1. In Visual Studio, select **File** > **New** > **Project**.

1. In the **Create a new project** window, select **C#**, **Windows**, and **Library** in the dropdown lists.

1. In the resulting list of project templates, select **Class Library** (with the description, *A project for creating a class library that targets .NET or .NET Standard*), and then select **Next**.

1. In the **Configure your new project** window, enter *AppLogger* for the **Project name**, and then select **Next**.

1. In the **Additional information** window, select an appropriate **Framework**, and then select **Create**.

   If you're unsure which framework to select, the latest is a good choice, and can be easily changed later. For information about which framework to use, see [When to target .NET 5.0 or .NET 6.0 vs. .NET Standard](/dotnet/standard/net-standard#when-to-target-net50-or-net60-vs-netstandard).

1. To ensure the project was created properly, select **Build** > **Build Solution**. The DLL is found within the Debug folder (or Release if you build that configuration instead).

1. (Optional) For this quickstart, you don't need to write any additional code for the NuGet package because the template class library is sufficient to create a package. However, if you'd like some functional code for the package, include the following code:

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

After you've created your project, you can configure the NuGet package properties by following these steps:

1. Select your project in **Solution Explorer**, and then select **Project** > **\<project name> Properties**, where \<project name> is the name of your project.

1. Expand the **Package** node, and then select **General**.

   The **Package** node appears only for SDK-style projects in Visual Studio. If you're' targeting a non-SDK style project (typically .NET Framework), either [migrate the project](../consume-packages/migrate-packages-config-to-package-reference.md), or see [Create and publish a .NET Framework package](create-and-publish-a-package-using-visual-studio-net-framework.md) for step-by-step instructions.

    :::image type="content" source="media/qs-create-vs-project-properties.png" alt-text="Screenshot showing NuGet package properties in a Visual Studio project.":::

1. For packages built for public consumption, pay special attention to the **Tags** property, as tags help others find your package and understand what it does.

1. Give your package a unique **Package ID** and fill out any other desired properties. For a table that shows how MSBuild properties (SDK-style projects) map to *.nuspec* file properties, see [pack targets](../reference/msbuild-targets.md#pack-target). For a description of *.nuspec* file properties, see the [.nuspec file reference](../reference/nuspec.md). All of these properties go into the `.nuspec` manifest that Visual Studio creates for the project.

    > [!IMPORTANT]
    > You must give the package an identifier that's unique across nuget.org or whatever host you're using. Otherwise, an error occurs. For this quickstart we recommend including *Sample* or *Test* in the name because the publishing step makes the package publicly visible.

1. (Optional) To see the properties directly in the *AppLogger.csproj* project file, select **Project** > **Edit Project File**.

   The **AppLogger.csproj** tab loads.

   This option is available starting in Visual Studio 2017 for projects that use the SDK-style attribute. For earlier Visual Studio versions, you must select **Project** > **Unload Project** before you can edit the project file.

## Run the pack command

To create a NuGet package from your project, follow these steps:

1. Select **Build** > **Configuration Manager**, and then set the **Active solution configuration** to **Release**.

1. Select the AppLogger project in **Solution Explorer**, and then select **Build** > **Pack**.

    Visual Studio builds the project and creates the *.nupkg* file.

1. Examine the **Output** window for details, which contains the path to the package file. In this example, the built assembly is in *bin\Release\net6.0* as befits a .NET 6.0 target:

    ```output
    1>------ Build started: Project: AppLogger, Configuration: Release Any CPU ------
    1>AppLogger -> d:\proj\AppLogger\AppLogger\bin\Release\net6.0\AppLogger.dll
    1>Successfully created package 'd:\proj\AppLogger\AppLogger\bin\Release\AppLogger.1.0.0.nupkg'.
    ========== Build: 1 succeeded, 0 failed, 0 up-to-date, 0 skipped ==========
    ```

1. If you don't see the **Pack** command on the menu, your project is probably not an SDK-style project, and you need to use the NuGet CLI. Either [migrate the project](../consume-packages/migrate-packages-config-to-package-reference.md) and use .NET CLI, or see [Create and publish a .NET Framework package](create-and-publish-a-package-using-visual-studio-net-framework.md) for step-by-step instructions.

### (Optional) Generate package on build

You can configure Visual Studio to automatically generate the NuGet package when you build the project:

1. Select your project in **Solution Explorer**, and then select **Project** > **\<project name> Properties**, where \<project name> is the name of your project (AppLogger in this case).

1. Expand the **Package** node, select **General**, and then select **Generate NuGet package on build**.

    :::image type="content" source="media/qs-create-vs-generate-on-build.png" alt-text="Screenshot showing package properties with Generate NuGet package on build selected.":::

> [!NOTE]
> When you automatically generate the package, the extra time to pack increases the overall build time for your project.

### (Optional) Pack with MSBuild

As an alternative to using the **Pack** menu command, NuGet 4.x+ and MSBuild 15.1+ supports a `pack` target when the project contains the necessary package data:

1. With your project open in **Solution Explorer**, open a command prompt by selecting **Tools** > **Command Line** >  **Developer Command Prompt**.

   The command prompt opens in your project directory.

1. Run the following command: `msbuild -t:pack`.

For more information, see [Create a package using MSBuild](../create-packages/creating-a-package-msbuild.md).

## Publish the package

After you've created a *.nupkg* file, publish it to nuget.org by using either the .NET CLI or the NuGet CLI, along with an API key acquired from nuget.org.

[!INCLUDE [publish-notes](includes/publish-notes.md)]

### Acquire your API key

Before you publish your NuGet package, create an API key:

[!INCLUDE [publish-api-key](includes/publish-api-key.md)]

### Publish with the .NET CLI or NuGet CLI

Each of the following CLI tools allows you to push a package to the server and publish it. Select the tab for your CLI tool, either **.NET CLI** or **NuGet CLI**.

#### [.NET CLI](#tab/netcore-cli)

Using the .NET CLI (*dotnet.exe*) is the recommended alternative to using the NuGet CLI.

[!INCLUDE [publish-dotnet](includes/publish-dotnet.md)]

#### [NuGet CLI](#tab/nuget)

Using the NuGet CLI (*nuget.exe*) is an alternative to using the .NET CLI:

1. Open a command prompt and change to the folder containing the *.nupkg* file.

1. Run the following command. Replace \<package filename> with the file name of your package and replace \<api key value> with your API key.

   The NuGet CLI generates a *.nupkg* file in the form of *package ID-version.nupkg*. For example, *AppLogger.1.0.0.nupkg*:

    ```nuget
    nuget push <package filename> <api key value> -Source https://api.nuget.org/v3/index.json
    ```

    The result of the publishing process is displayed as follows:

    ```output
    Pushing <package filename> to 'https://www.nuget.org/api/v2/package'...
        PUT https://www.nuget.org/api/v2/package/
        Created https://www.nuget.org/api/v2/package/ 6829ms
    Your package was pushed.
    ```

For more information, see [nuget push](../reference/cli-reference/cli-ref-push.md).

---

### Publish errors

[!INCLUDE [publish-errors](includes/publish-errors.md)]

### Manage the published package

[!INCLUDE [publish-manage](includes/publish-manage.md)]

## Add a readme or another file

To directly specify files to include in the package, edit the project file and add the `content` property:

```xml
<ItemGroup>
  <Content Include="readme.txt">
    <Pack>true</Pack>
    <PackagePath>\</PackagePath>
  </Content>
</ItemGroup>
```

In this example, the property specifies a file named *readme.txt* in the project root. Visual Studio displays the contents of that file as plain text immediately after it installs the package. Readme files aren't displayed for packages installed as dependencies. For example, here's the readme for the HtmlAgilityPack package:

```output
1 ----------------------------------------------------
2 ---------- Html Agility Pack Nuget Readme ----------
3 ----------------------------------------------------
4
5 ----Silverlight 4 and Windows Phone 7.1+ projects-----
6 To use XPATH features: System.Xml.Xpath.dll from the 3 Silverlight 4 SDK must be referenced. 
7 This is normally found at 
8 %ProgramFiles(x86)%\Microsoft SDKs\Microsoft SDKs\Silverlight\v4.0\Libraries\Client 
9 or 
10 %ProgramFiles%\Microsoft SDKs\Microsoft SDKs\Silverlight\v4.0\Libraries\Client
11
12 ----Silverlight 5 projects-----
13 To use XPATH features: System.Xml.Xpath.dll from the Silverlight 5 SDK must be referenced. 
14 This is normally found at 
15 %ProgramFiles(x86)%\Microsoft SDKs\Microsoft SDKs\Silverlight\v5.0\Libraries\Client 
16 or 
17 %ProgramFiles%\Microsoft SDKs\Microsoft SDKs\Silverlight\v5.0\Libraries\Client
```

> [!NOTE]
> If you only add *readme.txt* at the project root without including it in the `content` property of the project file, it won't be included in the package.

## Related video

> [!VIDEO https://docs.microsoft.com/shows/NuGet-101/Create-and-Publish-a-NuGet-Package-with-Visual-Studio-4-of-5/player]

Find more NuGet videos on [Channel 9](/shows/NuGet-101/) and [YouTube](https://www.youtube.com/playlist?list=PLdo4fOcmZ0oVLvfkFk8O9h6v2Dcdh2bh_).

Congratulations on creating a NuGet package by using a Visual Studio .NET class library. Advance to the next article to learn how to create a NuGet package with the Visual Studio .NET Framework.

> [!div class="nextstepaction"]
> [Create a package using the NuGet CLI](../create-packages/creating-a-package.md)

To explore more that NuGet has to offer, see the following articles:

- [Create a NuGet package](../create-packages/creating-a-package-dotnet-cli.md)
- [Publish a package](../nuget-org/publish-a-package.md)
- [Build a prerelease package](../create-packages/Prerelease-Packages.md)
- [Support multiple .NET Framework versions](../create-packages/multiple-target-frameworks-project-file.md)
- [Package versioning](../concepts/package-versioning.md)
- [Create localized NuGet packages](../create-packages/creating-localized-packages.md)
- [Porting to .NET Core from .NET Framework](/dotnet/articles/core/porting/index)
