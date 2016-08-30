# Nuspec Reference

A .nuspec file is a manifest that uses XML to describe a package. NuGet uses the manifest to build a package and then stores it in the package after the package is built. For information about how to build a package, see the [Creating a Package](/ndocs/create-packages/create-a-package) page.

At the minimum, you need the following elements in in the metadata of your .nuspec file.

<table class="reference">
    <tbody>
        <tr>
            <th>Element</th> <th>Description</th>
        </tr>
        <tr>
            <td><strong>id</strong></td>
            <td>
                <p>
                The unique identifier for the package. This is the package name that is shown 
                when packages are listed using the <b>Package Manager Console</b>. These are also used 
                when installing a package using the Install-Package command within the Package 
                Manager Console. Package IDs may not contain any spaces or characters that are 
                invalid in an URL. In  general, they follow the same rules as .NET namespaces do. 
                So <code>Foo.Bar</code> is a valid ID, <code>Foo!</code> and <code>Foo Bar</code>
                are not.
                </p>
                <p>
                Package IDs are treating as case-insensitive strings. <code>NuGet.Core</code> is
                considered equivalent to <code>nuget.core</code>. However, for display purposes,
                the original case in the .nuspec is used.
                </p>
            </td>
        </tr>
        <tr>
            <td><strong>version</strong></td>
            <td>The version of the package, in a format like <code>1.2.3</code>.</td>
        </tr>
        <tr>
            <td>title</td>
            <td>
                The human-friendly title of the package displayed in the NuGet.org Gallery and
                older versions of the <b>Manage NuGet Packages</b> dialog in Visual Studio. If no title
                is specified, the ID is used instead.
            </td>
        </tr>
        <tr>
            <td><strong>authors</strong></td>
            <td>A comma-separated list of authors of the package code.</td>
        </tr>
        <tr>
            <td><strong>description</strong></td>
            <td>
                A long description of the package. This shows up in the right pane of the 
                <b>Add Package</b> dialog as well as in the <b>Package Manager Console</b> when listing 
                packages using the <code>Get-Package</code> command.
            </td>
        </tr>
        <tr>
            <td>owners</td>
            <td>
                A comma-separated list of the package creators. This is often the 
                same list as in authors. This is ignored when uploading the package 
                to the NuGet.org Gallery.
            </td>
        </tr>
    </tbody>
</table>

However, for you package to work great and provide enough contextual information to you consumers, you should consider filling in the rest of the elements in the following list.

