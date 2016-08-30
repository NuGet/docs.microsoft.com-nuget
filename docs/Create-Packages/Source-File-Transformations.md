# Configuration File and Source Code Transformations

NuGet supports the ability to make transformations to source code and configuration files at package install and uninstall times. 

## Overview
In general, when you create a package, the files that you include in the package are not 
modified in any way and are simply copied to the appropriate location in the target solution. 
However, in some cases you might want a file to be modified or transformed during installation. 

### Configuration File Transformations
Some packages need to customize the configuration files (*web.config* or *app.config*) of the project 
that the package is installed in. 

For example, [ELMAH (Error Logging Modules and Handlers for ASP.NET)](http://code.google.com/p/elmah/) 
needs to add an entry in the `httpModules` section of a *web.config* file.

You can specify changes to make in the project's *app.config* or *web.config* file by including a file 
named *web.config.transform* or `app.config.transform`.

### Source Code Transformations

To indicate a source code file will have source code transformations, *.pp* must be appended to the source code file name. 

You can then embed variables in the source code that are replaced at installation
with values appropriate to the target project. 

For example, to make source code transformations in *Contoso.cs*, the file should be renamed, *Contoso.cs.pp*.

In the file, `namespace $rootnamespace$` will be transformed to `namespace Fabrikam` when the file is installed in a target project whose 
root namespace is `Fabrikam`. 

In the target project, the source code file no longer has the *.pp* 
extension. 

## Specifying Configuration File Transformations

Starting with NuGet 2.6, XDT is supported to transform XML files inside a project. The
[XDT syntax](http://msdn.microsoft.com/en-us/library/dd465326.aspx) can be utilized in the *.install.xdt* and
*.uninstall.xdt* file(s) under the package's content folder. These transformations will be applied during package install
or uninstall time, respectively (will not transform *.config files referenced as a link in Visual Studio).

For example, suppose the starting project contains the following *web.config* file:

    <configuration>
        <system.webServer>
            <modules>
                <add name="MyModule" type="Sample.MyModule" />
            </modules>
        <system.webServer>
    </configuration>
    
To add `MyNuModule` element to the `modules` section of web.config, the *web.config.install.xdt* file can be written as:

    <?xml version="1.0"?>
    <configuration xmlns:xdt="http://schemas.microsoft.com/XML-Document-Transform">
        <system.webServer>
            <modules>
                <add name="MyNuModule" type="Sample.MyNuModule" xdt:Transform="Insert" />
            </modules>
        </system.webServer>
    </configuration>
    
After installing the package, the resulting *web.config* of the project will look like the following:

    <configuration>
        <system.webServer>
            <modules>
                <add name="MyModule" type="Sample.MyModule" />
                <add name="MyNuModule" type="Sample.MyNuModule" /> 
            </modules>
        <system.webServer>
    </configuration>

To remove only the `MyNuModule` element during package uninstall, the web.config.uninstall.xdt file can be written as:

    <?xml version="1.0"?>
    <configuration xmlns:xdt="http://schemas.microsoft.com/XML-Document-Transform">
        <system.webServer>
            <modules>
                <add name="MyNuModule" xdt:Transform="Remove" xdt:Locator="Match(name)" />
            </modules>
        </system.webServer>
    </configuration>

### What you can do with XDT

One of XDT’s greatest strengths is its [simple but powerful syntax](http://msdn.microsoft.com/en-us/library/dd465326.aspx)
for manipulating the structure of an XML DOM. Rather than simply overlaying one fixed document
structure onto another structure, XDT provides controls for matching elements in a variety of
ways, from simple attribute name matching to full XPath support. 

Once a matching element or
set of elements is found, XDT provides a rich set of functions for manipulating the elements. These functions include: 

* Adding, updating, or removing attributes
* Placing a new element at a specific
location
* Replacing or removing an element and its children

While this is a very basic example, you now have the full power of XDT at your disposal for *web.config* transforms.
This will allow your packages to conditionally insert, remove, or modify elements and attribute values.  

Like the
PowerShell install.ps1 and uninstall.ps1 support though, package uninstallation should make a best effort to
return the web.config file to its previous state in all scenarios.

In addition to XDT transformation, NuGet also supports the traditional way of transforming web.config files. 

For more information, please refer to [Transform Web.Config using .transform Files](Config-File-Transformations.md).

## Specifying Source Code Transformations

NuGet also supports source-code transformations that work like Visual Studio project templates. 
These transformations are useful if your NuGet package includes source code to be added to the project it's 
installed in. 

For example, you might want to include source code that's used to initialize your library, 
but you want the code to exist in the target project’s namespace.

You specify a source code transformation by including Visual Studio project properties in the code. The 
properties are delimited using dollar signs `$`. 

For example, to specify that the target 
project’s root namespace should be inserted into the namespace statement. The source code that you include 
in the project might look like the following (**Note:** rootnamespace project enclosed by dollar signs `$`):

    namespace $rootnamespace$.Models {
        public struct CategoryInfo {
            public string categoryid;
            public string description;
            public string htmlUrl;
            public string rssUrl;
            public string title;
        }
    }

In order to enable the transformation process for a source-code file, append the .pp file extension 
to the name of the source code file, as shown in the following example:

![](/images/Create/pp.files.png)

When the source-code files example above are installed, NuGet transforms the files, removes 
the .pp extension, and adds them to the target project’s ~/Models directory.

The `$rootnamespace$` token is the most commonly used project property, but any project property that's 
available when a package is installed can be used. (Some project properties might be specific to the 
current project type.) 

For more information about project properties, see [MSDN documentation for project 
properties](http://msdn.microsoft.com/en-us/library/vslangproj.projectproperties_properties(VS.80).aspx).

An example of source code that you might need to customize in the target project for is code that you 
would normally put in the *global.asax* file because it needs to run when the application starts. 

For more information about how to achieve this effect without updating the *global.asax* file, see 
[WebActivator](https://bitbucket.org/davidebbo/webactivator/wiki/Home).

Please note that project properties can be transformed using XDT as well (starting with NuGet 2.6).  The following
example illustrates how to add app.config settings to a project, via the *app.config.install.xdt* file: 

    <?xml version="1.0"?>
    <configuration xmlns:xdt="http://schemas.microsoft.com/XML-Document-Transform">
        <appSettings xdt:Transform="Insert">
         <add key="FullPath" value="$FullPath$" />
         <add key="FileName" value="$filename$" />
         <add key="ActiveConfigurationSettings " value="$ActiveConfigurationSettings$" />
        </appSettings>
    </configuration>
