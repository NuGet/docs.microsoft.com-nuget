---
title: nuget.exe Credential Providers
description: nuget.exe credential providers authenticate with a feed, and are implemented as command-line executables that follow specific conventions.
author: JonDouglas
ms.author: jodou
ms.date: 12/12/2017
ms.topic: concept-article
---

# Authenticating feeds with nuget.exe credential providers

In version `3.3` support was added for `nuget.exe` specific (v1) credential providers. Since then, in version `4.8` [support for (v2) credential providers](NuGet-Cross-Platform-Authentication-Plugin.md) that work across all command line scenarios (`nuget.exe`, `dotnet.exe`, `msbuild.exe`) was added.

See [Consuming Packages from authenticated feeds](../../consume-packages/consuming-packages-authenticated-feeds.md) for more details on all authentication approaches.

## nuget.exe credential provider discovery

nuget.exe credential providers can be used in 3 ways:

- **Globally**: to make a credential provider available to all instances of `nuget.exe` run under the current user's profile, add it to `%LocalAppData%\NuGet\CredentialProviders`. You may need to create the `CredentialProviders` folder. Credential providers can be installed at the root of the `CredentialProviders`  folder or within a subfolder. If a credential provider has multiple files/assemblies, you can use subfolders to keep the providers organized.

- **From an environment variable**: Credential providers can be stored anywhere and made accessible to `nuget.exe` by setting the `%NUGET_CREDENTIALPROVIDERS_PATH%` environment variable to the provider location. This variable can be a semicolon-separated list (for example, `path1;path2`) if you have multiple locations.

- **Alongside nuget.exe**: nuget.exe credential providers can be placed in the same folder as `nuget.exe`.

When loading credential providers, `nuget.exe` searches the above locations, in order, for any file named `credentialprovider*.exe`, then loads those files in the order they're found. If multiple credential providers exist in the same folder, they're loaded in alphabetical order.

## Creating a nuget.exe credential provider

A credential provider is a command-line executable, named in the form `CredentialProvider*.exe`, that gathers inputs, acquires credentials as appropriate, and then returns the appropriate exit status code and standard output.

A provider must do the following:

- Determine whether it can provide credentials for the targeted URI before initiating credential acquisition. If not, it should return status code 1 with no credentials.
- Not modify `NuGet.Config` (such as setting credentials there).
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

```
{ "Username" : "freddy@example.com",
    "Password" : "bwm3bcx6txhprzmxhl2x63mdsul6grctazoomtdb6kfbof7m3a3z",
    "Message"  : "" }
```

## Troubleshooting a credential provider

At present, NuGet doesn't provide much direct support for debugging custom credential providers; [issue 4598](https://github.com/NuGet/Home/issues/4598) is tracking this work.

You can also do the following:

- Run nuget.exe with the `-verbosity` switch to inspect detailed output.
- Add debug messages to `stdout` in appropriate places.
- Be sure that you're using nuget.exe 3.3 or higher.
- Attach debugger on startup with this code snippet:

    ```cs
    while (!Debugger.IsAttached)
    {
        System.Threading.Thread.Sleep(100);
    }
    Debugger.Break();
    ```
