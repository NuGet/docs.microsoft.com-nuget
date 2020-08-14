---
title: NuGet CLI spec command
description: Reference for the nuget.exe spec command
author: karann-msft
ms.author: karann
ms.date: 01/18/2018
ms.topic: reference
---

# spec command (NuGet CLI)

**Applies to:** package creation &bullet; **Supported versions:** all

Generates a `.nuspec` file for a new package. If run in the same folder as a project file (`.csproj`, `.vbproj`, `.fsproj`), `spec` creates a tokenized `.nuspec` file. For additional information, see [Creating a Package](../../create-packages/creating-a-package.md).

## Usage

```cli
nuget spec [<packageID>] [options]
```

where `<packageID>` is an optional package identifier to save in the `.nuspec` file.

## Options

- **`-a|-AssemblyPath`**

  Specifies the path to the assembly to use for metadata.

- **`-Force`**

  Overwrites any existing `.nuspec` file.


- **`-ForceEnglishOutput`**

  *(3.5+)* Forces nuget.exe to run using an invariant, English-based culture.

- **`-?|-h|-help`**

  Displays help information for the command.

- **`-n|-NonInteractive`**

  Suppresses prompts for user input or confirmations.

- **`-v|-Verbosity [normal|quiet|detailed]`**

  Specifies the amount of detail displayed in the output: `normal` (the default), `quiet`, or `detailed`.

Also see [Environment variables](cli-ref-environment-variables.md)

## Examples

```cli
nuget spec

nuget spec MyPackage

nuget spec -AssemblyPath MyAssembly.dll
```
