---
title: Set a NuGet package type
description: Describes packages types to indicate intended use of a package.
author: JonDouglas
ms.author: jodou
ms.date: 07/09/2019
ms.topic: conceptual
---

# Set a NuGet package type

Packages can be marked with one more more *package types* to indicate its intended use. Packages not marked with a type, including all packages created with earlier versions of NuGet, default to the `Dependency` type.

- `Dependency` type packages add build- or run-time assets to libraries and applications, and can be installed in any project type (assuming they are compatible).

- `DotnetTool` type packages are .NET tools that can be installed by the [dotnet CLI](/dotnet/articles/core/tools/index).

- `Template` type packages provide [custom templates](/dotnet/core/tools/custom-templates) that can be used to create files or projects like an app, service, tool, or class library.

- Custom type packages use an arbitrary type identifier that conforms to the same format rules as package IDs. Any type other than `Dependency` and `DotnetTool`, however, are not recognized by the NuGet Package Manager in Visual Studio.

> [!NOTE]
> Support for package types was added in NuGet 3.5.
> If you don't need a custom package type, it's best to *not* explicitly set the `Dependency` type. NuGet defaults to `Dependency` this type when no type is specified.

# [Using dotnet CLI](#tab/dotnet)

Package types can be set in the project file (`.csproj`):

```xml
<Project Sdk="Microsoft.NET.Sdk">

  <PropertyGroup>
    <TargetFramework>net5.0</TargetFramework>
    
    <PackageType>MyCustomType, 1.0.0.0;Dependency, 1.0.0.0</PackageType>
  </PropertyGroup>

</Project>
```

# [Using nuget.exe](#tab/nugetexe)

Package types are set in the `.nuspec` file. It's best for backwards compatibility to *not* explicitly set the `Dependency` type and to instead rely on NuGet assuming this type when no type is specified.

With NuGet 3.5+

Indicate the package type within a `packageTypes\packageType` node under the `<metadata>` element:

```xml
<?xml version="1.0" encoding="utf-8"?>
<package xmlns="http://schemas.microsoft.com/packaging/2010/07/nuspec.xsd">
    <metadata>
    <!-- ... -->
    <packageTypes>
        <packageType name="DotnetTool" />
    </packageTypes>
    </metadata>
</package>
```

---

> [!NOTE]
> Support for package types was added in NuGet 3.5.
> If you don't need a custom package type, it's best to *not* explicitly set the `Dependency` type. NuGet defaults to `Dependency` this type when no type is specified.
