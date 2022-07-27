---
title: NuGet CLI config command
description: Reference for the nuget.exe config command
author: JonDouglas
ms.author: jodou
ms.date: 01/18/2018
ms.topic: reference
---

# config command (NuGet CLI)

**Applies to:** all &bullet; **Supported versions**: all

Gets or sets NuGet configuration values. For additional usage, see [Common NuGet configurations](../../consume-packages/configuring-nuget-behavior.md). For details on allowable key names, refer to the [NuGet config file reference](../nuget-config-file.md).

## Usage

```cli
nuget config -Set <name>=[<value>] [<name>=<value> ...] [options]
nuget config -AsPath <name> [options]
```

where `<name>` and `<value>` specify a key-value pair to be set in the configuration. You can specify as many pairs as desired. To remove a value, specify the name and the `=` sign but no value.

For allowable key names, see the [NuGet config file reference](../nuget-config-file.md).

In NuGet 3.4+, `<value>` can use [environment variables](cli-ref-environment-variables.md).

## Options


- **`AsPath`**

  Returns the config value as a path, ignored when `-Set` is used.

- **`-ConfigFile`**

  The NuGet configuration file to apply. If not specified, `%AppData%\NuGet\NuGet.Config` (Windows), or `~/.nuget/NuGet/NuGet.Config` or `~/.config/NuGet/NuGet.Config` (Mac/Linux) is used. See [On Mac/Linux, the user-level config file location varies by tooling.](../../consume-packages/configuring-nuget-behavior.md#on-maclinux-the-user-level-config-file-location-varies-by-tooling).

- **`-ForceEnglishOutput`**

  *(3.5+)* Forces nuget.exe to run using an invariant, English-based culture.

- **`-?|-help`**

  Displays help information for the command.

- **`-NonInteractive`**

  Suppresses prompts for user input or confirmations.

- **`-Set`**

  One on more key-value pairs to be set in config.

- **`-Verbosity [normal|quiet|detailed]`**

  Specifies the amount of detail displayed in the output: `normal` (the default), `quiet`, or `detailed`.

Also see [Environment variables](cli-ref-environment-variables.md)

### Examples

```cli
nuget config -Set repositoryPath=c:\packages -configfile c:\my.config

nuget config -Set repositoryPath=

nuget config -Set repositoryPath=%PACKAGE_REPO% -configfile %ProgramData%\NuGet\NuGetDefaults.Config

nuget config -Set HTTP_PROXY=http://127.0.0.1 -set HTTP_PROXY.USER=domain\user
```
