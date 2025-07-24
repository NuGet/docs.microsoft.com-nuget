---
title: MCP servers in NuGet packages
description: How can MCP servers be distributed using NuGet?
author: joelverhagen
ms.author: jver
ms.topic: conceptual
ms.date: 07/23/2025
---

# MCP servers in NuGet packages

NuGet provides a convenient way to package and distribute MCP servers written in .NET.

This document describes how MCP relates to NuGet and what benefits there are for using NuGet and the .NET ecosystem to author and distribute local MCP servers.

The intended audience of this document is anyone who is interested in **creating their own local MCP server** and is considering implementing the MCP server using .NET.

For more information about Model Context Protocol (MCP) in general, see the [introduction on the MCP website](https://modelcontextprotocol.io/introduction).

To create your own MCP server and package it using NuGet, see the [quickstart guide](/dotnet/ai/quickstarts/build-mcp-server).

## Key considerations when building an MCP server

If you're interested in building an MCP server, ask yourself the following questions. The answers will help you decide if using a NuGet MCP server is the right decision for you.

- **What programming language do you want to write the MCP server in?** MCP is a language agnostic protocol and there are several official SDKs. The [MCP website](https://modelcontextprotocol.io/quickstart/server) lists several official SDKs.
- **Do you want the MCP server to be a local process running in the same context as the MCP client** or should it be a central web service that you manage the operations of? This document focuses on local MCP servers, instead of remote ones.
- **What runtime requirement are tolerable for your clients when launching the local MCP server process?** Popular MCP clients such as VS Code launch a local MCP server using a process like `npx` (for npm-based MCP servers) or `dnx` (for NuGet-based MCP servers). The client must have the appropriate runtime in order to acquire and execute the MCP server package.

If you or your team is familiar with C# and the .NET ecosystem, shipping your MCP server via NuGet is a great option.

If C# is new to you or your team, consider some of the trade-offs mentioned below. The .NET SDK provides a great way to package MCP servers into a single, optimized package with minimal runtime dependencies.

## Applicable scenarios

Shipping your MCP server via NuGet does not apply to all situations. The term "MCP client" is used in this document and refers to an application that orchestrates the interaction between an AI agent or LLM and calls made to an MCP server. Some example MCP clients are [Visual Studio Code](https://code.visualstudio.com/docs/copilot/chat/mcp-servers), [Visual Studio](https://learn.microsoft.com/en-us/visualstudio/ide/mcp-servers), [GitHub Copilot coding agent](https://docs.github.com/copilot/concepts/coding-agent/about-copilot-coding-agent), Claude Code, or Cursor.

Consider the following criteria to determine if shipping your MCP server as a NuGet package makes sense:

- ✅ You want your MCP server to run **locally** on the user's system (i.e. in the same context as the MCP client).
  - Instead of locally running MCP servers, MCP servers can also be hosted on a web server and communicated with via a streaming HTTP protocol. Local MCP servers, such as those shipped in NuGet package,s run in the same context as the MCP client communicate with the MCP client via a standard IO (stdio) transport. The MCP client is responsible for launching the local MCP server process.
- ✅ The .NET SDK is available to the MCP client.
  - NuGet MCP servers are [.NET tool packages](https://learn.microsoft.com/en-us/dotnet/core/tools/global-tools), which are installed and executed using the .NET SDK.
- ✅ You have have a NuGet package feed to host your MCP server package.
  - NuGet.org can be used to public MCP server packages and provides a tailored MCP browsing and consumption experience. However, any NuGet package feed, such as Azure Artifacts, can be used for hosting MCP servers if you wish to keep your MCP server package private.

## Benefits of using NuGet for MCP servers

There are several benefits to using NuGet for hosting your MCP server:

- **Official SDK** - the [MCP C# SDK](https://github.com/modelcontextprotocol/csharp-sdk) provides a familiar interface for implementing your MCP server in C# and makes it easy to expose tools to MCP clients. 
- **Flexible runtime options** - the .NET SDK provides several options for how your MCP is compiled and packaged. Generally, you can choose between using a on a runtime available on the client system, include a specific runtime inside the package, or compile your tool using [AOT](https://learn.microsoft.com/dotnet/core/deploying/native-aot/).
- **Familiar authoring workflows** - if you already use NuGet for creating dependency packages (libraries distributed via NuGet), creating and publishing an MCP server be a very similar experience.
- **MCP server browsing experience** - NuGet.org provides a way to showcase your MCP server, allowing potential users to find your MCP server and easily use it from inside from VS Code or Visual Studio.

## Comparison with other ecosystems

We will compare NuGet and .NET to other ecosystems that are commonly used for implementing local MCP servers: npm (JavaScript, node.js), Python, and Docker.  

### Package aquisition

To execute a local MCP server, the code for the server must be located and downloaded. This is generally done with a "single-shot" command which takes the package name and arguments to download and then execute the package as a command-line application.

- **NuGet/.NET**: the `dnx` command is available in the .NET SDK starting with .NET 10 preview 6. By default it downloads packages from the central registry, NuGet.org.
- **npm**: the `npx` command downloads and executes from npmjs.com.
- **Python**: the `uvx` command downloads and executes from PyPI.org.
- **Docker**: the `docker run` command downloads and executes from Docker Hub by default, but can fetch from another container registry with a fully qualified name.

In all of these cases, the MCP client needs to have the need ecosystem-specific tool (e.g. `dnx`, `npx`) to download and launch the package-based MCP server.
