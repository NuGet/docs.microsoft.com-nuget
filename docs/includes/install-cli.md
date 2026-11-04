---
title: Include
description: Find out how to install the nuget.exe CLI tool on Windows.
author: JonDouglas
ms.author: jodou
ms.date: 04/13/2026
ms.topic: include
ai-usage: ai-generated
---

> [!IMPORTANT]
> `nuget.exe` is supported only on Windows.
> Running `nuget.exe` on Mono isn't supported, including on Linux and macOS.
> On Linux and macOS, use the [dotnet CLI](/dotnet/core/tools/) included with the [.NET SDK](https://dotnet.microsoft.com/download) instead.

# [Windows](#tab/windows)

Always install the **latest** version of the tool that supports your configuration.

If the `nuget.exe` CLI tool is already installed, you can update the tool to the latest version by using the command `nuget update -self`.

1. Download `nuget.exe`:

   - To download the latest recommended version, go to [https://dist.nuget.org/win-x86-commandline/latest/nuget.exe](https://dist.nuget.org/win-x86-commandline/latest/nuget.exe).
   - To select a version to download, go to [nuget.org/downloads](https://nuget.org/downloads).
     - Version 5.0 and later require .NET Framework 4.7.2 or later.
     - Version 4.1.0 or later is required to publish packages to nuget.org.

1. When prompted, save the file to a folder of your choice. The *nuget.exe* file is downloaded directly. The downloaded file isn't an installer, so there's no need to run the file directly from the browser.

1. To use the CLI tool from any folder, add the folder that contains the *nuget.exe* file to your `PATH` environment variable.

# [macOS / Linux](#tab/macos+linux)

`nuget.exe` isn't supported on Linux or macOS, including when running on Mono.
Install the [.NET SDK](https://dotnet.microsoft.com/download) to use the [dotnet CLI](/dotnet/core/tools/) for NuGet operations instead.

---