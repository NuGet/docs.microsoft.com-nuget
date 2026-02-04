---
title: NuGet cross platform authentication plugin
description: NuGet cross platform authentication plugins for NuGet.exe, dotnet.exe, msbuild.exe and Visual Studio
author: nkolev92
ms.author: nikolev
ms.date: 07/01/2018
ms.topic: concept-article
---

# NuGet cross platform authentication plugin

In version 4.8+, all NuGet clients (NuGet.exe, Visual Studio, dotnet.exe and MSBuild.exe) can use an authentication plugin built on top of the [NuGet cross platform plugins](NuGet-Cross-Platform-Plugins.md) model.

## Authentication in dotnet.exe

Visual Studio and NuGet.exe are by default interactive. NuGet.exe contains a switch to make it [non interactive](../nuget-exe-CLI-Reference.md).
Additionally the NuGet.exe and Visual Studio plugins prompt the user for input.
In dotnet.exe there is no prompting and the default is non interactive.

The authentication mechanism in dotnet.exe is device flow. When the restore or add package operation is run interactively, the operation blocks and instructions to the user how to complete the authentications will be provided on the command line.
When the user completes the authentication the operation will continue.

To make the operation interactive, one should pass `--interactive`.
Currently only the explicit `dotnet restore` and `dotnet package add` commands support an interactive switch.
There is no interactive switch on `dotnet build` and `dotnet publish`.

## Authentication in MSBuild

Similar to dotnet.exe, MSBuild.exe is by default non interactive the MSBuild.exe authentication mechanism is device flow.
To allow the restore to pause and wait for authentication, call restore with `msbuild -t:restore -p:NuGetInteractive="true"`.

## Creating a cross platform authentication plugin

A sample implementation can be found in [Microsoft Credential Provider plugin](https://github.com/Microsoft/artifacts-credprovider).

It's very important that the plugins conforms to the security requirements set forth by the NuGet client tools.
The minimum required version for a plugin to be an authentication plugin is *2.0.0*.
NuGet will perform the handshake with the plugin and query for the supported operation claims.
Please refer to the NuGet cross platform plugin [protocol messages](NuGet-Cross-Platform-Plugins.md#protocol-messages-index) for more details about the specific messages.

NuGet will set the log level and provide proxy information to the plugin when applicable.
Logging to the NuGet console is only acceptable after NuGet has set the log level to the plugin.

- .NET Framework plugin authentication behavior

In .NET Framework, the plugins are allowed to prompt a user for input, in the form of a dialog.

- .NET Core plugin authentication behavior

In .NET Core, a dialog cannot be shown. The plugins should use device flow to authenticate.
The plugin can send log messages to NuGet with instructions to the user.
Note that logging is available after the log level has been set to the plugin.
NuGet will not take any interactive input from the command line.

When the client calls the plugin with a Get Authentication Credentials, the plugins need to conform to the interactivity switch and respect the dialog switch. 

The following table summarizes how the plugin should behave for all combinations.

| IsNonInteractive | CanShowDialog | Plugin behavior |
| ---------------- | ------------- | --------------- |
| true | true | The IsNonInteractive switch takes precedence over the dialog switch. The plugin is not allowed to block. |
| true | false | The IsNonInteractive switch takes precedence over the dialog switch. The plugin is not allowed to block. |
| false | true | The plugin can show a dialog if required. For example, interactive login, or account selection. |
| false | false | The plugin should/can not show a dialog. The plugin should use device flow to authenticate by logging an instruction message via the logger. |

Prior to [NuGet 7.0](../../release-notes/NuGet-7.0.md), NuGet would always set `CanShowDialog` to false on the dotnet CLI, and true for MSBuild restore.
From 7.0, NuGet will always set `CanShowDialog` to true, but plugins should still detect when graphical interfaces are not available.
For example when running on Linux over an SSH connection without X forwarding, or a PowerShell remote session.

Please refer to the following specs before writing a plugin.

- [NuGet Package Download Plugin](https://github.com/NuGet/Home/wiki/NuGet-Package-Download-Plugin)
- [NuGet cross plat authentication plugin](https://github.com/NuGet/Home/wiki/NuGet-cross-plat-authentication-plugin)
