# Packages in Visual Studio Templates

NuGet supports adding NuGet packages to Visual Studio project templates and item templates. When Visual Studio creates a project using a template that contains these NuGet packages, the resulting project will have these packages installed.

This is useful when you want your project/item template to reference a library(e.g. JSON.NET or EntityFramework). For example, project templates for ASP.NET MVC 3 include jQuery, Modernizr and other nuget packages.

This provides a better experience for end-users who can easily update the nuget packages installed by the project/item template long after the template has shipped. At the same time it makes it easier to author templates since you only need to reference one or more nuget packages instead of individual libraries.

## Authoring Visual Studio templates

This article does not describe how to author a Visual Studio template. Follow these links to find more information on how to create templates [directly using Visual Studio](http://msdn.microsoft.com/en-us/library/s365byhx.aspx)
or [using the Visual Studio SDK](http://msdn.microsoft.com/en-us/library/ff527340.aspx).

<div class="block-callout-info">
    <strong>Note:</strong><br>
    You should author your template such that it does _not_ include the `packages.config` file or any references or content that installing the NuGet package(s) would add to the project.
</div>

## Adding packages to a template

Preinstalled packages work using [template wizards](http://msdn.microsoft.com/en-us/library/ms185301.aspx). A special wizard gets invoked when the template gets instantiated. The wizard loads the list of packages that need to be installed and passes that information to the appropriate NuGet APIs.

The template needs to specify where to find the package nupkg files. Currently, three package repositories are supported:

<div>
1. Packages embedded inside of a VSIX package <br/>
2. Packages embedded inside of the project/item template itself<br/>
3. Packages installed on hard drive, with a registry key referencing the path<br/>
<div>

To add preinstalled packages to your project/item template you need to:

**Step 1**

Edit your vstemplate file and add a reference to the NuGet template wizard by
adding a [`WizardExtension`](http://msdn.microsoft.com/en-us/library/ms171411.aspx) element:
    <pre><code class="html">&lt;WizardExtension&gt;
        &lt;Assembly&gt;NuGet.VisualStudio.Interop, Version=1.0.0.0, Culture=neutral, PublicKeyToken=b03f5f7f11d50a3a&lt;/Assembly&gt;
        &lt;FullClassName&gt;NuGet.VisualStudio.TemplateWizard&lt;/FullClassName&gt;
    &lt;/WizardExtension&gt;</code></pre>

`NuGet.VisualStudio.Interop.dll` is a new assembly that only contains the `TemplateWizard` class. This class is a simple wrapper that calls into the actual implementation that lives in `NuGet.VisualStudio.dll`. The assembly version will never change so that project/item templates continue to work with new versions of NuGet.

**Step 2**

Add the list of packages to install in the project: 
    <pre><code class="html">&lt;WizardData&gt;
        &lt;packages&gt;
            &lt;package id="jQuery" version="1.6.2" /&gt;
        &lt;/packages&gt;
    &lt;/WizardData&gt;</code></pre>

The wizard supports multiple `<package>` elements. Both the `id` and `version` attributes are required. An important consequence of this is that a _specific_ version of a package will be installed even if a newer version
is available in the online package feed.
    
The reason for this behavior is that a future version of a package might introduce a change that is not compatible with the project/item template. The choice to upgrade the package to the latest version using NuGet is left to the developer who is in the best position to assume the risks of upgrading the package to the latest version.

<div class="block-callout-info">
    <strong>Support of multiple packages elements:</strong><br>
    Starting with NuGet 2.2.1, the wizard also supports multiple `<packages>` elements. This enables scenarios where some packages are installed from one repository, but other packages are installed from a different repository.
</div>

The remaining step is to specify the repository where NuGet can find the package files. As mentioned earlier, three package repository modes are supported:

### VSIX package repository

The recommended approach for deploying Visual Studio project/item templates is through
a VSIX package ([read more about VSIX deployment here](http://msdn.microsoft.com/en-us/library/ff363239.aspx)). The VSIX method is preferable because it allows you to package multiple project/item templates together and allows developers to easily discover your templates using the VS Extension Manager or the Visual Studio Gallery. On top of that you can easily push updates to your users using the [Visual Studio Extension Manager automatic update mechanism](http://msdn.microsoft.com/en-us/library/dd997169.aspx).

**Step 1**

To specify a VSIX as a package repository you modify the `<packages>` element in the `.vstemplate` file:
    <pre><code class="html">&lt;packages repository="extension"
              repositoryId="MyTemplateContainerExtensionId"&gt;
    ...
    &lt;/packages&gt;</code></pre>

The `repository` attribute specifies the type of repository (“extension”) while `repositoryId` is the unique identifier of your VSIX (i.e. the value of the [`ID` attribute](http://msdn.microsoft.com/en-us/library/dd393688.aspx) in the extension’s vsixmanifest file).

**Step 2**

Add your nupkg files as [custom extension content](http://msdn.microsoft.com/en-us/library/dd393737.aspx) in your `source.extension.vsixmanifest` file. If you're using the 2.0 schema it should look like this:
    <pre><code class="html">&lt;Asset Type="Moq.4.0.10827.nupkg" d:Source="File" 
           Path="Packages\Moq.4.0.10827.nupkg" d:VsixSubPath="Packages" /&gt;
    </code></pre>
    Or if you're using the 1.0 schema it should look like this:
    <pre><code class="html">&lt;CustomExtension Type="Moq.4.0.10827.nupkg"&gt;
              packages/Moq.4.0.10827.nupkg&lt;/CustomExtension&gt;
    </code></pre>

Ensure that your `nupkg` files are located under a folder called `Packages` within the VSIX package. 

You can place the nupkg files in the same VSIX as your project templates or you can have the packages be located in a separate VSIX if that makes more sense for your scenario ().

<div class="block-callout-info">
    <strong>Note:</strong><br>
    You should not reference VSIXs you do not have control over since they could change in the future and your project/item templates would break
</div>

### Template package repository

If packaging multiple projects is not important to you (e.g. you are only distributing a single project/item template), a simpler but also more limited approach is to include the nupkg files in the project/item template zip file itself.

However, if you are bundling a set of project/item templates that relate to each other and share NuGet packages (e.g. you are shipping a custom MVC project template with versions for Razor, Web Forms, C#, and VB.NET), we do not recommend adding the NuGet packages directly to each project/item template zip file. It needlessly increases the size of the project/item template bundle.

To specify the project/item template as a package repository you modify the `<package>` element:
    <pre><code class="html">&lt;packages repository="template"&gt;
        ...
    &lt;/packages&gt;</code></pre>

The `repository` attribute now has the value "template" and the `repositoryId` attribute is not longer required. The nupkg files need to be placed into the root directory of the project/item template zip file.

### Registry-specified folder path

Many SDKs are installed via MSI.  These MSIs have the ability to install NuGet packages on disk for efficient
package installation during project creation, avoiding the need to extract the packages during project creation.  ASP.NET uses this approach for its preinstalled packages in project templates.

This approach requires a few moving parts:

**Step 1**

Put the packages on disk during MSI installation

* You can install only the nupkg files or the nupkg files along with their expanded contents.

* If the expanded contents are also installed, this saves one additional step during project creation.

* The file/folder format matches the standard packages folder used by NuGet where the nupkg files are all at the root and then each package has a subfolder with the id/version pair as the subfolder name.

**Step 2**

Write a registry key that can be used to reference the package installation folder

* Use HKEY_LOCAL_MACHINE\SOFTWARE[\Wow6432Node]\NuGet\Repository for the key location.

* The key name must be something unique to you.  ASP.NET MVC 4 in VS 2012 uses "AspNetMvc4VS11" for instance.

* The value must be the full path to your packages folder.  ASP.NET MVC 4 uses "C:\Program Files (x86)\Microsoft ASP.NET\ASP.NET MVC 4\Packages\" for instance.

**Step 3**

Use the repository value of "registry" within the `<packages>` node

* Specifying your registry key name in the `keyName` attribute.

* If you have pre-unzipped your packages, use the `isPreunzipped="true"` attribute.

* If you want to force a design time build at the end of packages installation, use `forceDesignTimeBuild="true"` attribute. This attribute is supported on NuGet versions 3.2 and above.

Here's an example `<packages>` element using the registry-specified folder repository:
<pre><code class="html">&lt;packages repository="registry" keyName="AspNetMvc4VS11" isPreunzipped="true"&gt;
    &lt;package id="EntityFramework" version="5.0.0" skipAssemblyReferences="true" /&gt;
    ...
&lt;/packages&gt;</code></pre>
    
Note that the above example also uses the `skipAssemblyReferences="true"` attribute, which is another performance
optimization.  The VS template itself already includes this assembly reference, so we can tell NuGet to skip
adding assembly references from the package.

## Best Practices

Make your VSIX declare a dependency on the NuGet VSIX by adding a reference to it in your VSIX manifest:
    <pre><code class="html">&lt;Reference Id="NuPackToolsVsix.Microsoft.67e54e40-0ae3-42c5-a949-fddf5739e7a5" MinVersion="1.7.30402.9028"&gt;
     &lt;Name&gt;NuGet Package Manager&lt;/Name&gt;
     &lt;MoreInfoUrl&gt;http://docs.nuget.org/&lt;/MoreInfoUrl&gt;
    &lt;/Reference&gt;
    ....
    </code></pre>

Require project/item templates to be saved on creation by setting [`<PromptForSaveOnCreation>`](http://msdn.microsoft.com/en-us/library/twfxayz5.aspx) in the `.vstemplate` file.

## Packages in VS Templates sample project

A sample project is available for you to get started. The source code is available [here](https://bitbucket.org/marcind/nugetinvstemplates).
