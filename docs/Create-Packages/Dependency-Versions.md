# Dependency Versions

When you [create a NuGet package](creating-a-package), you can specify dependencies for your package in the **&lt;dependencies&gt;** node of the `.nuspec` file, where each dependency is listed with a **&lt;dependency&gt;** tag: 
  
	<?xml version="1.0"?>
	<package xmlns="http://schemas.microsoft.com/packaging/2016/06/nuspec.xsd">
	  <metadata>
	    <!-- ... -->
        
		<dependencies>
		  <dependency id="Newtonsoft.Json" version="9.0" />
          <dependency id="EntityFramework" version="6.1.0" />
		</dependencies>
	  </metadata>
	</package>

Dependencies are installed whenever the dependent package is installed, [reinstalled](/ndocs/consume-packages/reinstalling-and-updating-packages), or included in a [package restore](/ndocs/consume-packages/package-restore). NuGet chooses the exact version of the installed dependency by using the value of the `version` attribute specified for that dependency as described in the following sections.  

- [Version ranges](#version-ranges)
- [Normalized version numbers](#normalized-version-numbers)
- [Dependency updates during package install](#dependency-updates-during-package-install).

<div class="block-callout-info">
	<strong>Best practice</strong><br>
	Developers most commonly specify a minimum version, which is a version number without adornmants as shown above, like 6.1.0. This allows NuGet to install that version or later.
</div>

For additional details on how dependencies are installed, see [Reinstalling and updating packages](/ndocs/consume-packages/reinstalling-and-updating-packages) and [Dependency resolution](/ndocs/consume-packages/dependency-resolution).


## Version ranges

NuGet supports using interval notation for specifying version ranges, summarized as follows:

<table>
	<tr>
		<th>Notation</th>
		<th>Applied rule</th>
		<th>Description</th>
	</tr>
	<tr>
		<td>1.0</td>
		<td>1.0 ≤ x</td>
		<td>Minimum version, inclusive</td>
	</tr>
	<tr>
		<td>(1.0,)</td>
		<td>1.0 < x</td>
		<td>Mininum version, exclusive</td>
	</tr>
	<tr>
		<td>[1.0]</td>
		<td>x == 1.0</td>
		<td>Exact version match</td>
	</tr>
	<tr>
		<td>(,1.0]</td>
		<td>x ≤ 1.0</td>
		<td>Maximum version, inclusive</td>
	</tr>
	<tr>
		<td>(,1.0)</td>
		<td>x < 1.0</td>
		<td>Maximum version, exclusive</td>
	</tr>
	<tr>
		<td>[1.0,2.0]</td>
		<td>1.0 ≤ x ≤ 2.0</td>
		<td>Exact range, inclusive</td>
	</tr>
	<tr>
		<td>(1.0,2.0)</td>
		<td>1.0 < x < 2.0</td>
		<td>Exact range, exclusive</td>
	</tr>
	<tr>
		<td>(1.0)</td>
		<td>invalid</td>
		<td>invalid</td>
	</tr>
</table>


A few examples:

    <!-- Accepts any version 6.1 and above -->    
    <dependency id="ExamplePackage" version="6.1" />

    <!-- Accepts any version above, but not include 4.1.3. This might be
         used to guarantee a dependency with a specific bug fix. -->    
    <dependency id="ExamplePackage" version="(4.1.3,)" />

    <!-- Accepts any version up below 5.x, which might be used to prevent
         pulling in a later version of a dependency that changed its interface. -->    
    <dependency id="ExamplePackage" version="(,5.0)" />

	<!-- Accepts any 1.x or 2.x version, but no 0.x or 3.x and higher versions -->    
    <dependency id="ExamplePackage" version="[1,3)" />

	<!-- Accepts 1.3.2 up to 1.4.x, but not 1.5 and higher. -->	
	<dependency id="ExamplePackage" version="[1.3.2,1.5)" />


If no version is specified for a dependency, NuGet behaves as follows:

- NuGet v2.7.2 and earlier: The **latest** package version will be used
- NuGet v2.8 and later:  The **lowest** package version will be used

For consistent behavior, it's recommended to always specify a version or version range for package dependencies.

(For the curious, the NuGet version notation is inspired by Maven Version Range Specification, but is not identical to it.)

## Normalized version numbers

<div class="block-callout-warning">    
    This is a breaking change for NuGet 3.4 and later.
</div>

When obtaining packages from a repository during install, reinstall, or restore operations, NuGet 3.4 and later will treat version numbers as follows:

- Leading zeroes are removed from version numbers:

	1.00 is treated as 1.0	
	1.01.1 is treated as 1.1.1
	1.00.0.1 is treated as 1.0.0.1

- A zero in the fourth part of the version number will be omitted
	
	1.0.0.0 is treated as 1.0.0
	1.0.01.0 is treated as 1.0.1 
 
This normalization does not affect the version numbers in the packages themselves; it affects only how NuGet matches versions.

However, NuGet repositories must treat these values in the same way as NuGet to prevent package version duplication. Thus a repository that contains v1.0 of a package should not also host v1.0.0 as a separate and different package.

## Dependency updates during package install

With NuGet 2.4.x and earlier, when a package is installed whose dependency already exists in the project, the dependency is updated to the latest version that satisfies the version constraints, even if the existing version also satisfies those constraints.

For example, consider package A that depends on package B and specifies 1.0 for the version number. The source repository contains both versions 1.0, 1.1, and 1.2 of package B. If A is installed in a project that already contains B version 1.0, then B is updated to version 1.2.  

With NuGet 2.5 and later, if a dependency version is already satisfied, the dependency will not be updated during other package installations.

In the same example above, installing package A into a project with NuGet 2.5 and later will leave package B 1.0 in the project, as it already satisfies the version constraint. However, if package A had requests version 1.1 or higher of B, then B 1.2 would be installed.
