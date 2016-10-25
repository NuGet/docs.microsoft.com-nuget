# Pre-release Versions

Whenever you release an updated package with a new version number, NuGet considers that one as the "latest stable release" as shown, for example in the Package Manager UI within Visual Studio:

![Package Manager UI showing the latest stable release](/images/Create/Prerelease_01-LatestStable.png)

A stable release is one that's considered reliable enough to be used in production. The latest stable release is also the one that will be installed as a package update or during package restore (subject to constraints as described in [Reinstalling and updating packages](/ndocs/consume-packages/reinstalling-and-updating-packages)).

To support the software release lifecycle, NuGet 1.6 and later allows for the distribution of pre-release packages, where the version number includes a semantic versioning suffix such as `-alpha`, `-beta`, or `-rc` (as described in [semantic versioning](#semantic-versioning) later in this topic).

You can specify such versions in two ways:

- `.nuspec` file: include the semantic version suffix in the `version` element: 

	    <version>1.0.1-alpha</version>

- Assembly attributes: when building a package from a Visual Studio project (`.csproj` or `.vbproj`), use the `AssemblyInformationalVersionAttribute` to specify the version:

    	[assembly: AssemblyInformationalVersion("1.0.1-beta")]

	NuGet will pick up this value instead of the one specified in the `AssemblyVersion` attribute, which does not support SemVer.

<div class="block-callout-info">
    <strong>Note</strong><br>
    A stable package release cannot have a pre-release dependency. This avoids accidentally installing a potentially unstable releases.
</div>

When you’re ready to release a stable version, just remove the suffix and the package will take precedence over any pre-release versions.


## Installing and updating pre-release packages

By default, NuGet does not include pre-release versions when working with packages, but you can change this behavior as follows:

- **Package Manager UI in Visual Studio**: In the **Manage NuGet Packages** UI, check the **Include prerelease** box:

	![The Include prerelese checkbox in Visual Studio](/images/Create/Prerelease_02-CheckPrerelease.png)

	Checking or unchecking this box will refresh the Package Manager UI and the list of available versions you can install.

- **Package Manager Console**: Use the `-IncludePrerelease` switch with the `Find-Package`, `Get-Package`, `Install-Package`, `Sync-Package`, and `Update-Package` commands. Refer to the [PowerShell Reference](/ndocs/tools/powershell-reference).

- **NuGet CLI**: Use the `-prerelease` switch with the `install`, `update`, `delete`, and `mirror` commands. Refer to the [NuGet CLI reference](/ndocs/tools/nuget.exe-cli-reference) 


## Semantic versioning

The [Semantic Versioning or SemVer convention](http://semver.org/spec/v1.0.0.html) describes how to utilize strings in version numbers to convey they meaning of the underlying code.

In this convention, each version has three parts, `Major.Minor.Patch`, with the following meaning:

* `Major`: Breaking changes
* `Minor`: New features, but backwards compatible
* `Patch`: Backwards compatible bug fixes only

Pre-release versions are then denoted by appending a hyphen and a string after the patch number. Technically speaking, you can use *any *string after the hyphen and NuGet will treat the package as pre-release. NuGet then displays the full version number in the applicable UI, leaving consumers to interpret the meaning for themselves.

With this in mind, it's generally good to follow recognized naming conventions such as the following:

- `-alpha`: Alpha release, typically used for work-in-progress and experimentation
- `-beta`: Beta release, typically one that is feature complete for the next planned release, but may contain known bugs. 
- `-rc`: Release candidate, typically a release that's potentially final (stable) unless significant bugs emerge.

<div class="block-callout-info">
    <strong>Note</strong><br>
    NuGet does not support SemVer-compatible build numbers with dot notation, as in <em>1.0.1-build.23</em>. You can use a form like <em>1.0.1-build23</em> but this is always considered a pre-release version.  
</div>

Whatever suffixes you use, however, NuGet will give them precedence in reverse alphabetical order: 

	1.0.1
	1.0.1-zzz 
	1.0.1-rc
    1.0.1-open
	1.0.1-beta
    1.0.1-alpha2
    1.0.1-alpha	 

As shown, the version without any suffix will always take precedence over pre-release versions.
