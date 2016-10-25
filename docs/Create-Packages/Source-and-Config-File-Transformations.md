# Source Code and Configuration File Transformations

NuGet supports the ability to make transformations to source code and configuration files at package install and uninstall times. 

A **source code transformation** applies one-way token replacement to files in the package's `content` folder when the package is installed, where tokens refer to Visual Studio [project properties](https://msdn.microsoft.com/library/vslangproj.projectproperties_properties.aspx). This allows you to insert a file into the project's namespace, or to customize code that would typically go into `global.asax` in an ASP.NET project. 

A **config file transformation** allows you to modify files that already exist in a target project, such as `web.config` and `app.config`. For example, your package might need to add an item to the `modules` section in the config file. This transformation is done by including special files in the package that describe the sections to add to the configuration files. When a package is uninstalled, those same changes are then reversed, making this a two-way transformation. 
 

## Specifying source code transformations

1. Files that you want to insert from the package into the project must be located within the package's `content` folder. For example, if you want a file called `ContosoData.cs` to be installed in a `Models` folder of the target project, it must be inside the `content\Models` folder in the package.

2. To instruct NuGet to apply token replacement at install time, append `.pp` to the source code file name. After installation, the file will not have the `.pp` extension. 

	For example, to make transformations in `ContosoData.cs`, name the file in the package `ContosoData.cs.pp`. After installation it will appear as `ContosoData.cs`.

3. In the source code file, use case-insensitive tokens of the form `$token$` to indicate values that NuGet should replace with project properties:  

	    namespace $rootnamespace$.Models {
	        public struct CategoryInfo {
	            public string categoryid;
	            public string description;
	            public string htmlUrl;
	            public string rssUrl;
	            public string title;
	        }
	    }

	Upon installation, NuGet replaces `$rootnamespace$` with `Fabrikam` assuming the target project's whose root namespace is `Fabrikam`.

