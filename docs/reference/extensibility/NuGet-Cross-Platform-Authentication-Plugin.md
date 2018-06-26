---
title: NuGet cross authentication plugin
description: NuGet cross platform authentication plugins for NuGet.exe, dotnet.exe, msbuild.exe and Visual Studio
author: nkolev92
ms.author: nikolev
manager: rrelyea
ms.date: 07/01/2018
ms.topic: conceptual
---

# NuGet cross platform authentication plugin

Starting with NuGet version 4.8, all NuGet clients (NuGet.exe, Visual Studio, dotnet.exe and MSBuild.exe) can use an authentication plugin built on top of the [NuGet cross platform plugins](NuGet-Cross-Platform-Plugins.md) model.

## Available NuGet cross platform authentication plugins

There is a an authentication provider built in Visual Studio (and the Build Tools SKU) to support Visual Studio Team Services.
This plugin covers the MSBuild.exe scenarios and serves as a fallback in Visual Studio.

## Creating a cross platform authentication plugin

A sample implementation can be found in [Cross platform authentication plugin](https://github.com/NuGet/Samples/tree/master/CrossPlatformCredentialProviderTBD)
It's very important that the plugins conform to the security requirement set forth by the NuGet client tools.

The minimum required version for a plugin to be an authentication plugin is **2.0.0**.
NuGet will perform the handshake with the plugin and query for the supported operation claims.
Please refer to the NuGet cross platform plugin [protocol messages](NuGet-Cross-Platform-Plugins.md#protocol-messages)

### .NET Framework plugins authentication behavior

Please refer to the [authentication plugin spec](https://github.com/NuGet/Home/wiki/NuGet-cross-plat-authentication-plugin) and the [download plugin spec](https://github.com/NuGet/Home/wiki/NuGet-Package-Download-Plugin) for more details about the specific messages. 