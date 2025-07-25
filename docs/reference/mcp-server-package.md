---
title: Defining MCP server inputs using server.json
description: Use the server.json metadata file to define CLI arguments and environment variables needed by your MCP server
author: joelverhagen
ms.author: jver
ms.topic: conceptual
ms.date: 07/23/2025
---

# NOT DONE YET

## Defining MCP server inputs using server.json

## Anatomy of an MCP server package

An MCP server NuGet package is a [.NET tool package](/dotnet/core/tools/global-tools) with two extra things:

1. A package type `McpServer`, in additional to the `DotnetTool` package type present on all .NET tool packages.
2. An embedded `.mcp/server.json` file used for declaring inputs.

An MCP server package will contain your tool assembly (containing the `Program` entry point) as well as all of your dependencies. Depending on whether you made platform-specific packages, the MCP server package may be split into several platform-specific child packages. See [the runtime requirements](#runtime-requirements) section for more information.
