---
# required metadata

title: NuGet.Config File Reference | Microsoft Docs
author: kraigb
ms.author: kraigb
manager: ghogen
ms.date: 1/9/2017
ms.topic: article
ms.prod: nuget
#ms.service:
ms.technology: nuget
ms.assetid: fbf31530-3bf4-478c-b26c-c2b24dd3406d

# optional metadata

#description:
#keywords:
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

# NuGet.Config Reference

NuGet behavior is controlled by settings in `NuGet.Config` files as described in [Configuring NuGet Behavior](../consume-packages/configuring-nuget-behavior.md). Setting names are case-insensitive.

`NuGet.Config` is an XML file containing a top-level &lt;configuration&gt; node, which then contains the section elements described in this topic. Each section then contains zero or more &lt;add&gt; elements with `key` and `value` attributes. See the [examples config file](#example-config-file) at the end of this topic.

Note that with NuGet 3.4+ you can use environment variables in `NuGet.Config` values to apply machine-specific settings at run time. For example, if the `HOME` environment variable is set to `c:\users\username`, then the value of `%HOME%\NuGetRepository` in the configuration file will resolve to `c:\users\username\NuGetRepository`. If an environment variable is not found, NuGet will leave the value unmodified.

Also note that with Visual Studio 2017+ and NuGet 4.0+, the machine-wide `NuGet.config` is now located at `%ProgramFiles(x86)%\NuGet\Config\` to improve security in multi-user scenarios. You will need to manually migrate existing config files from `%ProgramData%` to `%ProgramFiles(x86)%`. Going forward,NuGet 4.0+ will also treat this as the new location for the machine-wide configuration. `NuGet.config` in `%ProgramData%\NuGet\Config\` will no longer be implicitly referenced or considered for hierarchical merging of `nuget.config`.

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

<a name="dependencyVersion"></a>
<a name="globalPackagesFolder"></a>
<a name="repositoryPath"></a>
<a name="proxy-settings"></a>

## config section

Contains miscellaneous configuration settings, which can be set using the [`nuget config` command](../tools/nuget-exe-cli-reference.md#config).

Note: `dependencyVersion` and `repositoryPath` apply only to projects using `packages.config`. `globalPackagesFolder` applies only to projects using `project.json`.

Key | Value
--- | ---
dependencyVersion (package.config only) | The default `DependencyVersion` value for package install, restore, and update, when the `-DependencyVersion` switch is not specified directly. This value is also used by the NuGet Package Manager UI. Values are `Lowest`, `HighestPatch`, `HighestMinor`, `Highest`.
globalPackagesFolder (project.json only) | The location of the default global packages folder. The default is `%userprofile%\.nuget\packages`. A relative path can be used in project-specific `nuget.config` files.
repositoryPath (packages.config only) | The location in which to install NuGet packages instead of the default `$(Solutiondir)\packages` folder. A relative path can be used in project-specific `nuget.config` files.
defaultPushSource | Identifies the URL or path of the package source that should be used as the default if no other package sources are found for an operation.
http_proxy http_proxy.user http_proxy.password no_proxy | Proxy settings to use when connecting to package sources; `http_proxy` should be in the format `http://&lt;username&gt;:&lt;password&gt@&lt;domain&gt`. Passwords are encrypted and cannot be added manually. For `no_proxy`, the value is a comma-separated list of domains the bypass the proxy server. You can alternately use the http_proxy and no_proxy environment variables for those values. For additional details, see [NuGet proxy settings](http://skolima.blogspot.com/2012/07/nuget-proxy-settings.html) (skolima.blogspot.com).


**Example**:

  ```xml
      <config>
        <add key="dependencyVersion" value="Highest" />
        <add key="globalPackagesFolder" value="c:\packages" />
        <add key="repositoryPath" value="c:\repo" />
        <add key="http_proxy" value="http://company-squid:3128@contoso.com" />
      </config>
  ```


## bindingRedirects section

Configures whether NuGet does automatic binding redirects when a package is installed.

Key | Value
--- | ---
skip | A Boolean indicating whether to skip automatic binding redirects. The default is false.

**Example**:

```xml
    <bindingRedirects>
        <add key="skip" value="True" />
    </bindingRedirects>
```

## packageRestore section

*Ignored in 2.7+*

Controls package restore during builds.

Key | Value
--- | ---
enabled | A Boolean indicating whether NuGet can perform automatic restore. You can also set the `EnableNuGetPackageRestore` environment variable with a value of `True` instead of setting this key in the config file.
automatic | A Boolean indicating whether NuGet should check for missing packages during a build.

**Example**:

```xml
    <packageRestore>
      <add key="enabled" value="true" />
      <add key="automatic" value="true" />
    </packageRestore>
```

## solution section

Controls whether the `packages` folder of a solution is included in source control. This section works only in `nuget.config` files in a solution folder.

Key | Value
--- | ---
disableSourceControlIntegration | A Boolean indicating whether to ignore the packages folder when working with source control. The default value is false.


**Example**:

```xml
    <solution>
      <add key="disableSourceControlIntegration" value="true" />
    </solution>
```


## Package source sections

The `packageSources`, `packageSourceCredentials`, `apikeys`, `activePackageSource`, and `disabledPackageSources` all work together to configure how NuGet works with package repositories during install, restore, and update operations.

The [`nuget sources` command](../tools/nuget-exe-cli-reference.md#sources) is generally used to manage these settings, except for `apikeys` which is managed using the [`nuget setapikey` command](../tools/nuget-exe-cli-reference.md#setapikey).

### packageSources

Lists all known package sources.

Key | Value
--- | ---
(name to assign to the package source) | The path or URL of the package source.

**Example**:

```xml
    <packageSources>
        <add key="nuget.org" value="https://api.nuget.org/v3/index.json" protocolVersion="3" />
        <add key="Contoso Package Source" value="https://contoso.com/packages/" />
        <add key="Test source" value="c:\packages" />
    </packageSources>
```


### packageSourceCredentials

Stores usernames and passwords for sources, typically specified with the `-username` and `-password` switches with `nuget sources`. Passwords are encrypted by default unless the `-storepasswordincleartext` option is also used.

Key | Value
--- | ---
username | The user name for the source in plain text.
password | The encrypted password for the source.
cleartextpassword | The unencrypted password for the source.

**Example:**

In the config file, the &lt;packageSourceCredentials&gt; element will contain child nodes for each applicable source name. That is, for a source named "Contoso", the config file will contain the following when using an encrypted password:

```xml
    <packageSourceCredentials>
      <Contoso>
        <add key="Username" value="user@contoso.com" />
        <add key="Password" value="..." />
      </Contoro>
    </packageSourceCredentials>
```

When using an unencrypted password:

```xml
    <packageSourceCredentials>
      <Contoso>
        <add key="Username" value="user@contoso.com" />
        <add key="ClearTextPassword" value="33f!!lloppa" />
      </Contoso>
    </packageSourceCredentials>
```

### apikeys

Stores keys for sources that use API key authentication, as set with the [`nuget setapikey` command](../tools/nuget-exe-cli-reference.md#setapikey).

Key | Value
--- | ---
(source URL) | The encrypted API key.

**Example**:

  ```xml
      <apikeys>
        <add key="https://MyRepo/ES/api/v2/package" value="encrypted_api_key" />
      </apikeys>
  ```


### disabledPackageSources

Identified currently disabled sources. May be empty.

Key | Value
--- | ---
(name of source) | A Boolean indicating whether the source is disabled.



**Example:**

```xml
    <disabledPackageSources>
        <add key="Contoso Package Source" value="true" />
    </disabledPackageSources>

    <!-- Empty list -->
    <disabledPackageSources />
```

### activePackageSource

*(2.x only; deprecated in 3.x+)*

Identifies to the currently active source or indicates the aggregate of all sources.

Key | Value
--- | ---
(name of source) or `All` | If key is the name of a source, the value is the source path or URL. If `All`, value should be `(Aggregate source)` to combine all package sources that are not otherwise disabled.

**Example**:

```xml
    <activePackageSource>
        <!-- Only one active source-->
        <add key="nuget.org" value="https://nuget.org/api/v2/" />

        <!-- All non-disabled sources are active -->
        <add key="All" value="(Aggregate source)" />
    </activePackageSource>
```


## Example config file

Below is an example `NuGet.Config` file that illustrates a number of settings:

```xml
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
```
