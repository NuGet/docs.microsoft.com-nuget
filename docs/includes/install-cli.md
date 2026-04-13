---
title: Include
description: Find out how to install the nuget.exe CLI tool on Windows, macOS, and Linux. Get step-by-step instructions and information about various versions.
author: JonDouglas
ms.author: jodou
ms.date: 04/13/2026
ms.topic: include
---

# [Windows](#tab/windows)

Always install the **latest** version of the tool that supports your configuration.

If the `nuget.exe` CLI tool is already installed, you can update the tool to the latest version by using the command `nuget update -self`.

1. Download `nuget.exe`:

   - To download the latest recommended version, go to [https://dist.nuget.org/win-x86-commandline/latest/nuget.exe](https://dist.nuget.org/win-x86-commandline/latest/nuget.exe).
   - To download the deprecated version 2.8.6, to maintain compatibility with older continuous integration systems, go to [https://dist.nuget.org/win-x86-commandline/v2.8.6/nuget.exe](https://dist.nuget.org/win-x86-commandline/v2.8.6/nuget.exe). Version 2.8.6 isn't compatible with [Mono](https://www.mono-project.com/docs/getting-started/install/).
   - To select a version to download, go to [nuget.org/downloads](https://nuget.org/downloads).
     - Version 5.0 and later versions require the .NET Framework version 4.7.2 or later.
     - Version 4.1.0 or later is required to publish packages to nuget.org.

1. When prompted, save the file to a folder of your choice. The *nuget.exe* file is downloaded directly. The downloaded file isn't an installer, so there's no need to run the file directly from the browser.

1. To use the CLI tool from any folder, add the *nuget.exe* path to your `PATH` environment variable.

# [macOS / Linux](#tab/macos+linux)

Behaviors can vary slightly based on your operating system distribution.

> [!NOTE]
> Visual Studio for Mac was retired on August 31, 2024 in accordance with [Microsoft's Modern Lifecycle Policy](/lifecycle/policies/modern). You can continue to work with Visual Studio for Mac, but there are several other options for developers on Mac, such as the [C# Dev Kit extension for VS Code](https://marketplace.visualstudio.com/items?itemName=ms-dotnettools.csdevkit).
>
> For more information about support timelines and alternatives, see [Visual Studio for Mac retired August 31, 2024](/lifecycle/announcements/visual-studio-mac-end-of-servicing).

1. Install [Mono version 4.4.2 or later](https://www.mono-project.com/docs/getting-started/install/).

1. At a shell prompt, run the following command:

   ```bash
   # Download the latest stable version of `nuget.exe` to /usr/local/bin.
   sudo curl -o /usr/local/bin/nuget.exe https://dist.nuget.org/win-x86-commandline/latest/nuget.exe
   ```

1. Create an alias by adding the following code to the file that your operating system uses to store Bash aliases or configuration information. Typically, the *~/.bash_aliases* or *~/.bash_profile* file is used for this purpose.

   ```bash
   # Create an alias for `nuget`.
   alias nuget="mono /usr/local/bin/nuget.exe"
   ```

1. Reload the shell. Test the installation by entering the command `nuget` with no parameters. The command should print NuGet CLI help information to the screen.

---