---
# required metadata

title: nuget.exe Credential Providers | Microsoft Docs
author: kraigb
ms.author: kraigb
manager: ghogen
ms.date: 1/9/2017
ms.topic: article
ms.prod: nuget
#ms.service:
ms.technology: null
ms.assetid: 3cf592de-39f2-4e7f-a597-62635fdcedfa

# optional metadata

description: nuget.exe credential providers authenticate with a feed, and are implemented as command-line executables that follow specific conventions.
keywords: nuget.exe credential providers, credential provider API, authenticate with feed, authenticate with gallery
#ROBOTS:
#audience:
#ms.devlang:
ms.reviewer:
- karann-msft
- unniravindranathan
#ms.suite:
#ms.tgt_pltfrm:
#ms.custom:

---

# Authenticating feeds with nuget.exe credential providers

*NuGet 3.3+*

When `nuget.exe` needs credentials to authenticate with a feed, it looks for them in the following manner:

1. NuGet first looks for credentials in `Nuget.Config` files.
1. NuGet then uses plug-in credential providers, subject to the order given below. (And example is the [Visual Studio Team Services Credential Provider](https://www.visualstudio.com/docs/package/get-started/nuget/auth#vsts-credential-provider).)
1. NuGet then prompts the user for credentials on the command line.

> [!Note]
> nuget.exe credential providers only work in nuget.exe (not in dotnet restore or Visual Studio). For credential providers with Visual Studio, see [nuget.exe Credential Providers for Visual Studio](../api/nuget-credential-providers-for-visual-studio.md)
  
nuget.exe credential providers can be used in 3 ways:

- **Globally**: to make a credential provider available to all instances of `nuget.exe` run under the current user's profile, add it to `%LocalAppData%\NuGet\CredentialProviders`. You may need to create the `CredentialProviders` folder. Credential providers can be installed at the root of the `CredentialProviders`  folder or within a subfolder. If a credential provider has multiple files/assemblies, you can use subfolders to keep the providers organized.

- **From an environment variable**: Credential providers can be stored anywhere and made accessible to `nuget.exe` by setting the `%NUGET_CREDENTIALPROVIDERS_PATH%` environment variable to the provider location. This variable can be a semicolon-separated list (for example, `path1;path2`) if you have multiple locations.

- **Alongside nuget.exe**: nuget.exe credential providers can be placed in the same folder as `nuget.exe`.

When loading credential providers, `nuget.exe` searches the above locations, in order, for any file named `credentialprovider*.exe`, then loads those files in the order they're found. If multiple credential providers exist in the same folder, they're loaded in alphabetical order.


## Creating a nuget.exe credential provider

A credential provider is a command-line executable, named in the form `CredentialProvider*.exe`, that gathers inputs, acquires credentials as appropriate, and then returns the appropriate exit status code and standard output.

A provider must do the following:

- Determine whether it can provide credentials for the targeted URI before initiating credential acquisition. If not, it should return status code 1 with no credentials.
- Not modify `Nuget.Config` (such as setting credentials there).
- Handle HTTP proxy configuration on its own, as NuGet does not provide proxy information to the plugin.
- Return credentials or error details to `nuget.exe` by writing a JSON response object (see below) to stdout, using UTF-8 encoding.
- Optionally emit additional trace logging to stderr. No secrets should ever be written to stderr, since at verbosity levels "normal" or "detailed" such traces are echoed by NuGet to the console.
- Unexpected parameters should be ignored, providing forward compatibility with future versions of NuGet.

### Input parameters


| Parameter/Switch |Description|
|----------------|-----------|
| Uri {value} | The package source URI requiring credentials.|
| NonInteractive | If present, provider does not issue interactive prompts. |
| IsRetry | If present, indicates that this attempt is a retry of a previously failed attempt. Providers typically use this flag to ensure that they bypass any existing cache and prompt for new credentials if possible.|
| Verbosity {value} | If present, one of the following values: "normal", "quiet", or "detailed". If no value is supplied, defaults to "normal". Providers should use this as an indication of the level of optional logging to emit to the standard error stream. |

### Exit codes  

| Code |Result | Description |
|----------------|-----------|-----------|
| 0 | Success | Credentials were successfully acquired and have been written to stdout.|
| 1 | ProviderNotApplicable | The current provider does not provide credentials for the given URI.|
| 2 | Failure | The provider is the correct provider for the given URI, but cannot provide credentials. In this case, nuget.exe will not retry authentication and will fail. A typical example is when a user cancels an interactive login. |

### Standard output

| Property |Notes|
|----------------|-----------|
| Username | Username for authenticated requests.|
| Password | Password for authenticated requests.|
| Message | Optional details about the response, used only to show additional details in failure cases. |

Example stdout:

    { "Username" : "freddy@example.com",
      "Password" : "bwm3bcx6txhprzmxhl2x63mdsul6grctazoomtdb6kfbof7m3a3z",
      "Message"  : "" }
