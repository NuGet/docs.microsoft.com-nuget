---
title: NuGet cross platform plugins
description: NuGet cross platform plugins for NuGet.exe, dotnet.exe, msbuild.exe and Visual Studio
author: nkolev92
ms.author: nikolev
ms.date: 07/01/2018
ms.topic: conceptual
---

# NuGet cross platform plugins

In NuGet 4.8+ support for cross platform plugins has been added.
This was achieved with by building a new plugin extensibility model, that has to conform to a strict set of rules of operation.
The plugins are self-contained executables (runnables in the .NET Core world), that the NuGet Clients launch in a separate process.
This is a true write once, run everywhere plugin. It will work with all NuGet client tools.
The plugins can be either .NET Framework (NuGet.exe, MSBuild.exe and Visual Studio), or .NET Core (dotnet.exe).
A versioned communication protocol between the NuGet Client and the plugin is defined. During the startup handshake, the 2 processes negotiate the protocol version.

In order to cover all NuGet client tools scenarios, one would need both a .NET Framework and a .NET Core plugin.
The below describes the client/framework combinations of the plugins.

| Client tool  | Framework |
| ------------ | --------- |
| Visual Studio | .NET Framework |
| dotnet.exe | .NET Core |
| NuGet.exe | .NET Framework |
| MSBuild.exe | .NET Framework |
| NuGet.exe on Mono | .NET Framework |

## How does it work

The high level workflow can be described as follows:

1. NuGet discovers available plugins.
1. When applicable, NuGet will iterate over the plugins in priority order and starts them one by one.
1. NuGet will use the first plugin that can service the request.
1. The plugins will be shut down when they are no longer needed.

## General plugin requirements

The current protocol version is *2.0.0*.
Under this version, the requirements are as follows:

