---
# required metadata

title: NuGet CLI locals command | Microsoft Docs
author: kraigb
ms.author: kraigb
manager: ghogen
ms.date: 10/24/2017
ms.topic: reference
ms.prod: nuget
ms.technology: null
ms.assetid: 7f672c7c-74c9-4296-bc27-4d47882b541c

# optional metadata

description: Reference for the nuget.exe locals command
keywords: nuget locals reference, locals command
ms.reviewer:
- karann-msft
- unniravindranathan

---

## locals command (NuGet CLI)

**Applies to:** package consumption &bullet; **Supported versions:** 3.3+

Clears or lists local NuGet resources such as the http-request cache, packages cache, and the machine-wide global packages folder. The `locals` command can also be used to display a list of those locations. For more information, see [Managing the NuGet Cache](../consume-packages/managing-the-nuget-cache.md).

## Usage

```
nuget locals <cache> [options]
```

where `<cache>` is one of `all`, `http-cache`, `packages-cache`, `global-packages`, and `temp` *(3.4+)*.

## Options

| Option | Description |
| --- | --- |
| Clear | Clears the specified cache. |
| ConfigFile | The NuGet configuration file to apply. If not specified, *%AppData%\NuGet\NuGet.Config* is used. |
| ForceEnglishOutput | *(3.5+)* Forces nuget.exe to run using an invariant, English-based culture. |
| Help | Displays help information for the command. |
| List | Lists the location of the specified cache, or the locations of all caches when used with *all*. |
| NonInteractive | Suppresses prompts for user input or confirmations. |
| Verbosity | Specifies the amount of detail displayed in the output: *normal*, *quiet*, *detailed*. |

Also see [Environment variables](cli-ref-environment-variables.md)

## Examples

```
nuget locals all -list
nuget locals http-cache -clear
```
