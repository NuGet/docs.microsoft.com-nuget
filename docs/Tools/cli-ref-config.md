---
title: NuGet CLI config command | Microsoft Docs
author: kraigb
ms.author: kraigb
manager: ghogen
ms.date: 10/24/2017
ms.topic: reference
ms.prod: nuget
ms.technology: null
ms.assetid: a50295ff-8be9-47d9-a260-822e899334cb
description: Reference for the nuget.exe config command
keywords: nuget config reference, config command
ms.reviewer:
- karann-msft
- unniravindranathan
---

# config command (NuGet CLI)

**Applies to:** all &bullet; **Supported versions**: all

Gets or sets NuGet configuration values. For additional usage, see [Configuring NuGet Behavior](../consume-packages/configuring-nuget-behavior.md). For details on allowable key names, refer to the [NuGet config file reference](../Schema/nuget-config-file.md).

## Usage

```
nuget config -Set <name>=[<value>] [<name>=<value> ...] [options]
nuget config -AsPath <name> [options]
```

where `<name>` and `<value>` specify a key-value pair to be set in the configuration. You can specify as many pairs as desired. To remove a value, specify the name and the `=` sign but no value.

For allowable key names, see the [NuGet config file reference](../Schema/nuget-config-file.md).

In NuGet 3.4+, `<value>` can use [environment variables](cli-ref-environment-variables.md).

## Options

| Option | Description |
| --- | --- |
| AsPath | Returns the config value as a path, ignored when `-Set` is used. |
| ConfigFile | *(2.5+)* The NuGet configuration file to modify. If not specified, *%AppData%\NuGet\NuGet.Config* is used. |
| ForceEnglishOutput | *(3.5+)* Forces nuget.exe to run using an invariant, English-based culture. |
| Help | Displays help information for the command. |
| NonInteractive | Suppresses prompts for user input or confirmations. |
| Verbosity | Specifies the amount of detail displayed in the output: *normal*, *quiet*, *detailed (2.5+)*. |

Also see [Environment variables](cli-ref-environment-variables.md)

### Examples

```
nuget config -Set repositoryPath=c:\packages -configfile c:\my.config

nuget config -Set repositoryPath=

nuget config -Set repositoryPath=%PACKAGE_REPO% -configfile %ProgramData%\NuGet\NuGetDefaults.Config

nuget config -Set HTTP_PROXY=http://127.0.0.1 -set HTTP_PROXY.USER=domain\user
```
