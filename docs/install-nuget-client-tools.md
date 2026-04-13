---
title: Install NuGet Client Tools
description: Find out how to install and use the dotnet and NuGet client command-line interface (CLI) tools and the Package Manager tools for Visual Studio.
author: JonDouglas
ms.author: jodou
ms.date: 04/13/2026
ms.topic: how-to
# customer intent: As a developer, I want to become familiar with NuGet client tools and find out how to install them so that I can use them to manage NuGet packages in my projects.
---

# Install NuGet client tools

**Looking to install a package? See [Ways to install a NuGet package](consume-packages/overview-and-workflow.md#ways-to-install-a-nuget-package).**

To work with NuGet as a package consumer or creator, you can use command-line interface (CLI) tools and NuGet features in Visual Studio. This article briefly outlines the capabilities of the various tools. It also explains how to install them and [compares the availability of features across the tools](#feature-availability).

## NuGet quickstarts

To get started using NuGet to consume packages, see the following articles:

- [Install and use a package with the dotnet CLI](quickstart/install-and-use-a-package-using-the-dotnet-cli.md)
- [Install and use a NuGet package in Visual Studio (Windows only)](quickstart/install-and-use-a-package-in-visual-studio.md)

To get started creating NuGet packages, see these articles:

- [Create and publish a package with the dotnet CLI](quickstart/create-and-publish-a-package-using-the-dotnet-cli.md)
- [Create and publish a NuGet package using Visual Studio (Windows only)](quickstart/create-and-publish-a-package-using-visual-studio.md)

## Tools overview

| Tool | Description | Download |
| --- | --- | --- |
| [dotnet SDK](#dotnet-sdk) | The CLI tool for .NET and .NET Standard libraries, and for any [SDK-style project](resources/check-project-format.md) such as one that targets the .NET Framework. This CLI tool is included with the .NET SDK and provides core NuGet features on all platforms. In Visual Studio 2017 and later, the dotnet CLI is automatically installed with any .NET-related workloads. | [.NET SDK](https://dotnet.microsoft.com/download) |
| [nuget.exe](#nugetexe-cli) | The CLI tool for .NET Framework libraries and for any [non-SDK-style project](resources/check-project-format.md) such as one that targets .NET Standard libraries. This CLI tool provides all NuGet capabilities on Windows and most features on macOS and Linux when running under [Mono](https://www.mono-project.com/docs/getting-started/install/). | [nuget.exe](https://dist.nuget.org/win-x86-commandline/latest/nuget.exe) |
| [Visual Studio](#visual-studio) | A full-fledged integrated development environment (IDE) that includes NuGet Package Manager. Visual Studio provides the [Package Manager UI](consume-packages/install-use-packages-visual-studio.md) and the [Package Manager Console (PowerShell on Windows)](consume-packages/install-use-packages-powershell.md). You can use these tools to run most NuGet operations. | [Visual Studio](https://visualstudio.microsoft.com/downloads/) |
| [Visual Studio Code](https://code.visualstudio.com/docs) | A lightweight, open-source code editor for Windows, macOS, and Linux that offers NuGet capabilities through marketplace extensions. You can also use the dotnet SDK or `nuget.exe` CLI tools from within Visual Studio Code. | [Visual Studio Code](https://code.visualstudio.com/Download/) |

You can also use the [Microsoft Build Engine (MSBuild) CLI](reference/msbuild-targets.md) to restore and create packages. But MSBuild isn't a general-purpose tool for working with NuGet. This CLI tool is primarily useful on build servers.

Package Manager Console commands work only within Visual Studio on Windows and don't work within other PowerShell environments.

## Support policy

For the Visual Studio for Windows support policy, see [Visual Studio Product Lifecycle and Servicing](/visualstudio/releases/2026/servicing-vs).

The most recent version of `nuget.exe` is fully supported and can be relied on for bug fixes, updates, and enhancements.
For more information about the `nuget.exe` support policy, see [Modern Lifecycle Policy](/lifecycle/policies/modern).

For the .NET SDK support policy, see [.NET and .NET Core Support Policy](https://dotnet.microsoft.com/platform/support/policy/dotnet-core).

### Patch releases

Patched versions of `nuget.exe` are released exclusively when critical security fixes are required for a long-term support (LTS) version of Visual Studio or the .NET SDK.

All security bugs should be reported to the Microsoft Security Response Center (MSRC) at the [MSRC report page](https://aka.ms/opensource/security/create-report).
For detailed information about reporting security issues, see the [security policy in the NuGet.Client repo](https://github.com/NuGet/NuGet.Client/blob/dev/SECURITY.md).

### NuGet.exe unlisting

Out-of-support, deprecated, or vulnerable `nuget.exe` versions are removed from the [`tools.json` endpoint](./api/tools-json.md).

## Visual Studio

In Visual Studio 2017 and later, the Visual Studio installer includes the NuGet Package Manager with any workload that employs .NET.

You can also install the Package Manager separately or verify your installation. Run the Visual Studio installer and check the option setting under **Individual components** > **Code tools** > **NuGet package manager**. For more information, see [Install and manage packages in Visual Studio using the NuGet Package Manager](consume-packages/install-use-packages-visual-studio.md).

## CLI tools

You can use either the dotnet CLI or the `nuget.exe` CLI to support NuGet features in the Visual Studio IDE. The dotnet CLI is installed with some Visual Studio workloads, such as .NET. The `nuget.exe` CLI must be installed separately as described earlier. For a feature comparison of the tools, see the [Feature availability](#feature-availability) section.

- To target .NET or .NET Standard, use the dotnet SDK CLI tool. This CLI is required for the SDK-style project format, which uses the [`SDK` attribute](/dotnet/core/project-sdk/overview#project-files).

- To target .NET Framework (non-SDK-style projects only), use the `nuget.exe` CLI tool. If the project is migrated from the `packages.config` format to `PackageReference`, use the dotnet SDK CLI tool instead.

### dotnet SDK

The dotnet SDK is the .NET CLI tool. It works on all platforms (Windows, macOS, and Linux) and provides core NuGet features such as installing, restoring, and publishing packages. The dotnet CLI provides direct integration with .NET project files, such as *.csproj* files, which is helpful in most scenarios. This CLI is also built directly for each platform and doesn't require installation of [Mono](https://www.mono-project.com/docs/getting-started/install/).

#### Install the dotnet SDK

- On developer computers, install the [.NET SDK](https://dotnet.microsoft.com/download). In Visual Studio 2017 and later, the dotnet CLI is automatically installed with any .NET-related workloads.

- For build servers, follow the instructions to [use the .NET SDK in continuous integration (CI) environments](/dotnet/devops/dotnet-cli-and-continuous-integration).

To find out how to use basic commands with the dotnet SDK CLI tool, see [Install and manage NuGet packages with the dotnet CLI](consume-packages/install-use-packages-dotnet-cli.md).

### nuget.exe CLI

The NuGet CLI, `nuget.exe`, is the command-line utility for Windows that provides all NuGet capabilities. This CLI can also run on macOS and Linux by using [Mono](https://www.mono-project.com/docs/getting-started/install/) with some limitations.

To find out how to use basic commands with the `nuget.exe` CLI tool, see [Manage NuGet packages with the nuget.exe CLI](consume-packages/install-use-packages-nuget-cli.md).

#### Install nuget.exe

[!INCLUDE [install-cli](includes/install-cli.md)]

## Feature availability

The following table compares the available features for the dotnet CLI, `nuget.exe` CLI, and Visual Studio tools for supported platforms.

| Feature | dotnet CLI | nuget CLI (Windows) | nuget CLI (Mono) | Visual Studio |
| --- | --- | --- | --- | --- |
| Search packages | &#10004; | &#10004; | &#10004; | &#10004; |
| Install or uninstall packages | &#10004; | &#10004; (1) | &#10004; | &#10004; |
| Update packages | &#10004; | &#10004; | | &#10004; |
| Restore packages | &#10004; | &#10004; | &#10004; (2) | &#10004; |
| Manage package feeds (sources) | &#10004; | &#10004; | &#10004; | &#10004; |
| Manage packages on a feed | &#10004; | &#10004; | &#10004; | |
| Set API keys for feeds | | &#10004; | &#10004; | |
| Create packages (3) | &#10004; | &#10004; | &#10004; (4) | &#10004; |
| Publish packages | &#10004; | &#10004; | &#10004; | &#10004; |
| Replicate packages | | &#10004; | &#10004; | |
| Manage *global-packages* and cache folders | &#10004; | &#10004; | &#10004; | |
| Manage NuGet configuration | &#10004; | &#10004; | &#10004; | |

Feature notes:

- (1) Using this feature doesn't affect project files. Use the dotnet SDK CLI tool instead.
- (2) This feature works only with *packages.config* files and not with solution (*.sln* or *.slnx*) files.
- (3) Various advanced package features are available through the CLI only, because they aren't represented in the Visual Studio UI tools.
- (4) This feature works with *.nuspec* files but not with project files.

## Upcoming features

If you want to preview upcoming NuGet features, install the [Insiders Channel version of Visual Studio](https://visualstudio.microsoft.com/insiders/), which works side by side with stable releases of Visual Studio. To report problems or share ideas for previews, open an issue on the [NuGet GitHub repository](https://github.com/Nuget/Home/issues).

## Related content

- [Install and manage packages in Visual Studio using the NuGet Package Manager](consume-packages/install-use-packages-visual-studio.md)
- [Install and manage NuGet packages with the dotnet CLI](consume-packages/install-use-packages-dotnet-cli.md)
- [Manage NuGet packages with the NuGet CLI](consume-packages/install-use-packages-nuget-cli.md)
- [Manage packages with the Visual Studio Package Manager Console (PowerShell)](consume-packages/install-use-packages-powershell.md)
- [Create a package using the nuget.exe CLI](create-packages/creating-a-package.md)
- [Publish NuGet packages](nuget-org/publish-a-package.md)
- [Package Manager Console PowerShell reference](reference/powershell-reference.md)

Developers who work on Windows can also explore the [NuGet Package Explorer](https://github.com/NuGetPackageExplorer/NuGetPackageExplorer). This application is an open-source standalone tool that you can use to visually explore, create, and edit NuGet packages. It's helpful for many scenarios, such as making experimental changes to a package structure without rebuilding the package.
