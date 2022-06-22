---
title: NuGet CLI push command
description: Reference for the nuget.exe push command
author: JonDouglas
ms.author: jodou
ms.date: 01/18/2018
ms.topic: reference
---

# push command (NuGet CLI)

**Applies to:** package publishing &bullet; **Supported versions:** all; 4.1.0+ required for nuget.org

> [!Important]
> To push packages to nuget.org you must use nuget.exe v4.1.0+, which implements the required [NuGet protocols](../../api/nuget-protocols.md).

Pushes a package to a package source and publishes it.

NuGet's default configuration is obtained by loading `%AppData%\NuGet\NuGet.Config` (Windows) or `~/.nuget/NuGet/NuGet.Config` (Mac/Linux), then loading any `Nuget.Config` or `.nuget\Nuget.Config` files starting from root of drive and ending in current directory (see [Common NuGet configurations](../../consume-packages/configuring-nuget-behavior.md))

## Usage

```cli
nuget push <packagePath> [options]
```

where `<packagePath>` identifies the package to push to the server.

## Options

- **`-ApiKey`**

  The API key for the target repository. If not present,  the one specified in the config file is used.

- **`-ConfigFile`**

  The NuGet configuration file to apply. If not specified, `%AppData%\NuGet\NuGet.Config` (Windows), or `~/.nuget/NuGet/NuGet.Config` or `~/.config/NuGet/NuGet.Config` (Mac/Linux) is used.

- **`-DisableBuffering`**

  Disables buffering when pushing to an HTTP(s) server to decrease memory usages. Caution: when this option is used, integrated Windows authentication might not work.

- **`-ForceEnglishOutput`**

  *(3.5+)* Forces nuget.exe to run using an invariant, English-based culture.

- **`-?|-help`**

  Displays help information for the command.

- **`-NonInteractive`**

  Suppresses prompts for user input or confirmations.

- **`-NoServiceEndpoint`**

  Does not append `api/v2/packages` to the source URL.

- **`-NoSymbols`**

  *(3.5+)* If a symbols package exists, it will not be pushed to a symbol server.

- **`-src|-Source`**

  Specifies the server URL. NuGet identifies a UNC or local folder source and simply copies the file there instead of pushing it using HTTP.  Also, starting with NuGet 3.4.2, this is a mandatory parameter unless the `NuGet.Config` file specifies a *DefaultPushSource* value (see [Configuring NuGet behavior](../../consume-packages/configuring-nuget-behavior.md)).

- **`-SkipDuplicate`**

  *(5.1+)* If a package and version already exists, skip it and continue with the next package in the push, if any.

- **`-SymbolSource`**

  *(3.5+)* Specifies the symbol server URL; nuget.smbsrc.net is used when pushing to nuget.org

- **`-SymbolApiKey`**

  *(3.5+)* Specifies the API key for the URL specified in `-SymbolSource`.

- **`-Timeout`**

  Specifies the timeout, in seconds, for pushing to a server. The default is 300 seconds (5 minutes).

- **`-Verbosity [normal|quiet|detailed]`**

  Specifies the amount of detail displayed in the output: `normal` (the default), `quiet`, or `detailed`.


Also see [Environment variables](cli-ref-environment-variables.md)

## Examples

```cli
nuget push foo.nupkg

nuget push foo.symbols.nupkg

nuget push foo.nupkg -Timeout 360

nuget push *.nupkg

nuget.exe push -source \\mycompany\repo\ mypackage.1.0.0.nupkg

nuget push foo.nupkg 4003d786-cc37-4004-bfdf-c4f3e8ef9b3a -Source https://api.nuget.org/v3/index.json

nuget push foo.nupkg 4003d786-cc37-4004-bfdf-c4f3e8ef9b3a

nuget push foo.nupkg 4003d786-cc37-4004-bfdf-c4f3e8ef9b3a -src https://customsource/

:: In the example below -SkipDuplicate will skip pushing the package if package "Foo" version "5.0.2" already exists on NuGet.org
nuget push Foo.5.0.2.nupkg 4003d786-cc37-4004-bfdf-c4f3e8ef9b3a -src https://api.nuget.org/v3/index.json -SkipDuplicate
```

- For Azure DevOps Artifacts push examples, see [Azure Devops examples](https://docs.microsoft.com/en-us/azure/devops/artifacts/nuget/publish?view=azure-devops#examples).
