---
title: Set a NuGet package type
description: Describes packages types to indicate intended use of a package.
author: JonDouglas
ms.author: jodou
ms.date: 07/09/2019
ms.topic: how-to
---

# Set a NuGet package type

Packages can be marked with one more more *package types* to indicate its intended use.

## Known package types

- `Dependency` type packages add build- or run-time assets to libraries and applications, and can be installed in any project type (assuming they are compatible).

- `DotnetTool` type packages are .NET tools that can be installed by the [dotnet CLI](/dotnet/articles/core/tools/index).

- `MSBuildSdk` type packages are [MSBuild project SDKs](/visualstudio/msbuild/how-to-use-project-sdk) that simplifies using software development kits.

- `Template` type packages provide [custom templates](/dotnet/core/tools/custom-templates) that can be used to create files or projects like an app, service, tool, or class library.

- `McpServer` type packages contain MCP servers. This package type is always accompanied by the `DotnetTool` package type, because a local MCP server is distributed as a .NET tool. For information on MCP server and NuGet, see [MCP servers in NuGet packages](../concepts/nuget-mcp.md).

Packages not marked with a type, including all packages created with earlier versions of NuGet, default to the `Dependency` type.

> [!NOTE]
> Support for package types was added in NuGet 3.5.
> If you don't need a custom package type, it's best to *not* explicitly set the package type.
> NuGet defaults to the `Dependency` type when no type is specified.

## Custom package types

You can mark your package with one or more custom package types if its use does not fit the [known package types](#known-package-types).

For example, imagine that customers of the `Contoso` app can install extensions. The app could require extension authors to use the custom package type `ContosoExtension` to identify their packages as proper extensions that follow the required conventions.

> [!WARNING]
> A package with a custom package type cannot be installed by Visual Studio or nuget.exe. See [NuGet/Home#10468](https://github.com/NuGet/Home/issues/10468) for more information.

# [Using dotnet CLI](#tab/dotnet)

Package types can be set in the project file (`.csproj`):

```xml
<Project Sdk="Microsoft.NET.Sdk">

  <PropertyGroup>
    <TargetFramework>net5.0</TargetFramework>
    
    <PackageType>ContosoExtension</PackageType>
  </PropertyGroup>

</Project>
```

Packages with multiple intended uses can be marked with multiple package types using the `;` delimiter:

```xml
<Project Sdk="Microsoft.NET.Sdk">

  <PropertyGroup>
    <TargetFramework>net5.0</TargetFramework>
    
    <PackageType>PackageType1;PackageType2</PackageType>
  </PropertyGroup>

</Project>
```

Package types can be versioned using a `,` delimiter between the package type and its [`Version`](/dotnet/api/system.version) string:

```xml
<Project Sdk="Microsoft.NET.Sdk">

  <PropertyGroup>
    <TargetFramework>net5.0</TargetFramework>
    
    <PackageType>PackageType1, 1.0.0.0;PackageType2</PackageType>
  </PropertyGroup>

</Project>
```

# [Using nuget.exe](#tab/nugetexe)

Package types are set in the `.nuspec` file within a `packageTypes\packageType` node under the `<metadata>` element:

```xml
<?xml version="1.0" encoding="utf-8"?>
<package xmlns="http://schemas.microsoft.com/packaging/2010/07/nuspec.xsd">
    <metadata>
        <!-- ... -->
        <packageTypes>
            <packageType name="ContosoExtension" />
        </packageTypes>
    </metadata>
</package>
```

Packages with multiple intended uses may be marked with multiple package types:

```xml
<?xml version="1.0" encoding="utf-8"?>
<package xmlns="http://schemas.microsoft.com/packaging/2010/07/nuspec.xsd">
    <metadata>
        <!-- ... -->
        <packageTypes>
            <packageType name="PackageType1" />
            <packageType name="PackageType2" />
        </packageTypes>
    </metadata>
</package>
```

Package types can be versioned using a [`Version`](/dotnet/api/system.version) string:

```xml
<?xml version="1.0" encoding="utf-8"?>
<package xmlns="http://schemas.microsoft.com/packaging/2010/07/nuspec.xsd">
    <metadata>
        <!-- ... -->
        <packageTypes>
            <packageType name="PackageType1" version="1.0.0.0" />
            <packageType name="PackageType2" />
        </packageTypes>
    </metadata>
</package>
```

---

The format of a package type string is exactly like a package ID. That is, a package type is a case-insensitive string matching the regular expression `^\w+([_.-]\w+)*$` having at least one character and at most 100 characters.

If provided, the package type version is a [`Version`](/dotnet/api/system.version) string. The package type version is optional and defaults to `0.0`.
