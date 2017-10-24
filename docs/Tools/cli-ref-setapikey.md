---
# required metadata

title: NuGet CLI setapikey command | Microsoft Docs
author: kraigb
ms.author: kraigb
manager: ghogen
ms.date: 10/24/2017
ms.topic: reference
ms.prod: nuget
ms.technology: null
ms.assetid: a64c0462-973d-4100-ba3f-8902a2b127f7

# optional metadata

description: Reference for the nuget.exe setapikey command
keywords: nuget setapikey reference, setapikey command
ms.reviewer:
- karann-msft
- unniravindranathan

---

## setapikey command (NuGet CLI)

**Applicable roles**: Package consumption, publishing
**Supported versions**: All

Saves an API key for a given server URL into `NuGet.Config` so that it doesn't need to be entered for subsequent commands.

## Usage

```
nuget setapikey <key> -Source <url> [options]
```

where `<source>` identifies the server and `<key>` is the key or password to save. If `<source>` is omitted, nuget.org is assumed.

## Options

| Option | Description |
| --- | --- |
| ConfigFile | *(2.5+)* The NuGet configuration file to modify. If not specified, *%AppData%\NuGet\NuGet.Config* is used. |
| ForceEnglishOutput | *(3.5+)* Forces nuget.exe to run using an invariant, English-based culture. |
| Help | Displays help information for the command. |
| NonInteractive | Suppresses prompts for user input or confirmations. |
| Verbosity | Specifies the amount of detail displayed in the output: *normal*, *quiet*, *detailed (2.5+)*. |

Also see [Environment variables](cli-ref-environment-variables.md)

## Examples

```
nuget setapikey 4003d786-cc37-4004-bfdf-c4f3e8ef9b3a

nuget setapikey 4003d786-cc37-4004-bfdf-c4f3e8ef9b3a -source https://example.com/nugetfeed
```
