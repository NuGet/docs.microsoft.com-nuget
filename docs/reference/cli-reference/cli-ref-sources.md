---
title: NuGet CLI sources command
description: Reference for the nuget.exe sources command
author: JonDouglas
ms.author: jodou
ms.date: 01/18/2018
ms.topic: reference
---

# sources command (NuGet CLI)

**Applies to:** package consumption, publishing &bullet; **Supported versions:** all

Manages the list of sources located in the user scope configuration file or a specified configuration file. The user scope configuration file is located at `%appdata%\NuGet\NuGet.Config` (Windows) and `~/.nuget/NuGet/NuGet.Config` (Mac/Linux).

> [!NOTE]
> Use package sources that you trust.

Note that the source URL for nuget.org is `https://api.nuget.org/v3/index.json`.

## Usage

```cli
nuget sources <operation> -Name <name> -Source <source>
```

where `<operation>` is one of *List, Add, Remove, Enable, Disable,* or *Update*, `<name>` is the name of the source, and `<source>` is the source's URL. You can operate on only one source at a time.

## Options

- **`-ConfigFile`**

  The NuGet configuration file to apply. If not specified, `%AppData%\NuGet\NuGet.Config` (Windows), or `~/.nuget/NuGet/NuGet.Config` or `~/.config/NuGet/NuGet.Config` (Mac/Linux) is used. See [On Mac/Linux, the user-level config file location varies by tooling.](../../consume-packages/configuring-nuget-behavior.md#on-maclinux-the-user-level-config-file-location-varies-by-tooling).

- **`-ForceEnglishOutput`**

  *(3.5+)* Forces nuget.exe to run using an invariant, English-based culture.

- **`-Format`**

  Applies to the `list` action and can be `Detailed` (the default) or `Short`.

- **`-?|-help`**

  Displays help information for the command.

- **`-Name`**

  Name of the source.

- **`-NonInteractive`**

  Suppresses prompts for user input or confirmations.

- **`-Password`**

  Specifies the password for authenticating with the source.
  
  > [!NOTE]
  > When using an Azure DevOps Artifacts NuGet feed, this value should be set to a Personal Access Token (PAT) that is set with Read&Write under Packaging.
  > For information about provisioning Azure DevOps Personal Access Tokens, please review [Azure DevOps - Use personal access tokens](azure/devops/organizations/accounts/use-personal-access-tokens-to-authenticate)

  > [!NOTE]
  > Be aware that encrypted passwords are only supported on Windows.
  > Moreover, they can only be decrypted on the same machine and by the same user who originally encrypted them.

- **`-src|-Source`**

  Path to the package(s) source.

- **`-StorePasswordInClearText`**

  Indicates to store the password in unencrypted text instead of the default behavior of storing an encrypted form.

  > [!WARNING]
  > Storing passwords in clear text is strongly discouraged.
  > For more information on managing credentials securely, refer to the [security best practices for consuming packages from private feeds](../../consume-packages/consuming-packages-authenticated-feeds.md#security-best-practices-for-managing-credentials).

- **`-UserName`**

  Specifies the user name for authenticating with the source.

  > [!NOTE]
  > When using an Azure DevOps Artifacts NuGet feed, this value is ignored in favor of the Personal Access Toek provided in the Password parameter.

- **`-ValidAuthenticationTypes`**

  Comma-separated list of valid authentication types for this source. By default, all authentication types are valid. Example: `basic,negotiate`.

- **`-ProtocolVersion`**

  The NuGet server protocol version to be used.
  See [NuGet.Config's packageSources documentation](../nuget-config-file.md#packagesources) for more information.
  
  Available in NuGet command line from version 6.8.
  
- **`-Verbosity [normal|quiet|detailed]`**

  Specifies the amount of detail displayed in the output: `normal` (the default), `quiet`, or `detailed`.
  
- **`-AllowInsecureConnections`**
  Allows HTTP connections for adding or updating packages. Note: This method is not secure. Available starting version 6.12.

Also see [Environment variables](cli-ref-environment-variables.md)

## Examples

```cli
nuget sources Add -Name "MyServer" -Source \\myserver\packages

nuget sources Disable -Name "MyServer"

nuget sources Enable -Name "nuget.org"

nuget sources add -name foo.bar -source C:\NuGet\local -username foo -password bar -StorePasswordInClearText -configfile %AppData%\NuGet\my.config

nuget sources add -name MyAzureDevOpsSource -source "https://pkgs.dev.azure.com/yourorgname/yourprojectname/_packaging/yourfeedname/nuget/v3/index.json" -username foo -password <Personal Access Token>

nuget sources Update -Name "nuget.org" -ProtocolVersion 3
```
