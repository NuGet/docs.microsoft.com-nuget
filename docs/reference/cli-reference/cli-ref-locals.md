---
title: NuGet CLI locals command
description: Reference for the nuget.exe locals command
author: karann-msft
ms.author: karann
ms.date: 03/19/2018
ms.topic: reference
---

# locals command (NuGet CLI)

**Applies to:** package consumption &bullet; **Supported versions:** 3.3+

Clears or lists local NuGet resources such as the *http-cache*, *global-packages* folder, and the temp folder. The `locals` command can also be used to display a list of those locations. For more information, see [Managing the global packages and cache folders](../../consume-packages/managing-the-global-packages-and-cache-folders.md).

## Usage

```cli
nuget locals <folder> [options]
```

where `<folder>` is one of `all`, `http-cache`, `packages-cache` *(3.5 and earlier)*, `global-packages`, `temp` *(3.4+)*, and `plugins-cache` *(4.8+)*.

## Options

| Option | Description |
| --- | --- |
| Clear | Clears the specified folder. |
| ConfigFile | The NuGet configuration file to apply. If not specified, `%AppData%\NuGet\NuGet.Config` (Windows) or `~/.nuget/NuGet/NuGet.Config` (Mac/Linux) is used.|
| ForceEnglishOutput | *(3.5+)* Forces nuget.exe to run using an invariant, English-based culture. |
| Help | Displays help information for the command. |
| List | Lists the location of the specified folder, or the locations of all folders when used with *all*. |
| NonInteractive | Suppresses prompts for user input or confirmations. |
| Verbosity | Specifies the amount of detail displayed in the output: *normal*, *quiet*, *detailed*. |

Also see [Environment variables](cli-ref-environment-variables.md)

## Examples

```cli
nuget locals all -list
nuget locals http-cache -clear
```

For additional examples, see [Managing the global packages and cache folders](../../consume-packages/managing-the-global-packages-and-cache-folders.md).
