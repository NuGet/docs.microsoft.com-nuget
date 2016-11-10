---
# required metadata

title: "Credential Providers and NuGet | Microsoft Docs"
author: kraigb
ms.author: kraigb
manager: ghogen
ms.date: 11/11/2016
ms.topic: article
ms.prod: nuget
#ms.service:
ms.technology: nuget
ms.assetid: 9c7f6d16-f437-47c4-82d4-6c996e0b18ec

# optional metadata

#description:
#keywords:
#ROBOTS:
#audience:
#ms.devlang:
ms.reviewer:  
- karann
- harikm
#ms.suite:  
#ms.tgt_pltfrm:
#ms.custom:

---


# Credential Providers and NuGet

*NuGet 3.3+*

When `nuget.exe` needs credentials to authenticate with a feed, it looks for them in the following manner:

1. NuGet first looks for credentials in `nuget.config` files.
2. NuGet then uses plug-in credential providers, subject to the order given below. (And example is the [Visual Studio Team Services Credential Provider](https://www.visualstudio.com/en-us/docs/package/get-started/nuget/auth#vsts-credential-provider).)
3. NuGet then prompts the user for credentials on the command line.


> [!Note]
> Credential providers do not apply to `dotnet restore`, or the Package Manager UI or Console in Visual Studio. NuGet in Visual Studio uses a single built-in credential provider that supports Visual Studio Team Services.

Plug-in credential providers thus provide a way to hook into custom authentication process for different feeds. They can be used in three ways:

- **Globally**: to make a credential provider available to all instances of `nuget.exe` run under the current user's profile, add it to `%LocalAppData%\NuGet\CredentialProviders`. You may need to create the `CredentialProviders` folder. Credential providers can be installed at the root of the `CredentialProviders`  folder or within a subfolder. If a credential provider has multiple files/assemblies, you can use subfolders to keep the providers organized.
- **From an environment variable**: Credential providers can be stored anywhere and made accessible to `nuget.exe` by setting the `%NUGET_CREDENTIALPROVIDERS_PATH%` environment variable to the provider location. This variable can be a semicolon-separated list if you have multiple locations.
- **Alongside nuget.exe**: Credential providers can be placed in the same folder as `nuget.exe`.

The loading plug-in credential providers, `nuget.exe` searches the above locations, in order, for any file named `credentialprovider*.exe`, then loads those files in the order they're found. If multiple credential providers exist in the same folder, they're loaded in alphabetical order.


## Creating a credential provider

A credential provider is a command-line executable, named in the form `CredentialProvider*.exe`, that gathers inputs, acquires credentials as appropriate, and then returns the appropriate exit status code and standard output.

A provider must also do the following:

- Determine whether it can provide credentials for the targeted URI before initiating credential acquisition. If not, it should return status code 1 with no credentials.
- Not modify `nuget.config` (such as setting credentials there).
- Handle HTTP proxy configuration on its own, as NuGet does not provide proxy information to the plugin.
- Encode `stdout` responses using UTF-8 encoding.

### Input parameters


|Parameter/Switch |Description|
|----------------|-----------|
|Uri {value} | The package source URI requiring credentials.|
| NonInteractive | If present, provider does not issue interactive prompts. |
| IsRetry | If present, indicates that this attempt is a retry of a previously failed attempt. Providers typically use this flag to ensure that they bypass any existing cache and prompt for new credentials if possible.|

### Exit codes

|Code |Result |Description
|----------------|-----------|-----------|
|0 | Success| Credentials were successfully acquired and have been written to stdout.|
|1 | ProviderNotApplicable | The current provider does not provide credentials for the given URI.|
|2 | Failure | The provider is the correct provider for the given URI, but cannot provide credentials. In this case, nuget.exe will not retry authentication and will fail. A typical example is when a user cancels an interactive login. |

### Standard output

|Property |Notes|
|----------------|-----------|
|Username | Username for authenticated requests.|
| Password | Password for authenticated requests.|
| Message | Optional details about the response, used only to show additional details in failure cases. |

Example stdout:

    { "Username" : "freddy@example.com",
      "Password" : "bwm3bcx6txhprzmxhl2x63mdsul6grctazoomtdb6kfbof7m3a3z",
      "Message"  : "" }