The `$rootnamespace$` token is the most commonly used project property; all others are listed in the [Project Properties](https://msdn.microsoft.com/library/vslangproj.projectproperties_properties.aspx) documentation on MSDN. Be mindful, of course, that some properties might be specific to the project type.


## Specifying config file transformations

As described in the sections that follow, config file transformations can be done in two ways:

- Include `app.config.transform` and `web.config.transform` files in your package's `content` folder, where the `.transform` extension tells NuGet that these files contain the XML to merge with existing config files when the package is installed. When a package is uninstalled, that same XML is removed.
- (NuGet 2.6 and later) Include `app.config.install.xdt` and `web.config.install.xdt` files in your package's `content` folder, using [XDT syntax](https://msdn.microsoft.com/library/dd465326.aspx) to describe the desired changes. With this option you can also include a `.uninstall.xdt` file to reverse changes when the package is removed from a project.

<div class="block-callout-info">
	<strong>Note</strong><br>
	Transformations are not applied to <em>.config</em> files referenced as a link in Visual Studio. 
</div>

The advantage of using XDT is that instead of simply merging two static files, it provides a syntax for manipulating the structure of an XML DOM using element and attribute matching using full XPath support. XDT can then add, update, or remove elements, place new elements at a specific location, or replace/remove elements (including child nodes). This makes it straightforward to create uninstall transforms that back out all transformations done during package installation. 

### XML transforms

The `app.config.transform` and `web.config.transform` in a package's `content` folder contain only those elements to merge into the project's existing `app.config` and `web.config` files.

As an example, suppose the project initially contains the following content in `web.config`:

    <configuration>
        <system.webServer>
            <modules>
                <add name="ContosoUtilities" type="Contoso.Utilities" />
            </modules>
        <system.webServer>
    </configuration>

To add a `MyNuModule` element to the `modules` section during package install, create a `web.config.transform` file in the package's `content` folder that looks like this:

    <configuration>
        <system.webServer>
            <modules>
                <add name="MyNuModule" type="Sample.MyNuModule" />
            </modules>
        <system.webServer>
    </configuration>

After NuGet installs the package, `web.config` will appear as follows:

    <configuration>
        <system.webServer>
            <modules>
                <add name="ContosoUtilities" type="Contoso.Utilities" />
                <add name="MyNuModule" type="Sample.MyNuModule" />
            </modules>
        <system.webServer>
    </configuration>

Notice that NuGet didn't replace the `modules` section, it just merged the new entry into it by adding only new elements and attributes. NuGet will not change any existing elements or attributes. 

When the package is uninstalled, NuGet will examine the `.transform` files again and remove the elements it contains from the appropriate `.config` files. Note that this process will not affect any lines in the `.config` file that you modify after package installation.

As a more extensive example, the [Error Logging Modules and Handlers for ASP.NET (ELMAH)](https://www.nuget.org/packages/elmah/) package adds many entries into `web.config`, which are again removed when a package is uninstalled.

To examine its `web.config.transform` file, download the ELMAH package from the link above, change the package extension from `.nupkg` to `.zip`, and then open `content\web.config.transform` in that ZIP file.

To see the effect of installing and uninstalling the package, create a new ASP.NET project in Visual Studio (the template is under **Visual C# > Web** in the New Project dialog), and select an empty ASP.NET application. Open `web.config` to see its initial state. Then right-click the project, select **Manage NuGet Packages**, browse for ELMAH on nuget.org, and install the latest version. Notice all the changes to `web.config`. Now uninstall the package and you'll see `web.config` revert to its prior state.  


### XDT transforms

With NuGet 2.6 and later, you can modify config files using [XDT syntax](https://msdn.microsoft.com/library/dd465326.aspx). You can also have NuGet replace tokens with [Project Properties](https://msdn.microsoft.com/library/vslangproj.projectproperties_properties.aspx) by including the property name within `$` delimeters (case-insensitive).

For example, the following `app.config.install.xdt` file will insert an `appSettings` element into `app.config` containing the `FullPath`, `FileName`, and `ActiveConfigurationSettings` values from the project: 
  
    <?xml version="1.0"?>
    <configuration xmlns:xdt="http://schemas.microsoft.com/XML-Document-Transform">
        <appSettings xdt:Transform="Insert">
         <add key="FullPath" value="$FullPath$" />
         <add key="FileName" value="$filename$" />
         <add key="ActiveConfigurationSettings " value="$ActiveConfigurationSettings$" />
        </appSettings>
    </configuration>

For another example, suppose the project initially contains the following content in `web.config`:

    <configuration>
        <system.webServer>
            <modules>
                <add name="ContosoUtilities" type="Contoso.Utilities" />
            </modules>
        <system.webServer>
    </configuration>
    
To add a `MyNuModule` element to the `modules` section during package install, the package's `web.config.install.xdt` would contain the following:

    <?xml version="1.0"?>
    <configuration xmlns:xdt="http://schemas.microsoft.com/XML-Document-Transform">
        <system.webServer>
            <modules>
                <add name="MyNuModule" type="Sample.MyNuModule" xdt:Transform="Insert" />
            </modules>
        </system.webServer>
    </configuration>
    
After installing the package, `web.config` will look like this:

    <configuration>
        <system.webServer>
            <modules>
                <add name="ContosoUtilities" type="Contoso.Utilities" />
                <add name="MyNuModule" type="Sample.MyNuModule" /> 
            </modules>
        <system.webServer>
    </configuration>

To remove only the `MyNuModule` element during package uninstall, the `web.config.uninstall.xdt` file should contain the following:

    <?xml version="1.0"?>
    <configuration xmlns:xdt="http://schemas.microsoft.com/XML-Document-Transform">
        <system.webServer>
            <modules>
                <add name="MyNuModule" xdt:Transform="Remove" xdt:Locator="Match(name)" />
            </modules>
        </system.webServer>
    </configuration>




