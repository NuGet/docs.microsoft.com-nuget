---
title: NuGet HTTPS Everywhere
description: Learn why NuGet enforces HTTPS connections for package sources, what errors like NU1302 mean, and how to safely allow HTTP feeds when necessary.
author: Nigusu-Allehu
ms.author: nyenework
ms.date: 10/28/2025
ms.topic: conceptual
---

# NuGet HTTPS Everywhere

NuGet now requires all package sources to use **HTTPS** instead of **HTTP**.
This change enhances the security of the software supply chain by preventing tampering and interception during package restore and other operations.

## Why HTTPS Everywhere?

NuGet’s move to HTTPS Everywhere is part of a broader industry effort to secure the software supply chain.
HTTPS prevents attackers from intercepting or modifying package data and ensures that responses truly come from trusted sources.

NuGet has gradually transitioned all traffic to HTTPS and now enforces it across client tools.
This protects developers from man-in-the-middle (MITM) attacks and aligns with federal and ecosystem-wide security standards.
For additional background, see the [.NET Blog: HTTPS Everywhere](https://devblogs.microsoft.com/dotnet/https-everywhere/).

## Understanding the HTTP Error

This error occurs when one or more package sources in your configuration use an **HTTP** URL instead of **HTTPS**.

In earlier SDK versions, this scenario produced a **warning** ([`NU1803`](../reference/errors-and-warnings/nu1803.md)).
Beginning with **.NET SDK 9.0.100** and later, it now results in an **error** unless the use of HTTP sources is explicitly permitted.

### Recommended Resolution

Before allowing HTTP connections, confirm whether your package source supports HTTPS.
If it does, update the feed URL to use the secure protocol:

```xml
<add key="MyFeed" value="https://contoso/packages/v3/index.json" />
```

Switching to HTTPS ensures end-to-end encryption and is the recommended and more secure approach.

### Allowing HTTP Feeds (Opt-Out)

If HTTPS is not available and you operate in a trusted or isolated environment, you can explicitly allow HTTP sources.

#### Option 1: Configure in `NuGet.Config`

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

#### Commands that support these options

| Tool           | Commands                  | Introduced In               |
| -------------- | ------------------------- | --------------------------- |
| **nuget.exe**  | `push`                    |  NuGet **7.0**              |
| **dotnet CLI** | `dotnet nuget push`       | .NET **10.0.1xx** and newer |
| **dotnet CLI** | `dotnet nuget add source` | .NET **9.0.1xx** and newer  |

For **Visual Studio** steps, refer to
[NuGet Visual Studio Options – Allow Insecure Connections](/nuget/consume-packages/nuget-visual-studio-options#allow-insecure-connections).

## HTTPS Enforcement Rollout Across Tools

NuGet’s HTTPS enforcement was introduced gradually across releases.
The following table summarizes the progression from **warnings (NU1803)** to **errors (NU1302)**.

| Versions Affected                                     | Behavior                                                              |
| ----------------------------------------------------- | --------------------------------------------------------------------- |
| Nuget.exe 6.3+, Visual Studio 17.3+, .NET 6.0.100+   |  ⚠️ **Warning (NU1803)** – HTTP sources allowed but discouraged        |
| NuGet.exe 6.12+, Visual Studio 17.12+, .NET 9.0.100+  | ❌ **Error (NU1302)** – HTTP sources blocked unless explicitly allowed|

## See Also

* [NU1302](../reference/errors-and-warnings/nu1302.md)
* [NU1803](../reference/errors-and-warnings/nu1803.md)
* [NuGet.Config Reference](../reference/nuget-config-file.md#packagesources)
* [NuGet Visual Studio Options](../consume-packages/nuget-visual-studio-options.md)
