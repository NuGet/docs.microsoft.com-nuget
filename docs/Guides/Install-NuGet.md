--- 
# required metadata 
 
title: Installing NuGet | Microsoft Docs
author: kraigb 
ms.author: kraigb 
manager: ghogen 
ms.date: 7/17/2017
ms.topic: article 
ms.prod: nuget 
#ms.service: 
ms.technology: null 
ms.assetid: 683b8b34-a6f4-4d56-b9cd-2483bfbad1ad 
 
# optional metadata 
 
description: Guidance on installing the NuGet command-line interface (CLI) and the NuGet Package Manager for Visual Studio.
keywords: nuget.exe CLI, NuGet package manager, NuGet package manager console, NuGet for Visual Studio, NuGet beta channel
#ROBOTS: 
#audience: 
#ms.devlang: 
ms.reviewer:  
- karann 
- unnir 
#ms.suite:  
#ms.tgt_pltfrm: 
#ms.custom: 
 
---

# Installing NuGet

There are two primary tools available to help you build, publish and consume NuGet packages:

1. The [**NuGet CLI**](#nuget-cli) is the command-line utility for Windows that provides all NuGet capabilities; it can also be run on Mac OSX and Linux using Mono, or through the .NET Core CLI (`dotnet`).
1. The [**NuGet Package Manager in Visual Studio**](#nuget-package-manager-in-visual-studio) (Windows only) is a GUI tool for managing packages and includes a PowerShell console through which you can use certain NuGet commands directly within Visual Studio. The Package Manager UI and Console are both included with Visual Studio (on Windows) 2012 and later and can be installed manually for earlier versions.

    With Visual Studio for Mac, NuGet capabilities are built in directly. See [Including a NuGet package in your project](https://docs.microsoft.com/visualstudio/mac/nuget-walkthrough) for a walkthrough.

The NuGet CLI and Package Manager both support the following operations:

- Search packages
- Install packages
- Update packages
- Uninstall packages
- Restore packages (UI only in the Package Manager)
- Manage NuGet sources

The following capabilities are supported only in the NuGet CLI:

- Manage packages (nuget.org or private feed)
- Create packages 
- Publish packages
- Manage Nuget.Config
- Manage the NuGet cache
- Replication a package

> [!Note]
> Another good tool is the [NuGet Package Explorer](https://github.com/NuGetPackageExplorer/NuGetPackageExplorer), an open-source, stand-alone tool to visually explore, create, and edit NuGet packages. It's very helpful, for example, to make experimental changes to a package structure without having to rebuild the package each time.
> The cross-platform [.NET Core CLI](https://docs.microsoft.com/dotnet/articles/core/tools/index#installation) toolchain, used for developing .NET Core applications, supports several NuGet commands, such as delete, locals, push, pack, and restore. 


## NuGet CLI

The NuGet command-line interface provides access to all NuGet capabilities, and can be run on Windows, Mac OSX, and Linux as described in the following sections.

### Windows

On Windows, install the NuGet CLI using any of the following methods:

1. **nuget.org**: Various versions of `nuget.exe` are available on [nuget.org/downloads](https://nuget.org/downloads). Each download link points directly to an `.exe` file, so be sure to right-click and save the file to your computer rather than running it from the browser. If desired, add the save location to your PATH environment variable so you can NuGet from anywhere.

> [!Note]
> With NuGet 1.4+, you can use `nuget update -self` to update your existing nuget.exe to the latest version.


1. **Chocolatey**: Install the [NuGet.CommandLine](http://chocolatey.org/packages/NuGet.CommandLine) Chocolatey package using the [Chocolatey](http://chocolatey.org) client. 

    ```
    choco install nuget.commandline
    ```
    
1. **Visual Studio**: Install the [NuGet.CommandLine](http://www.nuget.org/packages/NuGet.CommandLine/) package from the Package Manager Console in Visual Studio.

> [!Note]
> **For NuGet 2.x users**: Because of breaking changes introduced in NuGet 3.2, [https://nuget.org/nuget.exe](https://nuget.org/nuget.exe) points to the latest stable NuGet 2.x release to prevent continuous integration systems from potentially breaking.

<a name="compatibility-with-mono"></a>

## Mac OSX and Linux

On Mac OSX and Linux, there are two ways to run the NuGet CLI:

1. Install the [.NET Core SDK](https://www.microsoft.com/net/download/core), which includes the core NuGet capabilities. Downloads are also listed on [github.com/dotnet/cli](https://github.com/dotnet/cli). If you need fuller capabilities, then use the second option below to use `nuget.exe` with Mono.

1. Install [Mono](http://www.mono-project.com/docs/getting-started/install/) and then use the `nuget.exe` command-line executable for Windows (version 3.2 and later) from [nuget.org/downloads](https://nuget.org/downloads). Running NuGet on Mono is subject to the following limitations:

    - Commands tested to work:
        - config
        - delete
        - help
        - install
        - list
        - push
        - setApiKey
        - sources
        - spec

    - Partially-working commands:
        - pack: works with `.nuspec` files but not with project files.
        - restore: works with `packages.config` and `project.json` files but not with solution (`.sln`) files.

    - Commands that do not work:
        - update


### Related topics

- [NuGet CLI reference](../tools/nuget-exe-cli-reference.md)
- [Creating a package](../create-packages/creating-a-package.md)
- [Publishing a Package](../create-packages/publish-a-package.md)


## NuGet Package Manager in Visual Studio

The NuGet Package Manager is included in every edition of Visual Studio on Windows, 2012 and later. It includes the Package Manager UI ([reference](../tools/package-manager-ui.md)), and the Package Manager Console through which you can access tools that come with certain packages ([reference](../tools/package-manager-console.md)).

> [!Note]
> The console requires [PowerShell 2.0](http://support.microsoft.com/kb/968929), which will already be installed on Windows 7 or higher and Windows Server 2008 R2 or higher.
>
> Package Manager Console commands also work only within Visual Studio on Windows. Use the NuGet CLI outside of that environment, including with Visual Studio for Mac.


### Package Manager installation for Visual Studio 2010 and earlier

*These steps are not necessary for Visual Studio 2012 and later, which already include the Package Manager.*

1. In Visual Studio, click **Tools > Extension and Updates**.
1. Navigate to **Online**, then search for "NuGet Package Manager for Visual Studio" and click **Download**.
1. In the Installer dialog box, click **Install**.
1. When installation is complete, restart Visual Studio.

> [!Tip]
> If you're unable to use the **Extensions and Updates** dialog in Visual Studio (for example, its blocked by a proxy), you can download extensions for Visual Studio 2013 and 2015 directly at [https://dist.nuget.org/index.html](https://dist.nuget.org/index.html).

### Updating the Package Manager

For Visual Studio 2015 Update 2 and later, the Package Manager is automatically updated to the latest stable release.

For earlier versions of Visual Studio, select the **Tools > Extensions and Updates** command and click on the **Updates** tab to see if a new version of the Package Manager is available.  

### NuGet previews

If you'd like to preview upcoming NuGet features, install the [Visual Studio 2017 Preview](https://www.visualstudio.com/vs/preview/), which works side-by-side with stable releases of Visual Studio.

Note that the previous NuGet Beta Channel (`https://dotnet.myget.org/F/nuget-beta/vsix/`) for Visual Studio 2015 is no longer used.

To report problems with any release of NuGet or to share ideas, open an issue on the [NuGet GitHub repository](https://github.com/Nuget/Home).

### Related topics

- [Package Manager UI reference](../tools/package-manager-ui.md)
- [Package Manager Console reference](../tools/package-manager-console.md)
- [Package Manager Console PowerShell reference](../tools/powershell-reference.md)