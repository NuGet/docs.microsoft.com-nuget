# NuGet Configuration File

NuGet's configuration file allows for persisting configuration settings and changing default configuration values.

## Default Location

The default location for NuGet's configuration file is `%APPDATA%\NuGet\NuGet.Config`. APPDATA's value is OS, system drive, and current user dependent. For example: `c:\Users\username\AppData\Roaming\NuGet\NuGet.config`.

The default configuration file can be specified through the `-configfile` option for most of the commands when using NuGet CLI. For example, `nuget restore -configfile c:\my.config` means using file `c:\my.config` instead of `%APPDATA%\NuGet\NuGet.Config` as the default configuration file for doing the restore.

## Content

The configuration is stored in an XML file that can be changed with any text editor. The NuGet CLI 

For the full list of configuration settings, see the [NuGet Configuration Settings](../schema/nuget.config-file.md).


<div class="block-callout-info">
    <strong>Note:</strong><br>
    NuGet will silently ignore the entire configuration file if it encounters any XML parsing issues (such as mismatched begin/end nodes, invalid quotation, etc.); therefore an editor that reports parsing issues is recommended.
</div>


<div class="block-callout-info">
    <strong>Note:</strong><br>
	The keys are case sensitive.
</div>

Below is an example of NuGet configuration file that specifies some of the available settings and is annotated with comments.  

    <?xml version="1.0" encoding="utf-8"?>
    <configuration>
      <config>
	    <!-- 
		Used to specify the default location to expand packages.
		See: NuGet.exe help install
		See: NuGet.exe help update
		-->
        <add key="repositoryPath" value="External\Packages" />
		<!-- 
		Used to specify default source for the push command.
		See: NuGet.exe help push
		-->
        <add key="DefaultPushSource" value="https://MyRepo/ES/api/v2/package" />
		<!-- 
		Proxy settings
		-->
		<add key="http_proxy" value="host" />
		<add key="http_proxy.user" value="username" />
		<add key="http_proxy.password" value="encrypted_password" />
      </config>
	  <packageRestore>
        <!-- Allow NuGet to download missing packages -->
        <add key="enabled" value="True" />

        <!-- Automatically check for missing packages during build in Visual Studio -->
        <add key="automatic" value="True" />
      </packageRestore>
	  <!--
	  Used to specify the default Sources for list, install and update.
	  See: NuGet.exe help list
	  See: NuGet.exe help install
	  See: NuGet.exe help update
	  -->
      <packageSources>
        <add key="NuGet official package source" value="https://nuget.org/api/v2/" />
        <add key="MyRepo - ES" value="https://MyRepo/ES/nuget" />
      </packageSources>
	  <!-- used to store credentials -->
	  <packageSourceCredentials />
	  <!-- Used to specify which one of the sources are active -->
      <activePackageSource>
	    <!-- this tells only one given source is active -->
        <add key="NuGet official package source" value="https://nuget.org/api/v2/" />
		<!-- this tells that all of them are active -->
		<add key="All" value="(Aggregate source)" />
      </activePackageSource>
	  <!-- Used to disable package sources  -->
      <disabledPackageSources />
	  <!-- 
	  Used to specify default API key associated with sources. 
	  See: NuGet.exe help setApiKey
	  See: NuGet.exe help push
	  See: NuGet.exe help mirror
	  -->
      <apikeys>
        <add key="https://MyRepo/ES/api/v2/package" value="encrypted_api_key" />
      </apikeys>
    </configuration>

## Change the configuration from the CLI

NuGet CLI allows you to change the configuration values using the [config command](../tools/nuget-cli-reference#config). For example, to change the value of the repository path you can use:

    nuget config -set repositorypath=MyNewPath

You can also use the `-configfile` parameter to specify the file to update:

    nuget config -set repositorypath=MyNewPath -configfile MyNugetConfig.file

## Chaining multiple configuration files

In general the NuGet.config file closest to the folder nuget.exe runs from wins, the section below walks through the details.

NuGet first loads NuGet.config from the default location, then loads any file named NuGet.config starting from the root of the current drive and ending in the current directory.

**Current directory is defined as:**

a) Directory from which NuGet.exe is invoked (CLI scenario).

b) Directory in which the current solution was loaded from (Visual Studio scenario).


**The following rules are used to determine the actual configuration values:**

a) When `<clear />` is present for a given node, previously defined configuration items for this node are ignored.