- Have a valid, trusted Authenticode signature assemblies that will run on Windows and Mono. There is no special trust requirement for assemblies run on Linux and Mac yet. [Relevant issue](https://github.com/NuGet/Home/issues/6702)
- Support stateless launching under the current security context of NuGet client tools. For example, NuGet client tools will not perform elevation or additional initialization outside of the plugin protocol described later.
- Be non interactive, unless explicitly specified.
- Adhere to the negotiated plugin protocol version.
- Respond to all requests within a reasonable time period.
- Honor cancellation requests for any in-progress operation.
- Plugins installed as global .NET tools starting with NuGet 6.13. These plugins:
  - Must follow the naming convention `nuget-plugin-*`.

The technical specification is described in more detail in the following specs:

- [NuGet Package Download Plugin](https://github.com/NuGet/Home/wiki/NuGet-Package-Download-Plugin)
- [NuGet cross plat authentication plugin](https://github.com/NuGet/Home/wiki/NuGet-cross-plat-authentication-plugin)
- [Dotnet Tools Plugins](https://github.com/NuGet/Home/blob/dev/accepted/2024/support-nuget-authentication-plugins-dotnet-tools.md)

## Client - Plugin interaction

NuGet client tools and the plugins communicate with JSON over standard streams (stdin, stdout, stderr). All data must be UTF-8 encoded.
The plugins are launched with the argument "-Plugin". In case a user directly launches a plugin executable without this argument, the plugin can give an informative message instead of waiting for a protocol handshake.
The protocol handshake timeout is 5 seconds. The plugin should complete the setup in as short of an amount as possible.
NuGet client tools will query a plugin’s supported operations by passing in the service index for a NuGet source. A plugin may use the service index to check for the presence of supported service types.

The communication between the NuGet client tools and the plugin is bidirectional. Each request has a timeout of 5 seconds. If operations are supposed to take longer the respective process should send out a progress message to prevent the request from timing out.
After 1 minute of inactivity a plugin is considered idle and is shut down.

## Plugin installation and discovery

The plugins will be discovered via a convention based directory structure.
CI/CD scenarios and power users can use environment variables to override the behavior. When using environment variables, only absolute paths are allowed. Note that `NUGET_NETFX_PLUGIN_PATHS` and `NUGET_NETCORE_PLUGIN_PATHS` are only available with 5.3+ version of the NuGet tooling and later.

- `NUGET_NETFX_PLUGIN_PATHS` - defines the plugins that will be used by the .NET Framework based tooling (NuGet.exe/MSBuild.exe/Visual Studio). Takes precedence over `NUGET_PLUGIN_PATHS`. (NuGet version 5.3+ only)
- `NUGET_NETCORE_PLUGIN_PATHS` - defines the plugins that will be used by the .NET Core based tooling (dotnet.exe). Takes precedence over `NUGET_PLUGIN_PATHS`. (NuGet version 5.3+ only)
- `NUGET_PLUGIN_PATHS`
  - defines the plugins that will be used for that NuGet process, priority preserved. If this environment variable is set, it overrides the convention based discovery. Ignored if either of the framework specific variables is specified.
  
  - Starting 6.13, this can also be used to specify a path to a .NET tools plugin path.
-  User-location, the NuGet Home location in `%UserProfile%/.nuget/plugins`. This location cannot be overriden. A different root directory will be used for .NET Core and .NET Framework plugins.

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
            myPlugin
                myPlugin.exe
                nuget.protocol.dll
                ...
        netcore
            myPlugin
                myPlugin.dll
                nuget.protocol.dll
                ...
```

> [!Note]
> There is currently no user story for the installation of the plugins. It's as simple as moving the required files into the predetermined location.

## Supported operations

Two operations are supported under the new plugin protocol.

| Operation name | Minimum protocol version | Minimum NuGet client version |
| -------------- | ----------------------- | --------------------- |
| Download Package | 1.0.0 | 4.3.0 |
| [Authentication](NuGet-Cross-Platform-Authentication-Plugin.md) | 2.0.0 | 4.8.0 |

## Running plugins under the correct runtime

For the NuGet in dotnet.exe scenarios, plugins need to be able to execute under that specific runtime of the dotnet.exe.
It's on the plugin provider and the consumer to make sure a compatible dotnet.exe/plugin combination is used.
A potential issue could arise with the user-location plugins when for example, a dotnet.exe under the 2.0 runtime tries to use a plugin written for the 2.1 runtime.

## Capabilities caching

The security verification and instantiation of the plugins is costly. The download operation happens way more frequently than the authentication operation, however the average NuGet user is only likely to have an authentication plugin.
To improve the experience, NuGet will cache the operation claims for the given request. This cache is per plugin with the plugin key being the plugin path, and the expiration for this capabilities cache is 30 days. 

The cache is located in `%LocalAppData%/NuGet/plugins-cache` and be overriden with the environment variable `NUGET_PLUGINS_CACHE_PATH`. 
To clear this [cache](../../consume-packages/managing-the-global-packages-and-cache-folders.md), one can run the locals command with the `plugins-cache` option.
The `all` locals option will now also delete the plugins cache. 

## Protocol messages index

Protocol Version *1.0.0* messages:

1.  Close
    * Request direction:  NuGet -> plugin
    * The request will contain no payload
    * No response is expected.  The proper response is for the plugin process to promptly exit.

2.  Copy files in package
    * Request direction:  NuGet -> plugin
    * The request will contain:
        * the package ID and version
        * the package source repository location
        * destination directory path
        * an enumerable of files in the package to be copied to the destination directory path
    * A response will contain:
        * a response code indicating the outcome of the operation
        * an enumerable of full paths for copied files in the destination directory if the operation was successful

3.  Copy package file (.nupkg)
    * Request direction:  NuGet -> plugin
    * The request will contain:
        * the package ID and version
        * the package source repository location
        * the destination file path
    * A response will contain:
        * a response code indicating the outcome of the operation

4.  Get credentials
    * Request direction:  plugin -> NuGet
    * The request will contain:
        * the package source repository location
        * the HTTP status code obtained from the package source repository using current credentials
    * A response will contain:
        * a response code indicating the outcome of the operation
        * a username, if available
        * a password, if available

5.  Get files in package
    * Request direction:  NuGet -> plugin
    * The request will contain:
        * the package ID and version
        * the package source repository location
    * A response will contain:
        * a response code indicating the outcome of the operation
        * an enumerable of file paths in the package if the operation was successful

6.  Get operation claims 
    * Request direction:  NuGet -> plugin
    * The request will contain:
        * the service index.json for a package source
        * the package source repository location
    * A response will contain:
        * a response code indicating the outcome of the operation
        * an enumerable of supported operations (e.g.:  package download) if the operation was successful.  If a plugin does not support the package source, the plugin must return an empty set of supported operations.

> [!Note]
> This message has been updated in version *2.0.0*. It is on the client to preserve backward compatibility.

7.  Get package hash
    * Request direction:  NuGet -> plugin
    * The request will contain:
        * the package ID and version
        * the package source repository location
        * the hash algorithm
    * A response will contain:
        * a response code indicating the outcome of the operation
        * a package file hash using the requested hash algorithm if the operation was successful

8.  Get package versions
    * Request direction:  NuGet -> plugin
    * The request will contain:
        * the package ID
        * the package source repository location
    * A response will contain:
        * a response code indicating the outcome of the operation
        * an enumerable of package versions if the operation was successful

9.  Get service index
    * Request direction:  plugin -> NuGet
    * The request will contain:
        * the package source repository location
    * A response will contain:
        * a response code indicating the outcome of the operation
        * the service index if the operation was successful

10.  Handshake
     * Request direction:  NuGet <-> plugin
     * The request will contain:
         * the current plugin protocol version
         * the minimum supported plugin protocol version
     * A response will contain:
         * a response code indicating the outcome of the operation
         * the negotiated protocol version if the operation was successful.  A failure will result in termination of the plugin.

11.  Initialize
     * Request direction:  NuGet -> plugin
     * The request will contain:
         * the NuGet client tool version
         * the NuGet client tool effective language.  This takes into consideration the ForceEnglishOutput setting, if used.
         * the default request timeout, which supersedes the protocol default.
     * A response will contain:
         * a response code indicating the outcome of the operation.  A failure will result in termination of the plugin.

12.  Log
     * Request direction:  plugin -> NuGet
     * The request will contain:
         * the log level for the request
         * a message to log
     * A response will contain:
         * a response code indicating the outcome of the operation.

13.  Monitor NuGet process exit
     * Request direction:  NuGet -> plugin
     * The request will contain:
         * the NuGet process ID
     * A response will contain:
         * a response code indicating the outcome of the operation.

14.  Prefetch package
     * Request direction:  NuGet -> plugin
     * The request will contain:
         * the package ID and version
         * the package source repository location
     * A response will contain:
         * a response code indicating the outcome of the operation

15.  Set credentials
     * Request direction:  NuGet -> plugin
     * The request will contain:
         * the package source repository location
         * the last known package source username, if available
         * the last known package source password, if available
         * the last known proxy username, if available
         * the last known proxy password, if available
     * A response will contain:
         * a response code indicating the outcome of the operation

16.  Set log level
     * Request direction:  NuGet -> plugin
     * The request will contain:
         * the default log level
     * A response will contain:
         * a response code indicating the outcome of the operation

Protocol Version *2.0.0* messages

17. Get Operation Claims

* Request direction:  NuGet -> plugin
    * The request will contain:
        * the service index.json for a package source
        * the package source repository location
    * A response will contain:
        * a response code indicating the outcome of the operation
        * an enumerable of supported operations if the operation was successful.  If a plugin does not support the package source, the plugin must return an empty set of supported operations.

    If the service index and package source are null, then the plugin can answer with authentication.

18. Get Authentication Credentials

* Request direction: NuGet -> plugin
* The request will contain:
    * Uri
    * isRetry
    * NonInteractive
    * CanShowDialog
* A response will contain
    * Username
    * Password
    * Message
    * List of Auth Types
    * MessageResponseCode
