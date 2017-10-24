---
# required metadata

title: NuGet CLI help command | Microsoft Docs
author: kraigb
ms.author: kraigb
manager: ghogen
ms.date: 10/24/2017
ms.topic: reference
ms.prod: nuget
ms.technology: null
ms.assetid: 780d7f52-d6c6-45cd-8a62-218ff8c0b270

# optional metadata

description: Reference for the nuget.exe help command
keywords: nuget help reference, help command
ms.reviewer:
- karann-msft
- unniravindranathan

---

# help or ? command (NuGet CLI)

**Applicable roles**: All
**Supported versions**: All

Displays general help information and help information about specific commands.

## Usage

```
nuget help [command] [options]
nuget ? [command] [options]
```

where [command] identifies a specific command for which to display help.

> [!Warning]
> With some commands, be mindful to specify *help* first, as with *nuget help install*, because there is a package named "help" on nuget.org. If you give the command *nuget install help*, you'll not get help on the install command but will instead install the package named help.

## Options

| Option | Description |
| --- | --- |
| All | Print detailed help for all available commands; ignored if a specific command is given. |
| ConfigFile | *(2.5+)* The NuGet configuration file to apply. If not specified, *%AppData%\NuGet\NuGet.Config* is used. |
| ForceEnglishOutput | *(3.5+)* Forces nuget.exe to run using an invariant, English-based culture. |
| Help | Displays help information for the help command itself. |
| Markdown | Print detailed help in markdown format when used with `-All`. Ignored otherwise. |
| NonInteractive | Suppresses prompts for user input or confirmations. |
| Verbosity | Specifies the amount of detail displayed in the output: *normal*, *quiet*, *detailed (2.5+)*. |

Also see [Environment variables](cli-ref-environment-variables.md)

## Examples

```
nuget help
nuget help push
nuget ?
nuget push -?
nuget help -All -Markdown
```
