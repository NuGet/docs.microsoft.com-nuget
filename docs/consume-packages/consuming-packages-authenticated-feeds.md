---
title: Consuming packages from authenticated feeds
description: Consuming packages from authenticated feeds in all NuGet client scenarios
author: nkolev92
ms.author: nikolev
ms.date: 02/28/2020
ms.topic: conceptual
---

# Consuming packages from authenticated feeds

In addition to the nuget.org [public feed](https://api.nuget.org/v3/index.json), NuGet clients have the ability to interact with file feeds and private http feeds.


To authenticate with private http feeds, the 2 approaches are:

* Add credentials in the [NuGet.config](../reference/nuget-config-file.md#packagesourcecredentials)
* Authenticate using one of the many extensibility models depending on the client used.

## NuGet clients' authentication extensibility

For the various NuGet clients, the private feed provider itself is responsible for authentication.
All NuGet clients have extensibility methods to support this. These are either a Visual Studio extension or a plugin that can communicate with NuGet to retrieve credentials.

### Visual Studio

In Visual Studio, NuGet exposes an interface that feed providers can implement and provide to their customers. For more details, please refer to the documentation on [how to create a Visual Studio credential provider](../reference/extensibility/NuGet-Credential-Providers-for-Visual-Studio.md).

#### Available NuGet credential providers for Visual Studio

There is a credential provider built into Visual Studio to support Azure DevOps.


Available plug-in credential providers include:

* [MyGet Credential Provider for Visual Studio](http://docs.myget.org/docs/reference/credential-provider-for-visual-studio)

### nuget.exe

When `nuget.exe` needs credentials to authenticate with a feed, it looks for them in the following manner:

1. Look for credentials in `NuGet.config` files.
1. Use V2 plug-in credential providers
1. Use V1 plug-in credential providers
1. NuGet then prompts the user for credentials on the command line.

#### nuget.exe and V2 credential providers

In version `4.8` NuGet defined a new authentication plugin mechanism, hereafter referred to as V2 credential providers.
For the installation and discovery of those providers, refer to [NuGet cross platform plugins](../reference/extensibility/NuGet-Cross-Platform-Plugins.md#plugin-installation-and-discovery).

#### nuget.exe and V1 credential providers

In version `3.3` NuGet introduced the first version of authentication plugins.
For the installation and discovery of those providers refer to [nuget.exe credential providers](../reference/extensibility/nuget-exe-Credential-Providers.md#nugetexe-credential-provider-discovery)

#### Available credential providers for nuget.exe

* [Azure DevOps V2 Credential Providers](/azure/devops/artifacts/nuget/nuget-exe#add-a-feed-to-nuget-482-or-later) or [Azure Artifacts Credential Provider](https://github.com/microsoft/artifacts-credprovider)

With Visual Studio 2017 version 15.9 and later, the Azure DevOps credential provider is bundled in Visual Studio.
If `nuget.exe` uses MSBuild from that specific Visual Studio toolset, then the plugin will be discovered automatically.

### dotnet.exe

When `dotnet.exe` needs credentials to authenticate with a feed, it looks for them in the following manner:

1. Look for credentials in `NuGet.config` files.
1. Use V2 plug-in credential providers

By default `dotnet.exe` is not interactive, so you might need to pass an `--interactive` flag to get the tool to block for authentication.

#### dotnet.exe and V2 credential providers

In version `2.2.100` of the SDK, NuGet defined an authentication plugin mechanism that works in all clients.
For the installation and discovery of those providers, refer to [NuGet cross platform plugins](../reference/extensibility/NuGet-Cross-Platform-Plugins.md#plugin-installation-and-discovery).

#### Available credential providers for dotnet.exe

* [Azure Artifacts Credential Provider](https://github.com/microsoft/artifacts-credprovider)

### MSBuild.exe

When `MSBuild.exe` needs credentials to authenticate with a feed, it looks for them in the following manner:

1. Look for credentials in `NuGet.config` files
1. Use V2 plug-in credential providers

By default `MSBuild.exe` is not interactive, so you might need to set the `/p:NuGetInteractive=true` property to get the tool to block for authentication.

#### MSBuild.exe and V2 credential providers

In Visual Studio 2019 Update 9, NuGet defined an authentication plugin mechanism that works in all clients.
For the installation and discovery of those providers, refer to [NuGet cross platform plugins](../reference/extensibility/NuGet-Cross-Platform-Plugins.md#plugin-installation-and-discovery).

#### Available credential providers for MSBuild.exe

* [Azure Artifacts Credential Provider](https://github.com/microsoft/artifacts-credprovider)

With Visual Studio 2017 Update 9 and later, the Azure DevOps credential provider is bundled in Visual Studio. No additional steps are required.
