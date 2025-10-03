---
title: HTTPS Everywhere - Secure Package Sources in NuGet
description: Learn about NuGet's HTTPS-everywhere initiative and how to configure secure package sources in Visual Studio and .NET CLI
author: Nigusu-Allehu
ms.date: 01/15/2025
ms.topic: conceptual
---

# HTTPS Everywhere - Secure Package Sources

NuGet requires HTTPS for all package sources to protect your software supply chain from security threats. Using HTTP sources exposes your development environment to man-in-the-middle attacks, where malicious actors could intercept and modify packages during download.

## Why HTTPS is Important

When you download packages over HTTP:
- **Package tampering**: Attackers can inject malicious code into packages during transit
- **Credential theft**: Authentication tokens and API keys can be intercepted
- **Supply chain attacks**: Compromised packages can spread malware throughout your organization

HTTPS encryption ensures that:
- Packages are downloaded from the intended source
- Package contents haven't been modified during transit
- Your credentials remain secure

For more information about the security benefits, see the [HTTPS everywhere blog post](https://devblogs.microsoft.com/nuget/https-everywhere).

## Visual Studio Package Source Settings

Starting with Visual Studio 2022, the Package Sources settings use a unified settings experience that displays warnings and errors directly when configuring package sources.

### Viewing and Managing Package Sources

To manage your package sources in Visual Studio:

1. Open **Tools** > **Options**
2. Navigate to **NuGet Package Manager** > **Package Sources**

![Screenshot showing the Options window with Package Sources selected](media/package-sources.png)

In this view, you can add, edit, enable, or disable package sources.

### HTTP Source Warnings in Visual Studio

When you add or edit a package source that uses HTTP instead of HTTPS, Visual Studio displays:

- **Visual indicators** in the Package Sources list showing which sources use insecure HTTP connections
- **Warning messages** in the Add/Edit Source dialog explaining the security risks
- **Error messages** during restore operations if HTTP sources are not explicitly allowed

These warnings and errors help you identify and resolve insecure configurations before they become security vulnerabilities.

> [!NOTE]
> Visual Studio 2022 and later use a unified settings experience that shows HTTP warnings and errors directly in the Package Sources settings UI. This makes it easier to identify and fix insecure package source configurations.

### Adding a Secure HTTPS Source

To add a new package source with HTTPS:

1. In **Tools** > **Options** > **NuGet Package Manager** > **Package Sources**
2. Click the **+** button to add a new source
3. Enter a **Name** for your source
4. Enter the **Source** URL using `https://` (not `http://`)
5. Click **Update** to save

![Screenshot showing the Package source selector](media/package-source-selector.png)

Example of a secure source URL:
```
https://api.nuget.org/v3/index.json
```

If you attempt to add an HTTP source, Visual Studio will display a warning in the settings UI. You should only proceed if you fully understand the security risks and trust the source.

## Command Line Configuration

### .NET CLI

When restoring packages with the .NET CLI, HTTP sources will generate errors:

```bash
dotnet restore
```

You'll see an error like:
```
error NU1302: Unable to load the service index for source http://example.com/nuget/
```

### NuGet.exe CLI

Similarly, when using nuget.exe:

```bash
nuget restore
```

You'll encounter the same NU1302 error for HTTP sources.

## Allowing HTTP Sources (Not Recommended)

> [!WARNING]
> Using HTTP sources is a security risk. Only configure HTTP sources if you fully trust the source and understand the risks. For more information about the security implications, see the [HTTPS everywhere blog post](https://devblogs.microsoft.com/nuget/https-everywhere).

If you must use an HTTP source temporarily, you can explicitly allow it by adding `allowInsecureConnections="true"` to your NuGet.Config file:

```xml
<configuration>
  <packageSources>
    <add key="InsecureSource" value="http://example.com/nuget/" allowInsecureConnections="true" />
  </packageSources>
</configuration>
```

### Alternative: Using SdkAnalysisLevel (Temporary Workaround)

For projects using the .NET SDK, you can temporarily downgrade HTTP source errors to warnings by adjusting the `SdkAnalysisLevel` property:

```xml
<PropertyGroup>
  <SdkAnalysisLevel>8.0.100</SdkAnalysisLevel>
</PropertyGroup>
```

> [!WARNING]
> Changing `SdkAnalysisLevel` affects multiple SDK features beyond NuGet. This should only be used as a temporary measure. For details, see [SdkAnalysisLevel](/dotnet/core/project-sdk/msbuild-props#sdkanalysislevel).

## Migrating from HTTP to HTTPS

To migrate your package sources from HTTP to HTTPS:

1. **Contact your feed administrator**: Ask if an HTTPS endpoint is available for your package source
2. **Update your NuGet.Config**: Change `http://` URLs to `https://` in all NuGet.Config files
3. **Update project files**: If you're using `RestoreSources` MSBuild properties, update those URLs as well
4. **Test the migration**: Run `dotnet restore` or `nuget restore` to verify the HTTPS sources work correctly

### Finding NuGet.Config Files

NuGet.Config files can exist at multiple levels:

- **Solution-level**: In the solution directory
- **User-level**: At `%AppData%\NuGet\NuGet.Config` (Windows) or `~/.config/NuGet/NuGet.Config` (Mac/Linux)
- **Machine-level**: In the NuGet installation directory

For more information, see [Common NuGet configurations](configuring-nuget-behavior.md).

## Related Errors and Warnings

When working with HTTP sources, you may encounter:

- [NU1302](../reference/errors-and-warnings/NU1302.md): Error indicating an HTTP source is not allowed
- [NU1803](../reference/errors-and-warnings/NU1803.md): Warning about HTTP source usage (in older SDK versions)

## See Also

- [HTTPS everywhere blog post](https://devblogs.microsoft.com/nuget/https-everywhere)
- [Package source mapping](package-source-mapping.md)
- [NuGet.Config reference](../reference/nuget-config-file.md)
- [Security best practices](../concepts/Security-Best-Practices.md)
