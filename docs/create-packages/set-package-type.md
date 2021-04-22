---
title: Set a NuGet package type
description: Describes packages types to indicate intended use of a package.
author: JonDouglas
ms.author: jodou
ms.date: 07/09/2019
ms.topic: conceptual
---

# Set a NuGet package type

Packages can be marked with one more more *package types* to indicate its intended use.

## Known package types

- `Dependency` type packages add build- or run-time assets to libraries and applications, and can be installed in any project type (assuming they are compatible).

- `DotnetTool` type packages are .NET tools that can be installed by the [dotnet CLI](/dotnet/articles/core/tools/index).

- `Template` type packages provide [custom templates](/dotnet/core/tools/custom-templates) that can be used to create files or projects like an app, service, tool, or class library.

Packages not marked with a type, including all packages created with earlier versions of NuGet, default to the `Dependency` type.

> [!NOTE]
> Support for package types was added in NuGet 3.5.
> If you don't need a custom package type, it's best to *not* explicitly set the package type.
> NuGet defaults to the `Dependency` type when no type is specified.

## Custom package types

You can mark your package with one or more custom package types if its use does not fit the known package types. For example, imagine if customers of the `Contoso` app can install extensions. Extension authors could use the custom package type `ContosoExtension` to identify these packages as `Contoso` app extensions.

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

Package types can be versioned using a `,` delimiter between the package type and its version:

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

Package types can be versioned:

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

The format of a package type version is exactly like a [`Version`](/dotnet/api/system.version) string. The package type version is optional and defaults to `0.0`.
