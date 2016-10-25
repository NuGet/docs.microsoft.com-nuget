# Packages in Visual Studio templates

Visual Studio project and item templates often need to ensure that certain packages are installed into when the project or item is created. For example, the ASP.NET MVC 3 template installs jQuery, Modernizr, and other packages. 

To support this, template authors can instruct NuGet to install the necessary packages, rather than individual libraries. Developers can then easily update those packages at any later time. 

To learn more about authoring templates themselves, refer to [Creating Project and Item Templates in Visual Studio](https://msdn.microsoft.com/library/s365byhx.aspx) pr [Creating Custom Project and Item Templates with the Visual Studio SDK](https://msdn.microsoft.com/library/ff527340.aspx).

The remainder of this section describes the specific steps to take when authoring a template to properly include NuGet packages.

- [Adding packages to a template](#adding-packages-to-a-template)
- [Best practices](#best-practices)

For an example, see the [NuGetInVsTemplates sample](https://bitbucket.org/marcind/nugetinvstemplates).


## Adding packages to a template

When a template is instantiated, a [template wizard](https://msdn.microsoft.com/library/ms185301.aspx) is invoked to load the list of packages to install along with information about where to find those packages. Packages can be embedded in the VSIX, embedded in the template, or located on the local hard drive in which case you use a registry key to reference the file path. Details on these locations are given later in this section.

Preinstalled packages work using [template wizards](http://msdn.microsoft.com/en-us/library/ms185301.aspx). A special wizard gets invoked when the template gets instantiated. The wizard loads the list of packages that need to be installed and passes that information to the appropriate NuGet APIs.

Steps to include packages in a template:

1. In your `vstemplate` file, add a reference to the NuGet template wizard by
adding a [`WizardExtension`](http://msdn.microsoft.com/library/ms171411.aspx) element:
		
	<code class="html">
		&lt;WizardExtension&gt;
	        &lt;Assembly&gt;NuGet.VisualStudio.Interop, Version=1.0.0.0, Culture=neutral, PublicKeyToken=b03f5f7f11d50a3a&lt;/Assembly&gt;
	        &lt;FullClassName&gt;NuGet.VisualStudio.TemplateWizard&lt;/FullClassName&gt;
	    &lt;/WizardExtension&gt;
	</code>

	`NuGet.VisualStudio.Interop.dll` is an assembly that contains only the `TemplateWizard` class, which is a simple wrapper that calls into the actual implementation in `NuGet.VisualStudio.dll`. The assembly version will never change so that project/item templates continue to work with new versions of NuGet.

2. Add the list of packages to install in the project:
  
    <code class="html">
		&lt;WizardData&gt;
	        &lt;packages&gt;
	            &lt;package id="jQuery" version="1.6.2" /&gt;
	        &lt;/packages&gt;
	    &lt;/WizardData&gt;
	</code>

	*(NuGet 2.2.1+)* The wizard supports multiple &lt;package&gt; elements to support multiple package sources. Both the `id` and `version` attributes are required, meaning that specific version of a package will be installed even if a newer version is available. This prevents package updates from breaking the template, leaving the choice to update the package to the developer using the template.
    
	
3. Specify the repository where NuGet can find the packages as described in the following sections.

### VSIX package repository

The recommended deployment approach for Visual Studio project/item templates is a [VSIX extension](http://msdn.microsoft.com/library/ff363239.aspx) because it allows you to package multiple project/item templates together and allows developers to easily discover your templates using the VS Extension Manager or the Visual Studio Gallery. Updates to the extension are also easy to deploy using the [Visual Studio Extension Manager automatic update mechanism](http://msdn.microsoft.com/library/dd997169.aspx).

The VSIX itself can serve as the source for packages required by the template:
 
1. Modify the `<packages>` element in the `.vstemplate` file as follows:

    <code class="html">
		&lt;packages repository="extension"
              repositoryId="MyTemplateContainerExtensionId"&gt;

    	&lt;!-- ... --&gt;
    	&lt;/packages&gt;
	</code>

	The `repository` attribute specifies the type of repository as `extension` while `repositoryId` is the unique identifier of the VSIX itself (This is the value of the [`ID` attribute](http://msdn.microsoft.com/library/dd393688.aspx) in the extension’s `vsixmanifest` file).

2. Place your `nupkg` files in a folder called `Packages` within the VSIX. 
3. Add the necessary package files as [custom extension content](http://msdn.microsoft.com/library/dd393737.aspx) in your `source.extension.vsixmanifest` file. If you're using the 2.0 schema it should look like this:

    <code class="html">
		&lt;Asset Type="Moq.4.0.10827.nupkg" d:Source="File" 
           Path="Packages\Moq.4.0.10827.nupkg" d:VsixSubPath="Packages" /&gt;
    </code>

    If you're using the 1.0 schema it should look like this:

    <code class="html">&lt;CustomExtension Type="Moq.4.0.10827.nupkg"&gt;
      packages/Moq.4.0.10827.nupkg&lt;/CustomExtension&gt;
    </code></pre>

	 
4. Note that you can deliver packages in the same VSIX as your project templates or you can put them in a separate VSIX if that makes more sense for your scenario. However, do not reference any VSIX over which you do not have control, because changes to that extension could break your template.


### Template package repository

If you are distributing only a single project/item template and do not need to package multiple templates together, you can use a simpler but more limited approach that includes packages directly in the project/item template ZIP file:

1. Modify the `<packages>` element in the `.vstemplate` file as follows:

	<code class="html">
		&lt;packages repository="template""&gt;
			&lt;!-- ... --&gt;
		&lt;/packages&gt;
	</code>
	
	The `repository` attribute has the value `template` and the `repositoryId` attribute is not required.

2. Place packages in the root folder of the project/item template ZIP file.

Note that using this approach in a VSIX that contains multiple templates leads to unnecessary bloat when one or more packages are common to the templates. In such cases, use the [VSIX as the repository](#vsix-package-repository) as described in the previous section.


### Registry-specified folder path

SDKs that are installed using an MSI can install NuGet packages directly on the developer's machine. This makes them immediately available when a project or item template is used, rather than having to extract them during that time. ASP.NET templates use this approach.
 
1. Have the MSI install packages to the machine. You can install only the `.nupkg` files, or you can install those along with the expanded contents, which saves an additional step when the template is used. In this case, follow NuGet's standard folder structure whereing the `.nupkg` files are in the root folder, and then each package has a subfolder with the id/version pair as the subfolder name.

2. Write a registry key to identify the package location:

	- Key location: `HKEY_LOCAL_MACHINE\SOFTWARE[\Wow6432Node]\NuGet\Repository`
	- Key name: use a name that's unique to you. For example, the ASP.NET MVC 4 templates for VS 2012 use `AspNetMvc4VS11`.
	- Values: the full path to the packages folder.

3. In the `<packages>` element in the `.vstemplate` file, add the attribute `repository="registry"` and specify your registry key name in the `keyName` attribute.
 
	- If you have pre-unzipped your packages, use the `isPreunzipped="true"` attribute.
	- *(NuGet 3.2+)* If you want to force a design-time build at the end of package installation, add the `forceDesignTimeBuild="true"` attribute.
	- As an optimization, add `skipAssemblyReferences="true"` because the template itself already includes the necessary references.
	
	<code class="html">
		&lt;packages repository="registry" keyName="AspNetMvc4VS11" isPreunzipped="true"&gt;
	    	&lt;package id="EntityFramework" version="5.0.0" skipAssemblyReferences="true" /&gt;
	    	&lt;-- ... --&gt;
		&lt;/packages&gt;
	</code>
    

## Best Practices

1. Declare a dependency on the NuGet VSIX by adding a reference to it in your VSIX manifest:
    <code class="html">
		&lt;Reference Id="NuPackToolsVsix.Microsoft.67e54e40-0ae3-42c5-a949-fddf5739e7a5" MinVersion="1.7.30402.9028"&gt;
	    	&lt;Name&gt;NuGet Package Manager&lt;/Name&gt;
	    	&lt;MoreInfoUrl&gt;http://docs.nuget.org/&lt;/MoreInfoUrl&gt;
	    &lt;/Reference&gt;
	    &lt-- ... --&gt;
    </code>

2. Require project/item templates to be saved on creation by setting [`<PromptForSaveOnCreation>`](http://msdn.microsoft.com/library/twfxayz5.aspx) in the `.vstemplate` file.

3. Templates do not include a `packages.config` or `project.json` file, and do not include or any references or content that would be added when NuGet packages are installed.

