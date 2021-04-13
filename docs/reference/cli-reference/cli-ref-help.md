---
title: NuGet CLI help command
description: Reference for the nuget.exe help command
author: JonDouglas
ms.author: jodou
ms.date: 01/18/2018
ms.topic: reference
---

# help or ? command (NuGet CLI)

**Applies to:** all &bullet; **Supported versions**: all

Displays general help information and help information about specific commands.

## Usage

```cli
nuget help [command] [options]
nuget ? [command] [options]
```

where [command] identifies a specific command for which to display help.

> [!Warning]
> With some commands, be mindful to specify *help* first, as with `nuget help install`, because there is a package named "help" on nuget.org. If you give the command `nuget install help`, you won't get help on the install command but will instead install the package named help.

## Options

- **`-All`**

  Print detailed help for all available commands; ignored if a specific command is given.

- **`-ConfigFile`**

  The NuGet configuration file to apply. If not specified, `%AppData%\NuGet\NuGet.Config` (Windows), or `~/.nuget/NuGet/NuGet.Config` or `~/.config/NuGet/NuGet.Config` (Mac/Linux) is used.

- **`-ForceEnglishOutput`**

  *(3.5+)* Forces nuget.exe to run using an invariant, English-based culture.

- **`-?|-help`**

  Displays help information for the command.

- **`-Markdown`**

  Print detailed help in markdown format when used with `-All`. Ignored otherwise.

- **`-NonInteractive`**

  Suppresses prompts for user input or confirmations.

- **`-Verbosity [normal|quiet|detailed]`**

  Specifies the amount of detail displayed in the output: `normal` (the default), `quiet`, or `detailed`.

Also see [Environment variables](cli-ref-environment-variables.md)

## Examples

```cli
nuget help
nuget help push
nuget ?
nuget push -?
nuget help -All -Markdown
```
