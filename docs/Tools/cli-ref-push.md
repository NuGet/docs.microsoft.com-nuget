---
# required metadata

title: NuGet CLI push command | Microsoft Docs
author: kraigb
ms.author: kraigb
manager: ghogen
ms.date: 10/24/2017
ms.topic: reference
ms.prod: nuget
ms.technology: null
ms.assetid: a9709eee-add2-47fb-98e6-eec0697087f6

# optional metadata

description: Reference for the nuget.exe push command
keywords: nuget push reference, push command
ms.reviewer:
- karann-msft
- unniravindranathan

---

# push command (NuGet CLI)

**Applies to:** package publishing &bullet; **Supported versions:** all; 4.1.0+ required for nuget.org

> [!Important]
> To push packages to nuget.org you must use nuget.exe v4.1.0+, which implements the required [NuGet protocols](../api/nuget-protocols.md).

Pushes a package to a package source and publishes it.

NuGet's default configuration is obtained by loading `%AppData%\NuGet\NuGet.Config`, then loading any `Nuget.Config` or `.nuget\Nuget.Config` files starting from root of drive and ending in current directory (see [Configuring NuGet Behavior](../consume-packages/configuring-nuget-behavior.md))

## Usage

```
nuget push <packagePath> [options]
```

where `<packagePath>` identifies the package to push to the server.

## Options

| Option | Description |
| --- | --- |
| ApiKey | The API key for the target repository. If not present,  the one specified in *%AppData%\NuGet\NuGet.Config* is used. |
| ConfigFile | *(2.5+)* The NuGet configuration file to apply. If not specified, *%AppData%\NuGet\NuGet.Config* is used. |
| DisableBuffering | Disables buffering when pushing to an HTTP(s) server to decrease memory usages. Caution: when this option is used, integrated Windows authentication might not work. |
| ForceEnglishOutput | *(3.5+)* Forces nuget.exe to run using an invariant, English-based culture. |
| Help | Displays help information for the command. |
| NonInteractive | Suppresses prompts for user input or confirmations. |
| NoSymbols | *(3.5+)* If a symbols package exists, it will not be pushed to a symbol server. |
| Source | Specifies the server URL. With NuGet 2.5+, NuGet will identify a UNC or local folder source and simply copy the file there instead of pushing it using HTTP.  Also, starting with NuGet 3.4.2, this is a mandatory parameter unless the `NuGet.Config` file specifies a *DefaultPushSource* value (see [Configuring NuGet behavior](../Consume-Packages/Configuring-NuGet-Behavior.md)). |
| SymbolSource | *(3.5+)* Specifies the symbol server URL; nuget.smbsrc.net is used when pushing to nuget.org |
| SymbolApiKey | *(3.5+)* Specifies the API key for the URL specified in `-SymbolSource`. |
| Timeout | Specifies the timeout, in seconds, for pushing to a server. The default is 300 seconds (5 minutes). |
| Verbosity | Specifies the amount of detail displayed in the output: *normal*, *quiet*, *detailed (2.5+)*. |

Also see [Environment variables](cli-ref-environment-variables.md)

## Examples

```
nuget push foo.nupkg

nuget push foo.symbols.nupkg

nuget push foo.nupkg -Timeout 360

nuget push *.nupkg

nuget.exe push -source \\mycompany\repo\ mypackage.1.0.0.nupkg

nuget push foo.nupkg 4003d786-cc37-4004-bfdf-c4f3e8ef9b3a -Source https://api.nuget.org/v3/index.json

nuget push foo.nupkg 4003d786-cc37-4004-bfdf-c4f3e8ef9b3a

nuget push foo.nupkg 4003d786-cc37-4004-bfdf-c4f3e8ef9b3a -s https://customsource/
```
