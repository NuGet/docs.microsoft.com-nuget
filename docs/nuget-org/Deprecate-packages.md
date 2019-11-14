---
title: Deprecating packages on nuget.org
description: Detailed description on the process of deprecating packages and how the clients shows this information
author: anangaur
ms.author: anangaur
ms.date: 09/23/2019
ms.topic: conceptual
ms.reviewer: karann-msft
---

# Deprecating packages

You can deprecate a package if you no longer maintain a package or if would like to encourage your package's consumers to move to another package. 

Package deprecation is different than **unlisting** your package as explained below:
* **Unlisting** a package prevents its discovery because it is hidden in search results. 
* **Deprecating** a package lets your package's existing consumers find out if they have it installed or used in their projects. It also lets them know the reason for deprecation and an alternate recommended package as specified by you (the package publisher). Deprecating a package does not unlist the package. 

As a publisher, you may choose to both unlist as well as deprecate packages.

## Deprecation workflow
1. To deprecate a package, go to **Manage packages** and select **Deprecation**:

    ![Go to deprecate package option](media/deprecation-select-option.png)

2. Select the version you would like to deprecate. If you want to deprecate all version, choose **Select all versions** option.

    ![Select package versions to deprecate](media/deprecation-select-version.png)

3. Choose a reason for deprecation. If the package is no longer maintained, choose the **Legacy** option. If the specific version has a critical bug, choose the **has critical bugs** option. For any other reason, select **Other**. You can always specify an alternate recommended package (and version) and a custom message to the owners. 

    ![Select reasons alternate package recommendation and custom message](media/deprecation-save.png)

> [!Note]
> Custom message is only shown on nuget.org but not from the clients. Currently, clients such as `dotnet.exe` and the NuGet Package Manager do not show the custom message.

## Client experience for deprecated packages
Once a package has been deprecated, its consumers are notified about it in the following ways (depending upon the client used).

### Visual Studio 
*Available starting in Visual Studio 2019 version 16.3*

Visual Studio warns about a deprecated package's usage on the `Installed` tab. It will show a warning for the package and its deprecation information (including the reason it was deprecated and the alternate package to use instead, if present).

   ![Deprecated packages on Visual Studio installed tab of package manager](media/deprecation-vs.png)

### dotnet.exe
*Available starting with .NET SDK 3.0*

If you use dotnet.exe, you can run the command `dotnet list package --deprecated` on the solution or project folder to get a list of deprecated packages along with the deprecation information:

```
> dotnet list package --deprecated

The following sources were used:
   https://api.nuget.org/v3/index.json

Project `My.Test.Project` has the following deprecated packages
   [netcoreapp3.0]:
   Top-level Package      Resolved   Reason(s)   Alternative
   > My.Sample.Lib        6.0.0      Legacy      My.Awesome.Package

```
