---
# required metadata

title: Create NuGet Packages for the Universal Windows Platform | Microsoft Docs
author: kraigb
ms.author: kraigb
manager: ghogen
ms.date: 7/17/2017
ms.topic: article
ms.prod: nuget
#ms.service:
ms.technology: null
ms.assetid: d98524b1-a674-4803-8ac5-3c6bce867f86

# optional metadata

description: An end-to-end walkthrough of creating NuGet packages using a Windows Runtime Component for the Universal Windows Platform.
keywords: create a package, packages for UWP, Windows Runtime Components
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

# Create UWP packages

The [Universal Windows Platform (UWP)](https://developer.microsoft.com/windows) provides a common app platform for every device that runs Windows 10. Within this model, UWP apps can call both the WinRT APIs that are common to all devices, and also APIs (including Win32 and .NET) that are specific to the device family on which the app is running.

In this walkthrough you'll create a NuGet package with a native UWP component (including a XAML control) that can be used in both Managed and Native projects.

1. [Pre-requisites](#pre-requisites)
1. [Create a UWP Windows Runtime Component](#create-a-uwp-windows-runtime-component)
1. [Create and update the .nuspec file](#create-and-update-the-nuspec-file)
1. [Package the component](#package-the-component)
1. [Related topics](#related-topics)

## Pre-requisites

1. Visual Studio 2017 or Visual Studio 2015. Install the Community edition for free from [visualstudio.com](https://www.visualstudio.com/); you can use the Professional and Enterprise editions as well.
1. NuGet CLI. Download the latest version of nuget.exe from [nuget.org/downloads](https://nuget.org/downloads), saving it to a location of your choice. Then add that location to your PATH environment variable if it isn't already.

> [!Note]
> nuget.exe is the CLI tool itself, not an installer, so be sure to save the downloaded file from your browser instead of running it.


## Create a UWP Windows Runtime Component

1. In Visual Studio, choose **File > New > Project**, expand the **Visual C++ > Windows > Universal** node, select the **Windows Runtime Component (Universal Windows)** template, change the name to ImageEnhancer, and click OK. Accept the default values for Target Version and Minimum Version when prompted.

    ![Creating a new UWP Windows Runtime Component project](media/UWP-NewProject.png)

1. Right click the project in Solution Explorer, select **Add > New Item**, click the **Visual C++ > XAML** node, select **Templated Control**, change the name to AwesomeImageControl.cpp, and click **Add**:

    ![Adding a new XAML Templated Control item to the project](media/UWP-NewXAMLControl.png)

1. Right-click the project in Solution Explorer and select **Properties.** In the Properties page, expand **Configuration Properties > C/C++** and click **Output Files**. In the pane on the right, change the value for **Generate XML Documentation Files** to Yes:

    ![Setting Generate XML Documentation Files to Yes](media/UWP-GenerateXMLDocFiles.png)

1. Right click the *solution* now, select **Batch Build**, check the three Debug boxes in the dialog as shown below. This makes sure that when you do a build, you'll generate a full set of artifacts for each of the target systems that Windows supports.

    ![Batch Build](media/UWP-BatchBuild.png)

1. In the Batch Build dialog, and click **Build** to verify the project and create the output files that you'll need for the NuGet package.

> [!Note]
> In this walkthrough you'll use the Debug artifacts for the package. For non-debug package, check the Release options in the Batch Build dialog instead, and refer to the resulting Release folders in the steps that follow.


## Create and update the .nuspec file

To create the initial `.nuspec` file, do the three steps below. The sections that follow then guide you through other necessary updates.

1. Open a command prompt and navigate to the folder containing `ImageEnhancer.vcxproj` (this will be a subfolder below where the solution file is).
1. Run the NuGet `spec` command to generate `ImageEnhancer.nuspec` (the name of the file is taken from the name of the `.vcxproj` file):

    ```
    nuget spec
    ```

1. Open `ImageEnhancer.nuspec` in an editor and update it to match the following, replacing YOUR_NAME with an appropriate value. The `<id>` value, specifically, must be unique across nuget.org (see the naming conventions described in [Creating a package](../create-packages/creating-a-package.md#choosing-a-unique-package-identifier-and-setting-the-version-number)). Also note that you must also update the author and description tags or you'll get an error during the packing step.

    ```xml
    <?xml version="1.0"?>
    <package >
        <metadata>
        <id>ImageEnhancer.YOUR_NAME</id>
        <version>1.0.0</version>
        <title>ImageEnhancer</title>
        <authors>YOUR_NAME</authors>
        <owners>YOUR_NAME</owners>
        <requireLicenseAcceptance>false</requireLicenseAcceptance>
        <description>Awesome Image Enhancer</description>
        <releaseNotes>First release</releaseNotes>
        <copyright>Copyright 2016</copyright>
        <tags>image enhancer imageenhancer</tags>
        </metadata>
    </package>
    ```

> [!Note]
> For packages built for public consumption, pay special attention to the `<tags>` element, as these tags help others find your package and understand what it does.



### Adding Windows metadata to the package

A Windows Runtime Component requires metadata that describes all of its publicly available types, which makes it possible for other apps and libraries to consume the component. This metadata is contained in a .winmd file, which is created when you compile the project and must be included in your NuGet package. An XML file with IntelliSense data is also built at the same time, and should be included as well.

Add the following `<files>` node to the `.nuspec` file:

```xml
<package>
    <metadata>
        ...
    </metadata>

    <files>
        <!-- WinMd and IntelliSense files -->
        <file src="..\Debug\ImageEnhancer\ImageEnhancer.winmd" target="lib\uap10.0"/>
        <file src="..\Debug\ImageEnhancer\ImageEnhancer.xml" target="lib\uap10.0"/>
    </files>
</package>
```

### Adding XAML content

To include a XAML control with your component, you need to add the XAML file that has the default template for the control (as generated by the project template). This also goes in the `<files>` section:

```xml
<?xml version="1.0"?>
<package >
    <metadata>
        ...
    </metadata>
    <files>
        ...

        <!-- XAML controls -->
        <file src="Themes\Generic.xaml" target="lib\uap10.0\Themes"/>

    </files>
</package>
```

### Adding the native implementation libraries

Within your component, the core logic of the ImageEnhancer type is in native code, which is contained in the various `ImageEnhancer.dll` assemblies that are generated for each target runtime (ARM, x86, and x64). To include these in the package, reference them in the `<files>` section along with their associated .pri resource files:

```xml
<?xml version="1.0"?>
<package >
    <metadata>
        ...
    </metadata>
    <files>
        ...

        <!-- DLLs and resources -->
        <file src="..\ARM\Debug\ImageEnhancer\ImageEnhancer.dll" target="runtimes\win10-arm\native"/>
        <file src="..\ARM\Debug\ImageEnhancer\ImageEnhancer.pri" target="runtimes\win10-arm\native"/>

        <file src="..\x64\Debug\ImageEnhancer\ImageEnhancer.dll" target="runtimes\win10-x64\native"/>
        <file src="..\x64\Debug\ImageEnhancer\ImageEnhancer.pri" target="runtimes\win10-x64\native"/>

        <file src="..\Debug\ImageEnhancer\ImageEnhancer.dll" target="runtimes\win10-x86\native"/>
        <file src="..\Debug\ImageEnhancer\ImageEnhancer.pri" target="runtimes\win10-x86\native"/>

    </files>
</package>
```

### Adding .targets

Next, C++ and JavaScript projects that might consume your NuGet package need a .targets file to identify the necessary assembly and winmd files. (C# and Visual Basic projects do this automatically.) Create this file by copying the text below into `ImageEnhancer.targets` and save it in the same folder as the `.nuspec` file:

```xml
<?xml version="1.0" encoding="utf-8"?>
<Project xmlns="http://schemas.microsoft.com/developer/msbuild/2003">
    <PropertyGroup>
        <ImageEnhancer-Platform Condition="'$(Platform)' == 'Win32'">x86</ImageEnhancer-Platform>
        <ImageEnhancer-Platform Condition="'$(Platform)' != 'Win32'">$(Platform)</ImageEnhancer-Platform>
    </PropertyGroup>
    <ItemGroup Condition="'$(TargetPlatformIdentifier)' == 'UAP'">
        <Reference Include="$(MSBuildThisFileDirectory)..\..\lib\uap10.0\ImageEnhancer.winmd">
            <Implementation>ImageEnhancer.dll</Implementation>
        </Reference>
    <ReferenceCopyLocalPaths Include="$(MSBuildThisFileDirectory)..\..\runtimes\win10-$(ImageEnhancer-Platform)\native\ImageEnhancer.dll" />
    </ItemGroup>
</Project>
```

Then refer to `ImageEnhancer.targets` in your `.nuspec` file:

```xml
<?xml version="1.0"?>
<package >
    <metadata>
        ...
    </metadata>
    <files>
        ...

        <!-- .targets -->
        <file src="ImageEnhancer.targets" target="build\native"/>

    </files>
</package>
```

### Final .nuspec

Your final `.nuspec` file should now look like the following, where again YOUR_NAME should be replaced with an appropriate value:

```xml
<?xml version="1.0"?>
<package >
    <metadata>
    <id>ImageEnhancer.YOUR_NAME</id>
    <version>1.0.0</version>
    <title>ImageEnhancer</title>
    <authors>YOUR_NAME</authors>
    <owners>YOUR_NAME</owners>
    <requireLicenseAcceptance>false</requireLicenseAcceptance>
    <description>Awesome Image Enhancer</description>
    <releaseNotes>First Release</releaseNotes>
    <copyright>Copyright 2016</copyright>
    <tags>image enhancer imageenhancer</tags>
    </metadata>
    <files>
    <!-- WinMd and IntelliSense -->
    <file src="..\Debug\ImageEnhancer\ImageEnhancer.winmd" target="lib\uap10.0"/>
    <file src="..\Debug\ImageEnhancer\ImageEnhancer.xml" target="lib\uap10.0"/>

    <!-- XAML controls -->
    <file src="Themes\Generic.xaml" target="lib\uap10.0\Themes"/>

    <!-- DLLs and resources -->
    <file src="..\ARM\Debug\ImageEnhancer\ImageEnhancer.dll" target="runtimes\win10-arm\native"/>
    <file src="..\ARM\Debug\ImageEnhancer\ImageEnhancer.pri" target="runtimes\win10-arm\native"/>
    <file src="..\x64\Debug\ImageEnhancer\ImageEnhancer.dll" target="runtimes\win10-x64\native"/>
    <file src="..\x64\Debug\ImageEnhancer\ImageEnhancer.pri" target="runtimes\win10-x64\native"/>
    <file src="..\Debug\ImageEnhancer\ImageEnhancer.dll" target="runtimes\win10-x86\native"/>
    <file src="..\Debug\ImageEnhancer\ImageEnhancer.pri" target="runtimes\win10-x86\native"/>

    <!-- .targets -->
    <file src="ImageEnhancer.targets" target="build\native"/>

    </files>
</package>
```


## Package the component

With the completed `.nuspec` referencing all the files you need to include in the package, you're ready to run the `pack` command:

```
nuget pack ImageEnhancer.nuspec
```

This will generate `ImageEnhancer.YOUR_NAME.1.0.0.nupkg`. Opening this file in a tool like the [NuGet Package Explorer](https://github.com/NuGetPackageExplorer/NuGetPackageExplorer) and expanding all the nodes, you'll see the following contents:

![NuGet Package Explorer showing the ImageEnhancer package](media/UWP-PackageExplorer.png)

> [!Tip]
> A `.nupkg` file is just a ZIP file with a different extension. You can also examine package contents, then, by changing `.nupkg` to `.zip`, but remember to restore the extension before uploading a package to nuget.org.

To make your package available to other developers,  follow the instructions on [Publish a package](../create-packages/publish-a-package.md).

## Related topics

- [.nuspec Reference](../schema/nuspec.md)
- [Symbol packages](../create-packages/symbol-packages.md)
- [Dependency Versions](../create-packages/dependency-versions.md)
- [Supporting Multiple .NET Framework Versions](../create-packages/supporting-multiple-target-frameworks.md)
- [Include MSBuild props and targets in a package](../create-packages/creating-a-package.md#including-msbuild-props-and-targets-in-a-package)
- [Creating Localized Packages](../create-packages/creating-localized-packages.md)