<table class="reference">
    <tbody>
        <tr>
            <th>Element</th> <th>Description</th>
        </tr>
        <tr>
            <td>releaseNotes</td>
            <td>
                <a href="/Release-Notes/NuGet-1.5">v1.5</a>
                A description of the changes made in each release of the package. This field 
                only shows up when the <b>Updates</b> tab is selected and the package is an update 
                to a previously installed package. It is displayed where the Description would 
                normally be displayed.
            </td>
        </tr>
        <tr>
            <td>summary</td>
            <td>
                A short description of the package. If specified, this shows up in the middle pane 
                of the <b>Add Package Dialog</b>. If not specified, a truncated version of the description 
                is used instead.
            </td>
        </tr>
        <tr>
            <td>language</td>
            <td>The locale ID for the package, such as <code>en-us</code>.</td>
        </tr>
        <tr>
            <td>projectUrl</td>
            <td>A URL for the home page of the package.</td>
        </tr>
        <tr>
            <td>iconUrl</td>
            <td>A URL for the image to use as the icon for the package in the <strong>Manage NuGet Packages</strong> dialog box. This should be a 64x64-pixel <em>.png</em> file that has a transparent background.</td>
        </tr>
        <tr>
            <td>licenseUrl</td>
            <td>A link to the license that the package is under.</td>
        </tr>
        <tr>
            <td>copyright</td>
            <td><a href="/Release-Notes/NuGet-1.5">v1.5</a> Copyright details for the package.</td>
        </tr>
        <tr>
            <td>requireLicenseAcceptance</td>
            <td>A boolean value that specifies whether the client needs to ensure that the package license (described by licenseUrl) is accepted before the package is installed.</td>
        </tr>
        <tr>
            <td>dependencies</td>
            <td>The list of dependencies for the package. For more information, see <a href="#specifying-dependencies">Specifying Dependencies</a> later in this document.</td>
        </tr>
        <tr>
            <td>references</td>
            <td><a href="/Release-Notes/NuGet-1.5">v1.5</a> Names of assemblies under lib that are added as project references. If unspecified, all references in lib are added as project references. When specifying a reference, only specify the name and not the path inside the package.</td>
        </tr>
        <tr>
            <td>frameworkAssemblies</td>
            <td><a href="/Release-Notes/NuGet-1.2">v1.2</a> The list of .NET Framework assembly references that this package requires. These are references to assemblies that exist in the .NET Framework and thus should already be in the GAC for any machine. Specifying framework assembly references  ensures these references are added when installing the package.</td>
        </tr>
        <tr>
            <td>tags</td>
            <td>A space-delimited list of tags and keywords that describe the package. This information is used to help make sure users can find the package using searches in the <strong>Add Package Reference</strong> dialog box or filtering in the <strong>Package Manager Console</strong> window.</td>
        </tr>
        <tr>
            <td>developmentDependency</td>
            <td><a href="/Release-Notes/NuGet-2.8">v2.8</a> A Boolean value that specifies whether the package will be marked as a <a href="../Release-Notes/NuGet-2.7#development-only-dependencies">development-only dependency</a> in the packages.config. This will cause the package to be excluded from the dependency list when the referencing project itself is later packaged.</td>
        </tr>
        <tr>
            <td>contentFiles</td>
            <td><a href="/Release-Notes/NuGet-3.3">v3.3</a>A collection of files that should be referenced in the consuming project.  These files can be specified with a set of 
            attributes that instruct the project system how they should be used in the project.</td>
        </tr>
        <tr>
            <td>minClientVersion</td>
            <td> <a href="/Release-Notes/NuGet-2.5">v2.5</a> Specifies the minimum version of the NuGet client that can install this package. This requirement is enforced by both the NuGet Visual Studio extension and nuget.exe program.</td>
        </tr>
        <tr>
            <td>packageTypes</td>
            <td>
                <a href="/Release-Notes/NuGet-3.5">v3.5</a> Specifies the type of the
                package. This is important if the package is something other than a traditional
                dependency package. For more information, see the <a href="../create-packages/package-types">package
                type</a> documentation.
            </td>
        </tr>
    </tbody>
</table>





### Replacement Tokens
When you create a package from a project file, replacement tokens might appear as values within the Metadata section of the .nuspec file.

For example, when you use the following command:

	nuget pack MyProject.csproj

