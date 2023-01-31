---
title: Installing NuGet client tools
description: Guidance on installing client tools, the dotnet and nuget command-line interfaces (CLI), and the Package Manager for Visual Studio.
author: JonDouglas
ms.author: jodou
ms.date: 06/20/2019
ms.topic: quickstart
---

# Install NuGet client tools

> **Looking to install a package? See [Ways to install NuGet packages](consume-packages/overview-and-workflow.md#ways-to-install-a-nuget-package).**

To work with NuGet, as a package consumer or creator, you can use command-line interface (CLI) tools as well as NuGet features in Visual Studio. This article briefly outlines the capabilities of the different tools, how to install them, and their comparative [feature availability](#feature-availability). 

To get started using NuGet to consume packages, see:
* [Install and use a package (dotnet CLI)](quickstart/install-and-use-a-package-using-the-dotnet-cli.md)
* [Install and use a package (Visual Studio)](quickstart/install-and-use-a-package-in-visual-studio.md)

To get started creating NuGet packages, see:
* [Create and publish a NET Standard package (dotnet CLI)](quickstart/create-and-publish-a-package-using-the-dotnet-cli.md)
* [Create and publish a NET Standard package (Visual Studio)](quickstart/create-and-publish-a-package-using-visual-studio.md)

| Tool&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; | Description | Download&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; |
|:------------- |:-------------|:-----|
| [dotnet.exe](#dotnetexe-cli) | CLI tool for .NET Core and .NET Standard libraries, and for any [SDK-style project](resources/check-project-format.md) such as one that targets .NET Framework. Included with the .NET Core SDK and provides core NuGet features on all platforms. (Starting in Visual Studio 2017, the dotnet CLI is automatically installed with any .NET Core related workloads.)| [.NET Core SDK](https://www.microsoft.com/net/download/) |
| [nuget.exe](#nugetexe-cli) | CLI tool for .NET Framework libraries and for any [non-SDK-style project](resources/check-project-format.md) such as one that targets .NET Standard libraries. Provides all NuGet capabilities on Windows, provides most features on Mac and Linux when running under Mono. | [nuget.exe](https://dist.nuget.org/win-x86-commandline/latest/nuget.exe) |
| [Visual Studio](#visual-studio) | On Windows, the **NuGet Package Manager** is included with Visual Studio 2012 and later. Visual Studio provides the [Package Manager UI](consume-packages/install-use-packages-visual-studio.md) and the [Package Manager Console](consume-packages/install-use-packages-powershell.md), through which you can run most NuGet operations. | [Visual Studio](https://www.visualstudio.com/downloads/) |
| [Visual Studio for Mac](/visualstudio/mac/nuget-walkthrough) | On Mac, certain NuGet capabilities are built-in directly. Package Manager Console is not presently available. For other capabilities, use the `dotnet.exe` or `nuget.exe` CLI tools.  | [Visual Studio for Mac](https://visualstudio.microsoft.com/vs/mac/) |
| [Visual Studio Code](https://code.visualstudio.com/docs) | On Windows, Mac, or Linux, NuGet capabilities are available through marketplace extensions, or use the `dotnet.exe` or `nuget.exe` CLI tools. | [Visual Studio Code](https://code.visualstudio.com/Download/)|

The [MSBuild CLI](reference/msbuild-targets.md) also provides the ability to restore and create packages, which is primarily useful on build servers. MSBuild is not a general-purpose tool for working with NuGet.

Package Manager Console commands work only within Visual Studio on Windows and do not work within other PowerShell environments.

## Visual Studio
### Install on Visual Studio 2017 and newer
Starting in Visual Studio 2017, the installer includes the NuGet Package Manager with any workload that employs .NET. To install separately, or to verify that the Package Manager is installed, run the Visual Studio installer and check the option under **Individual Components > Code tools > NuGet package manager**.

### Install on Visual Studio 2015 and older
NuGet Extensions for Visual Studio 2013 and 2015 can be downloaded from [https://dist.nuget.org/index.html](https://dist.nuget.org/index.html).

For Visual Studio 2010 and earlier, install the "NuGet Package Manager for Visual Studio" extension. Note, if you can't see the extension in the first page of search results, try changing the Sort By dropdown to "Most Downloads", or an alphabetical sort.

## CLI tools
You can use either the `dotnet` CLI or the `nuget.exe` CLI to support NuGet features in the IDE. The `dotnet` CLI is installed with some Visual Studio workloads, such as .NET Core. The `nuget.exe` CLI must be installed separately as described earlier.

The two NuGet CLI tools are `dotnet.exe` and `nuget.exe`. See [feature availability](#feature-availability) for a comparison.

* To target .NET Core or .NET Standard, use the dotnet CLI. The `dotnet` CLI is required for the SDK-style project format, which uses the [SDK attribute](/dotnet/core/tools/csproj#additions).
* To target .NET Framework (non-SDK-style project only), use the `nuget.exe` CLI. If the project is migrated from `packages.config` to PackageReference, use the dotnet CLI.

### dotnet.exe CLI

The .NET Core 2.0 CLI, `dotnet.exe`, works on all platforms (Windows, Mac, and Linux) and provides core NuGet features such as installing, restoring, and publishing packages. `dotnet` provides direct integration with .NET Core project files (such as `.csproj`), which is helpful in most scenarios. `dotnet` is also built directly for each platform and does not require you to install Mono.

Installation:

- On developer computers, install the [.NET Core SDK](https://aka.ms/dotnetcoregs). Starting in Visual Studio 2017, the dotnet CLI is automatically installed with any .NET Core related workloads.
- For build servers, follow the instructions on [Using .NET Core SDK and tools in Continuous Integration](/dotnet/core/tools/using-ci-with-cli).

To learn how to use basic commands with the dotnet CLI, see [Install and use packages using the dotnet CLI](consume-packages/install-use-packages-dotnet-cli.md).

### nuget.exe CLI

The `nuget.exe` CLI, `nuget.exe`, is the command-line utility for Windows that provides all NuGet capabilities; it can also be run on Mac OSX and Linux using [Mono](https://www.mono-project.com/docs/getting-started/install/) with some limitations.

To learn how to use basic commands with the `nuget.exe` CLI, see [Install and use packages using the nuget.exe CLI](consume-packages/install-use-packages-nuget-cli.md).

Installation:

[!INCLUDE [install-cli](includes/install-cli.md)]

> [!Tip]
> Use `nuget update -self` on Windows to update an existing nuget.exe to the latest version.

> [!Note]
> The latest recommended NuGet CLI is always available at `https://dist.nuget.org/win-x86-commandline/latest/nuget.exe`. For compatibility purposes  with older continuous integration systems, a previous URL, `https://nuget.org/nuget.exe` currently provides the [deprecated 2.8.6 CLI tool](https://github.com/NuGet/NuGetGallery/issues/5381).

## Feature availability

| Feature | dotnet CLI | nuget CLI (Windows) | nuget CLI (Mono) | Visual Studio (Windows) | Visual Studio for Mac |
| --- | --- | --- | --- | --- | --- |
| Search packages |  | &#10004; | &#10004; | &#10004; | &#10004; |
| Install/uninstall packages | &#10004; | &#10004;(1) | &#10004; | &#10004; | &#10004; |
| Update packages | &#10004; | &#10004; | | &#10004; | &#10004; |
| Restore packages | &#10004; | &#10004; | &#10004;(2) | &#10004; | &#10004; |
| Manage package feeds (sources) | | &#10004; | &#10004; | &#10004; | &#10004; |
| Manage packages on a feed | &#10004; | &#10004; | &#10004; | | |
| Set API keys for feeds | | &#10004; | &#10004; | | |
| Create packages(3) | &#10004; | &#10004; | &#10004;(4) | &#10004; | |
| Publish packages | &#10004; | &#10004; | &#10004; | &#10004; |  |
| Replicate packages |  | &#10004; | &#10004; | | |
| Manage *global-package* and cache folders | &#10004; | &#10004; | &#10004; | | |
| Manage NuGet configuration | | &#10004; | &#10004; | | |

(1) Does not affect project files; use `dotnet.exe` instead.

(2) Works only with `packages.config` file and not with solution (`.sln`) files.

(3) Various advanced package features are available through the CLI only as they aren't represented in the Visual Studio UI tools.

(4) Works with `.nuspec` files but not with project files.

## Upcoming Features
If you'd like to preview upcoming NuGet features, install a [Visual Studio Preview](https://www.visualstudio.com/vs/preview/), which works side-by-side with stable releases of Visual Studio. To report problems or share ideas for previews, open an issue on the [NuGet GitHub repository](https://github.com/Nuget/Home/issues).

### Related topics

- [Install and manage packages using Visual Studio](consume-packages/install-use-packages-visual-studio.md)
- [Install and manage packages using PowerShell](consume-packages/install-use-packages-powershell.md)
- [Install and manage packages using dotnet CLI](consume-packages/install-use-packages-dotnet-cli.md)
- [Install and manage packages using nuget.exe CLI](consume-packages/install-use-packages-nuget-cli.md)
- [Package Manager Console PowerShell reference](reference/powershell-reference.md)
- [Creating a package](create-packages/creating-a-package.md)
- [Publishing a Package](nuget-org/publish-a-package.md)

Developers working on Windows can also explore the [NuGet Package Explorer](https://github.com/NuGetPackageExplorer/NuGetPackageExplorer), an open-source, stand-alone tool to visually explore, create, and edit NuGet packages. It's very helpful, for example, to make experimental changes to a package structure without rebuilding the package.
