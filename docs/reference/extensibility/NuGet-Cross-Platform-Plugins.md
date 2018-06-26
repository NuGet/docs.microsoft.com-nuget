---
title: NuGet cross platform plugins
description: NuGet cross platform plugins for NuGet.exe, dotnet.exe, msbuild.exe and Visual Studio
author: nkolev92
ms.author: nikolev
manager: rrelyea
ms.date: 07/01/2018
ms.topic: conceptual
---

# NuGet cross platform plugins

Starting from version **4.8**, NuGet has added support for cross platform plugins.
This is achieved with a new plugin extensibility model, that has to conform to a strict set of rules of operation.
The plugins are self-contained executables (runnables in the .NET Core world), that the NuGet Client launches in a separate process.
The plugins can be either .NET Framework (NuGet.exe, MSBuild.exe and Visual Studio), or .NET Core (dotnet.exe).
A versioned communication protocol between the NuGet Client and the plugin is defined. During the startup handshake, the 2 processes negotiate the protocol version.

## How does it work

The high level workflow can be described as follows:

1. NuGet discovers available plugins.
2. When applicable, NuGet will iterate over the plugins in priority order and start them one by one.
3. NuGet will use the first plugin that can service the request.
4. The plugins will be shut down when they are no longer needed.

## General plugin requirements

The current protocol version is 2.0.0.
Under this version, the requirements are as follows:

- Have a valid, trusted Authenticode signature assemblies that will run on Windows and Mono. There is no special trust requirement for assemblies run on Linux and Mac. [Issue](https://github.com/nuget/home/issues/9000)
- Support stateless launching under the current security context of NuGet client tools. For example, NuGet client tools will not perform elevation or additional initialization outside of the plugin protocol described later.
- Be non interactive, unless explicitly specified.
- Adhere to the negotiated plugin protocol version.
- Respond to all requests within a reasonable time period.
- Honor cancellation requests for any in-progress operation.

The technical specification is described in more details in the following 2 specs:

- [NuGet Package Download Plugin](https://github.com/NuGet/Home/wiki/NuGet-Package-Download-Plugin)
- [NuGet cross plat authentication plugin](https://github.com/NuGet/Home/wiki/NuGet-cross-plat-authentication-plugin).

## Plugin installation and discovery

The plugins will be discovered via a convention based directory structure.
CI/CD scenarios and power users can use an environment variable to override the behavior.

1. `NUGET_PLUGIN_PATHS` - defines the plugins that will be used for that NuGet process, priority reserved. If this environment variable is set, it overrides the convention based discovery. 
2. User-location, the NuGet Home location in `%UserProfile%/.nuget/plugins`. This location cannot be overriden. A different root directory will be used for .NET Core and .NET Framework plugins. 

| Framework | Root discovery location  |
| ------- | ------------------------ |
| .NET Core |  `%UserProfile%/.nuget/plugins/netcore` |
| .NET Framework | `%UserProfile%/.nuget/plugins/netfx` |

Each plugin should be installed in its own folder.
The plugin entry point will be the name of the installed folder, with the .dll extensions for .NET Core, and .exe extension for .NET Framework.

```
    .nuget
        plugins
            netfx
                myFeedCredentialProvider
                    myFeedCredentialProvider.exe
                    nuget.protocol.dll
            netcore
                myFeedCredentialProvider
                    myFeedCredentialProvider.dll
                    nuget.protocol.dll
```

3. Pre-installed plugins. These are plugins that ship with Visual Studio in box.

## Supported operations

Two operations are supported under the new plugin protocol.

| Operation name | Minimum protocol vesion | Minimum NuGet version |
| -------------- | ----------------------- | --------------------- |
| Download Package | 1.0.0 | 4.3.0 |
| Authentication | 2.0.0 | 4.8.0 |

## Running plugins under the correct runtime

For the NuGet in dotnet.exe scenarios, plugins need to be able to execute under that specific runtime of the dotnet.exe.
It's on the plugin provider and the consumer to make sure a compatible dotnet.exe/plugin combination is used.
A potential issue could arise with the user-location plugins when for example, a dotnet.exe under the 2.0 runtime tries to use a plugin written for the 2.1 runtime.