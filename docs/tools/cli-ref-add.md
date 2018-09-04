---
title: NuGet CLI add command
description: Reference for the nuget.exe add command
author: karann-msft
ms.author: karann
ms.date: 01/18/2018
ms.topic: reference
---

# add command (NuGet CLI)

**Applies to**: package publishing &bullet; **Supported versions**: 3.3+

Adds a specified package to a non-HTTP package source (a folder or UNC path) in a hierarchical layout, wherein folders are created for the package ID and version number. For example:

    \\myserver\packages
      └─<packageID>
        └─<version>
          ├─<packageID>.<version>.nupkg
          ├─<packageID>.<version>.nupkg.sha512
          └─<packageID>.nuspec

When restoring or updating against the package source, hierarchical layout provides significantly better performance.

To expand all the files in the package to the destination package source, use the `-Expand` switch. This typically results in additional subfolders appearing in the destination, such as `tools` and `lib`.

## Usage

```cli
nuget add <packagePath> -Source <sourcePath> [options]
```

where `<packagePath>` is the pathname to the package to add, and `<sourcePath>` specifies the folder-based package source to which the package will be added. HTTP sources are not supported.

## Options

| Option | Description |
| --- | --- |
| ConfigFile | The NuGet configuration file to apply. If not specified, `%AppData%\NuGet\NuGet.Config` (Windows) or `~/.nuget/NuGet/NuGet.Config` (Mac/Linux) is used.|
| Expand | Adds all the files in the package to the package source. |
| ForceEnglishOutput | *(3.5+)* Forces nuget.exe to run using an invariant, English-based culture. |
| Help | Displays help information for the command. |
| NonInteractive | Suppresses prompts for user input or confirmations. |
| Verbosity | Specifies the amount of detail displayed in the output: *normal*, *quiet*, *detailed*. |

Also see [Environment variables](cli-ref-environment-variables.md)

## Examples

```cli
nuget add foo.nupkg -Source c:\bar\

nuget add foo.nupkg -Source \\bar\packages\
```
