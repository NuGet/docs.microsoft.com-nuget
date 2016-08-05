# Dependency Versions
When you create a NuGet package, you can specify dependencies for your package in the `.nuspec` file, as well as the versions required for the dependency packages. For information about creating NuGet packages and the .nuspec file format, see [Creating a Package](create-a-package) and [.nuspec file schema Specification](/ndocs/schema/nuspec). Dependency versions are specified in the version attribute of the dependency element.

    <dependency id="ExamplePackage" version="1.3.2" />

## Version ranges in .nuspec files

NuGet supports using interval notation for specifying version ranges. The NuGet specification was 
inspired by the Maven Version Range Specification but is not identical to it. The following summarizes 
how to specify version ranges.

    1.0	 = 1.0 ≤ x
    (,1.0]	= x ≤ 1.0
    (,1.0)	= x < 1.0
    [1.0] = x == 1.0
    (1.0) = invalid
    (1.0,) = 1.0 < x
    (1.0,2.0) = 1.0 < x < 2.0
    [1.0,2.0] = 1.0 ≤ x ≤ 2.0

When you fail to specify a package version for a dependency as follows:

    <dependency id="ExamplePackage" />

NuGet will perform the following actions:

    NuGet v2.7.2 and earlier - the latest package version will be used
    NuGet v2.8 and greater - the lowest package version will be used

It is recommended to always specify a version or version range for package dependencies.

## Versioning Examples
The following example specifies a dependency on any version of ExamplePackage that begins with a 1 or a 2. 
The square bracket indicates that the 1 is included, while the parenthesis indicates that 3 is excluded.

    <dependency id="ExamplePackage" version="[1,3)" />

In the example above, version 1 and version 2.9 would be acceptable, but not 0.9 or 3.0.

The following example specifies a dependency on ExamplePackage 1.3.2 through any version number that 
begins with 1.4. The square bracket indicates that the 1.3.2 is included, while the parenthesis 
indicates that 1.5 is excluded.

    <dependency id="ExamplePackage" version="[1.3.2,1.5)" />

In the example above, version 1.3.2.1 and version 1.4.999 would be acceptable, but not version 1.5.

## Guidance
Generally, the guidance is to only specify a lower bound, and leave the upper bound open, such as the following example:

    <dependency id="ExamplePackage" version="1.3.2" />
## Normalized Version Numbers
<div class="block-callout-warning">
    <strong>Breaking change</strong><br>
    This is a breaking change from NuGet clients prior to 3.4.
</div>

While querying a repository, NuGet clients recognize that there are as many as four numbers in a version that are separated by periods and will normalize those numbers by removing leading zeroes. Additionally, the clients will omit the fourth part of a version when the value is zero.  

This version parsing is executed when installing a package, updating a package, or restoring packages based on version numbers in packages.config or project.json.  However, the NuGet clients will not modify the version value stored in a nuspec and that value will be normalized for the purpose of supporting a uniform query.

<div class="block-callout-info">
    <strong>Note:</strong><br>
     NuGet repositories must treat these values in the same way as the NuGet client to prevent package version duplication.
     <br/>
     <br/>
     For example, repository A that contains v1.0 of a package should not also host v1.0.0 as a separate and different package.
</div>

The following table illustrates how NuGet will normalize these version numbers and the expected response from the NuGet repository:

<table class="reference">
<thead>
<tr>
<th>User input</th><th>Id in nuspec</th><th>Request</th><th>Response</th>
</tr>
<tr>
<td>1.0</td><td>1.0.0.0</td><td>1.0.0</td><td>Value in nuspec</td>
</tr>
<tr>
<td>1.00</td><td>1.0</td><td>1.0.0</td><td>Value in nuspec</td>
</tr>
<tr>
<td>1.00.0.1</td><td>1.0.0.1</td><td>1.0.0.1</td><td>Value in nuspec</td>
</tr>
<tr>
<td>1.0.01.0</td><td>1.0.01</td><td>1.0.1</td><td>Value in nuspec</td>
</tr>
</table>

## Constraining Upgrades To Allowed Versions
By default, when running the `Update-Package` command on a package (or updating the package using the UI), 
it will be updated to the latest version in the feed. With the new support for updating all packages, there 
may be cases in which you want to lock a package to a specific version range. 

For example, you may know in advance that your application will only work with version 2.* of a package, but not 3.0 and above. In order to prevent accidentally updating the package to 3, NuGet supports constraining the range of versions that packages can be upgraded to by hand editing the `packages.config` file using the `allowedVersions` attribute.

For example, the following example shows how to lock the version range to 2.0 - 3.0 
(exclusive). The `allowedVersions` attribute accepts values using the version range format described 
above.

    <?xml version="1.0" encoding="utf-8"?>
    <packages>
        <package id="SomePackage" version="2.1.0" allowedVersions="[2,3)" />
    </packages>

Now if you restore packages:
<code class="bash hljs">
PM> Enable-PackageRestore
Attempting to resolve dependency 'NuGet.CommandLine (≥ 1.4)'.<br>
Successfully installed 'NuGet.CommandLine 1.4.20615.182'.<br>
Successfully installed 'NuGet.Build 0.16'.<br><br>
Copying nuget.exe and msbuild scripts to D:\Code\StarterApps\Mvc3Application\.nuget<br>
Successfully uninstalled 'NuGet.Build 0.16'.<br>
Successfully uninstalled 'NuGet.CommandLine 1.4.20615.182'.<br><br>
Don't forget to commit the .nuget folder<br>
Updated 'Mvc3Application' to use 'NuGet.targets'<br>
Enabled package restore for Mvc3Application<br>
</code>

And you’re done! So basically, the first command installs a NuGet package which brings in some helpful commands, and the second one runs one of those commands.

After doing this, you’ll notice a new .nuget folder under your solution, containing nuget.exe plus a couple msbuild target files. Make sure you commit that folder to source control! You’ll also find a few changes in your csproj files to trigger the restore functionality when you build.

## Updates to dependencies during install

Before NuGet 2.5, when a package was installed that depended on a package already installed in the project, the dependency would be updated as part of the new installation, even if the existing version satisfied the dependency.

Starting with NuGet 2.5, if a dependency version is already satisifed, the dependency will not be updated during other package installations.

### Example

The source repository contains package B with version 1.0.0 and 1.0.2. It also contains package A which has a dependency on B (>= 1.0.0). Assume that the current project already has package B version 1.0.0 installed. Now you want to install package A. 

<div class="block-callout-info">
    <strong>NuGet 2.5+</strong><br>
    NuGet will no longer update B, because it detects that the existing version 1.0.0 satisfies the dependency version constraint.
</div>

<div class="block-callout-info">
    <strong>NuGet 2.2 and below</strong><br>
    When installing package A, NuGet will auto-update B to 1.0.2, even though the existing version 1.0.0 already satisfies the dependency version constraint, which is >= 1.0.0.
</div>
