---
# required metadata

title: Create .NET Standard NuGet Packages with Visual Studio 2015 | Microsoft Docs
author: kraigb
ms.author: kraigb
manager: ghogen
ms.date: 1/9/2017
ms.topic: article
ms.prod: nuget
#ms.service:
ms.technology: null
ms.assetid: 29b3bceb-0f35-4cdd-bbc3-a04eb823164c

# optional metadata

description: An end-to-end walkthrough of creating .NET standard NuGet packages using NuGet 3.x and Visual Studio 2015.
keywords: create a package, .NET Standard packages, .NET standard mapping table
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

# Create .NET standard packages with Visual Studio 2015

*Applies to NuGet 3.x. See [Create .NET Standard Packages with Visual Studio 2017](../guides/create-net-standard-packages-vs2017.md) for working with NuGet 4.x+.*

The [.NET Standard Library](https://docs.microsoft.com/dotnet/articles/standard/library) is a formal specification of .NET APIs intended to be available on all .NET runtimes, thus establishing greater uniformity in the .NET ecosystem. The .NET Standard Library defines a uniform set of BCL (Base Class Library) APIs for all .NET platforms to implement, independent of workload. It enables developers to produce PCLs that are usable across all .NET runtimes, and reduces if not eliminates platform-specific conditional compilation directives in shared code.

This guide will walk you through creating a nuget package targeting .NET Standard Library 1.4. This will work across .NET Framework 4.6.1, Universal Windows Platform 10, .NET Core, and Mono/Xamarin. For details, see the [.NET Standard mapping table](#net-standard-mapping-table) later in this topic.

1. [Pre-requisites](#pre-requisites)
1. [Create the class library project](#create-the-class-library-project)
1. [Create and update the .nuspec file](#create-and-update-the-nuspec-file)
1. [Package the component](#package-the-component)
1. [Additional options](#additional-options)
1. [.NET Standard mapping table](#net-standard-mapping-table)
1. [Related topics](#related-topics)


## Pre-requisites

1. Visual Studio 2015. Install the Community edition for free from [visualstudio.com](https://www.visualstudio.com/); you can use the Professional and Enterprise editions as well, of course.
1. .NET Core: Install .NET Core along with templates and other tools for Visual Studio 2015 from [https://go.microsoft.com/fwlink/?LinkId=824849](https://go.microsoft.com/fwlink/?LinkId=824849).
1. NuGet CLI. Download the latest version of nuget.exe from [nuget.org/downloads](https://nuget.org/downloads), saving it to a location of your choice. Then add that location to your PATH environment variable if it isn't already.

> [!Note]
> nuget.exe is the CLI tool itself, not an installer, so be sure to save the downloaded file from your browser instead of running it.



## Create the class library project

1. In Visual Studio, **File > New > Project**, expand the **Visual C# > Windows** node, select **Class Library (Portable)**, change the name to AppLogger, and click OK.

    ![Create new class library project](media/NetStandard-NewProject.png)

1. In the **Add Portable Class Library** dialog that appears, select the `.NET Framework 4.6` and `ASP.NET Core 1.0` options.
1. Right-click the `AppLogger (Portable)` in Solution Explorer, select **Properties**, select the **Library** tab, then click **Target .NET Platform Standard** in the **Targeting** section. This will prompt for confirmation, after which you can select `.NET Standard 1.4` from the drop down:

    ![Setting the target to .NET Standard 1.4](media/NetStandard-ChangeTarget.png)

1. Click on the **Build** tab, change the **Configuration** to `Release`, and check the box for **XML documentation file**.
1. Add your code to the component, for example:

    ```cs
    namespace AppLogger
    {
        public class Logger
        {
            public void Log(string text)
            {
                throw new NotImplementedException("Called Log");
            }
        }
    }
    ```

1. Build the project (with the Release configuration) and check that DLL and XML files are produced within the bin\Release folder.

## Create and update the .nuspec file

1. Open a command prompt, navigate to the folder containing `AppLogg.csproj` folder (one level below where the `.sln` file is), and run the NuGet `spec` command to create the initial `AppLogger.nuspec` file:

```
nuget spec
```

1. Open `AppLogger.nuspec` in an editor and update it to match the following, replacing YOUR_NAME with an appropriate value. The `<id>` value, specifically, must be unique across nuget.org (see the naming conventions described in [Creating a package](../create-packages/creating-a-package.md#choosing-a-unique-package-identifier-and-setting-the-version-number). Also note that you must also update the author and description tags or you'll get an error during the packing step.

```xml
<?xml version="1.0"?>
<package >
    <metadata>
    <id>AppLogger.YOUR_NAME</id>
    <version>1.0.0</version>
    <title>AppLogger</title>
    <authors>YOUR_NAME</authors>
    <owners>YOUR_NAME</owners>
    <requireLicenseAcceptance>false</requireLicenseAcceptance>
    <description>Awesome application logging utility</description>
    <releaseNotes>First release</releaseNotes>
    <copyright>Copyright 2016 (c) Contoso Corporation. All rights reserved.</copyright>
    <tags>logger logging logs</tags>
    </metadata>
</package>
```

1. Add reference assemblies to the `.nuspec` file, namely the library's DLL and the IntelliSense XML file:

    ```xml
    <!-- Insert below <metadata> element -->
    <files>
        <file src="bin\Release\AppLogger.dll" target="lib\netstandard1.4\AppLogger.dll" />
        <file src="bin\Release\AppLogger.xml" target="lib\netstandard1.4\AppLogger.xml" />
    </files>
    ```

1. Right-click the solution and select **Build Solution** to generate all the files for the package.


## Package the component

With the completed `.nuspec` referencing all the files you need to include in the package, you're ready to run the `pack` command:

```
nuget pack AppLogger.nuspec
```

This will generate `AppLogger.YOUR_NAME.1.0.0.nupkg`. Opening this file in a tool like the [NuGet Package Explorer](https://github.com/NuGetPackageExplorer/NuGetPackageExplorer) and expanding all the nodes, you'll see the following contents:

![NuGet Package Explorer showing the AppLogger package](media/NetStandard-PackageExplorer.png)

> [!Tip]
> A `.nupkg` file is just a ZIP file with a different extension. You can also examine package contents, then, by changing `.nupkg` to `.zip`, but remember to restore the extension before uploading a package to nuget.org.

To make your package available to other developers,  follow the instructions on [Publish a package](../create-packages/publish-a-package.md).

Note that `pack` requires Mono 4.4.2 on Mac OS X and does not work on Linux systems. On a Mac, you must also convert Windows pathnames in the `.nuspec` file to Unix-style paths.

## Additional options

The following sections go into additional options for NuGet package creation:

- [Declaring dependencies](#declaring-dependencies)
- [Supporting multiple target frameworks](#supporting-multiple-target-frameworks)
- [Adding targets and props for MSBuild](#adding-targets-and-props-for-msbuild)
- [Creating localized packages](#creating-localized-packages)
- [Adding a readme](#adding-a-readme)

### Declaring dependencies

If you have any dependencies on other NuGet packages, list those in the `<dependencies>` element with `<group>` elements. For example, to declare a dependency on NewtonSoft.Json 8.0.3 or above, add the following:

```xml
<!-- Insert within the <metadata> element -->
<dependencies>
    <group targetFramework="uap">
        <dependency id="Newtonsoft.Json" version="8.0.3" />
    </group>
</dependencies>
```

The syntax of the *version* attribute here indicates that version 8.0.3 or above is acceptable. To specify different version ranges, refer to [Dependency Versions](../create-packages/dependency-versions.md).

### Supporting multiple target frameworks

Suppose you'd like to take advantage of an API in .NET Framework 4.6.2 that is not available in .NET Standard 1.4. To do this, you'll first need to make sure the library compiles for .NET 4.6.2 by using conditional compilation or shared projects. (In Visual Studio, you can create a NetCore project, add the framework of choice to the multiple framework section, and then build.) Then you create the package using the simple convention-based working directory technique as follows:

1. In the project's root folder containing your `.nuspec` file, create a folder named `lib`.
1. Inside `lib`, create folders for each platform you want to support:

        \lib
            \netstandard1.4
                \AppLogger.dll
            \net462
                \AppLogger.dll

1. In the `.nuspec` file, add a `files` node under the `package` node and refer to the files in `lib` using wildcards. **Note:** Token replacements are not supported with the convention-based working directory approach, so replace them with literal values:

    ```xml
    <?xml version="1.0"?>
    <package >
        <metadata>
        <id>AppLogger.YOUR_NAME</id>
        <version>1.0.0.0</version>
        <title>AppLogger</title>
        <authors>YOUR_NAME</authors>
        <owners>YOUR_NAME</owners>
        <requireLicenseAcceptance>false</requireLicenseAcceptance>
        <description>Awesome application logging utility</description>
        <releaseNotes>First release.</releaseNotes>
        <copyright>Copyright 2016</copyright>
        <tags>logger logging logs</tags>
        </metadata>
        <files>
            <file src="lib\**" target="lib" />
        </files>
    </package>
    ```

1. Create the package again using `nuget pack AppLogger.spec`.

For more details on using this technique, see [Supporting Multiple .NET Framework Versions](../create-packages/supporting-multiple-target-frameworks.md)

### Adding targets and props for MSBuild

In some cases you might want to add custom build targets or properties in projects that consume your package, such as running a custom tool or process during build. You do this by adding files in a `\build` folder as described in the steps below. When NuGet installs a package with \build files, it adds an MSBuild element in the project file pointing to the .targets and .props files.

> [!Note]
> When using `project.json`, targets are not added to the project but are made available through the `project.lock.json`.


1. In the project folder containing the your `.nuspec` file, create a folder named `build`.
1. Inside `build`, create folders for each supported, and within those place your `.targets` and `.props` files:

        \build
            \netstandard1.4
                \AppLogger.props
                \AppLogger.targets
            \net462
                \AppLogger.props
                \AppLogger.targets

1. In the `.nuspec` file, add a `files` node under the `package` node and refer to the files in `build` using wildcards.

    ```xml
    <?xml version="1.0"?>
    <package >
        <metadata>...
        </metadata>
        <files>
            <file src="build\**" target="build" />
        </files>
    </package>
    ```

1. Create the package again using `nuget pack AppLogger.nuspec`.

For additional details, refer to [Include MSBuild props and targets in a package](../create-packages/creating-a-package.md#including-msbuild-props-and-targets-in-a-package).


### Creating localized packages

To create localized versions of your library, you can either create separate packages for different locales, or include localized resource assemblies within a single package. Here's how to do the latter approach for German and Italian:

1. Within each target framework folder under `lib`, create folders for each supported language other than the English default. In these folders you can place resource assemblies  and localized IntelliSense XML files. For example:

        lib
        ├───netstandard1.4
        │   │   AppLogger.dll
        │   │   AppLogger.xml
        │   │
        │   ├───de
        │   │       AppLogger.resources.dll
        │   │       AppLogger.xml
        │   │
        │   └───it
        │           AppLogger.resources.dll
        │           AppLogger.xml
        └───net462
            │   AppLogger.dll
            │   AppLogger.xml
            │
            ├───de
            │       AppLogger.resources.dll
            │       AppLogger.xml
            │
            └───it
                    AppLogger.resources.dll
                    AppLogger.xml

1. In the `.nuspec` file, reference these files in the `<files>` node:

    ```xml
    <?xml version="1.0"?>
    <package>
        <metadata>...
        </metadata>
        <files>
        <file src="lib\**" target="lib" />
        </files>
    </package>
    ```

1. Create the package again using `nuget pack AppLogger.nuspec`.


### Adding a readme

When you include a `readme.txt` file in the root of the package, Visual Studio will display it when the package is installed directly.

> [!Note]
> Readme files are not shown for packages that are installed as a dependency, or for .NET Core projects.


To do this, create your `readme.txt` file, place it in the project root folder, and refer to it in the `.nuspec` file:

```xml
<?xml version="1.0"?>
<package >
    <metadata>...
    </metadata>
    <files>
    <file src="readme.txt" target="" />
    </files>
</package>
```


## .NET Standard mapping table

|Platform Name |Alias|
|--------------|-----|
|.NET Standard | netstandard| 1.0| 1.1| 1.2| 1.3| 1.4| 1.5| 1.6|
|.NET Core | netcoreapp| &#x2192;| &#x2192;| &#x2192;| &#x2192;| &#x2192;| &#x2192;| 1.0|
|.NET Framework| net| 4.5| 4.5.1| 4.6| 4.6.1| 4.6.2| 4.6.3|
|Mono/Xamarin Platforms| &#x2192;| &#x2192;| &#x2192;| &#x2192;| &#x2192;| &#x2192;|
|Universal Windows Platform| uap| &#x2192;| &#x2192;| &#x2192;| &#x2192;|10.0|
|Windows| win| &#x2192;| 8.0| 8.1|
|Windows Phone| wpa| &#x2192;| &#x2192;|8.1|
|Windows Phone Silverlight| wp| 8.0|



## Related topics

- [Nuspec Reference](../schema/nuspec.md)
- [Symbol packages](../create-packages/symbol-packages.md)
- [Dependency Versions](../create-packages/dependency-versions.md)
-[Supporting Multiple .NET Framework Versions](../create-packages/supporting-multiple-target-frameworks.md)
- [Include MSBuild props and targets in a package](../create-packages/creating-a-package.md#including-msbuild-props-and-targets-in-a-package)
- [Creating Localized Packages](../create-packages/creating-localized-packages.md)
- [.NET Standard Library documentation](https://docs.microsoft.com/dotnet/articles/standard/library)
- [Porting to .NET Core from .NET Framework](https://docs.microsoft.com/dotnet/articles/core/porting/index)