The <code>myproject.nuspec</code> file adjacent to the <code>myproject.csproj</code> file may contain the following replacement tokens which 
are populated by the values within the project.  The file may also contain other replacement tokens that will be specified
using the [nuget pack -Properties](/ndocs/tools/nuget.exe-cli-reference#pack) feature.

<table class="reference">
    <tr><th>Token</th><th>Source</th></tr>
    <tr>
        <td>$id$</td>
        <td>The Assembly name</td>
    </tr>
    <tr>
        <td>$version$</td>
        <td>The assembly version as specified in the assembly's <code>AssemblyVersionAttribute</code>. If the assembly's <code>AssemblyInformationalVersionAttribute</code> is specified, that one is used instead.</td>
    </tr>
    <tr>
        <td>$author$</td>
        <td>The company as specified in the <code>AssemblyCompanyAttribute</code>.</td>
    </tr>
    <tr>
        <td>$description$</td>
        <td>The description as specified in the <code>AssemblyDescriptionAttribute</code>.</td> 
    </tr>
    <tr>
        <td>$references$</td>
        <td>
            This element contains a set of <code>reference</code> elements, each of which 
            specifies an assembly that will be referenced by the project. The existence of 
            this element overrides the convention of pulling everything in the <code>lib</code> 
            folder. More info below.
        </td>
    </tr>
</table>

In addition to using replacement tokens to modify the metadata of the package, you an also use the tokens to resolve paths when you [include files in the package](#specifying-files-to-include-in-the-package). When you build the package by using MSBuild, the properties defined for the build propagate through to the package build as values for replacement tokens. They will have the same names as the properties in MSBuild, which makes it possible to select files to be included depending on the current build configuration. For instance:


	<files>
	    <file src="bin\$configuration$\$id$.pdb" target="lib\net40\" />
	</files>

Assuming you are building the project that produces an assembly called Foo in release mode this will produce the following transformed xml:
	
	<files>
	    <file src="bin\Release\Foo.pdb" target="lib\net40" />
	</files>

##Dependencies

###Package Dependencies
The dependencies element is a child element of the metadata element and contains a set of dependency elements. Each dependency element is a reference to another package that this package depends on. 

	<dependencies>
	  <dependency id="RouteMagic" version="1.1.0" />
	  <dependency id="RouteDebugger" version="1.0.0" />
	</dependencies>

The following table lists attributes of the dependency element.

<table class="reference">
    <tr><th>Attribute</th><th>Description</th></tr>
    <tr>
        <td><code>id</code></td>
        <td>The package id of the package that this package is dependent upon. The package specified by the dependency id is required by this package and NuGet will install it when installing the current package.</td>
    </tr>
    <tr>
        <td><code>version</code></td>
        <td>The range of versions acceptable as a dependency. Typically this is just a version number which represents a minimum version. However a more explicit <a href="versioning#Specifying-Version-Ranges-in-.nuspec-Files">version range syntax</a> is supported.</td>
    </tr>
</table>

## Specifying Dependencies in version 2.0 and above

Starting from [version 2.0](/Release-Notes/NuGet-2.0), package dependencies can be specified to vary according to the framework profile of the target project. The <code>dependencies</code> element contains a set of <code>group</code> elements. Each group contains zero or more <code>dependency</code> element and a target framework attribute. All dependencies inside a group are installed together if the target framework is compatible with the project's framework profile.

	<dependencies> 
	   <group>
	      <dependency id="RouteMagic" version="1.1.0" />
	   </group>

	   <group targetFramework="net40">
	      <dependency id="jQuery" />
	      <dependency id="WebActivator" />
	   </group>
 
	   <group targetFramework="sl30">
	   </group>
	</dependencies>


The following table lists the attributes of a <code>group</code> element. 

<table class="reference">
    <tr><th>Attribute</th><th>Description</th></tr>
    <tr>
        <td><code>targetFramework</code></td>
        <td><em>Optional.</em> The target framework of the group. If not set, the group acts as a fallback group, which behaves exactly as before <a href="/Release-Notes/NuGet-2.0">version 2.0 </a></td>
    </tr>
</table>

The <code>dependency</code> element is the same as described in previous section.

A package can specify package dependencies in either two formats: either with a flat list of <code>dependency</code> as in pre-2.0, or in groups. However, mixing the two formats is disallowed. For example, the following snippet is **invalid** and will be rejected by NuGet.

	<dependencies> 
	   <dependency id="jQuery" />
	   <dependency id="WebActivator" />

	   <group>
	      <dependency id="RouteMagic" version="1.1.0" />
	   </group>
	</dependencies>

## Specifying Explicit Assembly References
Use the <code>&lt;references /&gt;</code> element to explicitly specify assemblies that the target project should 
reference.

For example, if you add the following:

	<references>
	  <reference file="xunit.dll" />
	  <reference file="xunit.extensions.dll" />
	</references>

Then only the _xunit.dll_ and _xunit.extensions.dll_ will be referenced from the appropriate 
[framework/profile subdirectory](../Create/Enforced-Package-Conventions#Grouping-Assemblies-by-Framework-Version) 
of the <code>lib</code> folder even if there are other assemblies in the folder.

If this element is omitted, then the usual behavior applies, which is to reference every assembly 
in the lib folder.

__What is this feature used for?__

This feature supports design-time only assemblies. For example, when using Code Contracts, the 
contract assemblies need to be next to the runtime assemblies that they augment so that Visual 
Studio can find them, but the contract assemblies should not actually be referenced by the project 
and should not be copied into the bin folder.

Likewise, the feature can be used to for unit test frameworks such as XUnit which need its tools 
assemblies to be located next to the runtime assemblies, but excluded from project references.

## Specifying Explicit Assembly References in version 2.5 and above

Starting from [version 2.5](/Release-Notes/NuGet-2.5), package assembly references can be specified to vary according to the framework profile of the target project. The <code>references</code> element contains a set of <code>group</code> elements. Each group contains zero or more <code>reference</code> element and a target framework attribute. All references inside a group are installed together if the target framework is compatible with the project's framework profile.

	<references> 
	  <group targetFramework="net45"> 
	      <reference file="a.dll" />
	  </group> 
	  <group targetFramework="netcore45"> 
	    <reference file="b.dll" /> 
	  </group>
	  <group>
	    <reference file="c.dll" />
	  </group>
	</references>


The following table lists the attributes of a <code>group</code> element. 

<table class="reference">
    <tr><th>Attribute</th><th>Description</th></tr>
    <tr>
        <td><code>targetFramework</code></td>
        <td><em>Optional.</em> The target framework of the group. If not set, the group acts as a fallback group, which behaves exactly as before version 2.5.</td>
    </tr>
</table>

The <code>reference</code> element is the same as described in previous section.

A package can specify package references in either two formats: either with a flat list of <code>reference</code> as in pre-2.5, or in groups. However, mixing the two formats is disallowed. For example, the following snippet is **invalid** and will be rejected by NuGet.

	<references>
	  <reference file="xunit.dll" />
	  <reference file="xunit.extensions.dll" />
	  <group>
	    <reference file="c.dll" />
	  </group>
	</references>
	

## Specifying Framework Assembly References GAC

In some cases, a package may depend on an assembly that's in the .NET Framework. Strictly speaking, it's 
not always necessary that the consumer of your package reference the framework assembly. But in some cases, 
it is important, such as when the developer needs to code against types in that assembly in order to use 
your package. The <code>frameworkAssemblies</code> element, a child element of the metadata element, allows you to 
specify a set of frameworkAssembly elements pointing to a Framework assembly in the GAC. Note the emphasis 
on Framework assembly. These assemblies are not included in your package as they are assumed to be on every 
machine  as part of the .NET Framework.

	<frameworkAssemblies>
	  <frameworkAssembly assemblyName="System.ServiceModel" targetFramework="net40" />
	  <frameworkAssembly assemblyName="System.SomethingElse"  />
	</frameworkAssemblies>

The following table lists attributes of the frameworkAssembly element.

<table class="reference">
    <tr>
        <th>Attribute</th>
        <th>Description</th>
    </tr>
    <tr>
        <td>assemblyName</td>
        <td>
            <em>Required.</em> The fully qualified assembly name.
        </td>
    </tr>
    <tr>
        <td>targetFramework</td>
        <td>
           <em>Optional.</em> If specified, the specific target framework that this reference applies to. 
            For example, if reference only applies to .NET 4, then the value should be "net40". If 
            the reference applies to *all* frameworks, then omit this attribute.
        </td>
    </tr>
</table>


## Specifying Files to Include in the Package

If you follow the conventions described in Creating a Package, you do not have to explicitly specify a list 
of files in the .nuspec file. Note that if you specify any files, the conventions are ignored and only the 
files listed in the .nuspec file are included in the package.

The files element is an optional child element of the package element and contains a set of file elements. 
Each file element specifies the source and destination of a file to include in the package via the <code>src</code> 
attribute and <code>target</code> attribute respectively.

<table class="reference">
    <tr>
        <th>Element</th>
        <th>Description</th>
    </tr>
    <tr>
        <td>src</td>
        <td>
            The location of the file or files to include. The path is relative to the NuSpec file 
            unless an absolute path is specified. The wildcard character, *,  is allowed. Using a double 
            wildcard, **, implies a recursive directory search.
        </td>
    </tr>
    <tr>
        <td>target</td>
        <td>
            This is a relative path to the directory within the package 
            where the source files will be placed. The relative path should begin with 'lib', 'content' or 'tools'.
        </td>
    </tr>
    <tr>
        <td>exclude</td>
        <td>
            The file or files to exclude. This is usually combined with a wildcard value in the <code>src</code>
            attribute. The <code>exclude</code> attribute can contain a semi-colon delimited list of files or 
            a file pattern. Using a double wildcard, **, implies a recursive exclude pattern.
        </td>
    </tr>
</table> 

The following is an example of a <code>files</code> element.

	<files>
	  <file src="bin\Debug\*.dll" target="lib" /> 
	  <file src="bin\Debug\*.pdb" target="lib" /> 
	  <file src="tools\**\*.*" exclude="**\*.log" />
	</files>
	
<p class="caution">NuGet does not add references to any dlls that end with '.resources.dll'. Please refer to the following Codeplex discussion: <a href="http://nuget.codeplex.com/discussions/280566" target="_blank" title="Nuget .resources.dll discussion">http://nuget.codeplex.com/discussions/280566</a></p> 

### File Element Examples
Let's look at some example usages of this element to gain a better understanding of how it's used.

#### Single Assembly
Copies a single assembly in the same folder as the NuSpec file into the package's <code>lib</code> folder.

Source Contains: <code>foo.dll</code>

	<file src="foo.dll" target="lib" />
	
Packaged Result: <code>lib\foo.dll</code>

#### Single Assembly In a deep path
Copies a single assembly  into the package's <code>lib\net40</code> folder so that it only applies to projects 
targeting the .NET 4 framework.

Source Contains: <code>foo.dll</code>

	<file src="assemblies\net40\foo.dll" target="lib\net40" />

Packaged Result: <code>lib\net40\foo.dll</code>


#### Set Of DLLs
Copies a set of assemblies within the <code>bin\release</code> folder into the package's <code>lib</code> folder.

Source contains: 

* <code>bin\release\MyLib.dll</code>
* <code>bin\release\CoolLib.dll</code>

NuSpec File Element

	<file src="bin\release\*.dll" target="lib" />

Packaged Result
* <code>lib\Mylib.dll</code>
* <code>lib\coollib.dll</code>

#### DLLs for different frameworks
Source contains:

* <code>lib\net40\foo.dll</code>
* <code>lib\net20\foo.dll</code>

NuSpec File Element:

	<file src="lib\**" target="lib" />

Packaged Result: 

* <code>lib\net40\foo.dll</code> 
* <code>lib\net20\foo.dll</code>

Note: The double wildcard implies a recursive search in the source for matching files.

#### Content Files

<div class="caution">
<strong>Caution:</strong><br/>
These instructions define a technique used in pre-3.0 versions of NuGet that include files in a package.  Newer project systems will not add or manage these files in your project.  NuGet recommends the newer contentFiles notation, described below.
</div>

Source Contains: 

* <code>css\mobile\style1.css</code>
* <code>css\mobile\style2.css</code>

NuSpec File Element:

	<file src="css\mobile\*.css" target="content\css\mobile" />

Packaged Result: 

* <code>content\css\mobile\style1.css</code>
* <code>content\css\mobile\style2.css</code>

#### Content files with directory structure
Source Contains:
 
* <code>css\mobile\style.css</code>
* <code>css\mobile\wp7\style.css</code>
* <code>css\browser\style.css</code>

NuSpec file element

	<file src="css\**\*.css" target="content\css" />

Packaged Result:

* <code>content\css\mobile\style.css</code> 
* <code>content\css\mobile\wp7\style.css</code>
* <code>content\css\browser\style.css</code>

#### Content File with deep path
Source Contains: <code>css\cool\style.css</code>

	<file src="css\cool\style.css" target="Content" />

Packaged Result: <code>content\style.css</code>

#### Content File copied to Folder with dot in name

	<file src="images\Neatpic.png" target="Content\images\foo.bar" />

Packaged Result: <code>content\images\foo.bar\neatpick.png</code>

Note: Because the target "extension" doesn't match the src extension, NuGet treats it as a directory.

#### Content files without extensions

To make sure extensionless files are captured by wildcard match use <code>&#42;&#42;</code> or <code>&#42;</code>

Source Contains: <code>flags\installed</code>

	<file src="flags\**" target="flags" />

Packaged Result: <code>flags\installed</code>


#### Content file with deep path and deep target

	<file src="css\cool\style.css" target="Content\css\cool" />

Packaged Result: <code>content\css\cool\style.css</code>

Note: This also works:

	<file src="css\cool\style.css" target="Content\css\cool\style.css" />

In this case, because the file extensions of the source and target match, it's assumed that the target 
is not a directory but is the file name.

#### Content file copy and rename

	<file src="ie\css\style.css" target="Content\css\ie.css" />

Packaged Result: <code>content\css\ie.css</code>

### Excluding files in the NuSpec
The <code>file</code> element within a NuSpec file can be used to include a specific file or a set of files 
using a wildcard. When using a wildcard, there's no way to exclude a specific subset of the included 
files. For example, suppose you want all text files within a directory except a specific one.

	<files>
	    <file src="docs\*.txt" target="content\docs" exclude="docs\admin.txt" />
	</files>

Use semicolons to specify multiple files.

	<files>
	    <file src="*.txt" target="content\docs" exclude="admin.txt;log.txt" />
	</files>

Use a wildcard to exclude a set of files such as all backup files.

	<files>
	    <file src="tools\*.*" target="tools" exclude="tools\*.bak" />
	</files>

Or use a double wildcard to exclude a set of files recursively across directories.

	<files>
	    <file src="tools\**\*.*" target="tools" exclude="**\*.log" />
	</files>

### ContentFiles with Visual Studio 2015 Update 1 and later

In order to provide a better experience and clearer definition of what files should be `` in a project, the new contentFiles element has been introduced to the nuspec.  These files are designed to be immutable, and should not be modified by developers who install the package.  This feature only applies to projects that are managed by project.json configuration files.

The contentFiles element contains a collection of files elements that define a mask of files and how those files should be references by a project.  The files elements reference a location inside of the NuGet package whose content could be placed there by either packaging the files directly on disk or by using a <code>files</code> element as demonstrated above.

Files child-elements of contentFiles in the nuspec are applied from top to bottom, with the topmost elements taking precedence over lower elements in the collection.  These elements apply to entries in the contentFiles folder of the package.  Type of content that work very well in this model include:

* Images that are embedded as resources
* Source files that are compiled
* PP files that are transformed to match the project they are included in
* Directories of scripts that need to be copied to the output directory with the same folder structure.


Content shall be stored in the package in folders that match this pattern:

	/contentFiles/{codeLanguage}/{TxM}/{any?}

* codeLanguages may be  <code>cs</code>, <code>fs</code>, <code>vb</code>, <code>any</code> or the lowercase equivalent of a given <code>$(ProjectLanguage)</code>
* TxM is any legal target framework moniker that NuGet supports
* Any folder structure may be appended to the end of this syntax.

Examples of legal folder locations include:

Language and framework agnostic: <code>/coNtentFiles/any/any/config.xml</code>
net45 content for all languages: <code>/coNtentFiles/any/net45/config.xml</code>
CSharp specific content for net45 and up: <code>/coNtentFiles/cs/net45/sample.cs</code>

Empty folders can use <code>_._</code> to opt out of providing content for certain combinations of language and TxM:

	/contentFiles/vb/any/code.vb
	/contentFiles/cs/any/_._
	
#### NuSpec contentFiles or files attributes

<table class="reference">
<tr>
 <th>Attribute</th><th>Description</th>
</tr>
<tr>
<td>include</td>    
<td>[Required attribute] Include provides either a file path or a wild card path. All matching files from the contentFiles folder will have the attributes for that files node applied. Examples: <code>**/*</code>, <code>**/*.cs</code>, <code>any/any/myfile.txt</code>, <code>**/net*/*</code>.</td>
</tr>
<tr>
<td>exclude</td>    
<td>Exclude provides either a file path or a wild card path. All matching files will be excluded from the include.</td>
</tr>
<tr>
<td>buildAction</td>
<td>Build action taken by msbuild for the content items. Examples: <code>None</code>, <code>Compile</code></td>
</tr>
<tr>
<td>copyToOutput</td>
<td>If True the content items will be copied to the build output folder</td>
</tr>
<tr>
<td>flatten</td>
<td>If False the content items will be copied to the build output folder using the full folder structure from the nupkg. This path will be relative to the TxM folder. Example: <code>cs/net45/config/data.xml</code> -> <code>config/data.xml</code></td>
</tr>
</table>

## Attributes Defaults

<table class="reference">
<tr>
    <th>Attribute</th>
    <th>Value</th>
</tr>
<tr>
<td>buildAction</td>
<td>Compile</td>
</tr>
<tr>
<td>copyToOutput</td>
<td>False</td>
</tr>
<tr>
<td>flatten</td>
<td>False</td>
</tr>
</table>

## Example contentFiles section

	<contentFiles>
	    <!-- Embed image resources -->
	    <files include="any/any/images/dnf.png" buildAction="EmbeddedResource" />
	    <files include="any/any/images/ui.png" buildAction="EmbeddedResource" />
	    <!-- Embed all image resources under contentFiles/cs/ using a wild card -->
	    <files include="cs/**/*.png" buildAction="EmbeddedResource" />
	    <!-- Copy config.xml to the root of the output folder -->
	    <files include="cs/uap10.0/config/config.xml" buildAction="None" copyToOutput="true" flatten="true" />
	    <!-- Copy run.cmd to the output folder and keep the directory structure -->
	    <!-- Include everything in the scripts folder except exe files -->
	    <files include="cs/uap10.0/scripts/*" exclude="**/*.exe"  buildAction="None" copyToOutput="true" />
	    <!-- All other files in shared are compiled and use the default options -->
	</contentFiles>

The include and exclude properties on contentFiles/files elements support wildcards using the aspnet syntax. https://github.com/aspnet/FileSystem

If multiple entries match the same file all entries will be applied. The top most entry will override the lower entries if there is a conflict for the same attribute.

The contentFiles section is optional, by default all files in the nupkg contentFiles directory will use the default attribute values defined above.


## Examples of .nuspec Files

The following example shows a simple .nuspec file that does not specify dependencies or files.

	<?xml version="1.0" encoding="utf-8"?>
	<package xmlns="http://schemas.microsoft.com/packaging/2010/07/nuspec.xsd">
	  <metadata>
	    <id>sample</id>
	    <version>1.2.3</version>
	    <authors>Kim Abercrombie, Franck Halmaert</authors>
	    <description>Sample exists only to show a sample .nuspec file.</description>
	    <language>en-US</language>
	    <projectUrl>http://xunit.codeplex.com/</projectUrl>
	    <licenseUrl>http://xunit.codeplex.com/license</licenseUrl>
	  </metadata>
	</package>

The following example shows a .nuspec file that specifies dependencies.

	<?xml version="1.0" encoding="utf-8"?>
	<package xmlns="http://schemas.microsoft.com/packaging/2010/07/nuspec.xsd">
	  <metadata>
	    <id>sample</id>
	    <version>1.0.0</version>
	    <authors>Microsoft</authors>
	    <dependencies>
	      <dependency id="another-package" version="3.0.0" />
	      <dependency id="yet-another-package"/>
	    </dependencies> 
	  </metadata>
	</package>

The following example shows a .nuspec file that specifies files.

	<?xml version="1.0"?>
	<package xmlns="http://schemas.microsoft.com/packaging/2010/07/nuspec.xsd">
	  <metadata>
	    <id>routedebugger</id>
	    <version>1.0.0</version>
	    <authors>Jay Hamlin</authors>
	    <requireLicenseAcceptance>false</requireLicenseAcceptance>
	    <description>Route Debugger is a little utility I wrote...</description>
	  </metadata>
	  <files>
	    <file src="bin\Debug\*.dll" target="lib" />
	  </files>
	</package>

The following example shows a .nuspec file that specifies framework assemblies.

	<?xml version="1.0"?>
	<package xmlns="http://schemas.microsoft.com/packaging/2010/07/nuspec.xsd">
	  <metadata>
	    <id>PackageWithGacReferences</id>
	    <version>1.0</version>
	    <authors>Author here</authors>
	    <requireLicenseAcceptance>false</requireLicenseAcceptance>
	    <description>
	        A package that has framework assemblyReferences depending 
	        on the target framework.
	    </description>
	    <frameworkAssemblies>
	      <frameworkAssembly assemblyName="System.Web" targetFramework="net40" />
	      <frameworkAssembly assemblyName="System.Net" targetFramework="net40-client, net40" />
	      <frameworkAssembly assemblyName="Microsoft.Devices.Sensors" targetFramework="sl4-wp" />
	      <frameworkAssembly assemblyName="System.Json" targetFramework="sl3" />
	    </frameworkAssemblies>
	  </metadata>
	</package>

Given the above package as an example, here is what will get installed for specific project targets:

* Project targeting .NET4 -> <code>System.Web</code>, <code>System.Net</code>
* Project targeting .NET4 Client Profile -> <code>System.Net  </code>
* Project targeting Silverlight 3 -> <code>System.Json</code>
* Project targeting Silverlight 4 -> <code>System.Windows.Controls.DomainServices</code> 
* Project targeting WindowsPhone -> <code>Microsoft.Devices.Sensors</code>
