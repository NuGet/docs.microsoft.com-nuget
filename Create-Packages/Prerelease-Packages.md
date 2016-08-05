# Prerelease Versions

When most people install packages from NuGet, they want the latest “stable” release of that package. 
Other developers like to live life on the edge and want to grab the latest prerelease version of a package. 

NuGet supports the creation of prerelease packages by specifying a prerelease string in the version number according to the [Semantic Versioning (SemVer) specification v1.0.0](http://semver.org/spec/v1.0.0.html). For more information on our SemVer Support, refer to the topic [here](/ndocs/create-packages/semver-support).

<div class="block-callout-info">
    <strong>Support:</strong><br>
    This feature is only supported in NuGet 1.6+.
</div>

## Create

To create a prerelease package, simply give it a version that has a prerelease string. For more information around how to create packages, refer to the [Create a Package](/ndocs/create-packages/create-a-package) topic. 

There are two ways this can be accomplished.

### In .nuspec

Within the `.nuspec` file, specify the version in the version element

    <version>1.0.1-alpha</version>

### Assembly Attributes

If building a package from a project (.csproj or .vbproj), use the `AssemblyInformationalVersionAttribute` to specify the version.

    [assembly: AssemblyInformationalVersion("1.0.1-alpha")]

NuGet will pick up this value instead of the one specified in the `AssemblyVersion` attribute which does not support SemVer.

## Install

By default, NuGet does not display prerelease packages in the dialog or in the console. Enabling the "Include Prelease" option supports the display and installation of prerelease packages via _Manage NuGet Packages_ UI.

You can also use the _Package Manager Console_ and specify the `-IncludePrerelease` flag as follows.

    Install-Package CoolStuff -IncludePrerelease

This command includes prerelease packages in the set of packages considered. If a prerelease package is 
the latest version, then it is installed. If a stable package is the latest, it is installed. This flag 
does not force a prerelease package to be installed.

You can use the `-Pre` alias for installing prerelease packages.

    Install-Package CoolStuff -Pre

<div class="block-callout-info">
    <strong>Stable referencing prerelease:</strong><br>
    NuGet does not allow a stable package to reference a prerelease package. This avoids accidentally installing 
    a prerelease package when you're only interested in stable packages.
</div>

## Update

The `Update-Package` command will update all packages to the latest stable version. As before, use the 
`-IncludePrerelease` flag to upgrade to the latest stable or prelease package, whichever is latest.

    Update-Package CoolStuff -IncludePrerelease
