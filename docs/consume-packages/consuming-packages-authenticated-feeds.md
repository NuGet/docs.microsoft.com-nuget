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
For HTTP feeds, NuGet will with make an unauthenticated request, and if the server responds with an HTTP 401 response, NuGet will search for credentials in the following order:

1. [An environment variable `NuGetPackageSourceCredentials_{name}`](#credentials-in-environment-variables).
1. [Add credentials in a *nuget.config* file](#credentials-in-nugetconfig-files).
1. [Use a NuGet credential provider, if your package source provides one](#credential-providers).
1. [Interactive login](#interactive-login)

We recommend using a credential provider when possible.
Secrets wil not be saved in the *nuget.config* file, reducing risk of accidentally leaking secrets.
Additionally, it typically reduces the number of places you need to update when a credential expires or changes.
If the credential provider supports single sign on, it may reduce the number of time you need to log in, or the number of places that credentials need to be saved.

The credentials you need to use are determined by the package source.
Therefore, unless you're using a credential provider, you should check with your package source what credentials to use.
It is very common for package sources to forbid using your password (that you log into the website with) with NuGet.
Typically you need to create a Personal Access Token to use as NuGet's password, but you should check the documentation for the NuGet server you're using.
Some package sources, such as Azure DevOps and GitHub, have scoped access tokens, so you may need to ensure that any tokens you create include the required scope.

## Credentials in environment variables

NuGet will search for an environment variable named `NuGetPackageSourceCredentials_{name}`, where `{name}` is the value of `key="name"` in your *nuget.config* file's package source.
The value of the environment variable must be `Username={username};Password={password}`, and may optionally include `;ValidAuthenticationTypes={types}`.
If the environment variable doesn't match NuGet's convention, NuGet will silently ignore the environment variable, and continue searching for credentials for the package source elsewhere.

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

Note that environment variables have restrictions on allowed characters, and different operating systems may have different restrictions.
For example, spaces are not allowed.
Therefore, you use this environment variable feature to specify NuGet credentials for package sources that use any characters that are invalid for your platform's environment variables.
In such cases, you should rename the package source in your *nuget.config* file.

## Credentials in *nuget.config* files

*nuget.config* files can contain package source credentials.
See [the *nuget.config* file reference doc section on package source credentials](../reference/nuget-config-file.md#packagesourcecredentials) for more information, including syntax.
However, it's easier to use [`dotnet nuget update source`](/dotnet/core/tools/dotnet-nuget-update-source) on the command line to set the credentials.

> ![Warning]
> Take care when setting credentials in *nuget.config* files, especially when saving the credential as plain text.
> If the credential is written to a *nuget.config* file that is in source control, there is an increased risk of accidentally leaking the secret.

The username and plain text password in a *nuget.config* file can use an environment variable by adding `%` to the beginning and end of the environment variable name you would like to use.
For more information, see [the *nuget.config* reference docs on using environment variables](../reference/nuget-config-file.md#using-environment-variables).

## Credential providers

NuGet has an extensibility model, allowing [plugins to provide NuGet credentials](../reference/extensibility/NuGet-Cross-Platform-Authentication-Plugin.md).
The [path that credential providers must be installed](../reference/extensibility/NuGet-Cross-Platform-Plugins.md#plugin-installation-and-discovery), for NuGet to discover, is different for .NET Framework (NuGet.exe, MSBuild, and Visual Studio), and the .NET SDK (running on the .NET 5+ runtime).

NuGet passes its "interactive mode" flag to credential providers, so if the credential provider is not able to silently obtain credentials, it may fail unless [interactive mode is specified](#interactive-login).

In Visual Studio, NuGet has a [Visual Studio Credential Provider interface](../reference/extensibility/NuGet-Credential-Providers-for-Visual-Studio.md), which credential providers can use to provide a graphical login experience, or call Visual Studio APIs if necessary.
NuGet in Visual Studio will fall back to the command line credential providers if it can't find a Visual Studio credential provider that handles the source.

NuGet.exe supports both V1 and V2 credential providers, while MSBuild, the .NET SDK, and Visual Studio only support V2 plugins.

Visual Studio 2017 version 17.9, and above, includes a credential provider for [Azure Artifacts](/azure/devops/artifacts/), that works within Visual Studio, MSBuild, and NuGet.exe.
However, the credential provider for the .NET SDK is not included by Visual Studio, so [must be installed separately](https://github.com/microsoft/artifacts-credprovider?tab=readme-ov-file#setup) to work with the `dotnet` CLI.

### List of credential providers

There is a [feature request to make credential providers installable via .NET tools](https://github.com/NuGet/Home/issues/12567), and this will likely make it easier to discover other credential providers.
Until this is implemented, here is a list of credential providers we are aware of:

* [AWS CodeArtifact NuGet Credential Provider](https://docs.aws.amazon.com/codeartifact/latest/ug/nuget-cli.html#nuget-configure-cli)
* [Azure Artifacts Credential Provider](https://github.com/microsoft/artifacts-credprovider). This link is just for the command line credential provider.
* [MyGet Credential Provider for Visual Studio](http://docs.myget.org/docs/reference/credential-provider-for-visual-studio).

## Interactive login

When NuGet can not find credentials from any of the previous steps, NuGet will pause restore and prompt for credentials when in interactive mode.

|Tool|Default|Toggle|
|--|--|--|
|`dotnet` CLI|non-interactive|`--interactive` argument. For example, `dotnet restore --interactive`.|
|MSBuild|non-interactive|`NuGetInteractive` MSBuild property. For example, `msbuild -t:restore -p:NuGetInteractive=true`.|
|NuGet.exe|interactive|`-NonInteractive` argument. For example, `nuget.exe restore -NonInteractive`.|
|Visual Studio|interactive|not possible to run without prompting.|
