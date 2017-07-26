---
# required metadata

title: How to create a NuGet package | Microsoft Docs
author: kraigb
ms.author: kraigb
manager: ghogen
ms.date: 7/24/2017
ms.topic: article
ms.prod: nuget
#ms.service:
ms.technology: null
ms.assetid: 456797cb-e3e4-4b88-9b01-8b5153cee802

# optional metadata

description: A detailed guide to the process of designing and creating a NuGet package, including key decision points like files and versioning.
keywords: NuGet package creation, creating a package, nuspec manifest, NuGet package conventions, NuGet package version
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
# Creating NuGet packages

No matter what your package does or what code it contains, NuGet is how you package that functionality into a component that can be shared with and used by any number of other developers.

The packaging process begins with the compiled code (assemblies), symbols, and/or other files that you want to deliver as a package (see [Overview and workflow](Overview-and-Workflow.md). That is, creating a package is an independent process from compiling or otherwise generating the files that go into the package. That said, the packaging process can can use information in a project file, helping you keep compiled assemblies and packages containing those assemblies in sync.

This topic describes the detailed process of creating a package and serves as a reference for different aspects of that process. For a focused walkthrough, refer to [Create and Publish a Package Quickstart](../quickstart/create-and-publish-a-package.md).

In this topic:

- [Deciding which assemblies to package](#deciding-which-assemblies-to-package)
- [The role and structure of the `.nuspec` file](#the-role-and-structure-of-the-nuspec-file)
- [Creating the `.nuspec` file](#creating-the-nuspec-file) from:
    - [An assembly DLL](#from-an-assembly-dll)
    - [A Visual Studio project](#from-a-visual-studio-project)
    - [A convention-based working directory](#from-a-convention-based-working-directory)
- [Choosing a unique package identifier and setting the version number](#choosing-a-unique-package-identifier-and-setting-the-version-number)
- [Setting a package type](#setting-a-package-type) (NuGet 3.5 and later)
- [Adding a readme and other files](#adding-a-readme-and-other-files)
- [Including MSBuild props and targets in a package](#including-msbuild-props-and-targets-in-a-package)
- [Creating the package](#creating-the-package)

After these core steps, you can incorporate a variety of other features as described elsewhere in this documentation. See [Next steps](#next-steps) below.

> [!Note]
> This topic applies to project types other than .NET Core projects using Visual Studio 2017 and NuGet 4.0+. In those .NET Core projects, NuGet uses information in the `.csproj` file directly. For details, see [Create .NET Standard Packages with Visual Studio 2017](../guides/create-net-standard-packages-vs2017.md) and [NuGet pack and restore as MSBuild targets](../schema/msbuild-targets.md).

## Deciding which assemblies to package

Most general-purpose packages contain one or more assemblies that other developers can use in their own projects.

In general, it's best to have one assembly per NuGet package, provided that each assembly is independently useful. For example, if you have a `Utilities.dll` that depends on `Parser.dll`, and `Parser.dll` is useful on its own, then create one package for each. Doing so allows developers to use `Parser.dll` independently of `Utilities.dll`.

However, if your library is composed of multiple assemblies that aren't independently useful, then it's fine to combine them into one package. Using the previous example, if `Parser.dll` contains code that's used only by `Utilities.dll`, then it's fine to keep `Parser.dll` in the same package.

Similarly, if `Utilities.dll` depends on `Utilities.resources.dll`, where again the latter is not useful on its own, then put both in the same package.

Resources are, in fact, a special case. When a package is installed into a project, NuGet automatically adds assembly references to the package's DLLs, *excluding* those that are named `.resources.dll` because they are assumed to be localized satellite assemblies (see [Creating localized packages](creating-localized-packages.md)). For this reason, avoid using `.resources.dll` for files that otherwise contain essential package code.

## The role and structure of the .nuspec file

Once you know what files you want to package, the next step is creating a `.nuspec` package manifest. The manifest is an XML file that describes the package's contents and is itself included in the package. The manifest both drives the creation of the package and instructs NuGet on how to install the package into a project. The manifest, for example, identifies other package dependencies such that NuGet can also install those dependencies when the main package is installed.

The manifest contains both required an optional properties as described below. For exact details, including other properties not mentioned here, see  the [.nuspec reference](../schema/nuspec.md).

Required properties:
- The package identifier, which must be unique across the gallery that hosts the package.
- A specific version number in the form *Major.Minor.Patch.Build[-Suffix]* where *-Suffix* identifies [pre-release versions](Prerelease-Packages.md)- - The package title as it should appears on the host (like nuget.org)
- Author and owner information.
- A long description of the package.

Common optional properties:
- Release note
- Copyright information
- A short description for the [Package Manager UI in Visual Studio](../Tools/Package-Manager-UI.md)
- A locale ID
- Home page and license URLs
- An icon URL
- Lists of dependencies and references
- Tags that assist in gallery searches

The following is a typical (but fictitious) `.nuspec` file, with comments describing the properties:

```xml
<?xml version="1.0"?>
<package xmlns="http://schemas.microsoft.com/packaging/2013/05/nuspec.xsd">
    <metadata>
    <!-- The identifier that must be unique within the hosting gallery -->
    <id>Contoso.Utility.UsefulStuff</id>

    <!-- The package version number that is used when resolving dependencies -->
    <version>1.8.3.331</version>

    <!-- Authors contain text that appears directly on the gallery -->
    <authors>Dejana Tesic, Rajeev Dey</authors>

    <!-- Owners are typically nuget.org identities that allow gallery
            users to easily find other packages by the same owners.  -->
    <owners>dejanatc, rjdey</owners>

    <!-- License and project URLs provide links for the gallery -->
    <licenseUrl>http://opensource.org/licenses/MS-PL</licenseUrl>
    <projectUrl>http://github.com/contoso/UsefulStuff</projectUrl>

    <!-- The icon is used in Visual Studio's package manager UI -->
    <iconUrl>http://github.com/contoso/UsefulStuff/nuget_icon.png</iconUrl>

    <!-- If true, this value prompts the user to accept the license when
            installing the package. -->
    <requireLicenseAcceptance>false</requireLicenseAcceptance>

    <!-- Any details about this particular release -->
    <releaseNotes>Bug fixes and performance improvements</releaseNotes>

    <!-- The description can be used in package manager UI. Note that the
            nuget.org gallery uses information you add in the portal. -->
    <description>Core utility functions for web applications</description>

    <!-- Copyright information -->
    <copyright>Copyright ©2016 Contoso Corporation</copyright>

    <!-- Tags appear in the gallery and can be used for tag searches -->
    <tags>web utility http json url parsing</tags>

    <!-- Dependencies are automatically installed when the package is installed -->
    <dependencies>
        <dependency id="Newtonsoft.Json" version="9.0" />
    </dependencies>
    </metadata>

    <!-- A readme.txt is be displayed when the package is installed -->
    <files>
    <file src="readme.txt" target="" />
    </files>
</package>
```

For details on declaring dependencies and specifying version numbers, see [Dependency versions](../create-packages/dependency-versions.md).

Because the manifest is included in the package created from it, you can find any number of additional examples by examining existing packages. A good source is the global package cache on your machine, the location of which is returned by the following command:

```
nuget locals -list global-packages
```

Go into any *package\version* folder, copy the `.nupkg` file to a `.zip` file, then open that `.zip` file and examine the `.nuspec` within it.

> [!Note]
> When creating a `.nuspec` from a Visual Studio project, the manifest contains tokens that are be replaced with information from the project when the package is built. See [Creating the .nuspec from a Visual Studio project](#from-a-visual-studio-project).

## Creating the .nuspec file

Creating a complete manifest typically begins with a basic `.nuspec` file, which you then edit by hand into its final form so that it describes the package contains the content you want.

The best way to create an initial `.nuspec` file is by using the [nuget spec](../Tools/nuget-exe-CLI-Reference.md#spec) command to create a template manifest. This ensures that you start with the proper file structure. You can also start with a manifest from another package and modify it to suit your needs.

With `nuget spec`, you can create a new file with default values, or you can create a file using information in an existing assembly DLL, a project file, or a convention-based working directory as explained in the following sections.

> [!Important]
> Generated `.nuspec` files contain placeholders that must be modified before creating the package with the `nuget pack` command. That command fails if the `.nuspec` contains any placeholders.

### New file with default values

The following command creates a default manifest with placeholders:

```    
nuget spec [<package-name>]
```

If you omit `<package-name>`, the resulting file is `Package.nuspec`. If you provide a name such as `Contoso.Utility.UsefulStuff`, the file is `Contoso.Utility.UsefulStuff.nuspec`.

### From an assembly DLL

If you have an assembly DLL, generate a `.nuspec` file from the metadata in the assembly using the following command:

```
nuget spec <assembly-name>.dll
```

Using this form of the command replaces a few placeholders in the manifest with specific values from the assembly. For example, the `<id>` property is set to the assembly name, and `<version>` is set to the assembly version. Other properties in the manifest, however, don't have matching values in the assembly and thus still contain placeholders.

### From a Visual Studio project

Using `nuget spec` in a folder containing a project file creates a `.nuspec` using that project information:

```
# Use in a folder containing a project file <project-name>.csproj or <project-name>.vbproj
nuget spec
```

The resulting `<project-name>.nuspec` file contains *tokens* that are replaced at packaging time with values from the project, including references to any other packages that have already been installed.

A token is delimited by `$` symbols on both sides of the project property. For example, the `<id>` value in a manifest generated in this way typically appears as follows:

```xml
<id>$id$</id>
```

This token is replaced with the `AssemblyName` value from the project file at packing time. For the exact mapping of project values to `.nuspec` tokens, see the [Replacement Tokens reference](../schema/nuspec.md#replacement-tokens).

Tokens relieve you from needing to update crucial values like the version number in the `.nuspec` as you update the project. (You can always replace the tokens with literal values, if desired). 

Note that there are several additional packaging options available when working from a Visual Studio project, as described in the [Creating the package](#creating-the-package) section later on.

#### Solution-level packages

*NuGet 2.x only. This capability is not present in NuGet 3.0+.*

NuGet 2.x supported the notion of a solution-level package that installs tools or additional commands for the Package Manager Console, but does not add references, content, or build customizations to any projects in the solution. Such packages contain no files in its direct `lib`, `content`, or `build` folders, and none of its dependencies have files in their respective `lib`, `content`, or `build` folders.

NuGet tracks installed solution-level packages in a `packages.config` file in the `.nuget` folder, rather than the project's `packages.config` file.

### From a convention-based working directory

Technically speaking, a NuGet package is just a ZIP file that's been renamed with the `.nupkg` extension and whose contents match certain conventions. The package's assembly DLLs, for instance, are always placed in a `lib` folder.

The practical upshot of this is that you can simply create a folder structure that follows the expected conventions and contains all the files you want to include in the package *excluding any folders that begin with `.`*. You then generate a manifest directly from that folder tree. When you create the package using that manifest, NuGet automatically adds all the files in the folder structure.

An advantage to this approach is that you don't need to edit the manifest to specifically identify which files you want to include in the package (as explained later in this topic). That is, when creating a manifest and package from the contents of a project tree, NuGet includes only the files that you specify in the manifest. Otherwise you'd risk including temporary build files, project files, `.gitignore` files, and so forth. 

When using a convention-based working directory, on the other hand, NuGet assumes that everything in the folder structure is exactly what you want in the final package (again excluding folders starting with `.`). You don't need to specifically reference any files in the manifest. 

Such a folder structure also allows you to easily include files that might not be part of a project at all, including:

- Content and source code that should be injected into the target project.
- PowerShell scripts (packages used in NuGet 2.x can include installation scripts as well, which is not supported in NuGet 3.x and later).
- Transformations to existing configuration and source code files in a project.

The folder conventions are as follows:

| Folder | Description | Action upon package install |
| --- | --- | --- |
| (root) | Location for readme.txt | Visual Studio displays a readme.txt file in the package root when the package is installed. |
| lib | Assembly files (`.dll`), documentation (`.xml`) files, and symbol (`.pdb`) files | Assemblies are added as references; `.xml` and `.pdb` copied into project folders. |
| content | Arbitrary files | Contents are copied to the project root |
| build | MSBuild `.targets` and `.props` files | Automatically inserted into the project file (NuGet 2.x) or `project.lock.json` (NuGet 3.x+). |
| tools | Powershell scripts and programs accessible from the Package Manager Console | Contents are copied to the project folder, and the `tools` folder is added to the PATH environment variable.|

Think of the **content** folder as the root of the target application. To have the package add an image in the application's */images* folder, place it in the package's *content/images* folder.

Next, from the root folder of this layout, run the following command to create the `.nuspec` file:

```
nuget spec
```

In this case, the `.nuspec` contains no explicit references to files in the folder structure. Again, NuGet automatically includes all files when the package is created.  

## Choosing a unique package identifier and setting the version number

The package identifier (`<id>` element) and the version number (`<version>` element) are the two most important values in the manifest because they uniquely identify the exact code that's contained in the package.

**Best practices for the package identifier:**

- **Uniqueness**: The identifier must be unique across nuget.org or whatever gallery hosts the package. Before deciding on an identifier, search the applicable gallery to check if the name is already in use. To avoid conflicts, a good pattern is to use your company name as the first part of the identifier, such as `Contoso.`.
- **Namespace-like names**: Follow a pattern similar to namespaces in .NET, using dot notation instead of hyphens. For example, use `Contoso.Utility.UsefulStuff` rather than `Contoso-Utility-UsefulStuff` or `Contoso_Utility_UsefulStuff`. Consumers also find it helpful when the package identifier matches the namespaces used in the code.
- **Sample Packages**: If you produce a package of sample code that demonstrates how to use another package, attach `.Sample` as a suffix to the identifier, as in `Contoso.Utility.UsefulStuff.Sample`. (The sample package would of course have a dependency on the other package.) When creating a sample package, use the convention-based working directory method described earlier. In the `content` folder, arrange the sample code in a folder called `\Samples\<identifier>` as in `\Samples\Contoso.Utility.UsefulStuff.Sample`.

**Best practices for the package version:**

- In general, set the version of the package to match the library, though this is not strictly required. This is a simple matter when you limit a package to a single assembly, as described earlier in [Deciding which assemblies to package](#deciding-which-assemblies-to-package). Overall, remember that NuGet itself deals with package versions when resolving dependencies, not assembly versions.
- When using a non-standard version scheme, be sure to consider the NuGet versioning rules as explained in [Handling Dependencies](../create-packages/dependency-versions.md).

> The following series of brief blog posts are also helpful to understand versioning:
>
> - [Part 1: Taking on DLL Hell](http://blog.davidebbo.com/2011/01/nuget-versioning-part-1-taking-on-dll.html)
> - [Part 2: The core algorithm](http://blog.davidebbo.com/2011/01/nuget-versioning-part-2-core-algorithm.html)
> - [Part 3: Unification via Binding Redirects](http://blog.davidebbo.com/2011/01/nuget-versioning-part-3-unification-via.html)


## Setting a package type

With NuGet 3.5+, packages can be marked with a specific *package type* to indicate its intended use. Packages not marked with a type, including all packages created with earlier versions of NuGet, default to the `Dependency` type.

- `Dependency` type packages add build- or run-time assets to libraries and applications, and can be installed in any project type (assuming they are compatible).

- `DotnetCliTool` type packages are extensions to the [.NET CLI](https://docs.microsoft.com/dotnet/articles/core/tools/index) and are invoked from the command line. Such packages can be installed only in .NET Core projects and have no effect on restore operations. More details about these per-project extensions are available in the  [.NET Core extensibility](https://docs.microsoft.com/dotnet/articles/core/tools/extensibility#per-project-based-extensibility) documentation.

    When a DotnetCliTool package is installed, Visual Studio places the package in the `project.json` `tools` node instead of the `dependencies` node.

- Custom type packages use an arbitrary type identifier that conforms to the same format rules as package IDs. Any type other than `Dependency` and `DotnetCliTool`, however, are not recognized by the NuGet Package Manager in Visual Studio.

Package types are set either in the `.nuspec` file or in `project.json`. In both cases, it's best for backwards compatibility to *not* explicitly set the `Dependency` type and to instead rely on NuGet assuming this type when no type is specified.

- `.nuspec`: Indicate the package type within a `packageTypes\packageType` node under the `<metadata>` element:

    ```xml
    <?xml version="1.0" encoding="utf-8"?>
    <package xmlns="http://schemas.microsoft.com/packaging/2012/06/nuspec.xsd">
        <metadata>
        <!-- ... -->
        <packageTypes>
            <packageType name="DotnetCliTool" />
        </packageTypes>
        </metadata>
    </package>
    ```

- `project.json`: Indicate the package type within a `packOptions.packageType` property json:

    ```json
    {
        // ...
        "packOptions": {
        "packageType": "DotnetCliTool"
        }
    }
    ```

## Adding a readme and other files

To directly specify files to include in the package, use the `<files>` node in the `.nuspec` file, which *follows* the `<metadata>` tag:

```xml
<?xml version="1.0"?>
<package xmlns="http://schemas.microsoft.com/packaging/2013/05/nuspec.xsd">
    <metadata>
    <!-- ... -->
    </metadata>
    <files>
    <!-- Add a readme -->
    <file src="readme.txt" target="" />

    <!-- Add files from an arbitrary folder that's not necessarily in the project -->
    <file src="..\..\SomeRoot\**\*.*" target="" />
    </files>
</package>
```

> [!Tip]
> When using the convention-based working directory approach, you can place the readme.txt in the package root and other content in the `content` folder. No `<file>` elements are necessary in the manifest.

When you include a file named `readme.txt` in the package root, Visual Studio displays the contents of that file as plain text immediately after installing the package directly. (Readme files are not displayed for packages installed as dependencies). For example, here's how the readme for the HtmlAgilityPack package appears:

![The display of a readme file for a NuGet package upon installation](media/Create_01-ShowReadme.png)

> [!Note]
> If you include an empty `<files>` node in the `.nuspec` file, NuGet doesn't include any other content in the package other than what's in the `lib` folder.

## Including MSBuild props and targets in a package

In some cases, you might want to add custom build targets or properties in projects that consume your package, such as running a custom tool or process during build. You do this by placing files in the form `<package_id>.targets` or `<package_id>.props` (such as `Contoso.Utility.UsefulStuff.targets`) within the `\build` folder of the project.

Files in the root `\build` folder are considered suitable for all target frameworks. To provide framework-specific files, first place those within appropriate subfolders, such as the following:

    \build
        \netstandard1.4
            \Contoso.Utility.UsefulStuff.props
            \Contoso.Utility.UsefulStuff.targets
        \net462
            \Contoso.Utility.UsefulStuff.props
            \Contoso.Utility.UsefulStuff.targets

Then in the `.nuspec` file, be sure to refer to these files in the `<files>` node:

```xml
<?xml version="1.0"?>
<package >
    <metadata>
    <!-- ... -->
    </metadata>
    <files>
    <!-- Include everything in \build -->
    <file src="build\**" target="build" />

    <!-- Other files -->
    <!-- ... -->
    </files>
</package>
```

When NuGet 2.x installs a package with `\build` files, it adds an MSBuild `<Import>` elements in the project file pointing to the `.targets` and `.props` files. (`.props` is added at the top of the project file; `.targets` is added at the bottom.)

With NuGet 3.x, targets are not added to the project but are instead made available through the `project.lock.json`.


## Creating the package

When using an assembly or the convention-based working directory, create a package by running `nuget pack` with your `.nuspec` file, replacing `<manifest-name>` with your specific filename:

```
nuget pack <project-name>.nuspec
```

When using a Visual Studio project, run `nuget pack` with your project file, which automatically loads the project's `.nuspec` file and replaces any tokens within it using values in the project file:

```
nuget pack <project-name>.csproj
```

> [!Note]
> Using the project file directly is necessary for token replacement because the project is the source of the token values. Token replacement does not happen if you use `nuget pack` with a `.nuspec` file.


In all cases, `nuget pack` excludes folders that start with a period, such as `.git` or `.hg`.

NuGet indicates if there are any errors in the `.nuspec` file that need correcting, such as forgetting to change placeholder values in the manifest.

Once `nuget pack` succeeds, you have a `.nupkg` file that you can publish to a suitable gallery as described in [Publishing a Package](../create-packages/publish-a-package.md).

> [!Tip]
> A helpful way to examine a package after creating it is to open it in the [Package Explorer](https://github.com/NuGetPackageExplorer/NuGetPackageExplorer) tool. This gives you a graphical view of the package contents and its manifest. You can also rename the resulting `.nupkg` file to a `.zip` file and explore its contents directly.


### Additional options

You can use various command-line switches with `nuget pack` to exclude files, override the version number in the manifest, and change the output folder, among other features. For a complete list, refer to the [pack command reference](../tools/nuget-exe-cli-reference.md#pack).

The following options are a few that are common with Visual Studio projects:

- **Referenced projects**: If the project references other projects, you can add the referenced projects as part of the package, or as dependencies, by using the `-IncludeReferencedProjects` option:

    ```
    nuget pack MyProject.csproj -IncludeReferencedProjects
    ```

    This inclusion process is recursive, so if `MyProject.csproj` references projects B and C, and those projects reference D, E, and F, then files from B, C, D, E, and F are be included in the package.

    If a referenced project includes a `.nuspec` file of its own, then NuGet adds that referenced project as a dependency instead.  You need to package and publish that project separately.

- **Build configuration**: By default, NuGet uses the default build configuration set in the project file, typically *Debug*. To pack files from a different build configuration, such as *Release*, use the `-properties` option with the configuration:

    ```
    nuget pack MyProject.csproj -properties Configuration=Release
    ```

- **Symbols**: to include symbols that allow consumers to step through your package code in the debugger, use the `-Symbols` option:

    ```
    nuget pack MyProject.csproj -symbols
    ```
    
## Next Steps

Once you've created a package, which is a `.nupkg` file, you can publish it to the gallery of your choice as described on [Publishing a Package](../create-packages/publish-a-package.md).

You might also want to extend the capabilities of your package or otherwise support other scenarios as described in the following topics:

- [Handling dependencies](../create-packages/dependency-versions.md)
- [Supporting multiple target frameworks](../create-packages/supporting-multiple-target-frameworks.md)
- [Transformations of source and configuration files](../create-packages/source-and-config-file-transformations.md)
- [Localization](../create-packages/creating-localized-packages.md)
- [Pre-release versions](../create-packages/prerelease-packages.md)

Finally, there are additional package types to be aware of:

- [Native Packages](../create-packages/native-packages.md)
- [Symbol Packages](../create-packages/symbol-packages.md)
