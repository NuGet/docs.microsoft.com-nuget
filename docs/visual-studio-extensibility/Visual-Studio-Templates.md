---
title: NuGet Packages in Visual Studio templates
description: Instructions for including NuGet packages as part of Visual Studio project and item templates.
author: JonDouglas
ms.author: jodou
ms.date: 01/03/2018
ms.topic: article
---

# Packages in Visual Studio templates

Visual Studio project and item templates often need to ensure that certain packages are installed when a project or item is created. For example, the ASP.NET MVC 3 template installs jQuery, Modernizr, and other packages.

To support this, template authors can instruct NuGet to install the necessary packages, rather than individual libraries. Developers can then easily update those packages at any later time.

To learn more about authoring templates themselves, refer to [How to: Create Project Templates](/visualstudio/ide/how-to-create-project-templates) or [Creating Custom Project and Item Templates](/visualstudio/extensibility/creating-custom-project-and-item-templates).

The remainder of this section describes the specific steps to take when authoring a template to properly include NuGet packages.

- [Adding packages to a template](#adding-packages-to-a-template)
- [Best practices](#best-practices)

## Samples

The [Preinstalled-Packages](https://github.com/NuGet/Samples/tree/main/Preinstalled-Packages) sample is available in the NuGet/Samples repository on GitHub.

## Adding packages to a template

When a template is instantiated, a [template wizard](/visualstudio/extensibility/how-to-use-wizards-with-project-templates) is invoked to load the list of packages to install along with information about where to find those packages. Packages can be embedded in the VSIX, embedded in the template, or located on the local hard drive in which case you use a registry key to reference the file path. Details on these locations are given later in this section.

Preinstalled packages work using [template wizards](/visualstudio/extensibility/how-to-use-wizards-with-project-templates). A special wizard gets invoked when the template gets instantiated. The wizard loads the list of packages that need to be installed and passes that information to the appropriate NuGet APIs.

Steps to include packages in a template:

1. In your `vstemplate` file, add a reference to the NuGet template wizard by adding a [`WizardExtension`](/visualstudio/extensibility/wizardextension-element-visual-studio-templates) element:

    ```xml
    <WizardExtension>
        <Assembly>NuGet.VisualStudio.Interop, Version=1.0.0.0, Culture=neutral, PublicKeyToken=b03f5f7f11d50a3a</Assembly>
        <FullClassName>NuGet.VisualStudio.TemplateWizard</FullClassName>
    </WizardExtension>
    ```

    `NuGet.VisualStudio.Interop.dll` is an assembly that contains only the `TemplateWizard` class, which is a simple wrapper that calls into the actual implementation in `NuGet.VisualStudio.dll`. The assembly version will never change so that project/item templates continue to work with new versions of NuGet.

1. Add the list of packages to install in the project:

    ```xml
    <WizardData>
        <packages>
            <package id="jQuery" version="1.6.2" />
        </packages>
    </WizardData>
    ```

    The wizard supports multiple `<package>` elements to support multiple package sources. Both the `id` and `version` attributes are required, meaning that specific version of a package will be installed even if a newer version is available. This prevents package updates from breaking the template, leaving the choice to update the package to the developer using the template.

1. Specify the repository where NuGet can find the packages as described in the following sections.

### VSIX package repository

The recommended deployment approach for Visual Studio project/item templates is a [VSIX extension](/visualstudio/extensibility/shipping-visual-studio-extensions) because it allows you to package multiple project/item templates together and allows developers to easily discover your templates using the VS Extension Manager or the Visual Studio Gallery. Updates to the extension are also easy to deploy using the [Visual Studio Extension Manager automatic update mechanism](/visualstudio/extensibility/how-to-update-a-visual-studio-extension).

The VSIX itself can serve as the source for packages required by the template:

1. Modify the `<packages>` element in the `.vstemplate` file as follows:

    ```xml
    <packages repository="extension" repositoryId="MyTemplateContainerExtensionId">
        <!-- ... -->
    </packages>
    ```

    The `repository` attribute specifies the type of repository as `extension` while `repositoryId` is the unique identifier of the VSIX itself (This is the value of the `ID` attribute in the extension’s `vsixmanifest` file, see [VSIX Extension Schema 2.0 Reference](/visualstudio/extensibility/vsix-extension-schema-2-0-reference)).

1. Place your `nupkg` files in a folder called `Packages` within the VSIX.

1. Add the necessary package files as `<Asset>` in your `vsixmanifest` file (see [VSIX Extension Schema 2.0 Reference](/visualstudio/extensibility/vsix-extension-schema-2-0-reference)):

    ```xml
    <Asset Type="Moq.4.0.10827.nupkg" d:Source="File" Path="Packages\Moq.4.0.10827.nupkg" d:VsixSubPath="Packages" />
    ```

1. Note that you can deliver packages in the same VSIX as your project templates or you can put them in a separate VSIX if that makes more sense for your scenario. However, do not reference any VSIX over which you do not have control, because changes to that extension could break your template.

### Template package repository

If you are distributing only a single project/item template and do not need to package multiple templates together, you can use a simpler but more limited approach that includes packages directly in the project/item template ZIP file:

1. Modify the `<packages>` element in the `.vstemplate` file as follows:

    ```xml
    <packages repository="template">
        <!-- ... -->
    </packages>
    ```

    The `repository` attribute has the value `template` and the `repositoryId` attribute is not required.

1. Place packages in the root folder of the project/item template ZIP file.

Note that using this approach in a VSIX that contains multiple templates leads to unnecessary bloat when one or more packages are common to the templates. In such cases, use the [VSIX as the repository](#vsix-package-repository) as described in the previous section.

### Registry-specified folder path

SDKs that are installed using an MSI can install NuGet packages directly on the developer's machine. This makes them immediately available when a project or item template is used, rather than having to extract them during that time. ASP.NET templates use this approach.

1. Have the MSI install packages to the machine. You can install only the `.nupkg` files, or you can install those along with the expanded contents, which saves an additional step when the template is used. In this case, follow NuGet's standard folder structure wherein the `.nupkg` files are in the root folder, and then each package has a subfolder with the id/version pair as the subfolder name.

1. Write a registry key to identify the package location:

    - Key location: Either the machine-wide `HKEY_LOCAL_MACHINE\SOFTWARE[\Wow6432Node]\NuGet\Repository` or if it's per-user installed templates and packages, alternatively use `HKEY_CURRENT_USER\SOFTWARE\NuGet\Repository`
    - Key name: use a name that's unique to you. For example, the ASP.NET MVC 4 templates for VS 2012 use `AspNetMvc4VS11`.
    - Values: the full path to the packages folder.

1. In the `<packages>` element in the `.vstemplate` file, add the attribute `repository="registry"` and specify your registry key name in the `keyName` attribute.

    - If you have pre-unzipped your packages, use the `isPreunzipped="true"` attribute.
    - *(NuGet 3.2+)* If you want to force a design-time build at the end of package installation, add the `forceDesignTimeBuild="true"` attribute.
    - As an optimization, add `skipAssemblyReferences="true"` because the template itself already includes the necessary references.

        ```xml
        <packages repository="registry" keyName="AspNetMvc4VS11" isPreunzipped="true">
            <package id="EntityFramework" version="5.0.0" skipAssemblyReferences="true" />
            <-- ... -->
        </packages>
        ```

## Best Practices

1. Declare a dependency on the NuGet VSIX by adding a reference to it in your VSIX manifest:

    ```xml
    <Reference Id="NuPackToolsVsix.Microsoft.67e54e40-0ae3-42c5-a949-fddf5739e7a5" MinVersion="1.7.30402.9028">
        <Name>NuGet Package Manager</Name>
        <MoreInfoUrl>http://learn.microsoft.com/nuget/</MoreInfoUrl>
    </Reference>
    <!-- ... -->
    ```

1. Require project/item templates to be saved on creation by including [`<PromptForSaveOnCreation>true</PromptForSaveOnCreation>`](/visualstudio/extensibility/promptforsaveoncreation-element-visual-studio-templates) in the `.vstemplate` file.

1. Templates do not include a `packages.config` file, and do not include or any references or content that would be added when NuGet packages are installed.
