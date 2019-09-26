---
title: Create and publish a .NET Framework NuGet package using Visual Studio on Windows
description: A walkthrough tutorial on creating and publishing a .NET Framework NuGet package using Visual Studio on Windows.
author: karann-msft
ms.author: karann
ms.date: 05/13/2018
ms.topic: quickstart
---

# Quickstart: Create and publish a package using Visual Studio (.NET Framework, Windows)

Creating a NuGet package from a .NET Framework Class Library involves creating the DLL in Visual Studio on Windows, then using the nuget.exe command line tool to create and publish the package.

> [!Note]
> This Quickstart applies to Visual Studio 2017 and higher versions for Windows only. Visual Studio for Mac does not include the capabilities described here. Use the [dotnet CLI tools](create-and-publish-a-package-using-the-dotnet-cli.md) instead.

## Prerequisites

1. Install any edition of Visual Studio 2017 or higher from [visualstudio.com](https://www.visualstudio.com/) with any .NET-related workload. Visual Studio 2017 automatically includes NuGet capabilities when a .NET workload is installed.

1. Install the `nuget.exe` CLI by downloading it from [nuget.org](https://dist.nuget.org/win-x86-commandline/latest/nuget.exe), saving that `.exe` file to a suitable folder, and adding that folder to your PATH environment variable.

1. [Register for a free account on nuget.org](https://www.nuget.org/users/account/LogOn?returnUrl=%2F) if you don't have one already. Creating a new account sends a confirmation email. You must confirm the account before you can upload a package.

## Create a class library project

You can use an existing .NET Framework Class Library project for the code you want to package, or create a simple one as follows:

1. In Visual Studio, choose **File > New > Project**, select the **Visual C#** node, select the "Class Library (.NET Framework)" template, name the project AppLogger, and click **OK**.

1. Right-click on the resulting project file and select **Build** to make sure the project was created properly. The DLL is found within the Debug folder (or Release if you build that configuration instead).

Within a real NuGet package, of course, you implement many useful features with which others can build applications. You can also set the target frameworks however you like. For example, see the guides for [UWP](../guides/create-uwp-packages.md) and [Xamarin](../guides/create-packages-for-xamarin.md).

For this walkthrough, however, you won't write any additional code because a class library from the template is sufficient to create a package. Still, if you'd like some functional code for the package, use the following:

```cs
using System;

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

> [!Tip]
> Unless you have a reason to choose otherwise, .NET Standard is the preferred target for NuGet packages, as it provides compatibility with the widest range of consuming projects. See [Create and publish a package using Visual Studio (.NET Standard)](create-and-publish-a-package-using-visual-studio.md).

## Configure project properties for the package

A NuGet package contains a manifest (a `.nuspec` file), that contains relevant metadata such as the package identifier, version number, description, and more. Some of these can be drawn from the project properties directly, which avoids having to separately update them in both the project and the manifest. This section describes where to set the applicable properties.

1. Select the **Project > Properties** menu command, then select the **Application** tab.

1. In the **Assembly name** field, give your package a unique identifier.

    > [!Important]
    > You must give the package an identifier that's unique across nuget.org or whatever host you're using. For this walkthrough we recommend including "Sample" or "Test" in the name as the later publishing step does make the package publicly visible (though it's unlikely anyone will actually use it).
    >
    > If you attempt to publish a package with a name that already exists, you see an error.

1. Select the **Assembly Information...** button, which brings up a dialog box in which you can enter other properties that carry into the manifest (see [.nuspec file reference - replacement tokens](../reference/nuspec.md#replacement-tokens)). The most commonly used fields are **Title**, **Description**, **Company**, **Copyright**, and **Assembly version**. These properties ultimately appear with your package on a host like nuget.org, so make sure they're fully descriptive.

    ![Assembly information in a .NET Framework project in Visual Studio](media/qs_create-vs-01b-project-properties.png)

1. Optional: to see and edit the properties directly, open the `Properties/AssemblyInfo.cs` file in the project.

1. When the properties are set, set the project configuration to **Release** and rebuild the project to generate the updated DLL.

## Generate the initial manifest

With a DLL in hand and project properties set, you now use the `nuget spec` command to generate an initial `.nuspec` file from the project. This step includes the relevant replacement tokens to draw information from the project file.

You run `nuget spec` only once to generate the initial manifest. When updating the package, you either change values in your project or edit the manifest directly.

1. Open a command prompt and navigate to the project folder containing `AppLogger.csproj` file.

1. Run the following command: `nuget spec AppLogger.csproj`. By specifying a project, NuGet creates a manifest that matches the name of the project, in this case `AppLogger.nuspec`. It also include replacement tokens in the manifest.

1. Open `AppLogger.nuspec` in a text editor to examine its contents, which should appear as follows:

    ```xml
    <?xml version="1.0"?>
    <package >
      <metadata>
        <id>Package</id>
        <version>1.0.0</version>
        <authors>YourUsername</authors>
        <owners>YourUsername</owners>
        <license type="expression">MIT</license>
        <projectUrl>http://PROJECT_URL_HERE_OR_DELETE_THIS_LINE</projectUrl>
        <iconUrl>http://ICON_URL_HERE_OR_DELETE_THIS_LINE</iconUrl>
        <requireLicenseAcceptance>false</requireLicenseAcceptance>
        <description>Package description</description>
        <releaseNotes>Summary of changes made in this release of the package.</releaseNotes>
        <copyright>Copyright 2019</copyright>
        <tags>Tag1 Tag2</tags>
      </metadata>
    </package>
    ```

## Edit the manifest

1. NuGet produces an error if you try to create a package with default values in your `.nuspec` file, so you must edit the following fields before proceeding. See [.nuspec file reference - optional metadata elements](../reference/nuspec.md#optional-metadata-elements) for a description of how these are used.

    - licenseUrl
    - projectUrl
    - iconUrl
    - releaseNotes
    - tags

1. For packages built for public consumption, pay special attention to the **Tags** property, as tags help others find your package on sources like nuget.org and understand what it does.

1. You can also add any other elements to the manifest at this time, as described on [.nuspec file reference](../reference/nuspec.md).

1. Save the file before proceeding.

## Run the pack command

1. From a command prompt in the folder containing your `.nuspec` file, run the command `nuget pack`.

1. NuGet generates a `.nupkg` file in the form of *identifier-version.nupkg*, which you'll find in the current folder.

## Publish the package

Once you have a `.nupkg` file, you publish it to nuget.org using `nuget.exe` with an API key acquired from nuget.org. For nuget.org you must use `nuget.exe` 4.1.0 or higher.

[!INCLUDE [publish-notes](includes/publish-notes.md)]

### Acquire your API key

[!INCLUDE [publish-api-key](includes/publish-api-key.md)]

### Publish with nuget push

1. Open a command line and change to the folder containing the `.nupkg` file.

1. Run the following command, specifying your package name and replacing the key value with your API key:

    ```cli
    nuget push AppLogger.1.0.0.nupkg qz2jga8pl3dvn2akksyquwcs9ygggg4exypy3bhxy6w6x6 -Source https://api.nuget.org/v3/index.json
    ```

1. nuget.exe displays the results of the publishing process:

    ```output
    Pushing AppLogger.1.0.0.nupkg to 'https://www.nuget.org/api/v2/package'...
        PUT https://www.nuget.org/api/v2/package/
        Created https://www.nuget.org/api/v2/package/ 6829ms
    Your package was pushed.
    ```

See [nuget push](../reference/cli-reference/cli-ref-push.md).

### Publish errors

[!INCLUDE [publish-errors](includes/publish-errors.md)]

### Manage the published package

[!INCLUDE [publish-manage](includes/publish-manage.md)]

## Next steps

Congratulations on creating your first NuGet package!

> [!div class="nextstepaction"]
> [Create a Package](../create-packages/creating-a-package.md)

To explore more that NuGet has to offer, select the links below.

- [Publish a Package](../nuget-org/publish-a-package.md)
- [Pre-release Packages](../create-packages/Prerelease-Packages.md)
- [Support multiple target frameworks](../create-packages/supporting-multiple-target-frameworks.md)
- [Package versioning](../concepts/package-versioning.md)
- [Creating localized packages](../create-packages/creating-localized-packages.md)
