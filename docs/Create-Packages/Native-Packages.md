---
# required metadata

title: Creating Native NuGet Packages | Microsoft Docs
author: kraigb
ms.author: kraigb
manager: ghogen
ms.date: 1/9/2017
ms.topic: article
ms.prod: nuget
#ms.service:
ms.technology: nuget
ms.assetid: 7a70d748-efe2-4f8f-a3fd-67ddb0f6214e

# optional metadata

description: Details on creating native NuGet packages that contains C++ code instead of managed code, for use in C++ projects.
keywords: NuGet native packages, NuGet C++ packages, native code packages, targeting C++ projects
#ROBOTS:
#audience:
#ms.devlang:
ms.reviewer:
- karann
- unnir
#ms.suite:
#ms.tgt_pltfrm:
#ms.custom:

---
# Creating native packages

A native package contains native C++ code instead of managed code, allowing it to be used within C++ projects. (See [Native C++ Packages](../consume-packages/finding-and-choosing-packages.md#native-cpp-packages) in the Consume section.)

To be consumable in a C++ project, a package must target the `native` framework. At present there are not any version numbers associated with this framework as NuGet treats all C++ projects the same.

> [!Note]
> Be sure to include *native* in the `<tags>` section of your `.nuspec` to help other developers find your package by searching on that tag.

Native NuGet packages targeting `native` then provide files in `\build`, `\content`, and `\tools` folders; `\lib` is not used in this case (NuGet cannot directly add references to a C++ project). A package may also include targets and props files in `\build` that NuGet will automatically import into projects that consume the package. Those files must be named the same as the package ID with the `.targets` and/or `.props` extensions. For example, the [cpprestsdk](https://nuget.org/packages/cpprestsdk/) package includes a `cpprestsdk.targets` file in its `\build` folder.

The `\build` folder can be used for all NuGet packages and not just native packages. The `\build` folder respects target frameworks just like the `\content`, `\lib`, and `\tools` folders. This means you can create a `\build\net40` folder and a `\build\net45` folder and NuGet will import the appropriate props and targets files into the project. (Use of PowerShell scripts to import MSBuild targets is not needed.)