b) For nodes used as collections (for example `<packageSources>`), new elements are added to the collection.

c) For elements used as single items (for example elements under `<config>` or `<packageRestore>`), new elements replace previous elements with the same key.

For example, assuming this directory structure:

    C:\
    └───Users
    F:\
    ├───Project1
    │   └───Source
	├───Project2
    │   └───Source
	└───Tmp
       
With these four NuGet config files present on the filesystem: 

a) %APPDATA%\NuGet\Nuget.config with content:

    <?xml version="1.0" encoding="utf-8"?>
    <configuration>
      <activePackageSource>
        <add key="NuGet official package source" value="https://nuget.org/api/v2/" />
      </activePackageSource>
    </configuration>

b) F:\NuGet.config with content:

    <?xml version="1.0" encoding="utf-8"?>
    <configuration>
	  <config>
        <add key="repositoryPath" value="F:\tmp" />
      </config>
      <packageRestore>
        <add key="enabled" value="True" />
      </packageRestore>
    </configuration>

c) F:\Project1\NuGet.config with content:

    <?xml version="1.0" encoding="utf-8"?>
    <configuration>
      <config>
        <add key="repositoryPath" value="External\Packages" />
        <add key="DefaultPushSource" value="https://MyPrivateRepo/ES/api/v2/package" />
      </config>
      <packageSources>
        <clear /> <!-- ensure only the sources defined below are used -->
        <add key="MyPrivateRepo - ES" value="https://MyPrivateRepo/ES/nuget" />
      </packageSources>
    </configuration>

d) F:\Project2\NuGet.config with content:

    <?xml version="1.0" encoding="utf-8"?>
    <configuration>
      <packageSources>
	    <!-- Add this repository to the list of available repositories -->
        <add key="MyPrivateRepo - DQ" value="https://MyPrivateRepo/DQ/nuget" />
      </packageSources>
    </configuration>


<em>NuGet will load:</em>

**When invoked from C:\Users**: Only 1. The default repository on NuGet.org is used.

**When invoked from F:\ or F:\tmp**: 1 and 2. The default repository on NuGet.org is used, package restore is enabled and packages get expanded in F:\tmp.

**When invoked from F:\Project1 or F:\Project1\Source**: 1, 2 and 3. The last config file that gets loaded overrides `repositoryPath` therefore packages get expanded in F:\Project1\External\Packages instead of F:\tmp. It also clears `<packageSources>` therefore nuget.org is no longer available as a source; instead only https://MyPrivateRepo/ES/nuget is available.

**When invoked from F:\Project2 or F:\Project2\Source**: 1, 2 and 4. This time `packageSources` is not cleared, therefore both nuget.org and https://MyPrivateRepo/DQ/nuget are available as source repositories. Packages get expanded in F:\tmp

## NuGet config extensibility point

<div class="block-callout-info">
    <strong>Note:</strong><br>
	Closest to the folder nuget.exe runs from wins.
</div>

Starting with NuGet 3.4, config files are treated in the following priority order, for example assuming the solution directory is c:\a\b\c:

	c:\a\b\c\.nuget\nuget.config - Only for solution level packages, and not supported in nuget 3.0+
	c:\a\b\c\nuget.config
	c:\a\b\nuget.config
	c:\a\nuget.config
	c:\nuget.config
	User specific config file, %AppData%\NuGet\nuget.config.

	Or the user specified file thru option `-configfile`.

Starting with NuGet 2.6 upto 3.3, with the new config extensibility point, a new location for machine wide config files located under directory %ProgramData%\NuGet\Config are read after the user specific config file. So, the above list now becomes:

	c:\a\b\c\\.nuget\nuget.config
		-c:\a\b\c\nuget.config
			-c:\a\b\nuget.config
				-c:\a\nuget.config
					-c:\nuget.config
						-User specific config file, %AppData%\NuGet\nuget.config
						-Or the user specified file thru option -ConfigFile
							-%ProgramData%\NuGet\Config\{IDE}\{Version}\{SKU}\*.config
								-%ProgramData%\NuGet\Config\{IDE}\{Version}\*.config
									-%ProgramData%\NuGet\Config\{IDE}\*.config
										-%ProgramData%\NuGet\Config\*.config

In the above path locations `{IDE}` can be VisualStudio and if you want to specify config for a particular SKU of Visual Studio `{SKU}` can be Pro, Enterprise or Community.
