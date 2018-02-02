---
title: NuGet CLI sources command | Microsoft Docs
author: kraigb
ms.author: kraigb
manager: ghogen
ms.date: 01/18/2018
ms.topic: reference
ms.prod: nuget
ms.technology: null
description: Reference for the nuget.exe sources command
keywords: nuget sources reference, sources command
ms.reviewer:
- karann-msft
- unniravindranathan
---

# sources command (NuGet CLI)

**Applies to:** package consumption, publishing &bullet; **Supported versions:** all

Manages the list of sources located in the user scope configuration file or a specified configuration file. The user scope configuration file is located at `%APPDATA%\NuGet\NuGet.Config` in Windows and at `~/.nuget/NuGet.Config` in Mac/Linux.


Note that the source URL for nuget.org is `https://api.nuget.org/v3/index.json`.

## Usage

```cli
nuget sources <operation> -Name <name> -Source <source>
```

where `<operation>` is one of *List, Add, Remove, Enable, Disable,* or *Update*, `<name>` is the name of the source, and `<source>` is the source's URL.

## Options

| Option | Description |
| --- | --- |
| ConfigFile | The NuGet configuration file to apply. If not specified, *%AppData%\NuGet\NuGet.Config* is used. |
| ForceEnglishOutput | *(3.5+)* Forces nuget.exe to run using an invariant, English-based culture. |
| Format | Applies to the `list` action and can be `Detailed` (the default) or `Short`. |
| Help | Displays help information for the command. |
| NonInteractive | Suppresses prompts for user input or confirmations. |
| Password | Specifies the password for authenticating with the source. |
| StorePasswordInClearText | Indicates to store the password in unencrypted text instead of the default behavior of storing an encrypted form. |
| UserName | Specifies the user name for authenticating with the source. |
| Verbosity | Specifies the amount of detail displayed in the output: *normal*, *quiet*, *detailed*. |

> [!Note]
> Make sure to add the sources' password under the same user context as the nuget.exe is later used to access the package source. The password will be stored encrypted in the config file and can only be decrypted in the same user context as it was encrypted. So for example when you use a build server to restore NuGet packages the password must be encrypted with the same Windows user under which  the build server task will run.

Also see [Environment variables](cli-ref-environment-variables.md)

## Examples

```cli
nuget sources Add -Name "MyServer" -Source \\myserver\packages

nuget sources Disable -Name "MyServer"

nuget source Enable -Source "nuget.org"

nuget sources add -name foo.bar -source C:\NuGet\local -username foo -password bar -StorePasswordInClearText -configfile %AppData%\NuGet\my.config
```
