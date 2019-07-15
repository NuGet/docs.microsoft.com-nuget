---
title: NuGet CLI init command
description: Reference for the nuget.exe init command
author: karann-msft
ms.author: karann
ms.date: 01/18/2018
ms.topic: reference
---

# init command (NuGet CLI)

**Applies to:** package creation &bullet; **Supported versions:** 3.3+

Copies all the packages from a flat folder to a destination folder using a hierarchical layout as described for the [add command](cli-ref-add.md). That is, using `init` is equivalent to using the `add` command on each package in the folder.

As with `add`, the destination must be either a local folder or a UNC path; HTTP package repositories such as nuget.org or private servers are not supported.

## Usage

```cli
nuget init <source> <destination> [options]
```

where `<source>` is the folder containing packages and `<destination>` is the local folder or UNC pathname to which the packages are copied.

## Options

| Option | Description |
| --- | --- |
| ConfigFile | The NuGet configuration file to apply. If not specified, `%AppData%\NuGet\NuGet.Config` (Windows) or `~/.nuget/NuGet/NuGet.Config` (Mac/Linux) is used.|
| ForceEnglishOutput | *(3.5+)* Forces nuget.exe to run using an invariant, English-based culture. |
| Expand | Adds all files in each package that's added to the package source; same as `-Expand` with the `add` command. |
| Help | Displays help information for the command. |
| NonInteractive | Suppresses prompts for user input or confirmations. |
| Verbosity | Specifies the amount of detail displayed in the output: *normal*, *quiet*, *detailed*. |

Also see [Environment variables](cli-ref-environment-variables.md)

## Examples

```cli
nuget init c:\foo c:\bar
nuget init \\foo\packages \\bar\packages -Expand
```
