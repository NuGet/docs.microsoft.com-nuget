---
title: Installing NuGet client tools
description: Guidance on installing client tools, the dotnet and nuget command-line interfaces (CLI), and the Package Manager for Visual Studio.
author: kraigb
ms.author: kraigb
manager: douge
ms.date: 04/09/2018
ms.topic: quickstart
---

# Installing NuGet client tools

> **Looking to install a package? See [Ways to install NuGet packages](consume-packages/ways-to-install-a-package.md).**

To work with NuGet, as a package consumer or creator, you can use [command-line interface (CLI) tools](#cli-tools) as well as [NuGet features in Visual Studio](#visual-studio). This article briefly outlines the capabilities of the different tools, how to install them, and their comparative [feature availability](#feature-availability). To get started using NuGet to consume packages, see [Install and use a package (.NET CLI)](quickstart/install-and-use-a-package-using-the-dotnet-cli.md) and [Install and use a package (Visual Studio)](quickstart/install-and-use-a-package-in-visual-studio.md). To get started creating NuGet packages, see [Create and publish a NET Standard package (dotnet CLI)](quickstart/create-and-publish-a-package-using-the-dotnet-cli.md) and [Create and publish a NET Standard package (Visual Studio)](quickstart/create-and-publish-a-package-using-visual-studio.md).

| Tool&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; | Description | Download&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; |
|:------------- |:-------------|:-----|
| [dotnet.exe](#dotnetexe-cli) | Included with the .NET Core SDK and provides core NuGet features on all platforms. | [.NET Core SDK](https://www.microsoft.com/net/download/) |
| [nuget.exe](#nugetexe-cli) | Provides all NuGet capabilities on Windows, provides most features on Mac and Linux when running under Mono. | [nuget.exe](https://dist.nuget.org/win-x86-commandline/latest/nuget.exe) |
| [Visual Studio](#visual-studio) | On Windows, provides NuGet capabilities through the Package Manager UI and Package Manager Console; included with .NET-related workloads. On Mac, provides certain features through the UI. In Visual Studio Code, NuGet features are provided through extensions. | [Visual Studio 2017](https://www.visualstudio.com/downloads/) |

The [MSBuild CLI](reference/msbuild-targets.md) also provides the ability to restore and create packages, which is primarily useful on build servers. MSBuild is not a general-purpose tool for working with NuGet.

## CLI tools

The two NuGet CLI tools are `dotnet.exe` and `nuget.exe`. See [feature availability](#feature-availability) for a comparison.

### dotnet.exe CLI

The .NET Core 2.0 CLI, `dotnet.exe`, works on all platforms (Windows, Mac, and Linux) and provides core NuGet features such as installing, restoring, and publishing packages. `dotnet` provides direct integration with .NET Core project files (such as `.csproj`), which is helpful in most scenarios. `dotnet` is also built directly for each platform and does not require you to install Mono.

Installation:

- On developer computers, install the [.NET Core SDK](https://aka.ms/dotnetcoregs).
- For build servers, follow the instructions on [Using .NET Core SDK and tools in Continuous Integration](/dotnet/core/tools/using-ci-with-cli).

For more information, see [.NET Core command-line interface tools](/dotnet/core/tools/index?tabs=netcore2x#tabpanel_fXL5YCOYDa_netcore2x).

### nuget.exe CLI

The NuGet CLI, `nuget.exe`, is the command-line utility for Windows that provides all NuGet capabilities; it can also be run on Mac OSX and Linux using [Mono](http://www.mono-project.com/docs/getting-started/install/) with some limitations. Unlike `dotnet`, the `nuget.exe` CLI does not affect project files and does not update `packages.config` when installing packages.

Installation:

[!INCLUDE [install-cli](includes/install-cli.md)]

> [!Tip]
> Use `nuget update -self` on Windows to update an existing nuget.exe to the latest version.

> [!Note]
> The latest recommended NuGet CLI is always available at `https://dist.nuget.org/win-x86-commandline/latest/nuget.exe`. For compatibility purposes  with older continuous integration systems, a previous URL, `https://nuget.org/nuget.exe` currently provides the [deprecated 2.8.6 CLI tool](https://github.com/NuGet/NuGetGallery/issues/5381).

## Visual Studio

- Visual Studio Code: NuGet capabilities are available through marketplace extensions, or use the `dotnet.exe` or `nuget.exe` CLI tools.

- Visual Studio for Mac: certain NuGet capabilities are built in directly. See [Including a NuGet package in your project](/visualstudio/mac/nuget-walkthrough) for a walkthrough. For other capabilities, use the `dotnet.exe` or `nuget.exe` CLI tools.

- Visual Studio on Windows: The **NuGet Package Manager** is included with Visual Studio 2012 and later. The Package Manager provides the [Package Manager UI](tools/package-manager-ui.md) and the [Package Manager Console](tools/package-manager-console.md), through which you can run most NuGet operations.
  - The Visual Studio 2017 installer includes the NuGet Package Manager with any workload that employs .NET. To install separately, or to verify that the Package Manager is installed, run the Visual Studio 2017 installer and check the option under **Individual Components > Code tools > NuGet package manager**.
  - The Package Manager UI and Console are unique to Visual Studio on Windows. They are not presently available on Visual Studio for Mac.
  - Visual Studio does not automatically include the `nuget.exe` CLI, which must be installed separately as described earlier.
  - Package Manager Console commands work only within Visual Studio on Windows and do not work within other PowerShell environments.
  - For Visual Studio 2010 and earlier, install the "NuGet Package Manager for Visual Studio" extension.
  - NuGet Extensions for Visual Studio 2013 and 2015 can also be downloaded from [https://dist.nuget.org/index.html](https://dist.nuget.org/index.html).
  - If you'd like to preview upcoming NuGet features, install the [Visual Studio 2017 Preview](https://www.visualstudio.com/vs/preview/), which works side-by-side with stable releases of Visual Studio. To report problems or share ideas for previews, open an issue on the [NuGet GitHub repository](https://github.com/Nuget/Home/issues).

## Feature availability

| Feature | dotnet CLI | nuget CLI (Windows) | nuget CLI (Mono) | Visual Studio (Windows) | Visual Studio for Mac |
| --- | --- | --- | --- | --- | --- |
| Search packages |  | &#10004; | &#10004; | &#10004; | &#10004; |
| Install/uninstall packages | &#10004;(1) | &#10004;(2) | &#10004; | &#10004; | &#10004; |
| Update packages | &#10004; | &#10004; | | &#10004; | &#10004; |
| Restore packages | &#10004; | &#10004; | &#10004;(3) | &#10004; | &#10004; |
| Manage package feeds (sources) | | &#10004; | &#10004; | &#10004; | &#10004; |
| Manage packages on a feed | &#10004;(1) | &#10004; | &#10004; | | |
| Set API keys for feeds | | &#10004; | &#10004; | | |
| Create packages(4) | &#10004; | &#10004; | &#10004;(5) | &#10004; | |
| Publish packages | &#10004;(1) | &#10004; | &#10004; | &#10004; |  |
| Replicate packages |  | &#10004; | &#10004; | | |
| Manage *global-package* and cache folders | &#10004; | &#10004; | &#10004; | | |
| Manage NuGet configuration | | &#10004; | &#10004; | | |

(1) Packages on nuget.org only

(2) Does not affect project files; use `dotnet.exe` instead.

(3) Works only with `packages.config` file and not with solution (`.sln`) files.

(4) Various advanced package features are available through the CLI only as they aren't represented in the Visual Studio UI tools.

(5) Works with `.nuspec` files but not with project files.

### Related topics

- [dotnet commands](tools/dotnet-commands.md)
- [NuGet CLI reference](tools/nuget-exe-cli-reference.md)
- [Package Manager UI reference](tools/package-manager-ui.md)
- [Package Manager Console reference](tools/package-manager-console.md)
- [Package Manager Console PowerShell reference](tools/powershell-reference.md)
- [Creating a package](create-packages/creating-a-package.md)
- [Publishing a Package](create-packages/publish-a-package.md)

Developers working on Windows can also explore the [NuGet Package Explorer](https://github.com/NuGetPackageExplorer/NuGetPackageExplorer), an open-source, stand-alone tool to visually explore, create, and edit NuGet packages. It's very helpful, for example, to make experimental changes to a package structure without rebuilding the package.
