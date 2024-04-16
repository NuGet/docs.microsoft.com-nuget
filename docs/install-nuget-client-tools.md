---
title: Install NuGet client tools
description: Learn how to install and use the dotnet and NuGet client command-line interface (CLI) tools and the Package Manager tool for Visual Studio.
author: JonDouglas
ms.author: jodou
ms.date: 11/03/2023
ms.topic: quickstart
---

# Install NuGet client tools

> **Looking to install a package? See [Ways to install NuGet packages](consume-packages/overview-and-workflow.md#ways-to-install-a-nuget-package).**

To work with NuGet as a package consumer or creator, you can use command-line interface (CLI) tools and NuGet features in Visual Studio. This article briefly outlines the capabilities of the different tools, how to install them, and their comparative [feature availability](#feature-availability). 

To get started using NuGet to consume packages, see the following articles:

- [Install and use a package (dotnet CLI)](quickstart/install-and-use-a-package-using-the-dotnet-cli.md)
- [Install and use a package (Visual Studio on Windows)](quickstart/install-and-use-a-package-in-visual-studio.md)

To get started creating NuGet packages, see these articles:

- [Create and publish a NET Standard package (dotnet CLI)](quickstart/create-and-publish-a-package-using-the-dotnet-cli.md)
- [Create and publish a NET Standard package (Visual Studio on Windows)](quickstart/create-and-publish-a-package-using-visual-studio.md)

| Tool | Description | Download |
|---|---|---|
| [dotnet SDK](#dotnet-sdk) | The CLI tool for .NET Core and .NET Standard libraries, and for any [SDK-style project](resources/check-project-format.md) such as one that targets the .NET Framework. This CLI tool is included with the .NET Core SDK and provides core NuGet features on all platforms. In Visual Studio 2017 and later, the dotnet CLI is automatically installed with any .NET Core related workloads. | [.NET Core SDK](https://www.microsoft.com/net/download/) |
| [nuget.exe](#nugetexe-cli) | The CLI tool for .NET Framework libraries and for any [**non**-SDK-style project](resources/check-project-format.md) such as one that targets .NET Standard libraries. This CLI tool provides all NuGet capabilities on Windows and most features on Mac and Linux when running under [Mono](https://www.mono-project.com/docs/getting-started/install/). | [nuget.exe](https://dist.nuget.org/win-x86-commandline/latest/nuget.exe) |
| [Visual Studio](#visual-studio) | On Windows, the **NuGet Package Manager** is included with Visual Studio 2012 and later. Visual Studio provides the [Package Manager UI](consume-packages/install-use-packages-visual-studio.md) and the [Package Manager Console (PowerShell on Windows)](consume-packages/install-use-packages-powershell.md). You can use these tools to run most NuGet operations. | [Visual Studio](https://www.visualstudio.com/downloads/) |
| [Visual Studio for Mac](/visualstudio/mac/nuget-walkthrough) | On Mac, certain NuGet capabilities are built in directly. Package Manager Console isn't currently available. For other capabilities, use the dotnet SDK or `nuget.exe` CLI tools. | [Visual Studio for Mac](https://visualstudio.microsoft.com/vs/mac/) |
| [Visual Studio Code](https://code.visualstudio.com/docs) | On Windows, Mac, and Linux, NuGet capabilities are available through marketplace extensions, or use the dotnet SDK or `nuget.exe` CLI tools. | [Visual Studio Code](https://code.visualstudio.com/Download/) |

> [!NOTE]
> Visual Studio for Mac is scheduled for retirement by August 31, 2024 in accordance with [Microsoft's Modern Lifecycle Policy](/lifecycle/policies/modern). For more information, see [What's happening to Visual Studio for Mac](/visualstudio/mac/what-happened-to-vs-for-mac).

The [MSBuild CLI](reference/msbuild-targets.md) also restores and creates packages. MSBuild isn't a general-purpose tool for working with NuGet. This CLI tool is primarily useful on build servers. 

Package Manager Console commands work only within Visual Studio on Windows and don't work within other PowerShell environments.

## Support policy

The most recent version of NuGet.exe is fully supported and can be relied on for bug fixes, updates, and enhancements.

Out-of-support NuGet.exe versions will be unlisted from [tools.json](../api/tools-json.md).

For more information on NuGet.exe's support policy, see the [Microsoft Modern Lifecycle Policy](https://aka.ms/lifecycle).

### Patch Releases

Patched versions of NuGet.exe will be released exclusively when critical security fixes are required for a long-term support (LTS) version of Visual Studio or .NET SDK.

All security bugs should be reported to the Microsoft Security Response Center (MSRC) at [https://aka.ms/opensource/security/create-report].
Also see the [security policy in the NuGet.Client repo](https://github.com/NuGet/NuGet.Client/blob/dev/SECURITY.md).

### NuGet.exe unlisting

Deprecated and vulnerable versions of NuGet.exe will be removed from [tools.json](../api/tools-json.md) in 2024.

## Visual Studio

In Visual Studio 2017 and later, the Visual Studio installer includes the NuGet Package Manager with any workload that employs .NET.

You can also install the Package Manager separately or verify your installation. Run the Visual Studio installer and check the option setting under **Individual Components > Code tools > NuGet package manager**. For more information, see [Install and manage packages in Visual Studio by using the NuGet Package Manager](consume-packages/install-use-packages-visual-studio.md).

> [!NOTE]
> For earlier versions of Visual Studio, you can download NuGet extensions at [https://dist.nuget.org/index.html](https://dist.nuget.org/index.html).

## CLI tools

You can use either the dotnet CLI or the `nuget.exe` CLI to support NuGet features in the Visual Studio IDE. The dotnet CLI is installed with some Visual Studio workloads, such as .NET Core. The `nuget.exe` CLI must be installed separately as described earlier. For a feature comparison of the tools, see the [feature availability](#feature-availability) section.

- To target .NET Core or .NET Standard, use the dotnet SDK CLI tool. This CLI is required for the SDK-style project format, which uses the [SDK attribute](/dotnet/core/tools/csproj#additions).

- To target the .NET Framework (non-SDK-style project only), use the `nuget.exe` CLI tool. If the project is migrated from `packages.config` to PackageReference, use the dotnet SDK CLI tool instead.

### dotnet SDK

The dotnet SDK is the .NET Core 2.0 CLI tool, which works on all platforms (Windows, Mac, and Linux) and provides core NuGet features such as installing, restoring, and publishing packages. The dotnet CLI provides direct integration with .NET Core project files (such as `.csproj`), which is helpful in most scenarios. This CLI is also built directly for each platform and doesn't require installation of [Mono](https://www.mono-project.com/docs/getting-started/install/).

#### Install the dotnet SDK

- On developer computers, install the [.NET Core SDK](https://aka.ms/dotnetcoregs). In Visual Studio 2017 and later, the dotnet CLI is automatically installed with any .NET Core related workloads.

- For build servers, follow the instructions to [Use the .NET Core SDK and tools in continuous integration](/dotnet/core/tools/using-ci-with-cli).

To learn how to use basic commands with the dotnet SDK CLI tool, see [Install and manage NuGet packages with the dotnet CLI](consume-packages/install-use-packages-dotnet-cli.md).

### nuget.exe CLI

The NuGet CLI, `nuget.exe`, is the command-line utility for Windows that provides all NuGet capabilities. This CLI can also run on Mac OSX and Linux by using [Mono](https://www.mono-project.com/docs/getting-started/install/) with some limitations.

To learn how to use basic commands with the `nuget.exe` CLI tool, see [Manage NuGet packages with the nuget.exe CLI](consume-packages/install-use-packages-nuget-cli.md).

#### Install nuget.exe

[!INCLUDE [install-cli](includes/install-cli.md)]

## Feature availability

The following table compares the available features for the dotnet and `nuget.exe` CLI tools for supported platforms.

| Feature | dotnet CLI | nuget CLI (Windows) | nuget CLI (Mono) | Visual Studio (Windows) | Visual Studio for Mac |
| --- | --- | --- | --- | --- | --- |
| Search packages | | &#10004; | &#10004; | &#10004; | &#10004; |
| Install/uninstall packages | &#10004; | &#10004; (1) | &#10004; | &#10004; | &#10004; |
| Update packages | &#10004; | &#10004; | | &#10004; | &#10004; |
| Restore packages | &#10004; | &#10004; | &#10004; (2) | &#10004; | &#10004; |
| Manage package feeds (sources) | | &#10004; | &#10004; | &#10004; | &#10004; |
| Manage packages on a feed | &#10004; | &#10004; | &#10004; | | |
| Set API keys for feeds | | &#10004; | &#10004; | | |
| Create packages (3) | &#10004; | &#10004; | &#10004; (4) | &#10004; | |
| Publish packages | &#10004; | &#10004; | &#10004; | &#10004; |  |
| Replicate packages | | &#10004; | &#10004; | | |
| Manage *global-package* and cache folders | &#10004; | &#10004; | &#10004; | | |
| Manage NuGet configuration | | &#10004; | &#10004; | | |

**Feature notes**

- (1) Doesn't affect project files. Use the dotnet SDK CLI tool instead.
- (2) Works only with `packages.config` file and not with solution (`.sln`) files.
- (3) Various advanced package features are available through the CLI only as they aren't represented in the Visual Studio UI tools.
- (4) Works with `.nuspec` files but not with project files.

## Upcoming features

If you want to preview upcoming NuGet features, install a [Visual Studio Preview](https://www.visualstudio.com/vs/preview/), which works side-by-side with stable releases of Visual Studio. To report problems or share ideas for previews, open an issue on the [NuGet GitHub repository](https://github.com/Nuget/Home/issues).

## Related articles

- [Install and manage packages by using Visual Studio](consume-packages/install-use-packages-visual-studio.md)
- [Install and manage packages by using the dotnet CLI](consume-packages/install-use-packages-dotnet-cli.md)
- [Install and manage packages by using the nuget.exe CLI](consume-packages/install-use-packages-nuget-cli.md)
- [Install and manage packages by using PowerShell](consume-packages/install-use-packages-powershell.md)
- [Create a package by using the nuget.exe CLI](create-packages/creating-a-package.md)
- [Publish NuGet packages](nuget-org/publish-a-package.md)
- [Package Manager Console PowerShell reference](reference/powershell-reference.md)

Developers working on Windows can also explore the [NuGet Package Explorer](https://github.com/NuGetPackageExplorer/NuGetPackageExplorer). This application is an open-source standalone tool that lets you visually explore, create, and edit NuGet packages. It's helpful for many scenarios, such as making experimental changes to a package structure without rebuilding the package.
