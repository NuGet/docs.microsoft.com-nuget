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

A sample implementation can be found in [MSCredProvider plugin](https://github.com/Microsoft/mscredprovider).

It's very important that the plugins conform to the security requirement set forth by the NuGet client tools.
The minimum required version for a plugin to be an authentication plugin is **2.0.0**.
NuGet will perform the handshake with the plugin and query for the supported operation claims.
Please refer to the NuGet cross platform plugin [protocol messages](NuGet-Cross-Platform-Plugins.md#protocol-messages) for more details about the specific messages.

NuGet will set the log level and provide proxy information to the plugin when applicable.
Logging to the NuGet console is only acceptable after NuGet has set the log level to the plugin.

When the client calls the plugin with a Get Authentication
The plugins need to conform to the interactivity switch.
1. .NET Framework plugins authentication behavior
