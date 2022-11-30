---
title: NuGet CLI setapikey command
description: Reference for the nuget.exe setapikey command
author: JonDouglas
ms.author: jodou
ms.date: 01/18/2018
ms.topic: reference
---

# setapikey command (NuGet CLI)

**Applies to:** package consumption, publishing &bullet; **Supported versions:** all

Saves an API key for a given server URL into `NuGet.Config` so that it doesn't need to be entered for subsequent commands.

## Usage

```cli
nuget setapikey <key> -Source <url> [options]
```

where `<source>` identifies the server and `<key>` is the key to save. If `<source>` is omitted, nuget.org is assumed. 

> [!NOTE]
> API key is not used for authenticating with the private feed. Refer to [`nuget sources` command](../cli-reference/cli-ref-sources.md) to manage credentials for authenticating with the source.
> API keys can be obtained from the individual NuGet servers. To create and manage APIKeys for nuget.org refer to [acquire-an-api-key](../../nuget-org/scoped-api-keys.md#acquire-an-api-key)

## Options

- **`-ConfigFile`**

  The NuGet configuration file to apply. If not specified, `%AppData%\NuGet\NuGet.Config` (Windows), or `~/.nuget/NuGet/NuGet.Config` or `~/.config/NuGet/NuGet.Config` (Mac/Linux) is used. See [On Mac/Linux, the user-level config file location varies by tooling.](../../consume-packages/configuring-nuget-behavior.md#on-maclinux-the-user-level-config-file-location-varies-by-tooling).

- **`-ForceEnglishOutput`**

  *(3.5+)* Forces nuget.exe to run using an invariant, English-based culture.

- **`-?|-help`**

  Displays help information for the command.

- **`-NonInteractive`**

  Suppresses prompts for user input or confirmations.

- **`-src|-Source`**

  Server URL where the API key is valid.

- **`-Verbosity [normal|quiet|detailed]`**

  Specifies the amount of detail displayed in the output: `normal` (the default), `quiet`, or `detailed`.

Also see [Environment variables](cli-ref-environment-variables.md)

## Examples

```cli
nuget setapikey 4003d786-cc37-4004-bfdf-c4f3e8ef9b3a

nuget setapikey 4003d786-cc37-4004-bfdf-c4f3e8ef9b3a -source https://example.com/nugetfeed
```
