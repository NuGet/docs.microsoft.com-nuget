---
title: "Quickstart: Create and publish a package using Visual Studio (.NET Framework, Windows)"
description: A quickstart that shows how to create and publish a .NET Framework NuGet package using Visual Studio on Windows.
author: JonDouglas
ms.author: jodou
ms.date: 08/21/2023
ms.topic: quickstart
---

# Quickstart: Create and publish a package using Visual Studio (.NET Framework, Windows)

With Microsoft Visual Studio, you can create a NuGet package from a .NET Framework class library, and then publish it to nuget.org using the NuGet CLI tool.

The quickstart is for Windows users only. If you're using Visual Studio for Mac, see [dotnet CLI tools](create-and-publish-a-package-using-the-dotnet-cli.md) instead.

## Prerequisites

- Install Visual Studio 2022 for Windows with any .NET-related workload.

  You can install the 2022 Community edition for free from [visualstudio.microsoft.com](https://visualstudio.microsoft.com/), or use the Professional or Enterprise edition.

  Visual Studio 2017 and higher automatically includes NuGet capabilities when a .NET workload is installed.

- [Register for a free account on nuget.org](../nuget-org/individual-accounts.md#add-a-new-individual-account) if you don't have one already. You must register and confirm the account before you can upload a NuGet package.

- Install the NuGet CLI by downloading it from [nuget.org](https://dist.nuget.org/win-x86-commandline/latest/nuget.exe). Add the *nuget.exe* file to a suitable folder, and add that folder to your PATH environment variable.

## Create a class library project

To create a class library project, follow these steps:

1. In Visual Studio, select **File** > **New** > **Project**.

1. In the **Create a new project** window, select **C#**, **Windows**, and **Library** in the dropdown lists.

1. In the resulting list of project templates, select **Class Library (.NET Framework)**, and then select **Next**.

1. In the **Configure your new project** window, enter *AppLogger* for the **Project name**, and then select **Create**.

1. To ensure the project was created properly, select **Build** > **Build Solution**. The DLL is found within the Debug folder (or Release if you build that configuration instead).

1. (Optional) For this quickstart, you don't need to write any additional code for the NuGet package because the template class library is sufficient to create a package. However, if you'd like some functional code for this sample package, include the following code:

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

   Within a real-world NuGet package, you'd likely implement many useful features with which others can build applications. You can also set the target frameworks. For an example, see [UWP](../guides/create-uwp-packages.md).

## Configure project properties for the package

A NuGet package includes a manifest (a `.nuspec` file), that contains relevant metadata such as the package identifier, version number, description, and more. Some of this metadata can be drawn from the project properties directly, which avoids having to separately update them in both the project and the manifest. The following steps describe how to set the applicable properties:

1. Select **Project > Properties**, and then select the **Application** tab.

1. For **Assembly name**, give your package a unique identifier. If you attempt to publish a package with a name that already exists, you see an error.

     > [!IMPORTANT]
    > You must give the package an identifier that's unique across nuget.org or whatever host you're using. Otherwise, an error occurs. For this quickstart we recommend including *Sample* or *Test* in the name because the publishing step makes the package publicly visible.

1. Select **Assembly Information**, which displays a dialog box in which you can enter other properties that carry into the manifest (see [Replacement tokens](../reference/nuspec.md#replacement-tokens)). The most commonly used fields are **Title**, **Description**, **Company**, **Copyright**, and **Assembly version**. Because these properties appear with your package on a host like nuget.org after you publish it, make sure they're fully descriptive.

    :::image type="content" source="media/qs-create-vs-assembly-information.png" alt-text="Screenshot showing the Assembly Information page in a .NET Framework project in Visual Studio.":::

1. (Optional) To see and edit the properties directly, open the *Properties/AssemblyInfo.cs* file in the project by selecting select **Project** > **Edit Project File**.

1. After you've set these properties, set the **Active solution configuration** in **Build** > **Configuration Manager** to **Release** and rebuild the project to generate the updated DLL.

## Generate the initial manifest

After you've set the project properties and created the DLL, you can now generate an initial *.nuspec* file from the project. This step includes the relevant replacement tokens to draw information from the project file.

Run `nuget spec` only once to generate the initial manifest. If you update the package, either change values in your project, or edit the manifest directly:

1. With your project open in **Solution Explorer**, open a command prompt by selecting **Tools** > **Command Line** >  **Developer Command Prompt**.

   The command prompt opens in your project directory where the `AppLogger.csproj` file is located.

1. Run the following command: `nuget spec AppLogger.csproj`.

   NuGet creates a manifest that matches the name of the project, in this case `AppLogger.nuspec`. It also includes replacement tokens in the manifest.

1. Open `AppLogger.nuspec` in a text editor to examine its contents, which will be similar to the following code:

    ```xml
    <?xml version="1.0"?>
    <package >
      <metadata>
        <id>Package</id>
        <version>1.0.0</version>
        <authors>Your username</authors>
        <owners>Your username</owners>
        <license type="expression">MIT</license>
        <!-- <icon>icon.png</icon> -->
        <projectUrl>http://PROJECT_URL_HERE_OR_DELETE_THIS_LINE</projectUrl>
        <requireLicenseAcceptance>false</requireLicenseAcceptance>
        <description>Package description</description>
        <releaseNotes>Summary of changes made in this release of the package.</releaseNotes>
        <copyright>Copyright 2022</copyright>
        <tags>Tag1 Tag2</tags>
      </metadata>
    </package>
    ```

## Edit the manifest

1. Edit the following properties before proceeding. Otherwise, if you try to create a NuGet package with the default values in your `.nuspec` file, an error occurs. For information about these properties, see [Optional metadata elements](../reference/nuspec.md#optional-metadata-elements):

    - licenseUrl
    - projectUrl
    - releaseNotes
    - tags

1. For packages built for public consumption, pay special attention to the **Tags** property, as tags help others find your package and understand what it does.

1. You can also add any other elements to the manifest at this time, as described in [.nuspec file reference](../reference/nuspec.md).

1. Save the file before proceeding.

## Run the pack command

1. With your project open in **Solution Explorer**, open a command prompt by selecting **Tools** > **Command Line** >  **Developer Command Prompt**.

   The command prompt opens in your project directory.

1. Run the following command: `nuget pack`.

   NuGet generates a *.nupkg* file in the form of *identifier.version.nupkg* in the current folder.

## Publish the package

After you've created a *.nupkg* file, publish it to nuget.org by using the NuGet CLI with an API key acquired from nuget.org. For nuget.org, you must use `nuget.exe` 4.1.0 or higher.

If you'd like to test and validate your package before publishing it a public gallery, you can upload it to a test environment like [int.nugettest.org](https://int.nugettest.org) instead of nuget.org. Note that packages uploaded to int.nugettest.org may not be preserved.

[!INCLUDE [publish-notes](includes/publish-notes.md)]

### Acquire your API key

[!INCLUDE [publish-api-key](includes/publish-api-key-with-link.md)]

### Publish with the NuGet CLI

Using the NuGet CLI (*nuget.exe*) is an alternative to using the .NET CLI:

1. Open a command prompt and change to the folder containing the *.nupkg* file.

1. Run the following command. Replace \<package filename> with the file name of your package and replace \<api key value> with your API key. The package filename is a concatenation of your package ID and version number with a *.nupkg* extension. For example, *AppLogger.1.0.0.nupkg*:

    ```cli
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

### Publish errors

[!INCLUDE [publish-errors](includes/publish-errors.md)]

### Manage the published package

[!INCLUDE [publish-manage](includes/publish-manage.md)]

## Next steps

Congratulations on creating a NuGet package by using the Visual Studio .NET Framework. Advance to the next article to learn how to create a NuGet package with the NuGet CLI.

> [!div class="nextstepaction"]
> [Create a package using the NuGet CLI](../create-packages/creating-a-package.md)

To explore more that NuGet has to offer, see the following articles:

- [Publish a package](../nuget-org/publish-a-package.md)
- [Build a prerelease package](../create-packages/Prerelease-Packages.md)
- [Support multiple target frameworks](../create-packages/supporting-multiple-target-frameworks.md)
- [Package versioning](../concepts/package-versioning.md)
- [Creating a localized package](../create-packages/creating-localized-packages.md)
