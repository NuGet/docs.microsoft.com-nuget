---
title: Include
description: Installation steps for the nuget.exe CLI tool on Windows, macOS, and Linux.
author: JonDouglas
ms.author: jodou
ms.date: 11/01/2023
ms.topic: include
---

# [Windows](#tab/windows)

Always install the **latest** version of the tool that supports your configuration.

- You can download the latest recommended version at `https://dist.nuget.org/win-x86-commandline/latest/nuget.exe`.
- If you already have the `nuget.exe` CLI tool installed, you can update the tool to the latest version with the command `nuget update -self`.
- For compatibility with older continuous integration systems, a previous URL, `https://nuget.org/nuget.exe` currently provides the [deprecated version 2.8.6](https://github.com/NuGet/NuGetGallery/issues/5381) of the CLI tool.

1. Visit [nuget.org/downloads](https://nuget.org/downloads) and download NuGet version 3.3 or later.

   - Version 5.0 and later requires the .NET Framework version 4.7.2 or later.
   - Version 4.1.0 and later is required to publish packages to `nuget.org`.
   - Version 2.8.6 isn't compatible with [Mono](https://www.mono-project.com/docs/getting-started/install/).

1. Each download is the `nuget.exe` file directly. Instruct your browser to save the file to a folder of your choice. The download file isn't an installer, so you don't see anything if you run the file directly from the browser.

1. To use the CLI tool from anywhere, add the folder location for the `nuget.exe` file to your PATH environment variable.

# [macOS / Linux](#tab/macos+linux)

Behaviors can vary slightly based on your operating system distribution.

1. Install [Mono version 4.4.2 or later](https://www.mono-project.com/docs/getting-started/install/).

1. Execute the following command at a shell prompt:

   ```bash
   # Download the latest stable `nuget.exe` to `/usr/local/bin`
   sudo curl -o /usr/local/bin/nuget.exe https://dist.nuget.org/win-x86-commandline/latest/nuget.exe
   ```

1. Create an alias by adding the following script to the appropriate file for your operating system (typically `~/.bash_aliases` or `~/.bash_profile`):

   ```bash
   # Create as alias for nuget
   alias nuget="mono /usr/local/bin/nuget.exe"
   ```

1. Reload the shell. Test the installation by entering the command `nuget` with no parameters. NuGet CLI help should display.

---