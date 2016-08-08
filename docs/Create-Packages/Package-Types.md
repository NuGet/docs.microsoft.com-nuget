# Package Types

<div class="block-callout-warning">
This feature is available in NuGet version 3.5 and later.
</div>

Packages can be marked with a package type which indicates how a package is intended to be used.

## Well known package types

There are well known package types that the NuGet client knows how to interact with.

### Dependency

`Dependency` type packages are used by libraries or applications for acquiring build-time or run-time assets necessary
for functioning properly. All packages authored prior to the concept of package type are not marked with any package
type at all and are therefore assumed to be `Dependency` packages.

### DotnetCliTool

`DotnetCliTool` type packages are extensions to the [.NET CLI](https://docs.microsoft.com/en-us/dotnet/articles/core/tools/index)
and are invoked via the command line. More details about these per-project extensions are available in the 
[.NET Core extensibility documention](https://docs.microsoft.com/en-us/dotnet/articles/core/tools/extensibility#per-project-based-extensibility).

When a package is marked as a `DotnetCliTool`, installation from NuGet in Visual Studio will the package in the
project.json `"tools"` node, instead of the `"dependencies"` node.

## Setting a package type

Package types are specified when [creating a package](create-a-package). If no package type
is set, the produced .nupkg is marked with no package type.

It is possible but cautioned to explicitly specify the `Dependency` package type as older clients do not recognize
package types. A more backwards-compatible approach is to specify no package type at all and depend on the fact the
the absense of a package type means the package is a `Dependency`. This is the legacy behavior.

There are two ways to set a package type.

### project.json

A package type can be set in the `"packOptions"` node of the project.json. When NuGet's pack operation is executed on
the project.json, the resulting .nupkg will have a package type set in the metadata.

	{
	  ...
	  "packOptions": {
		"packageType": "DotnetCliTool"
	  }
	}

### .nuspec

A package type can be set under the `<metadata>` element of a .nuspec along with all of the other supported elements.
When NuGet's pack operation is executed on the .nuspec, the resulting .nupkg will have a package type set in the
metadata.

	<?xml version="1.0" encoding="utf-8"?>
	<package xmlns="http://schemas.microsoft.com/packaging/2012/06/nuspec.xsd">
	  <metadata>
		...
		<packageTypes>
		  <packageType type="DotnetCliTool" />
		</packageTypes>
	  </metadata>
	</package>

## Effects on install and restore

When using the NuGet Visual Studio extension, `DotnetCliTool` packages can only be installed to .NET Core projects.
`Dependency` type projects can be installed to any project type (given all other compatibility checks pass).

Package types have no effect on the restore operation.

## Custom package types

As long as a package type identifier conforms to the same format rules as package IDs, package authors may specify
arbitrary package types. However, only `Dependency` and `DotnetCliTool` package types are specially recognized by the
NuGet installation experience in Visual Studio.
