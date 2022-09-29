---
title: nuget.config File Reference
description: NuGet.Config file reference including the config, bindingRedirects, packageRestore, solution, and packageSource sections.
author: JonDouglas
ms.author: jodou
ms.date: 08/13/2019
ms.topic: reference
---

# `nuget.config` reference

NuGet behavior is controlled by settings in different `NuGet.Config` or `nuget.config` files as described in [Common NuGet configurations](../consume-packages/configuring-nuget-behavior.md).

`nuget.config` is an XML file containing a top-level `<configuration>` node, which then contains the section elements described in this topic. Each section contains zero or more items. See the [examples config file](#example-config-file). Setting names are case-insensitive, and values can use [environment variables](#using-environment-variables).

<a name="dependencyVersion"></a>
<a name="globalPackagesFolder"></a>
<a name="repositoryPath"></a>
<a name="proxy-settings"></a>

> [!Tip]
> Add a `nuget.config` file in the root of your project repository. This is considered a best practice as it promotes repeatability and ensures that different users have the same NuGet configuration.
> You may need to configure `clear` elements to ensure no user or machine specific configuration is applied. [Read more about how settings are applied](../consume-packages/configuring-nuget-behavior.md#how-settings-are-applied).

## config section

Contains miscellaneous configuration settings, which can be set using the [`nuget config` command](../reference/cli-reference/cli-ref-config.md).

`dependencyVersion` and `repositoryPath` apply only to projects using `packages.config`. `globalPackagesFolder` applies only to projects using the PackageReference format.

| Key | Value |
| --- | --- |
| dependencyVersion (`packages.config` only) | The default `DependencyVersion` value for package install, restore, and update, when the `-DependencyVersion` switch is not specified directly. This value is also used by the NuGet Package Manager UI. Values are `Lowest`, `HighestPatch`, `HighestMinor`, `Highest`. |
| globalPackagesFolder (projects using PackageReference only) | The location of the default global packages folder. The default is `%userprofile%\.nuget\packages` (Windows) or `~/.nuget/packages` (Mac/Linux). A relative path can be used in project-specific `nuget.config` files. This setting is overridden by the `NUGET_PACKAGES` environment variable, which takes precedence. |
| repositoryPath (`packages.config` only) | The location in which to install NuGet packages instead of the default `$(Solutiondir)/packages` folder. A relative path can be used in project-specific `nuget.config` files. This setting is overridden by the `NUGET_PACKAGES` environment variable, which takes precedence. |
| defaultPushSource | Identifies the URL or path of the package source that should be used as the default if no other package sources are found for an operation. |
| http_proxy http_proxy.user http_proxy.password no_proxy | Proxy settings to use when connecting to package sources; `http_proxy` should be in the format `http://<username>:<password>@<domain>`. Passwords are encrypted and cannot be added manually. For `no_proxy`, the value is a comma-separated list of domains the bypass the proxy server. You can alternately use the http_proxy and no_proxy environment variables for those values. For additional details, see [NuGet proxy settings](http://skolima.blogspot.com/2012/07/nuget-proxy-settings.html) (skolima.blogspot.com). |
| signatureValidationMode | Specifies the validation mode used to verify package signatures for package install, and restore. Values are `accept`, `require`. Defaults to `accept`.

**Example**:

```xml
<config>
    <add key="dependencyVersion" value="Highest" />
    <add key="globalPackagesFolder" value="c:\packages" />
    <add key="repositoryPath" value="c:\installed_packages" />
    <add key="http_proxy" value="http://company-squid:3128@contoso.com" />
    <add key="signatureValidationMode" value="require" />
</config>
```

## bindingRedirects section

Configures whether NuGet does automatic binding redirects when a package is installed.

| Key | Value |
| --- | --- |
| skip | A Boolean indicating whether to skip automatic binding redirects. The default is false. |

**Example**:

```xml
<bindingRedirects>
    <add key="skip" value="True" />
</bindingRedirects>
```

## packageRestore section

Controls package restore during builds.

| Key | Value |
| --- | --- |
| enabled | A Boolean indicating whether NuGet can perform automatic restore. You can also set the `EnableNuGetPackageRestore` environment variable with a value of `True` instead of setting this key in the config file. |
| automatic | A Boolean indicating whether NuGet should check for missing packages during a build. |

**Example**:

```xml
<packageRestore>
    <add key="enabled" value="true" />
    <add key="automatic" value="true" />
</packageRestore>
```

## solution section

Controls whether the `packages` folder of a solution is included in source control. This section works only in `nuget.config` files in a solution folder.

| Key | Value |
| --- | --- |
| disableSourceControlIntegration | A Boolean indicating whether to ignore the packages folder when working with source control. The default value is false. |

**Example**:

```xml
<solution>
    <add key="disableSourceControlIntegration" value="true" />
</solution>
```

## Package source sections

The `packageSources`, `packageSourceCredentials`, `apikeys`, `activePackageSource`, `disabledPackageSources`, `trustedSigners` and `packageSourceMapping` all work together to configure how NuGet works with package repositories during install, restore, and update operations.

The [`nuget sources` command](../reference/cli-reference/cli-ref-sources.md) is generally used to manage these settings, except for `apikeys` which is managed using the [`nuget setapikey` command](../reference/cli-reference/cli-ref-setapikey.md), and `trustedSigners` which is managed using the [`nuget trusted-signers` command](../reference/cli-reference/cli-ref-trusted-signers.md).

Note that the source URL for nuget.org is `https://api.nuget.org/v3/index.json`.

### packageSources

Lists all known package sources. The order is ignored during restore operations and with any project using the PackageReference format. NuGet respects the order of sources for install and update operations with projects using `packages.config`.

| Key | Value |
| --- | --- |
| (name to assign to the package source) | The path or URL of the package source. |

**Example**:

```xml
<packageSources>
    <add key="nuget.org" value="https://api.nuget.org/v3/index.json" protocolVersion="3" />
    <add key="Contoso" value="https://contoso.com/packages/" />
    <add key="Test Source" value="c:\packages" />
</packageSources>
```

> [!NOTE]
> When using the CLI, you can express a [`RestoreSources`](../reference/msbuild-targets.md#restore-properties) MSBuild property or [`--source`(.NET CLI)](/dotnet/core/tools/dotnet-restore#options) | [`-Source`(NuGet CLI)](/nuget/reference/cli-reference/cli-ref-restore#options) to override the `<packageSources>` defined in the NuGet.config.

> [!Tip]
> When `<clear />` is present for a given node, NuGet ignores previously defined configuration values for that node. [Read more about how settings are applied](../consume-packages/configuring-nuget-behavior.md#how-settings-are-applied).

### packageSourceCredentials

Stores usernames and passwords for sources, typically specified with the `-username` and `-password` switches with `nuget sources`. Passwords are encrypted by default unless the `-storepasswordincleartext` option is also used.
Optionally, valid authentication types can be specified with the `-validauthenticationtypes` switch.

| Key | Value |
| --- | --- |
| username | The user name for the source in plain text. |
| password | The encrypted password for the source. Encrypted passwords are only supported on Windows, and only can be decrypted when used on the same machine and via the same user as the original encryption. |
| cleartextpassword | The unencrypted password for the source. Note: environment variables can be used for improved security. |
| validauthenticationtypes | Comma-separated list of valid authentication types for this source. Set this to `basic` if the server advertises NTLM or Negotiate and your credentials must be sent using the Basic mechanism, for instance when using a PAT with on-premises Azure DevOps Server. Other valid values include `negotiate`, `kerberos`, `ntlm`, and `digest`, but these values are unlikely to be useful. |

**Example:**

In the config file, the `<packageSourceCredentials>` element contains child nodes for each applicable source name (spaces in the name are replaced with `_x0020_`). That is, for sources named "Contoso" and "Test Source", the config file contains the following when using encrypted passwords:

```xml
<packageSourceCredentials>
    <Contoso>
        <add key="Username" value="user@contoso.com" />
        <add key="Password" value="..." />
    </Contoso>
    <Test_x0020_Source>
        <add key="Username" value="user" />
        <add key="Password" value="..." />
    </Test_x0020_Source>
</packageSourceCredentials>
```

When using unencrypted passwords stored in an environment variable:

```xml
<packageSourceCredentials>
    <Contoso>
        <add key="Username" value="user@contoso.com" />
        <add key="ClearTextPassword" value="%ContosoPassword%" />
    </Contoso>
    <Test_x0020_Source>
        <add key="Username" value="user" />
        <add key="ClearTextPassword" value="%TestSourcePassword%" />
    </Test_x0020_Source>
</packageSourceCredentials>
```

When using unencrypted passwords:

```xml
<packageSourceCredentials>
    <Contoso>
        <add key="Username" value="user@contoso.com" />
        <add key="ClearTextPassword" value="33f!!lloppa" />
    </Contoso>
    <Test_x0020_Source>
        <add key="Username" value="user" />
        <add key="ClearTextPassword" value="hal+9ooo_da!sY" />
    </Test_x0020_Source>
</packageSourceCredentials>
```

Additionally, valid authentication methods can be supplied:

```xml
<packageSourceCredentials>
    <Contoso>
        <add key="Username" value="user@contoso.com" />
        <add key="Password" value="..." />
        <add key="ValidAuthenticationTypes" value="basic" />
    </Contoso>
    <Test_x0020_Source>
        <add key="Username" value="user" />
        <add key="ClearTextPassword" value="hal+9ooo_da!sY" />
        <add key="ValidAuthenticationTypes" value="basic, negotiate" />
    </Test_x0020_Source>
</packageSourceCredentials>
```

### apikeys

Stores keys for sources that use API key authentication, as set with the [`nuget setapikey` command](../reference/cli-reference/cli-ref-setapikey.md).

| Key | Value |
| --- | --- |
| (source URL) | The encrypted API key. |

**Example**:

```xml
<apikeys>
    <add key="https://MyRepo/ES/api/v2/package" value="encrypted_api_key" />
</apikeys>
```

### disabledPackageSources

Identified currently disabled sources. May be empty. Unless specific sources are disabled in this section, they are enabled.

| Key | Value |
| --- | --- |
| (name of source) | A Boolean indicating whether the source is disabled. |

**Example:**

```xml
<disabledPackageSources>
    <add key="Contoso" value="true" />
</disabledPackageSources>

<!-- Empty list -->
<disabledPackageSources />
```

In the example above, the package source `Contoso` is disabled and will not be used to download or install packages.

### activePackageSource

*(2.x only; deprecated in 3.x+)*

Identifies to the currently active source or indicates the aggregate of all sources.

| Key | Value |
| --- | --- |
| (name of source) or `All` | If key is the name of a source, the value is the source path or URL. If `All`, value should be `(Aggregate source)` to combine all package sources that are not otherwise disabled. |

**Example**:

```xml
<activePackageSource>
    <!-- Only one active source-->
    <add key="nuget.org" value="https://api.nuget.org/v3/index.json" />

    <!-- All non-disabled sources are active -->
    <add key="All" value="(Aggregate source)" />
</activePackageSource>
```

## trustedSigners section

Stores trusted signers used to allow package while installing or restoring. This list cannot be empty when the user sets `signatureValidationMode` to `require`. 

This section can be updated with the [`nuget trusted-signers` command](../reference/cli-reference/cli-ref-trusted-signers.md).

**Schema**:

A trusted signer has a collection of `certificate` items that enlist all the certificates that identify a given signer. A trusted signer can be either an `Author` or a `Repository`.

A trusted *repository* also specifies the `serviceIndex` for the repository (which has to be a valid `https` uri) and can optionally specify a semi-colon delimited list of `owners` to restrict even more who is trusted from that specific repository.

The supported hash algorithms used for a certificate fingerprint are `SHA256`, `SHA384` and `SHA512`.

If a `certificate` specifies `allowUntrustedRoot` as `true` the given certificate is allowed to chain to an untrusted root while building the certificate chain as part of the signature verification.

**Example**:

```xml
<trustedSigners>
    <author name="microsoft">
        <certificate fingerprint="3F9001EA83C560D712C24CF213C3D312CB3BFF51EE89435D3430BD06B5D0EECE" hashAlgorithm="SHA256" allowUntrustedRoot="false" />
        <certificate fingerprint="AA12DA22A49BCE7D5C1AE64CC1F3D892F150DA76140F210ABD2CBFFCA2C18A27" hashAlgorithm="SHA256" allowUntrustedRoot="false" />
    </author>
    <repository name="nuget.org" serviceIndex="https://api.nuget.org/v3/index.json">
        <certificate fingerprint="0E5F38F57DC1BCC806D8494F4F90FBCEDD988B46760709CBEEC6F4219AA6157D" hashAlgorithm="SHA256" allowUntrustedRoot="false" />
        <certificate fingerprint="5A2901D6ADA3D18260B9C6DFE2133C95D74B9EEF6AE0E5DC334C8454D1477DF4" hashAlgorithm="SHA256" allowUntrustedRoot="false" />
        <owners>microsoft;aspnet;nuget</owners>
    </repository>
</trustedSigners>
```

## fallbackPackageFolders section

*(3.5+)* Provides a way to preinstall packages so that no work needs to be done if the package is found in the fallback folders. Fallback package folders have the exact same folder and file structure as the global package folder: *.nupkg* is present, and all files are extracted.

The lookup logic for this configuration is:

- Look in global package folder to see if the package/version is already downloaded.

- Look in the fallback folders for a package/version match.

If either lookup is successful, then no download is necessary.

If a match is not found, then NuGet checks file sources, and then http sources, and then it downloads the packages.

| Key | Value |
| --- | --- |
| (name of fallback folder) | Path to fallback folder. |

**Example**:

```xml
<fallbackPackageFolders>
   <add key="XYZ Offline Packages" value="C:\somePath\someFolder\"/>
</fallbackPackageFolders>
```

## Package source mapping section

The `packageSourceMapping` section contains the details that help the NuGet package operations determine where a package id should be downloaded from.

This section can only be managed manually right now.

A `packageSourceMapping` section can only contain `packageSource` sections.

### packageSource

A sub section of the [`packageSourceMapping`](#package-source-mapping-section) section. Contains a mapping to help NuGet determine whether the source should be considered for downloading the package of interest.

| Key |
| --- |
| Name of a package source declared in the [`packageSources`](#packagesources) section. The key must exactly match the the key of the package source. |

The `packageSource` sections under `packageSourceMapping` are uniquely identified by the `key`.

### package

The `package` is part of the [`packageSource`](#packagesource) section.

| Pattern |
| --- |
| A pattern as defined by the [syntax](../consume-packages/package-source-mapping.md) of Package Source mapping. |

**Example**:

```xml
<packageSourceMapping>
  <packageSource key="contoso.com">
    <package pattern="Contoso.*" />
  </packageSource>
</packageSourceMapping>
```

## packageManagement section

Sets the default package management format, either *packages.config* or PackageReference. SDK-style projects always use PackageReference.

| Key | Value |
| --- | --- |
| format | A Boolean indicating the default package management format. If `1`, format is PackageReference. If `0`, format is *packages.config*. |
| disabled | A Boolean indicating whether to show the prompt to select a default package format on first package install. `False` hides the prompt. |

**Example**:

```xml
<packageManagement>
   <add key="format" value="1" />
   <add key="disabled" value="False" />
</packageManagement>
```

> [!Tip]
> When `<clear />` is present for a given node, NuGet ignores previously defined configuration values for that node. [Read more about how settings are applied](../consume-packages/configuring-nuget-behavior.md#how-settings-are-applied).

## Using environment variables

You can use environment variables in `nuget.config` values (NuGet 3.4+) to apply settings at run time.

For example, if the `HOME` environment variable on Windows is set to `c:\users\username`, then the value of `%HOME%\NuGetRepository` in the configuration file resolves to `c:\users\username\NuGetRepository`.

Note that you have to use Windows-style environment variables (starts and ends with %) even on Mac/Linux. Having `$HOME/NuGetRepository` in a configuration file will not resolve. On Mac/Linux the value of `%HOME%/NuGetRepository` will resolve to `/home/myStuff/NuGetRepository`.

If an environment variable is not found, NuGet uses the literal value from the configuration file. For example `%MY_UNDEFINED_VAR%/NuGetRepository` will be resolved as `path/to/current_working_dir/$MY_UNDEFINED_VAR/NuGetRepository`

The table below show environnment variable syntax and path separator support for NuGet.Config files.

### `NuGet.Config` environment variable support

| Syntax | Dir separator | Windows nuget.exe | Windows dotnet.exe | Mac nuget.exe (in Mono) | Mac dotnet.exe |
|---|---|---|---|---|---|
| `%MY_VAR%` | `/`  | Yes | Yes | Yes | Yes |
| `%MY_VAR%` | `\`  | Yes | Yes | No | No |
| `$MY_VAR` | `/`  | No | No | No | No |
| `$MY_VAR` | `\`  | No | No | No | No |


## Example config file

Below is an example `nuget.config` file that illustrates a number of settings including optional ones:

```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
    <config>
        <!--
            Used to specify the default location to expand packages.
            See: nuget.exe help install
            See: nuget.exe help update

            In this example, %PACKAGEHOME% is an environment variable.
            This syntax works on Windows/Mac/Linux
        -->
        <add key="repositoryPath" value="%PACKAGEHOME%/External" />

        <!--
            Used to specify default source for the push command.
            See: nuget.exe help push
        -->

        <add key="defaultPushSource" value="https://MyRepo/ES/api/v2/package" />

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
        See: nuget.exe help list
        See: nuget.exe help install
        See: nuget.exe help update
    -->
    <packageSources>
        <add key="NuGet official package source" value="https://api.nuget.org/v3/index.json" />
        <add key="MyRepo - ES" value="https://MyRepo/ES/nuget" />
    </packageSources>

    <!-- Used to store credentials -->
    <packageSourceCredentials />

    <!-- Used to disable package sources  -->
    <disabledPackageSources />

    <!--
        Used to specify default API key associated with sources.
        See: nuget.exe help setApiKey
        See: nuget.exe help push
        See: nuget.exe help mirror
    -->
    <apikeys>
        <add key="https://MyRepo/ES/api/v2/package" value="encrypted_api_key" />
    </apikeys>

    <!--
        Used to specify trusted signers to allow during signature verification.
        See: nuget.exe help trusted-signers
    -->
    <trustedSigners>
        <author name="microsoft">
            <certificate fingerprint="3F9001EA83C560D712C24CF213C3D312CB3BFF51EE89435D3430BD06B5D0EECE" hashAlgorithm="SHA256" allowUntrustedRoot="false" />
            <certificate fingerprint="AA12DA22A49BCE7D5C1AE64CC1F3D892F150DA76140F210ABD2CBFFCA2C18A27" hashAlgorithm="SHA256" allowUntrustedRoot="false" />
        </author>
        <repository name="nuget.org" serviceIndex="https://api.nuget.org/v3/index.json">
            <certificate fingerprint="0E5F38F57DC1BCC806D8494F4F90FBCEDD988B46760709CBEEC6F4219AA6157D" hashAlgorithm="SHA256" allowUntrustedRoot="false" />
            <certificate fingerprint="5A2901D6ADA3D18260B9C6DFE2133C95D74B9EEF6AE0E5DC334C8454D1477DF4" hashAlgorithm="SHA256" allowUntrustedRoot="false" />
            <owners>microsoft;aspnet;nuget</owners>
        </repository>
    </trustedSigners>
</configuration>
```
