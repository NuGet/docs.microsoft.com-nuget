---
title: NuGet packages.config File Reference
description: In some project types, packages.config maintains the list of NuGet packages used in the project.
author: JonDouglas
ms.author: jodou
ms.date: 05/21/2018
ms.topic: reference
---

# packages.config reference

The `packages.config` file is used in some project types to maintain the list of packages referenced by the project. This allows NuGet to easily restore the project's dependencies when the project is to be transported to a different machine, such as a build server, without all those packages.

If used, `packages.config` must be located in a project root. It's automatically created when the first NuGet operation is run, but can also be created manually before running any commands such as `nuget restore`.

Projects that use [PackageReference](../consume-packages/Package-References-in-Project-Files.md) do not use `packages.config`.

## Schema

The schema is simple: following the standard XML header is a single `<packages>` node that contains one or more `<package>` elements, one for each reference. Each `<package>` element can have the following attributes:

| Attribute | Required | Description |
| --- | --- | --- |
| id | Yes | The identifier of the package, such as Newtonsoft.json or Microsoft.AspNet.Mvc. | 
| version | Yes | The exact version of the package to install, such as 3.1.1 or 4.2.5.11-beta. A version string must have at least three numbers; a fourth is optional, as is a pre-release suffix. Ranges are not allowed. | 
| targetFramework | No | The [target framework moniker (TFM)](target-frameworks.md) to apply when installing the package. This is initially set to the project's target when a package is installed. As a result, different `<package>` elements can have different TFMs. For example, if you create a project targeting .NET 4.5.2, packages installed at that point will use the TFM of net452. If you ;later retarget the project to .NET 4.6 and add more packages, those will use TFM of net46. A mismatch between the project's target and `targetFramework` attributes will generate warnings, in which case you can reinstall the affected packages. | 
| allowedVersions | No | A range of allowed versions for this package applied during package update (see [Constraining upgrade versions](../consume-packages/reinstalling-and-updating-packages.md#constraining-upgrade-versions). It does *not* affect what package is installed during an install or restore operation. See [Package versioning](../concepts/package-versioning.md#version-ranges) for syntax. The PackageManager UI also disables all versions outside the allowed range. | 
| developmentDependency | No | If the consuming project itself creates a NuGet package, setting this to `true` for a dependency prevents that package from being included when the consuming package is created. The default is `false`. | 

## Examples

The following `packages.config` refers to two dependencies:

```xml
<?xml version="1.0" encoding="utf-8"?>
<packages>
  <package id="jQuery" version="3.1.1" targetFramework="net46" />
  <package id="NLog" version="4.3.10" targetFramework="net46" />
</packages>
```

The following `packages.config` refers to nine packages, but `Microsoft.Net.Compilers` will not be included when building the consuming package because of the `developmentDependency` attribute. The reference to Newtonsoft.Json also restricts updates to 8.x and 9.x versions only.

```xml
<?xml version="1.0" encoding="utf-8"?>
<packages>
  <package id="Microsoft.CodeDom.Providers.DotNetCompilerPlatform" version="1.0.0" targetFramework="net46" />
  <package id="Microsoft.Net.Compilers" version="1.0.0" targetFramework="net46" developmentDependency="true" />
  <package id="Microsoft.Web.Infrastructure" version="1.0.0.0" targetFramework="net46" />
  <package id="Microsoft.Web.Xdt" version="2.1.1" targetFramework="net46" />
  <package id="Newtonsoft.Json" version="8.0.3" allowedVersions="[8,10)" targetFramework="net46" />
  <package id="NuGet.Core" version="2.11.1" targetFramework="net46" />
  <package id="NuGet.Server" version="2.11.2" targetFramework="net46" />
  <package id="RouteMagic" version="1.3" targetFramework="net46" />
  <package id="WebActivatorEx" version="2.1.0" targetFramework="net46" />
</packages>
```
