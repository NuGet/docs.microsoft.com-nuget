---
title: Creating Native NuGet Packages
description: Details on creating native NuGet packages that contains C++ code instead of managed code, for use in C++ projects.
author: JonDouglas
ms.author: jodou
ms.date: 01/09/2017
ms.topic: concept-article
---

# Creating native packages

A native package contains native binaries instead of managed assemblies, allowing it to be used within C++ (or similar) projects. (See [Native C++ Packages](../consume-packages/finding-and-choosing-packages.md#native-c-packages) in the Consume section.)

To be consumable in a C++ project, a package must target the `native` framework. At present there are not any version numbers associated with this framework as NuGet treats all C++ projects the same.

> [!Note]
> Be sure to include *native* in the `<tags>` section of your `.nuspec` to help other developers find your package by searching on that tag.

Native NuGet packages targeting `native` then provide files in `\build`, `\content`, and `\tools` folders; `\lib` is not used in this case (NuGet cannot directly add references to a C++ project). A package may also include targets and props files in `\build` that NuGet will automatically import into projects that consume the package. Those files must be named the same as the package ID with the `.targets` and/or `.props` extensions. For example, the [Microsoft.Web.WebView2](https://www.nuget.org/packages/Microsoft.Web.WebView2) package includes a `Microsoft.Web.WebView2.targets` file in its `\build` folder.

The `\build` folder can be used for all NuGet packages and not just native packages. The `\build` folder respects target frameworks just like the `\content`, `\lib`, and `\tools` folders. This means you can create a `\build\net40` folder and a `\build\net45` folder and NuGet will import the appropriate props and targets files into the project. (Use of PowerShell scripts to import MSBuild targets is not needed.)
