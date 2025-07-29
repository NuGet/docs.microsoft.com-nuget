---
title: MCP servers in NuGet packages
description: How can MCP servers be distributed using NuGet?
author: joelverhagen
ms.author: jver
ms.topic: conceptual
ms.date: 07/23/2025
---

# MCP servers in NuGet packages

NuGet provides a convenient way to package and distribute MCP servers written in .NET. The C# MCP SDK and .NET offer a robust platform for building MCP servers, and NuGet is ideal for delivering your MCP server to end users as a local tool. Self-contained, platform-specific packages reduce runtime compatibility issues, and AOT compilation can further improve the end-user experience.

For more information about the Model Context Protocol (MCP) in general, see the [introduction on the MCP website](https://modelcontextprotocol.io/introduction). To create your own MCP server and package it using NuGet, see the [quickstart guide](/dotnet/ai/quickstarts/build-mcp-server).

## Applicable scenarios

Shipping your MCP server via NuGet does not apply to all situations. The term "MCP client" is used in this document and refers to an application that orchestrates the interaction between an AI agent or LLM and calls made to an MCP server. Some example MCP clients are [Visual Studio Code](https://code.visualstudio.com/docs/copilot/chat/mcp-servers), [Visual Studio](/visualstudio/ide/mcp-servers), [GitHub Copilot coding agent](https://docs.github.com/copilot/concepts/coding-agent/about-copilot-coding-agent), Claude Code, or Cursor.

Consider the following criteria to determine whether shipping your MCP server as a NuGet package makes sense:

- ✅ You want your MCP server to run **locally** on the user's system (i.e., in the same context as the MCP client).
  - Local MCP servers, such as those shipped in NuGet packages, run in the same context as the MCP client and communicate with the MCP client via standard IO (stdio) transport. The MCP client is responsible for launching the local MCP server process.
- ✅ The .NET SDK is available to the MCP client.
  - NuGet MCP servers are [.NET tool packages](/dotnet/core/tools/global-tools), which are installed and executed using `dnx` from the .NET SDK.
- ✅ You have a NuGet package feed to host your MCP server package.
  - NuGet.org can be used to publish MCP server packages and provides a tailored MCP browsing and consumption experience. However, any NuGet package feed, such as Azure Artifacts, can be used for hosting MCP servers if you wish to keep your MCP server package private.

## Benefits of using .NET and NuGet for MCP servers

There are several benefits to using NuGet for hosting your MCP server:

- **Official SDK** - the [MCP C# SDK](https://github.com/modelcontextprotocol/csharp-sdk) provides a familiar interface for implementing your MCP server in C# and makes it easy to expose tools to MCP clients.
- **Flexible runtime options** - the .NET SDK provides several options for how your MCP server is compiled and packaged. See the [runtime requirements](#runtime-requirements) section for details.
- **Discoverability and distribution** - NuGet.org provides a way to showcase your MCP server, allowing potential users to find your MCP server and easily use it from inside VS Code or Visual Studio. NuGet.org encourages the use of an embedded [`.mcp/server.json`](https://github.com/modelcontextprotocol/registry/blob/main/docs/server-json/README.md) to declare inputs and an `McpServer` package type to allow MCP servers to be differentiated from other tool or dependency packages.
- **Familiar authoring workflows** - if you already use NuGet for creating dependency packages, creating and publishing an MCP server will be a very similar experience.

## Package download and execution

To fetch a local MCP server, the code for the server must be located and downloaded using a mechanism (protocol) specific to the package ecosystem. This is generally done with a "single-shot" command which takes the package name and arguments to download and then execute the package as a command-line application.

For NuGet-based MCP servers, we recommend using `dnx` (a new command shipped in .NET 10 Preview 6) to acquire and execute the package. `dnx` currently ships with the .NET SDK, but there [is discussion to include `dnx` in the .NET runtime](https://github.com/dotnet/sdk/issues/49796).

A command to start an MCP server would look something like this:

```bash
dnx NuGet.Mcp.Server@0.1.2-preview --yes
```

Environment variables and command-line arguments can both be used to configure your MCP server in custom ways. These inputs allow you to customize the behavior of your MCP server to suit specific needs.

This will download the `NuGet.Mcp.Server` package of version `0.1.2-preview` from your configured package sources (NuGet.org by default), and launch the contained CLI tool. For an MCP server, you may see log messages appear in stderr, but the process will appear to hang. This is expected, since the process is waiting for MCP protocol messages over stdin from your MCP client.

Typically, your MCP client will invoke this command via tool-specific MCP configuration, such as the `mcp.json` file used by Visual Studio Code and Visual Studio.

All of these tools support installing alternate sources. For example, `dnx` supports installing from Azure DevOps using the `--source` parameter, allowing consumption of private MCP servers, as long as the needed credential or credential providers are configured.

## Runtime requirements

Once the package is downloaded, a runtime is needed to execute the code inside the package. .NET tool packages (and by extension NuGet-based MCP servers) support a variety of options for how the tool is compiled and packaged. These options allow you, the MCP server author, to decide which runtime requirements should be placed on the users of your MCP server.

There are three main options for how to package your MCP server:

1. **Framework-dependent**: Requires that the MCP client has access to a compatible .NET runtime. If `dnx` is being used to download and execute the package, a runtime will be available.
2. **Self-contained**: Bundles the runtime with the package. [Using trimming](/dotnet/core/deploying/trimming/trimming-options) can reduce the size of the package. Self-contained .NET tools use `<PublishSelfContained>true</PublishSelfContained>`.
3. **Ahead-of-time (AOT) compiled**: A self-contained package with AOT compilation enabled. See [native AOT deployment](/dotnet/core/deploying/native-aot/) for more information. AOT .NET tools use `<PublishAot>true</PublishAot>`.

For MCP servers, we recommend using option #2 (self-contained package without AOT) because it eliminates the need for any specific .NET runtime version present in the user's environment. If you can guarantee a compatible runtime version on the intended execution environment, option #1 is reasonable. Option #3 (using AOT) is also a good option, but it forces you or your dependencies to make your code compatible with AOT compilation. The C# MCP SDK is AOT compatible, but other dependencies you intend to use may not yet be AOT compatible.

Consider using the `mcpserver` template in the [Microsoft.Extensions.AI.Templates](https://www.nuget.org/packages/Microsoft.Extensions.AI.Templates) template package to use the latest recommended defaults.

## Comparison to other ecosystems

Other ecosystems have similar requirements and workflows for packaging and running MCP servers:

- **NuGet/.NET**: Uses the `dnx` command to download and execute .NET tool packages. Requires the .NET SDK for `dnx`. Additional runtime dependencies can be bundled into the package.
- **npm**: Uses the `npx` command to download and execute npm packages, and install dependencies transitively. Requires Node.js.
- **Python**: Uses the `uvx` command to download and execute Python packages, and install dependencies transitively. Requires a Python runtime.
- **Docker**: Uses the `docker run` command to download and execute images. Requires the Docker Engine. Docker images can target multiple CPU architectures and provide excellent isolation, but are generally larger than NuGet, npm, or Python packages.

In all of these cases, the MCP client needs to have the necessary ecosystem-specific tool (e.g., `dnx`, `npx`) to download and execute the package-based MCP server.
