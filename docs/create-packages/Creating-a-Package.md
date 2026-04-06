---
title: Create a NuGet Package Using nuget.exe CLI
description: Find out how to use the nuget.exe CLI to create a NuGet package. Get information about package files, identifiers, and versioning.
author: JonDouglas
ms.author: jodou
ms.date: 04/06/2026
ms.topic: how-to
# customer intent: As a developer, I want to become familiar with NuGet package files and nuget.exe packaging commands so that I can use the nuget.exe CLI to create NuGet packages.
---

# Create a package using the nuget.exe CLI

No matter what your package does or what code it contains, you use one of the command-line interface (CLI) tools, either `nuget.exe` or `dotnet.exe`, to package that functionality into a component that can be shared with and used by other developers. To install NuGet CLI tools, see [Install NuGet client tools](../install-nuget-client-tools.md). Visual Studio doesn't automatically include a CLI tool.

- For non-SDK-style projects, typically .NET Framework projects, follow the steps described in this article to create a package. For a quickstart that provides step-by-step instructions for using Visual Studio and the `nuget.exe` CLI, see [Quickstart: Create and publish a package using Visual Studio (.NET Framework, Windows)](../quickstart/create-and-publish-a-package-using-visual-studio-net-framework.md).

- For .NET Core and .NET Standard projects that use the [SDK-style format](../resources/check-project-format.md), and any other SDK-style projects, see [Create a NuGet package with the dotnet CLI](creating-a-package-dotnet-cli.md).

