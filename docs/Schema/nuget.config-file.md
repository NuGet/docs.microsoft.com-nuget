# NuGet.Config Reference

NuGet behavior is controlled by settings in `NuGet.Config` files as described in [Configuring NuGet Behavior](/ndocs/consume-packages/configuring-nuget-behavior). Setting names are case-insensitive.

`NuGet.Config` is an XML file containing a top-level &lt;configuration&gt; node, which then contains the section elements described in this topic. Each section then contains zero or more &lt;add&gt; elements with `key` and `value` attributes. See the [examples config file](#example-config-file) at the end of this topic.

Note that with NuGet 3.4+ you can use environment variables in `NuGet.Config` values to apply machine-specific settings at run time. For example, if the `HOME` environment variable is set to `c:\users\username`, then the value of `%HOME%\NuGetRepository` in the configuration file will resolve to `c:\users\username\NuGetRepository`. If an environment variable is not found, NuGet will leave the value unmodified.

In this topic:

- [config section](#config-section)
- [bindingRedirects section](#bindingredirects-section)
- [packageRestore section](#packagerestore-section)
- [solution section](#solution-section)
- [Package source sections](#package-source-sections):
	- [packageSources](#packagesources)
	- [packageSourceCredentials](#packagesourcecredentials)
	- [apikeys](#apikeys)
	- [disabledPackageSources](#disabledpackagesources)
	- [activePackageSource](#activepackagesource)	
- [Example config file](#example-config-file)



## config section
<a name="dependencyVersion"></a>
<a name="globalPackagesFolder"></a>
<a name="repositoryPath"></a>
<a name="proxy-settings"></a>

Contains miscellaneous configuration settings, which can be set using the [`nuget config` command](/ndocs/tools/nuget.exe-cli-reference#config).

Note: `dependencyVersion` and `repositoryPath` apply only to projects using `packages.config`. `globalPackagesFolder` applies only to projects using `project.json`.

<table>
	<tr>
		<th>Key</th>
		<th>Value</th>
	</tr>
	<tr>
		<td>dependencyVersion<br>(package.config only)</td>
		<td>The default <code>DependencyVersion</code> value for package install, restore, and update, when the `-DependencyVersion` switch is not specified directly. This value is also used by the NuGet Package Manager UI. Values are <code>Lowest</code>, <code>HighestPatch</code>, <code>HighestMinor</code>, <code>Highest</code>.</td>
	</tr>
	<tr>
		<td>globalPackagesFolder<br>(project.json only)</td>
		<td>The location of the default global packages folder. The default is <code>%userprofile%\.nuget\packages</code>. A relative path can be used in project-specific <code>nuget.config</code> files.</td>
	</tr>	
	<tr>
		<td>repositoryPath<br>(packages.config only)</td>
		<td>The location in which to install NuGet packages instead of the default <code>$(Solutiondir)\packages</code> folder. A relative path can be used in project-specific <code>nuget.config</code> files.</td>
	</tr>
	<tr>
		<td>defaultPushSource</td>
		<td>Identifies the URL or path of the package source that should be used as the default if no other package sources are found for an operation.</td>
	</tr>
	<tr>
		<td>http_proxy<br>http_proxy.user<br>http_proxy.password<br>no_proxy</td>
		<td>Proxy settings to use when connecting to package sources; <code>http_proxy</code> should be in the format <code>http://&lt;username&gt;:&lt;password&gt@&lt;domain&gt</code>. Passwords are encrypted and cannot be added manually. For <code>no_proxy</code>, the value is a comma-separated list of domains the bypass the proxy server. You can alternately use the HTTP_PROXY and NO_PROXY environment variables for those values. For additional details, see <a href="http://skolima.blogspot.com/2012/07/nuget-proxy-settings.html">NuGet proxy settings</a> (skolima.blogspot.com).</td>
	</tr>	
</table>

**Example**:

	  <config>
        <add key="dependencyVersion" value="Highest" />
		<add key="globalPackagesFolder" value="c:\packages" />
        <add key="repositoryPath" value="c:\repo" />        
        <add key="http_proxy" value="http://company-squid:3128@contoso.com" />
      </config>


## bindingRedirects section

Configures whether NuGet does automatic binding redirects when a package is installed.

<table>
	<tr>
		<th>Key</th>
		<th>Value</th>
	</tr>
	<tr>
		<td>skip</td>
		<td>A Boolean indicating whether to skip automatic binding redirects. The default is false.</td>
	</tr>
</table>

**Example**:

    <bindingRedirects>
        <add key="skip" value="True" />
    </bindingRedirects>


## packageRestore section

*Ignored in 2.7+*

Controls package restore during builds.

<table>
	<tr>
		<th>Key</th>
		<th>Value</th>
	</tr>
	<tr>
		<td>enabled</td>
		<td>A Boolean indicating whether NuGet can perform automatic restore. You can also set the <code>EnableNuGetPackageRestore</code> environment variable with a value of <code>True</code> instead of setting this key in the config file.</td>
	</tr>
	<tr>
		<td>automatic</td>
		<td>A Boolean indicating whether NuGet shoudl check for missing packages during a build.</td>
	</tr>
</table>


**Example**:

    <packageRestore>
      <add key="enabled" value="true" />
      <add key="automatic" value="true" />
    </packageRestore>

## solution section

Controls whether the `packages` folder of a solution is included in source control. This section works only in `nuget.config` files in a solution folder.

<table>
	<tr>
		<th>Key</th>
		<th>Value</th>
	</tr>
	<tr>
		<td>disableSourceControlIntegration</td>
		<td>A Boolean indicating whether to ignore the packages folder when working with source control. The default value is true.</td>
	</tr>

</table>

**Example**:

    <solution>
      <add key="disableSourceControlIntegration" value="true" />
    </solution>


## Package source sections

The `packageSources`, `packageSourceCredentials`, `apikeys`, `activePackageSource`, and `disabledPackageSources` all work together to configure how NuGet works with package repositories during install, restore, and update operations.

The [`nuget sources` command](/ndocs/tools/nuget.exe-cli-reference#sources) is generally used to manage these settings, except for `apikeys` which is managed using the [`nuget setapikey` command](/ndocs/tools/nuget.exe-cli-reference#setapikey). 

### packageSources 

Lists all known package sources. Sources can be added using the 

<table>
	<tr>
		<th>Key</th>
		<th>Value</th>
	</tr>
	<tr>
		<td>(name to assign to the package source)</td>
		<td>The path or URL of the package source.</td>
	</tr>
</table>

**Example**:

    <packageSources>
	    <add key="nuget.org" value="https://api.nuget.org/v3/index.json" protocolVersion="3" />
	    <add key="Contoso Package Source" value="https://contoso.com/packages/" />
		<add key="Test source" value="c:\packages" />
	</packageSources>


### packageSourceCredentials

Stores usernames and passwords for sources, typically specified with the `-username` and `-password` switches with `nuget sources`. Passwords are encrypted by default unless the `-storepasswordincleartext` option is also used.  

<table>
	<tr>
		<th>Key</th>
		<th>Value</th>
	</tr>
	<tr>
		<td>username</td>
		<td>The user name for the source in plain text.</td>
	</tr>
	<tr>
		<td>password</td>
		<td>The encrypted password for the source.</td>
	</tr>
	<tr>
		<td>cleartextpassword</td>
		<td>The unencrypted password for the source.</td>
	</tr>
</table>

**Example:**

In the config file, the &lt;packageSourceCredentials&gt; element will contain child nodes for each applicable source name. That is, for a source named "Contoso", the config file will contain the following when using an encrypted password:

    <packageSourceCredentials>
      <Contoso>
        <add key="Username" value="user@contoso.com" />
        <add key="Password" value="..." />
      </Contoro>
    </packageSourceCredentials>

When using an unencrypted password:

    <packageSourceCredentials>
      <Contoso>
        <add key="Username" value="user@contoso.com" />
        <add key="ClearTextPassword" value="33f!!lloppa" />
      </Contoso>
    </packageSourceCredentials>

### apikeys

Stores keys for sources that use API key authentication, as set with the [`nuget -setapikey` command](/ndocs/tools/nuget.exe-cli-reference#setapikey).

<table>
	<tr>
		<th>Key</th>
		<th>Value</th>
	</tr>
	<tr>
		<td>(source URL)</td>
		<td>The encrypted API key.</td>
	</tr>
</table>

**Example**:

      <apikeys>
        <add key="https://MyRepo/ES/api/v2/package" value="encrypted_api_key" />
      </apikeys>


### disabledPackageSources

Identified currently disabled sources. May be empty.

<table>
	<tr>
		<th>Key</th>
		<th>Value</th>
	</tr>
	<tr>
		<td>(name of source)</td>
		<td>A Boolean indicating whether the source is disabled.</td>
	</tr>
</table>


**Example:**

	<disabledPackageSources>
    	<add key="Contoso Package Source" value="true" />
  	</disabledPackageSources>

	<!-- Empty list -->
    <disabledPackageSources />

### activePackageSource

*(2.x only; deprecated in 3.x+)*

Identifies to the currently active source or indicates the aggregate of all sources.

<table>
	<tr>
		<th>Key</th>
		<th>Value</th>
	</tr>
	<tr>
		<td>(name of source) or `All`</td>
		<td>If key is the name of a source, the value is the source path or URL. If `All`, value should be `(Aggregate source)` to combine all package sources that are not otherwise disabled.</td>
	</tr>
</table>

**Example**:

    <activePackageSource>
	    <!-- Only one active source-->
	    <add key="nuget.org" value="https://nuget.org/api/v2/" />

		<!-- All non-disabled sources are active -->
		<add key="All" value="(Aggregate source)" />
  	</activePackageSource>


## Example config file

Below is an example `NuGet.Config` file that illustrates a number of settings:  

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

		<!-- Proxy settings -->
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

	  <!-- Used to store credentials -->
	  <packageSourceCredentials />

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
