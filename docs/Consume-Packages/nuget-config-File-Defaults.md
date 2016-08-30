# NuGet Config File Defaults

Many companies are using NuGet internally, but have had a hard time setting up a build server or guiding their developers to use internal package sources instead of nuget.org. In addition to the standard [NuGet Configuration File](nuget-config-file-overview.md), NuGet 2.7 introduces a Configuration Defaults feature that allows machine-wide defaults to be specified for:

a) Enabled package sources

b) Registered, but disabled-by-default package sources

c) The default nuget.exe push source

Each of these can now be configured within a file located at `%ProgramData%\NuGet\NuGetDefaults.config`. If this config file specifies package sources, then the default nuget.org package source will not be registered automatically, and the ones in NuGetDefaults.config will be registered instead.

While not required, we expect many companies to use Group Policy to deploy NuGetDefaults.config files to developers' machines.

<div class="block-callout-info">
    <strong>Note:</strong><br>
    This feature will never cause a package source to be removed from a developer's NuGet configuration. That means if the developer has already used NuGet and therefore has the nuget.org package source registered, it won't be removed after the creation of a NuGetDefaults.config file.
</div>

## Location

NuGetDefaults.config, is located under `%ProgramData%\NuGet` folder, which is machine-wide and typically requires Administrator permissions to modify. Administrators are expected to set the correct permissions on this file based on the user and/or machine information.

## Default Package Sources

NuGet has always had the notion of a default package source that the user could not delete. It has always been the nuget.org package source. Without any default package sources specified in the NuGet Config Defaults file, NuGet will continue to use nuget.org as the default package source. However, if the NuGet Config Defaults file specifies default package sources, those defaults will be used in place of nuget.org, and those package sources will be added to the users' settings when they use NuGet.

This feature essentially allows administrators to replace the default nuget.org package source with their own package source(s).

## Enabled Package Sources

Within the NuGet Config Defaults, default package sources can be specified as either enabled or disabled by default. The simplest scenario is to specify the package sources as enabled by default. These package sources will be added to each user's NuGet configuration as enabled package sources the next time NuGet is used.

## Registered, but Disabled-by-Default Package Sources

The NuGet Config Defaults file also allows default package sources to be added as disabled. In the scenario where the user has not previously registered one of these package sources, it will be added to the user's configuration and marked a disabled. The user will then be able to easily enable this package source if needed.

There are various scenarios where this will be useful, but one example is when a company wants to register their internal package source as the only enabled package source by default, but allow developers to easily re-enable the nuget.org package source without having to hunt on the internet for its URL. 

Another scenario is when a company has multiple internal package sources matching their organization hierarchy, and by default the developer's team package source should be enabled, but other team's package sources should be available when needed.

## Default NuGet.exe Push Source

The NuGet Config Defaults file also allows specification of the Default Push Source. This will be the default push source if one is not provided in the command line argument of nuget.exe. Without a default push source specified in the NuGet Config Defaults file, nuget.org is the default nuget.exe push source. Using the new configuration though, administrators can change the default push source to an internal package soure. This will help prevent accidental publishing of packages to nuget.org.

## Sample NuGetDefaults.config

	<?xml version="1.0" encoding="UTF-8"?>
	<configuration>
		<!-- DefaultPushSource key is similar to the 'DefaultPushSource' key of NuGet.config schema-->
		<!-- This can be used by administrators to prevent accidental publishing of packages to nuget.org -->
		<config>
			<add key="DefaultPushSource" value="https://contoso.com/packages/" />
		</config>
	
		<!-- Default Package Sources -->
		<!-- They cannot be deleted or modified but can be disabled/enabled by user -->
		<!-- The following section is similar to 'packageSources' section of NuGet.config schema -->
		<packageSources>
			<add key="Contoso Package Source" value="https://contoso.com/packages/" />
			<add key="nuget.org" value="https://www.nuget.org/api/v2/" />
		</packageSources>

		<!-- Default Package Sources that are Disabled by Default -->
		<!-- They cannot be modified or deleted either but can be enabled/disabled by user -->
		<!-- The following section is similar to 'disabledPackageSources' section of NuGet.config schema -->
		<!-- The value part of the entry in 'disabledPackageSources' section has no effect -->
		<disabledPackageSources>
			<add key="nuget.org" value="true" />
		</disabledPackageSources>
	</configuration>

## Preventing Access to nuget.org

The NuGet Config Defaults feature cannot prevent access to nuget.org or any other package sources. Rather, this feature provides a mechanism for users' default NuGet package sources to be configured automatically, simplifying developers' on-ramp to using NuGet internally.

NuGet does not (and will not) provide any features to prevent access to a package source as it would be easy for a user to sidestep NuGet's tooling and download packages through the browser. Therefore to prevent access to nuget.org, companies must employ other techniques.