- For projects migrated from `packages.config` to [`PackageReference`](../consume-packages/package-references-in-project-files.md), use the [`msbuild -t:pack`](../consume-packages/migrate-packages-config-to-package-reference.md#create-a-package-after-migration) command.

Technically, a NuGet package is a ZIP file that's been renamed to have the extension *.nupkg* and whose contents match certain conventions. This article describes the detailed process of creating a package that meets those conventions.

Packaging begins with the compiled code (assemblies), symbols, and other files that you want to deliver as a package. For an overview of the packaging process, see [Package creation workflow](overview-and-workflow.md). This process is independent from compiling or otherwise generating the files that go into the package. But you can draw from information in a project file to keep the compiled assemblies and packages in sync.

> [!Important]
> This article applies to non-SDK-style projects, typically projects other than .NET Core and .NET Standard projects that use Visual Studio 2017 and later versions and NuGet 4.0+.

## Decide which assemblies to package

Most general-purpose packages contain one or more assemblies that other developers can use in their own projects.

In general, it's best to have one assembly per NuGet package, provided that each assembly is independently useful. For example, consider the following cases that involve an assembly called *Utilities.dll* that depends on an assembly called *Parser.dll*:

- If *Parser.dll* is useful on its own, create one package for *Utilities.dll* and one for *Parser.dll*. Doing so allows developers to use *Parser.dll* independently of *Utilities.dll*.

- If *Parser.dll* contains code that's used only by *Utilities.dll*, it's fine to keep *Parser.dll* in the same package. In general, if your library is composed of multiple assemblies that aren't independently useful, it's fine to combine them into one package.

- If *Utilities.dll* also depends on *Utilities.resources.dll*, and *Utilities.resources.dll* isn't useful on its own, put both in the same package.

Resources, such as the *Utilities.resources.dll* assembly in the preceding example, are a special case. When a package is installed into a project, NuGet automatically adds assembly references to the package's DLLs, *excluding* those that are named *.resources.dll*, because they are assumed to be localized satellite assemblies. For more information about localized versions of a library, see [Creating localized NuGet packages](creating-localized-packages.md). For this reason, avoid using *.resources.dll* for files that contain essential package code.

If your library contains Component Object Model (COM) interop assemblies, follow the additional guidelines in [Create NuGet packages that contain COM interop assemblies](author-packages-with-com-interop-assemblies.md).

## The role and structure of the .nuspec file

When you know what files you want to package, the next step is to create a package manifest in a *.nuspec* XML file.

The manifest:

- Describes the package's contents and is itself included in the package.
- Drives the creation of the package and tells NuGet how to install the package into a project. For example, the manifest identifies other package dependencies so that NuGet can also install those dependencies when the main package is installed.
- Contains both required and optional properties as described in the rest of this section. For detailed information, including other properties not mentioned here, see [.nuspec reference](../reference/nuspec.md).

The following properties are required in the manifest:

- The package identifier, which must be unique across the gallery that hosts the package
- A specific version number in the form *Major.Minor.Patch[-Suffix]*, where *-Suffix* identifies [pre-release versions](prerelease-packages.md)
- The package title as it should appear on the host (like nuget.org)
- Author information
- A long description of the package

The following properties are common optional ones:

- Release notes.
- Copyright information.
- A short description for the [Package Manager UI in Visual Studio](../consume-packages/install-use-packages-visual-studio.md).
- A locale ID.
- A project URL.
- A license as an expression or file. The `licenseUrl` property is deprecated. Use the [`license` nuspec metadata element](../reference/nuspec.md#license) instead.
- An icon file. The `iconUrl` property is deprecated. Use the [`icon` nuspec metadata element](../reference/nuspec.md#icon) instead.
- Lists of dependencies and references.
- Tags that assist in gallery searches.

The following code is a typical (but fictitious) *.nuspec* file, with comments that describe the properties:

```xml
<?xml version="1.0"?>
<package xmlns="http://schemas.microsoft.com/packaging/2013/05/nuspec.xsd">
    <metadata>
        <!-- An identifier that must be unique within the hosting gallery -->
        <id>Contoso.Utility.UsefulStuff</id>

        <!-- A package version number that's used when resolving dependencies -->
        <version>1.8.3</version>

        <!-- A comma-separated list of package authors that sometimes appears directly on the gallery -->
        <authors>Dejana Tesic, Rajeev Dey</authors>

        <!-- A project URL that provides a link for the gallery -->
        <projectUrl>http://github.com/contoso/UsefulStuff</projectUrl>

         <!-- License information that's displayed on the gallery -->
        <license type="expression">Apache-2.0</license>
        

        <!-- An icon that's used in the Visual Studio Package Manager UI -->
        <icon>icon.png</icon>

        <!-- 
            A property that when true, prompts the user to accept the license when
            installing the package
        -->
        <requireLicenseAcceptance>false</requireLicenseAcceptance>

        <!-- Detailed information about a particular release -->
        <releaseNotes>Bug fixes and performance improvements</releaseNotes>

        <!-- 
            A description that can be used in the Package Manager UI. The
            nuget.org gallery uses information you add in the portal. 
        -->
        <description>Core utility functions for web applications</description>

        <!-- Copyright information -->
        <copyright>Copyright ©2026 Contoso Corporation</copyright>

        <!-- Tags that appear in the gallery and can be used for tag searches -->
        <tags>web utility http json url parsing</tags>

        <!-- Dependencies that are automatically installed when the package is installed -->
        <dependencies>
            <dependency id="Newtonsoft.Json" version="9.0" />
        </dependencies>
    </metadata>

    <!-- A read-me Markdown file that's displayed in the Package Manager UI -->
    <files>
        <file src="readme.md" target="" />
        <file src="icon.png" target="" />
    </files>
</package>
```

For detailed information about declaring dependencies and specifying version numbers, see [packages.config](../reference/packages-config.md) and [Package versioning](../concepts/package-versioning.md). You can also use the `include` and `exclude` attributes on the `dependency` element to specify the dependency assets that you want to include or exclude in the package. For more information, see [.nuspec Reference - Dependencies element](../reference/nuspec.md#dependencies-element).

Because the manifest is included in the package created from it, you can find examples by examining existing packages. A good source is the *global-packages* folder on your computer. To find its location, use the following command:

```cli
nuget locals -list global-packages
```

After you know the location of the *global-packages* folder, take the following steps to find a manifest file:

1. Go to the *global-packages* folder.
1. In that folder, go to the subfolder for any package, and then go to a subfolder for any version of that package.
1. In the version subfolder, make a copy of the *.nupkg* file, and change the extension of the copy to *zip*.
1. Open the *.zip* file and examine the *.nuspec* file within it.

> [!Note]
> When you create a *.nuspec* file from a Visual Studio project, the manifest contains tokens that are replaced with information from the project when the package is built. For more information, see [Create the .nuspec file from a Visual Studio project](#from-a-visual-studio-project).

## Create the .nuspec file

Creating a complete manifest typically begins with a basic *.nuspec* file generated through one of the following methods:

- [A convention-based working directory](#from-a-convention-based-working-directory)
- [An assembly DLL](#from-an-assembly-dll)
- [A Visual Studio project](#from-a-visual-studio-project)
- [A new file with default values](#from-a-new-file-with-default-values)

You then edit the file by hand so that it describes the exact content you want in the final package.

> [!Important]
> Generated *.nuspec* files contain placeholders that you must modify before you create the package by using the `nuget pack` command. That command fails if the *.nuspec* file contains any placeholders.

### From a convention-based working directory

Because a NuGet package is a ZIP file that's been renamed with the *.nupkg* extension, it's often easiest to create the folder structure you want on your local file system and then create the *.nuspec* file directly from that structure. The `nuget pack` command then automatically adds all files in that folder structure, but it excludes any folders that begin with a period, so you can keep private files in the same structure.

The advantage to this approach is that you don't need to specify in the manifest which files you want to include in the package, as explained later in this section. Instead, you can have your build process produce the exact folder structure that goes into the package. You can also easily include other files that might not be part of a project otherwise:

- Content and source code that should be injected into the target project
- PowerShell scripts
- Transformations to existing configuration and source code files in a project

The folders align with the following conventions:

| Folder | Contents | Action upon package install |
| --- | --- | --- |
| *(root)* | The package manifest, top-level folders, and optionally, a read-me Markdown file and an icon image | This folder is used as the starting point for standardized subfolders, such as *lib* and *build*. |
| *lib/\<tfm\>* | Assembly (*.dll*), documentation (*.xml*), and symbol (*.pdb*) files for the given target framework moniker (TFM) | Assemblies are added as references for compile time and runtime. The *.xml* and *.pdb* files are copied into project folders. For information about creating framework target-specific subfolders, see [Support multiple .NET versions](supporting-multiple-target-frameworks.md). |
| *ref/\<tfm\>* | Assembly (*.dll*), and symbol (*.pdb*) files for the given TFM | Assemblies are added as references only for compile time. Nothing is copied into the project *bin* folder. |
| *runtimes* | Architecture-specific assembly (*.dll*), symbol (*.pdb*), and native resource (*.pri*) files | Assemblies are added as references only for runtime. Other files are copied into project folders. There should always be a corresponding (TFM) `AnyCPU` specific assembly under the */ref/\<tfm\>* folder to provide the corresponding compile-time assembly. See [Support multiple .NET versions](supporting-multiple-target-frameworks.md). |
| *content* | Arbitrary files | The contents are copied to the project root. Think of the *content* folder as the root of the target application that ultimately consumes the package. To have the package add an image in the application's */images* folder, place it in the package's *content/images* folder. |
| *build* | *(3.x+)* Microsoft Build Engine (MSBuild) *.targets* and *.props* files | These files are automatically inserted into the project. |
| *buildMultiTargeting* | *(4.0+)* MSBuild *.targets* and *.props* files for cross-framework targeting | These files are automatically inserted into the project. |
| *buildTransitive* | *(5.0+)* MSBuild *.targets* and *.props* files that flow transitively to any consuming project | These files are automatically inserted into the project.  See the [feature](https://github.com/NuGet/Home/wiki/Allow-package--authors-to-define-build-assets-transitive-behavior) page. |
| *tools* | PowerShell scripts and programs accessible from the Package Manager Console | The *tools* folder is added to the `PATH` environment variable for the Package Manager Console only. It's *not* added to the `PATH` value that MSBuild uses when it builds the project. |

Because your folder structure can contain assemblies for many target frameworks, this method is necessary when you create packages that support multiple frameworks.

When you have the desired folder structure in place, run the following command in that folder to create the *.nuspec* file:

```cli
nuget spec
```

The generated *.nuspec* file contains no explicit references to files in the folder structure. NuGet automatically includes all files when the package is created. You still need to edit placeholder values in other parts of the manifest, however.

### From an assembly DLL

In the basic case of creating a package from an assembly, you can generate a *.nuspec* file from the metadata in the assembly by using the following command:

```cli
nuget spec <assembly-name>.dll
```

Using this form replaces a few placeholders in the manifest with specific values from the assembly. For example, the `<id>` property is set to the assembly name, and `<version>` is set to the assembly version. Other properties in the manifest, however, don't have matching values in the assembly. Those properties still contain placeholders after you run the command.

### From a Visual Studio project

Creating a *.nuspec* file from a *.csproj* or *.vbproj* file is convenient, because other packages that are installed in the project are automatically referenced as dependencies. To create a manifest from a project file, use the following command in the folder that contains the project file:

```cli
# Use in a folder that contains a project file, such as <project-name>.csproj or <project-name>.vbproj.
nuget spec
```

The resulting *\<project-name\>.nuspec* file contains *tokens* that are replaced at packaging time with values from the project, including references to any other packages that have already been installed.

If you have package dependencies to include in the *.nuspec*, instead use `nuget pack`. Then get the *.nuspec* file from within the generated *.nupkg* file. For example, use the following command:

```cli
# Use in a folder that contains a project file, such as <project-name>.csproj or <project-name>.vbproj.
nuget pack myproject.csproj
```

A token is delimited by `$` symbols on both sides of the project property. For example, the `<id>` value in a manifest generated in this way typically looks like the following line:

```xml
<id>$id$</id>
```

This token is replaced with the `AssemblyName` value from the project file at packing time. For the exact mapping of project values to *.nuspec* file tokens, see [Replacement tokens](../reference/nuspec.md#replacement-tokens).

Tokens relieve you from needing to update crucial values like the version number in the *.nuspec* file as you update the project. But you can also replace the tokens with literal values.

Several extra packaging options are available when you work from a Visual Studio project, as described in [Run nuget pack to generate the .nupkg file](#run-nuget-pack-to-generate-the-nupkg-file) later in this article.

#### Solution-level packages

*NuGet 2.x only. Not available in NuGet 3.0+.*

NuGet 2.x supports the notion of a solution-level package that installs tools or extra commands for the Package Manager Console (the contents of the *tools* folder), but doesn't add references, content, or build customizations to any projects in the solution. Such packages contain no files in their direct *lib*, *content*, or *build* folders, and none of their dependencies have files in their respective *lib*, *content*, or *build* folders.

NuGet tracks installed solution-level packages in a *packages.config* file in the *.nuget* folder, rather than the project's *packages.config* file.

### From a new file with default values

The following command creates a default manifest with placeholders, which helps ensure you start with the proper file structure:

```cli
nuget spec [<package-name>]
```

If you omit `<package-name>`, the resulting file is named *Package.nuspec*. If you provide a name such as `Contoso.Utility.UsefulStuff`, the file is named *Contoso.Utility.UsefulStuff.nuspec*.

The resulting *.nuspec* file contains placeholders for values like `projectUrl`. Before you use the file to create the final *.nupkg* file, replace the placeholders with appropriate values.

## Choose a unique package identifier and set the version number

The package identifier (`<id>` element) and the version number (`<version>` element) are the two most important values in the manifest, because they uniquely identify the exact code that's contained in the package.

### Best practices for the package identifier

- **Uniqueness**: The identifier must be unique in the gallery that hosts the package, such as nuget.org. Before you decide on an identifier, search the applicable gallery to check whether the name is already in use. To avoid conflicts, a good pattern is to use your company name as the first part of the identifier, such as `Contoso`.
- **Namespace-like names**: Follow a pattern similar to namespaces in .NET, using dot notation instead of hyphens. For example, use `Contoso.Utility.UsefulStuff` rather than `Contoso-Utility-UsefulStuff` or `Contoso_Utility_UsefulStuff`. Consumers also find it helpful when the package identifier matches the namespaces used in the code.
- **Sample packages**: If you produce a package of sample code that demonstrates how to use another package, attach `.Sample` as a suffix to the identifier, as in `Contoso.Utility.UsefulStuff.Sample`. A sample package of this type has a dependency on the package it demonstrates how to use. When you create a sample package, use the convention-based working directory method described earlier. In the *content* folder, arrange the sample code in a folder called *\Samples\\<identifier\>*, as in *\Samples\Contoso.Utility.UsefulStuff.Sample*.

### Best practices for the package version

- In general, set the version of the package to match the library. This guidance is recommended but not strictly required. This practice is straightforward when you limit a package to a single assembly, as described earlier in [Decide which assemblies to package](#decide-which-assemblies-to-package). In general, remember that NuGet itself deals with package versions when resolving dependencies, not assembly versions.
- When you use a non-standard version scheme, consider the NuGet versioning rules as explained in [Package versioning](../concepts/package-versioning.md).

For other resources that are helpful for understanding versioning, see the following series of brief blog posts:

- [Part 1: Taking on DLL Hell](https://blog.davidebbo.com/2011/01/nuget-versioning-part-1-taking-on-dll.html)
- [Part 2: The core algorithm](https://blog.davidebbo.com/2011/01/nuget-versioning-part-2-core-algorithm.html)
- [Part 3: Unification via binding redirects](https://blog.davidebbo.com/2011/01/nuget-versioning-part-3-unification-via.html)

## Add a read-me file and other files

To directly specify files to include in the package, use the `<files>` node in the *.nuspec* file, which *follows* the `<metadata>` tag:

```xml
<?xml version="1.0"?>
<package xmlns="http://schemas.microsoft.com/packaging/2013/05/nuspec.xsd">
    <metadata>
    <!-- ... -->
    </metadata>
    <files>
        <!-- Add a read-me file. -->
        <file src="readme.md" target="" />

        <!-- Add files from an arbitrary folder that's not necessarily in the project. -->
        <file src="..\..\SomeRoot\**\*.*" target="" />
    </files>
</package>
```

> [!Tip]
> When you use the convention-based working directory approach, you can place the *readme.md* file in the package root and other content in the *content* folder. No `<file>` elements are necessary in the manifest.

When you include a file named *readme.md* in the package root, Visual Studio displays the contents of that file in the Package Manager UI. For example, the following screenshot shows the read-me file for the `HtmlAgilityPack` package:

:::image type="content" source="media/package-read-me-file.png" alt-text="Screenshot of the Visual Studio Package Manager UI that shows a package details pane. The README tab describes HTML parsing abilities of the package." lightbox="media/package-read-me-file.png":::

> [!Note]
> If you include an empty `<files>` node in the *.nuspec* file, NuGet includes the contents of the *lib* folder in the package but no other content.

## Include MSBuild props and targets in a package

In some cases, you might want to add custom build targets or properties to projects that consume your package, such as running a custom tool or process during build.
For more information about custom build targets and properties, see [MSBuild .props and .targets in a package](..\concepts\MSBuild-props-and-targets.md).

Create *\<package-id\>.targets* or *\<package-id\>.props* files, such as *Contoso.Utility.UsefulStuff.targets*, in the *build* folders of the project.

Then in the *.nuspec* file, refer to these files in the `<files>` node:

```xml
<?xml version="1.0"?>
<package >
    <metadata minClientVersion="2.5">
    <!-- ... -->
    </metadata>
    <files>
        <!-- In the package build folder, include everything that's in the local build folder. -->
        <file src="build\**" target="build" />

        <!-- Other files -->
        <!-- ... -->
    </files>
</package>
```

When packages are added to a project, NuGet automatically includes these properties and targets.

## Run nuget pack to generate the .nupkg file

When you use an assembly or the convention-based working directory, create a package by running `nuget pack` with your *.nuspec* file. In the following command, replace `<project-name>` with the name of your project:

```cli
nuget pack <project-name>.nuspec
```

When you use a Visual Studio project, run `nuget pack` with your project file. This command automatically loads the project's *.nuspec* file and replaces any tokens in it with the corresponding values in the project file:

```cli
nuget pack <project-name>.csproj
```

> [!Note]
> For token replacement, you must use the project file directly, because the project is the source of the token values. Token replacement doesn't succeed if you use `nuget pack` with a *.nuspec* file.

In all cases, `nuget pack` excludes folders that start with a period, such as *.git* or *.hg*.

NuGet indicates whether there are any errors in the *.nuspec* file that need correcting, such as placeholder values in the manifest that need to be updated.

After `nuget pack` succeeds, you have a *.nupkg* file that you can publish to a suitable gallery as described in [Publish NuGet packages](../nuget-org/publish-a-package.md).

> [!Tip]
> A helpful way to examine a package after creating it is to open it in the [Package Explorer](https://github.com/NuGetPackageExplorer/NuGetPackageExplorer) tool. This tool gives you a graphical view of the package contents and its manifest. You can also rename the resulting *.nupkg* file to a *.zip* file and explore its contents directly.

### Additional options

You can use various command-line switches with `nuget pack` to exclude files, override the version number in the manifest, and change the output folder, among other features. For a complete list, see [pack command (NuGet CLI)](../reference/cli-reference/cli-ref-pack.md).

The following options are a few that are common with Visual Studio projects:

- **Referenced projects**: If the project references other projects, you can add the referenced projects as part of the package, or as dependencies, by using the `-IncludeReferencedProjects` option:

    ```cli
    nuget pack MyProject.csproj -IncludeReferencedProjects
    ```

    This inclusion process is recursive. For example, if *MyProject.csproj* references projects B and C, and those projects reference D, E, and F, files from B, C, D, E, and F are included in the package.

    If a referenced project includes a *.nuspec* file of its own, NuGet adds that referenced project as a dependency instead. You need to package and publish that project separately.

- **Build configuration**: By default, NuGet uses the default build configuration that's set in the project file, typically `Debug`. To pack files from a different build configuration, such as `Release`, use the `-properties` option with the configuration:

    ```cli
    nuget pack MyProject.csproj -properties Configuration=Release
    ```

- **Symbols**: To include symbols that make it possible for consumers to step through your package code in the debugger, use the `-Symbols` and `-SymbolPackageFormat` options. For the `-SymbolPackageFormat` option, specify a format of `snupkg`:

    ```cli
    nuget pack MyProject.csproj -symbols -SymbolPackageFormat snupkg
    ```

### Test package installation

Before you publish a package, you typically want to test the process of installing the package into a project. A test helps ensure that all necessary files end up in the correct place in the project.

You can test installations manually in Visual Studio or on the command line by using the standard [package installation steps](../consume-packages/overview-and-workflow.md#ways-to-install-a-nuget-package).

For automated testing, you can use the following basic process:

1. Copy the *.nupkg* file to a local folder.
1. Add the folder to your package sources by using the `nuget sources add -name <name> -source <path>` command. For more information, see [sources command (NuGet CLI)](../reference/cli-reference/cli-ref-sources.md). You need to set this local source only once on any given computer.
1. Install the package from that source by using `nuget install <package-ID> -source <name>`. In this command, `<name>` should match the name of the source you use in the `nuget sources` command. Specifying the source tells NuGet to install the package from that source alone.
1. Examine your file system to check that files are installed correctly.

## Related content

After you create a package, which is a *.nupkg* file, you can publish it to the gallery of your choice. For more information, see [Publish NuGet packages](../nuget-org/publish-a-package.md).

You can also extend the capabilities of your package or support other scenarios. For more information, see the following articles:

- [Package versioning](../concepts/package-versioning.md)
- [Support multiple .NET versions](supporting-multiple-target-frameworks.md)
- [Transforming source code and configuration files](source-and-config-file-transformations.md)
- [Creating localized NuGet packages](creating-localized-packages.md)
- [Building pre-release packages](prerelease-packages.md)
- [Set a NuGet package type](set-package-type.md)
- [Create NuGet packages that contain COM interop assemblies](author-packages-with-COM-interop-assemblies.md)

For other package types to be aware of, see the following articles:

- [Creating native packages](../guides/native-packages.md)
- [Creating symbol packages (.snupkg)](symbol-packages-snupkg.md)
