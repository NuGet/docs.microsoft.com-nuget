---
title: Multi-targeting for NuGet Packages in your project file
description: Description of the various methods to target multiple .NET Framework versions from within a single NuGet package in your project file.
author: JonDouglas
ms.author: jodou
ms.date: 07/15/2019
ms.topic: how-to
---

# Support multiple .NET Framework versions in your project file

When you first create a project, we recommend you create a .NET Standard class library, as it provides compatibility with the widest range of consuming projects. By using .NET Standard, you add [cross-platform support](/dotnet/standard/library-guidance/cross-platform-targeting) to a .NET library by default. However, in some scenarios, you may also need to include code that targets a particular framework. This article shows you how to do that for [SDK-style](../resources/check-project-format.md) projects.

For SDK-style projects, you can configure support for multiple targets frameworks ([TFM](/dotnet/standard/frameworks)) in your project file, then use `dotnet pack` or `msbuild /t:pack` to create the package.

> [!NOTE]
> nuget.exe CLI does not support packing SDK-style projects, so you should only use `dotnet pack` or `msbuild /t:pack`. We recommend that you [include all the properties](../reference/msbuild-targets.md#pack-target) that are usually in the `.nuspec` file in the project file instead. To target multiple .NET Framework versions in a non-SDK-style project, see [Supporting multiple .NET Framework versions](supporting-multiple-target-frameworks.md).

## Create a project that supports multiple .NET Framework versions

1. Create a new .NET Standard class library either in Visual Studio or use `dotnet new classlib`.

   We recommend that you create a .NET Standard class library for best compatibility.

2. Edit the *.csproj* file to support the target frameworks. For example, change
   
   `<TargetFramework>netstandard2.0</TargetFramework>`
   
   to:
   
   `<TargetFrameworks>netstandard2.0;net45</TargetFrameworks>`

   Make sure that you change the XML element changed from singular to plural (add the "s" to both the open and close tags).

3. If you have any code that only works in one TFM, you can use `#if NET45` or `#if NETSTANDARD2_0` to separate TFM-dependent code. (For more information, see [How to multitarget](/dotnet/core/tutorials/libraries#how-to-multitarget).) For example, you can use the following code:

   ```csharp
   public string Platform {
      get {
   #if NET45
         return ".NET Framework"
   #elif NETSTANDARD2_0
         return ".NET Standard"
   #else
   #error This code block does not match csproj TargetFrameworks list
   #endif
      }
   }
   ```

4. Add any NuGet metadata you want to the *.csproj* as MSBuild properties.

   For the list of available package metadata and the MSBuild property names, see [pack target](../reference/msbuild-targets.md#pack-target). Also see [Controlling dependency assets](../consume-packages/package-references-in-project-files.md#controlling-dependency-assets).

   If you want to separate build-related properties from NuGet metadata, you can use a different `PropertyGroup`, or put the NuGet properties in another file and use MSBuild's `Import` directive to include it. `Directory.Build.Props` and `Directory.Build.Targets` are also supported starting with MSBuild 15.0.

5. Now, use `dotnet pack` and the resulting *.nupkg* targets both .NET Standard 2.0 and .NET Framework 4.5.

Here is the *.csproj* file that is generated using the preceding steps and .NET Core SDK 2.2.

```xml
<Project Sdk="Microsoft.NET.Sdk">

  <PropertyGroup>
    <TargetFrameworks>netstandard2.0;net45</TargetFrameworks>
    <Description>Sample project that targets multiple TFMs</Description>
  </PropertyGroup>

</Project>
```

## See also

* [How to specify target frameworks](/dotnet/standard/frameworks#how-to-specify-target-frameworks)
* [Cross-platform targeting](/dotnet/standard/library-guidance/cross-platform-targeting)
