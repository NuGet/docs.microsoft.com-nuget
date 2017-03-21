---
# required metadata

title: How to create a NuGet package | Microsoft Docs
author: kraigb
ms.author: kraigb
manager: ghogen
ms.date: 2/8/2017
ms.topic: article
ms.prod: nuget
#ms.service:
ms.technology: nuget
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
# Creating NuGet Packages

> [!Note]
> This topic is intended to be a reference for the process of creating a package. For a focused walkthrough example, refer to the [Create and Publish a Package Quickstart](../quickstart/create-and-publish-a-package.md).
>
> Additionally, this topic applies to all project types other than .NET Core projects using Visual Studio 2017 and NuGet 4.0+. In these cases, NuGet uses information from a .csproj file directly. These details are explained in [Create .NET Standard Packages with Visual Studio 2017](../guides/create-net-standard-packages-vs2017.md) and [NuGet pack and restore as MSBuild targets](../schema/msbuild-targets.md).

No matter what your package does or what code it contains, NuGet is how you package that functionality into a component that can be shared with and used by any number of other developers.

The process of creating a package always begins with creating a `.nuspec` package manifest file that describes your package contents. This manifest drives the creation of the package as well as its usage when installed into a project.

This topic covers the most common steps involved in package creation:

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

After these core steps, you can incorporate a variety of other features as described in the other topics in this documentation. See [Next steps](#next-steps) below for a list of those options.

## Deciding which assemblies to package

In general, it's a best practice to have one NuGet package per assembly, provided that each assembly is independently useful. For example, if you have a Utilities.dll that depends on Parser.dll, and Parser.dll is useful on its own, then create one package for each.

However, if your package contains assemblies that are used exclusively by your package, then it's fine to include them. For example, if Utilities.dll depends on Utilities.resources.dll, where the latter is not useful on its own, then you can put both in the same package.

> [!Note]
> When a package is installed into a project, NuGet automatically adds assembly references to the package's DLLs, *excluding* those that are named `.resources.dll` because they are assumed to be localized satellite assemblies (see [Creating localized packages](../create-packages/creating-localized-packages.md)). For this reason, avoid using ".resources.dll" for files that otherwise contain essential package code.

## The role and structure of the .nuspec file

A .nuspec is an XML manifest file that describes a package's contents and drives the process of creating a NuGet package. At a minimum, the manifest includes the package identifier, version number, the title that appears in a gallery, author and owner information, and a long description. It can also include release notes, copyright information, a short description for the Package Manager UI in Visual Studio, a locale ID, home page and license URLs, an icon URL, lists of dependencies and references, tags that assist in gallery searches, and more. See the [Nuspec reference](../schema/nuspec.md) for complete details.

Here's typical (but fictitious) `.nuspec` file, with annotation comments:

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
            users to earily find other packages by the same owners.  -->
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

    <!-- A readme.txt will be displayed when the package is installed -->
    <files>
    <file src="readme.txt" target="" />
    </files>
</package>
```

For details on declaring dependencies and specifying version numbers, see [Dependencies](../create-packages/dependency-versions.md).

Because the manifest is always included in a package, you can find any number of additional examples by examining existing packages. A good source is the global package cache on your machine, the location of which is returned by the following command:

```bash
nuget locals -list global-packages
```

Go into any *package\version* folder, copy the .nupkg file to a .zip file, then open that .zip file and examine the .nuspec within it.

Note that when creating a `.nuspec` from a Visual Studio project, the manifest will contain tokens that will be replaced with information from the project when the package is build. See [Creating the .nuspec from a Visual Studio project](#from-a-visual-studio-project) below.


## Creating the .nuspec file

You can create a `.nuspec` file from scratch in any text editor, or by editing a file from another project. You can also have NuGet create a template manifest for your by using the following command:

```bash    
nuget spec MyPackage
```

The resulting `MyPackage.nuspec` file (or `Package.nuspec` if you omit a specific name) will contain placeholders for values like the `projectUrl`, so be sure to edit it before using it to creating the package.

You can also use `nuget spec` with an existing assembly, a Visual Studio project, or a convention-based working directory as described in the following sections. Note that in all cases, the resulting `.nuspec` file will contain placeholders that you'll need to edit before creating the package itself.

### From an assembly DLL

If you have an assembly DLL, you can easily generate a `.nuspec` file from the metadata in the assembly using the following command:

```bash
nuget spec MyAssembly.dll
```

### From a Visual Studio project

Creating a `.nuspec` from a `.csproj` or `.vbproj` file is convenient because other packages that have been installed into those project will be automatically referenced as dependencies. Simply use the following command in the same folder as the project file:

```bash
nuget spec
```

This creates a template `<project_name>.nuspec` file as usual (where &lt;project_name&gt; is replaced with your project name), but includes tokens that will be replaced at packaging time with values from the project. This means you do not need to update crucial values like the version number in the `.nuspec` as you update the project (but you can always replace the tokens with literal values, if desired).

For example, the &lt;id&gt; value will typically appear as follows:

```xml
<id>$id$</id>
```

and will be replaced with the `AssemblyName` value from the project file. For the exact mapping of project values to `.nuspec` tokens, see the [Replacement Tokens reference](../schema/nuspec.md#replacement-tokens).

Note that there are several additional packaging options available when working from a Visual Studio project, as described in the [Creating the package](#creating-the-package) section later on.

> **Solution-level packages (NuGet 2.x only)**
>
> NuGet 2.x supports the notion of a solution-level package that installs tools or additional commands for the Package Manager Console, but does not add references, content, or build customizations to any projects in the solution.
>
> A package is considered a solution-level package if it does not contain any files in its lib, content, or build directories. If the package has dependencies, they also must not have files in their lib, content, or build directories.
>
> When a solution-level package is installed, it is tracked in a packages.config file in the .nuget directory, rather than in a packages.config file in a specific project.


### From a convention-based working directory

In addition to assemblies and simple files like a readme, some packages may contain the following:

- Content and source code that should be injected into the target project
- PowerShell scripts (packages used in NuGet 2.x can include installation scripts as well, which is no longer supported in NuGet 3.x and later.)
- Transformations to existing configuration and source code files in a project

To include all these files in a package, you lay out a folder structure using the following conventions:

|Folder |Description|Action upon package install|
|-------|-----------|---------------------------|
| tools |Powershell scripts and programs accessible from the Package Manager Console|Contents are copied to the project folder, and the tools folder is added to the PATH environment variable.|
| lib | Assembly(.dll) files (.dll), documentation (.xml) files, and symbol (.pdb) files | Assemblies are added as references; .xml and .pdb copied into project folders. |
| content | Arbitrary files | Contents are copied to the project root |
| build | MSBuild .targets and .props files | Automatically inserted into the project file (NuGet 2.x) or project.json.lock (NuGet 3.x).|


Think of the **content** folder as the root of the target application, so if you want the package to add an image in the application's */images* folder, place it in the *content/images* folder of the package.

Next, from the root folder of this layout, run the following command to create the `.nuspec` file:

```bash
nuget spec
```

In this case, the `.nuspec` will not contain any explicit references to the folder structure, but all those files will be automatically included when creating the package later on.

## Choosing a unique package identifier and setting the version number

The package identifier in **&lt;id&gt;** and the version number in **&lt;version&gt;** are the two most important values in the manifest because they uniquely identify the exact code that's contained in the package.

For the **&lt;id&gt;** value, the following best practices apply:

- **Uniqueness**: The identifier must be unique within nuget.org or whatever gallery will be hosting the package. Before deciding on an identifier, spend a little time searching in the applicable gallery to see if that name is already used. A good pattern to follow is to use your company name as the first part of the identifier, such as `Contoso.`.
- **Namespace-like names**: Identifiers should follow a pattern similar to namespaces in .NET, using dot notation instead of hyphens. For example, use `Contoso.Utility.UsefulStuff` rather than `Contoso-Utility-UsefulStuff` or `Contoso_Utility_UsefulStuff`. It's especially helpful to the consumers of your package to match the identifier to the namespaces of the code in the package, if applicable.
- **Sample Packages**: If you produce a package of sample code that demonstrates how to use another package, attach `.Sample` as a suffix to the identifier, as in `Contoso.Utility.UsefulStuff.Sample`. (The sample package would of course have a dependency on the other package.) When creating a sample package, use the convention-based working directory method described earlier. In the `content` folder, arrange the sample code in a folder called `\Samples\<identifier>` as in `\Samples\Contoso.Utility.UsefulStuff.Sample`.

For the **&lt;version&gt;** value:

- In general, set the version of the package to match the library, though this is not strictly required. This is a simple matter when you limit a package to a single assembly, as described earlier in [Deciding which assemblies to package](#deciding-which-assemblies-to-package). Overall, remember that NuGet itself deals with package versions when resolving dependencies, not assembly versions.
- When using a non-standard version scheme, be sure to consider the NuGet versioning rules as explained in [Handling Dependencies](../create-packages/dependency-versions.md).

> The following series of brief blog posts are also helpful to understand versioning:
>
> - [Part 1: Taking on DLL Hell](http://blog.davidebbo.com/2011/01/nuget-versioning-part-1-taking-on-dll.html)
> - [Part 2: The core algorithm](http://blog.davidebbo.com/2011/01/nuget-versioning-part-2-core-algorithm.html)
> - [Part 3: Unification via Binding Redirects](http://blog.davidebbo.com/2011/01/nuget-versioning-part-3-unification-via.html)


## Setting a package type

Beginning with NuGet 3.5, packages can be marked with a specific *package type* to indicate the package's intended use. Packages that are not marked with a type, including all packages created with earlier versions of NuGet, are assumed to be the `Dependency` type.

- `Dependency` type packages add build- or run-time assets to libraries and applications, and can be installed in any project type (assuming they are compatible).

- `DotnetCliTool` type packages are extensions to the [.NET CLI](https://docs.microsoft.com/en-us/dotnet/articles/core/tools/index) and are invoked from the command line. Such packages can be installed only in .NET Core projects and have no effect on restore operations. More details about these per-project extensions are available in the  [.NET Core extensibility](https://docs.microsoft.com/en-us/dotnet/articles/core/tools/extensibility#per-project-based-extensibility) documentation.

    When a DotnetCliTool package is installed, Visual Studio will place the package in the project.json `tools` node instead of the `dependencies` node.

- Custom type packages use an arbitrary type identifier that conforms to the same format rules as package IDs. Any type other than Dependency and `DotnetCliTool`, however, are not recognized by the NuGet Package Manager in Visual Studio.

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

To directly specify files to include in the package, use the **&lt;files&gt;** node in the `.nuspec` file, which *follows* the &lt;metadata&gt; tag:

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

When you include a file named `readme.txt` in the package, the contents of that file will be displayed in Visual Studio as plain text immediately after the package is installed directly (but not when when the package is installed as a dependency). For example, here's how the readme for the HtmlAgilityPack package appears:

![The display of a readme file for a NuGet package upon installation](media/Create_01-ShowReadme.png)

> [!Note]
> If you include an empty &lt;files&gt; node in the .nuspec file, NuGet will not include any other content in the package other than what's in the lib folder.

## Including MSBuild props and targets in a package

In some cases you might want to add custom build targets or properties in projects that consume your package, such as running a custom tool or process during build. You do this by placing files in the form `<package_id>.targets` or `<package_id>.props` (such as `Contoso.Utility.UsefulStuff.targets`) within the `\build` folder of the project.

Files in the root `\build` folder are considered suitable for all target frameworks. To provide framework-specific files, first place those within appropriate subfolders, such as the following:

    \build
        \netstandard1.4
            \Contoso.Utility.UsefulStuff.props
            \Contoso.Utility.UsefulStuff.targets
        \net462
            \Contoso.Utility.UsefulStuff.props
            \Contoso.Utility.UsefulStuff.targets

Then in the `.nuspec` file, be sure to refer to these files in the &lt;files&gt; node:

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

When NuGet 2.x installs a package with `\build` files, it will add an MSBuild &lt;Import&gt; elements in the project file pointing to the `.targets` and `.props` files. (`.props` is added at the top of the project file; `.targets` is added at the bottom.)

With NuGet 3.x, targets are not added to the project but are instead made available through the `project.lock.json`.


## Creating the package

When using an assembly or the convention-based working directory, create a package by running `nuget pack` with your `.nuspec` file:

```bash
nuget pack <your_project>.nuspec
```

When using a Visual Studio project, run `nuget pack` instead with your project file, which will automatically load the project's `.nuspec` file and replace any tokens within it using values in the project file:

```bash
nuget pack <your_project>.csproj
```

> [!Note]
> Using the project file directly is necessary for token replacement because the project is the source of the token values. Token replacement does not happen if you use `nuget pack` with a `.nuspec` file.


In all cases, `nuget pack` excludes folders that start with a period, such as `.git` or `.hg`.

NuGet will indicate if there are any errors in the `.nuspec` file that need correcting, such as forgetting to change values in the manifest from their defaults.

Once `nuget pack` succeeds, you'll have a `.nupkg` file that you can publish to a suitable gallery as described in [Publishing a Package](../create-packages/publish-a-package.md).

> **Package Explorer**
> A helpful way to examine a package after creating it is to open it in the [Package Explorer](https://github.com/NuGetPackageExplorer/NuGetPackageExplorer) tool. This gives you a graphical view of the package contents and its manifest. You can also rename the resulting .nupkg file to a .zip file and explore its contents directly.


### Additional options

You can use various command-line switches with `nuget pack` to exclude files, override the version number in the manifest, and change the output directory, among other features. For a complete list, refer to the [pack command reference](../tools/nuget-exe-cli-reference.md#pack).

The following options are a few that are common with Visual Studio projects:

- **Referenced projects**: If the project references other projects, you can add the referenced projects as part of the package, or as dependencies, by using the `-IncludeReferencedProjects` option:

    ```bash
    nuget pack MyProject.csproj -IncludeReferencedProjects
    ```

    This inclusion process is recursive, so if MyProject.csproj references projects B and C, and those projects reference D, E, and F, then files from B, C, D, E, and F will be included in the package.

    If a referenced project includes a `.nuspec` file of its own, then NuGet adds that referenced project as a dependency instead.  You will need to package and publish that project separately.

- **Build configuration**: By default, NuGet will use the default build configuration set in the project file, typically *Debug*. To pack files from a different build configuration, such as *Release*, use the `-properties` option with the configuration:

    ```bash
    nuget pack MyProject.csproj -properties Configuration=Release
    ```

- **Symbols**: to include symbols that allow consumers to step through your package code in the debugger, use the `-Symbols` option:

    ```bash
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
