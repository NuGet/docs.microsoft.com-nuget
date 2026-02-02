---
title: NuGet HTTPS Everywhere
description: Learn why NuGet enforces HTTPS connections for package sources, what errors like NU1302 mean, and how to safely allow HTTP feeds when necessary.
author: Nigusu-Allehu
ms.author: nyenework
ms.date: 10/28/2025
ms.topic: concept-article
ai-usage: ai-generated
---

# NuGet HTTPS Everywhere

NuGet requires all package sources to use **HTTPS** instead of **HTTP**.
This enforcement protects the software supply chain by preventing tampering and interception during package restore and related operations.
NuGet enforces this requirement by producing an error and stopping the operation when an HTTP source is used.

## Understanding the HTTP Error

This error occurs when one or more package sources in your configuration use an **HTTP** URL instead of **HTTPS**.

In earlier NuGet versions, this scenario produced a **warning** ([`NU1803`](../reference/errors-and-warnings/nu1803.md)).
Beginning with  [**NuGet 6.12**](../release-notes/NuGet-6.12.md) and later, it now results in an **error** unless the use of HTTP sources is explicitly permitted.

### Recommended Resolution

Before allowing HTTP connections, confirm whether your package source supports HTTPS.
If it does, update the feed URL to use the secure protocol:

```xml
<add key="MyFeed" value="https://contoso/packages/v3/index.json" />
```

Switching to HTTPS ensures end-to-end encryption and is the recommended and more secure approach.

### Allowing Insecure HTTP Feeds (Opt-Out)

If HTTPS is not available and you operate in a trusted or isolated environment, you can explicitly allow HTTP sources.

#### Option 1: Set allowInsecureConnections in your `NuGet.Config`

* **Use Visual Studio**

  Enable or disable allowing insecure HTTP connections with the [Package Sources settings](/nuget/consume-packages/nuget-visual-studio-options#allow-insecure-connections) under the Visual Studio options > **NuGet Package Manager**.

* **Edit `NuGet.Config` manually**

  Add the `allowInsecureConnections="true"` attribute to the affected source:

  ```xml
  <?xml version="1.0" encoding="utf-8"?>
  <configuration>
    <packageSources>
      <add key="MyHttpFeed" value="http://contoso/packages/v3/index.json" allowInsecureConnections="true" />
    </packageSources>
  </configuration>
  ```

#### Option 2: Use the Command-Line Parameter

For commands that support it, include the following flag to temporarily permit HTTP connections:

For **dotnet** commands:

```bash
--allow-insecure-connections
```

For **NuGet.exe** commands, use:

```powershell
-AllowInsecureConnections
```

#### Commands that support opt-out options

| Tool           | Commands                  | Support for Allow Insecure Connection |
| -------------- | ------------------------- | ------------------------------------- |
| **nuget.exe**  | `push`                    |  NuGet **7.0**                        |
| **dotnet CLI** | `dotnet nuget push`       | .NET **10.0.1xx** and newer           |
| **dotnet CLI** | `dotnet nuget add source` | .NET **9.0.1xx** and newer            |

## HTTPS Enforcement Rollout Across Tools

NuGet’s HTTPS enforcement was introduced gradually across releases.
The following table summarizes the progression from [**warnings (NU1803)**](../reference/errors-and-warnings/nu1803.md) to [**errors (NU1302)**](../reference/errors-and-warnings/nu1302.md).

| Versions Affected                                     | Behavior                                                              |
| ----------------------------------------------------- | --------------------------------------------------------------------- |
| [NuGet.exe 6.3](../release-notes/NuGet-6.3.md)+, Visual Studio 17.3+, .NET 6.0.100+   |  ⚠️ **Warning (NU1803)** – HTTP sources allowed but discouraged        |
| [NuGet.exe 6.12](../release-notes/NuGet-6.12.md)+, Visual Studio 17.12+, .NET 9.0.100+  | ❌ **Error (NU1302)** – HTTP sources blocked unless explicitly allowed|

## See Also

* [NU1302](../reference/errors-and-warnings/nu1302.md)
* [NU1803](../reference/errors-and-warnings/nu1803.md)
* [NuGet.Config Reference](../reference/nuget-config-file.md#packagesources)
* [NuGet Visual Studio Options](../consume-packages/nuget-visual-studio-options.md)
