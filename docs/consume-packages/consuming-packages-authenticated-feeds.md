---
title: Consuming packages from authenticated feeds
description: Consuming packages from authenticated feeds in all NuGet client scenarios
author: nkolev92
ms.author: nikolev
ms.date: 12/22/2023
ms.topic: conceptual
---

# Consuming packages from authenticated feeds

Many NuGet operations, such as restore and install, require communication with one or more package sources, which [can be configured in *nuget.config* files](../reference/nuget-config-file.md#packagesources).
For HTTP feeds, NuGet will make an unauthenticated request, and if the server responds with an HTTP 401 response, NuGet will search for credentials in the following order:

1. [An environment variable `NuGetPackageSourceCredentials_{name}`](#credentials-in-environment-variables).
1. [Credentials in *nuget.config* files](#credentials-in-nugetconfig-files).
1. [Use a NuGet credential provider, if your package source provides one](#credential-providers).

## Security best practices for managing credentials

Although NuGet searches for credentials in the order mentioned above, we recommend the following sequence for securely managing credentials when authenticating with private feeds:

1. **Credential Provider**: It is highly recommended to use a credential provider whenever possible.
This approach avoids storing secrets in plain text and minimizes the risk of accidentally exposing secrets through source control.
Moreover, it generally reduces the number of places you need to update when a credential expires or changes.
If the credential provider supports single sign-on, it may decrease the frequency of logins or the number of places where credentials need to be saved.
Refer to the [credential providers](#credential-providers) section for more information.

1. **Encrypted Credentials in nuget.config**: If a credential provider is not available, you should consider using encrypted credentials.
This approach provides an extra layer of security by storing the credentials in an encrypted format.
For more information, refer to the section on [credentials in *nuget.config* files](#credentials-in-nugetconfig-files).

    > [!NOTE]
    > :warning: **WARNING** :warning: Be aware that encrypted passwords are only supported on Windows. 
    > Moreover, they can only be decrypted on the same machine and by the same user who originally encrypted them.

1. **Using Environment Variable Macros in nuget.config**: If using encrypted credentials is not possible, consider storing the credentials in the *nuget.config* file with environment variable macros.
This approach allows you to reference environment variables that contain the actual credentials. 
It enhances transparency and helps end users understand how their credentials are configured.
For more information, refer to the section on [credentials in *nuget.config* files](#credentials-in-nugetconfig-files).

1. **Using Environment Variables Directly**: As a fallback option, you can store the credentials directly in environment variables.
However, be aware that this approach may offer less visibility and control compared to using environment variable macros in the *nuget.config* file.
For more information, refer to the section on [credentials in environment variables](#credentials-in-environment-variables).

1. **Clear Text Credentials in NuGet.Config**: It is highly recommended to use one of the previously mentioned options.
If these options are not feasible, you can store the credentials in the *nuget.config* file.
However, this option should only be used in environments where no other secure option is available.
For more information, refer to the section on [credentials in *nuget.config* files](#credentials-in-nugetconfig-files).

    > [!NOTE]
    > :warning: **WARNING** :warning: Storing credentials in clear text in the *nuget.config* file, especially when saving the file in source control, is risky as it increases the chances of accidental credential leaks.
    > If you must store credentials in the *nuget.config* file, consider using one of the more secure options mentioned above.

By adhering to these best practices, you can securely authenticate private feeds while minimizing the risk of sensitive information exposure.

The credentials you need to use are determined by the package source.
Therefore, unless you're using a credential provider, you should check with your package source for what credentials to use.
It is very common for package sources to forbid you from using your password (that you log into the website with) with NuGet.
Typically you need to create a Personal Access Token to use as NuGet's password, but you should check the documentation for the NuGet server you're using.
Some package sources, such as Azure DevOps and GitHub, have scoped access tokens, so you may need to ensure that any tokens you create include the required scope.

## Credentials in environment variables

NuGet will search for an environment variable named `NuGetPackageSourceCredentials_{name}`, where `{name}` is the value of `key="name"` in your *nuget.config* file's package source.
The value of the environment variable must be `Username={username};Password={password}`, and may optionally include `;ValidAuthenticationTypes={types}`.
If the environment variable doesn't match NuGet's convention, or the value doesn't meet NuGet's expected pattern, NuGet will silently ignore the environment variable, and continue searching for credentials for the package source elsewhere.
There are no logs to signal that NuGet uses the credential from the environment variable, which can cause difficulties in debugging authentication problems if the environment variable contains an expired secret, and the new secret is added to a *nuget.config* file, since the config file has lower precedence.

> [!TIP]
> Using environment variables in CI/CD pipelines is an excellent choice to minimize the risk of secrets being captured in logs.

For example, consider the following *nuget.config* file:

```xml
<configuration>
  <packageSources>
    <clear />
    <add key="Contoso" value="https://nuget.contoso.com/v3/index.json" />
  </packageSources>
</configuration>
```

In this case, the source name is `Contoso` and NuGet will look for the environment variable name `NuGetPackageSourceCredentials_Contoso`.
Some platforms are case-sensitive, so take care about using the correct upper and lower case characters for the environment name and the source name, as defined in your *nuget.config* file.

If the username is `nugetUser` and the password is `secret123`, the environment variable's value should be set to `Username=nugetUser;Password=secret123`.
If NuGet should only use this credential for HTTP Basic authentication, but not other authentication schemes, you can set the environment variable's value to `Username=nugetUser;Password=secret123;ValidAuthenticationTypes=Basic`.
For more information about valid authentication types, see [the docs on package credentials in *nuget.config* files](../reference/nuget-config-file.md#packagesourcecredentials).

> [!NOTE]
> Environment variables have restrictions on allowed characters, and different operating systems may have different restrictions.
> For example, spaces are not allowed.
> Therefore, you use this environment variable feature to specify NuGet credentials for package sources that use any characters that are invalid for your platform's environment variables.
> In such cases, you should rename the package source in your *nuget.config* file.

## Credentials in *nuget.config* files

*nuget.config* files can contain package source credentials.
See [the *nuget.config* file reference doc section on package source credentials](../reference/nuget-config-file.md#packagesourcecredentials) for more information, including syntax.
However, it's easier to use [`dotnet nuget update source`](/dotnet/core/tools/dotnet-nuget-update-source) on the command line to set the credentials.

> [!NOTE]
> :warning: **WARNING** :warning:
> Take care when setting credentials in *nuget.config* files, especially when saving the credential as plain text.
> If the credential is written to a *nuget.config* file that is in source control, there is an increased risk of accidentally leaking the secret.
>
> As [NuGet accumulates settings from multiple files](../consume-packages/configuring-nuget-behavior.md), it is recommended to save credentials to your user *nuget.config* file.
> We also recommend to save package sources in the solution (source code repository) *nuget.config* file, including a `<clear />` element, for build reliability.

The username and plain text password in a *nuget.config* file can use an environment variable by adding `%` to the beginning and end of the environment variable name you would like to use.
For more information, see [the *nuget.config* reference docs on using environment variables](../reference/nuget-config-file.md#using-environment-variables).

## Credential providers

NuGet has an extensibility model, allowing [plugins to provide NuGet credentials](../reference/extensibility/NuGet-Cross-Platform-Authentication-Plugin.md).
The [path that credential providers must be installed](../reference/extensibility/NuGet-Cross-Platform-Plugins.md#plugin-installation-and-discovery), for NuGet to discover, is different for .NET Framework (NuGet.exe, MSBuild, and Visual Studio), and the .NET SDK (running on the .NET 5+ runtime).

NuGet has a concept of being run in interactive mode or non-interactive mode.
When in non-interactive mode, credential providers are asked not to block NuGet.
While in interactive mode, the credential provider may prompt you to log in.
Different tools have different defaults, so interactive mode may need to be opt-in or opt-out, depending on your scenario.

|Tool|Default|Toggle|
|--|--|--|
|`dotnet` CLI|non-interactive|`--interactive` argument. For example, `dotnet restore --interactive`.|
|MSBuild|non-interactive|`NuGetInteractive` MSBuild property. For example, `msbuild -t:restore -p:NuGetInteractive=true`.|
|NuGet.exe|interactive|`-NonInteractive` argument. For example, `nuget.exe restore -NonInteractive`.|
|Visual Studio|interactive|not possible to run in non-interactive mode.|

[NuGet.exe supports both V1 and V2 credential providers](../reference/extensibility/nuget-exe-Credential-Providers.md), while MSBuild and the .NET SDK only support the cross platform (V2) plugins.

In Visual Studio, NuGet has a [Visual Studio Credential Provider interface](../reference/extensibility/NuGet-Credential-Providers-for-Visual-Studio.md), which credential providers can use to provide a graphical login experience, or call Visual Studio APIs if necessary.
NuGet in Visual Studio will fall back to the command line credential providers if it can't find a Visual Studio credential provider that handles the source.

Visual Studio 2017 version 15.9, and above, includes a credential provider for [Azure Artifacts](/azure/devops/artifacts/), that works within Visual Studio, MSBuild, and NuGet.exe.
However, the credential provider for the .NET SDK is not included by Visual Studio, so [must be installed separately](https://github.com/microsoft/artifacts-credprovider?tab=readme-ov-file#setup) to work with the `dotnet` CLI.

### List of credential providers

There is a [feature request to make credential providers installable via .NET tools](https://github.com/NuGet/Home/issues/12567), and this will likely make it easier to discover other credential providers.
Until this is implemented, here is a list of credential providers we are aware of:

* [AWS CodeArtifact NuGet Credential Provider](https://docs.aws.amazon.com/codeartifact/latest/ug/nuget-cli.html#nuget-configure-cli)
* [Azure Artifacts Credential Provider](https://github.com/microsoft/artifacts-credprovider). This link is just for the command line credential provider.
* [MyGet Credential Provider for Visual Studio](http://docs.myget.org/docs/reference/credential-provider-for-visual-studio).
