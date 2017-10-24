---
# required metadata

title: NuGet CLI init command | Microsoft Docs
author: kraigb
ms.author: kraigb
manager: ghogen
ms.date: 10/24/2017
ms.topic: reference
ms.prod: nuget
ms.technology: null
ms.assetid: d413e4f3-1b41-4a92-8df8-87d21b542bd3

# optional metadata

description: Reference for the nuget.exe init command
keywords: nuget init reference, init command
ms.reviewer:
- karann-msft
- unniravindranathan

---

# init command (NuGet CLI)

| Applicable roles | Supported Versions |
| --- | --- |
| Package creation | 3.3+ |

Copies all the packages from a flat folder to a destination folder using a hierarchical layout as described for the [add command](#add) above. That is, using `init` is equivalent to using the `add` command on each package in the folder.

As with `add`, the destination must be either a local folder or a UNC path; HTTP package repositories such as nuget.org or private servers are not supported.

## Usage

```
nuget init <source> <destination> [options]
```

where `<source>` is the folder containing packages and `<destination>` is the local folder or UNC pathname to which the packages will be copied.

## Options

| Option | Description |
| --- | --- |
| ConfigFile | The NuGet configuration file to apply. If not specified, *%AppData%\NuGet\NuGet.Config* is used. |
| ForceEnglishOutput | *(3.5+)* Forces nuget.exe to run using an invariant, English-based culture. |
| Expand | Adds all files in each package that's added to the package source; same as `-Expand` with the `add` command. |
| Help | Displays help information for the command. |
| NonInteractive | Suppresses prompts for user input or confirmations. |
| Verbosity | Specifies the amount of detail displayed in the output: *normal*, *quiet*, *detailed (2.5+)*. |

Also see [Environment variables](cli-ref-environment-variables.md)

## Examples

```
nuget init c:\foo c:\bar
nuget init \\foo\packages \\bar\packages -Expand
```
