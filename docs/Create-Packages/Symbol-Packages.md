# Creating and Publishing a Symbol Package

Apart from building library/content packages and publishing them to [NuGet.org](http://nuget.org),
NuGet also supports creating symbol/source packages and publishing them to the new [SymbolSource.org service](https://tripleemcoder.com/2015/10/04/moving-to-the-new-symbolsource-engine/).
When a package is published to both repositories, Visual Studio can be configured to automatically download 
PDB files associated with installed packages and allow the developer to use a debugger to step into source 
files on-demand from Visual Studio. This is a built-in feature of the IDE, that can also be used to debug 
.NET Framework code using [Microsoft Reference Source](http://referencesource.microsoft.com/) servers. It is only required to add a new symbol source (given below) in the debugger [options](https://msdn.microsoft.com/en-us/library/x54fht41(v=vs.90).aspx).

	https://nuget.smbsrc.net

## Creating a Symbol Package

When creating and naming symbol packages it is necessary to follow these conventions:

* The library/content package should be named `MyPackage.nupkg` and contain everything **except** PDB files.
* The symbol/source package should be named `MyPackage.symbols.nupkg` and contain only DLL, PDB, XMLDOC and 
source files.

You can create both packages with the `-Symbols` option, either from a nuspec file:

	NuGet Pack MyPackage.nuspec -Symbols

or from a project file:

	NuGet Pack MyProject.csproj -Symbols

## Symbol Package Structure

A symbol package can target multiple target frameworks in the same way that a library package does, so the 
structure of the `lib` folder should look exactly as described in [Creating and Publishing a Package](/Create/Creating-and-Publishing-a-Package),
plus it should contain PDB files alongside DLLs. An example symbol package that targets .NET 4.0 and Silverlight 
4 would have this layout:
	
	\lib
		\net40
			\MyAssembly.dll
			\MyAssembly.pdb
		\sl40
			\MyAssembly.dll
			\MyAssembly.pdb

Source files are placed in a separate special folder - `src`. This folder needs to follow the relative structure 
of your source repository, because PDBs contain absolute paths to source files used to compile a matching DLL, and 
they need to be found during publishing on [SymbolSource.org](http://www.symbolsource.org/Public). A base path (common path 
prefix) can be stripped out. Consider an example library built from these files:

	C:\Projects
		\MyProject
			\Common
				\MyClass.cs
			\Full
				\Properties
					\AssemblyInfo.cs
				\MyAssembly.csproj (producing \lib\net40\MyAssembly.dll)
			\Silverlight
				\Properties
					\AssemblyInfo.cs
				\MySilverlightExtensions.cs
				\MyAssembly.csproj (producing \lib\sl4\MyAssembly.dll)

Apart from the `lib` folder, a symbol package would need to contain this layout:

	\src
		\Common
			\MyClass.cs
		\Full
			\Properties
				\AssemblyInfo.cs
		\Silverlight
			\Properties
				\AssemblyInfo.cs
			\MySilverlightExtensions.cs

## Specifying Symbol Package Contents

A symbol package can be built by conventions, from a folder structured in the way described in the previous section,
or its contents can be specified using the `files` section. If you wanted to build the example package described previously,
you could put this into your nuspec file:

    <files>
      <file src="Full\bin\Debug\*.dll" target="lib\net40" /> 
	  <file src="Full\bin\Debug\*.pdb" target="lib\net40" /> 
      <file src="Silverlight\bin\Debug\*.dll" target="lib\sl40" /> 
	  <file src="Silverlight\bin\Debug\*.pdb" target="lib\sl40" /> 
      <file src="**\*.cs" target="src" />
    </files>

## Publishing a Symbol Package

To make life easier, first save your API key as usual:

    NuGet SetApiKey Your-API-Key

This will save your API key for both [NuGet.org](http://nuget.org) and [SymbolSource.org](http://symbolsource.org). 
When you publish to SymbolSource, it contacts the NuGet Gallery to verify that you are an owner of the project.

You can push a symbol package separately, which will automatically choose the new SymbolSource.org [service](https://nuget.smbsrc.net/) as the target:

 	NuGet Push MyPackage.symbols.nupkg

It is possible to override the symbol repository or to push a symbol package that doesn't follow the naming convention 
by using the `-Source` option:

 	NuGet Push MyPackage.symbols.nupkg -Source https://nuget.smbsrc.net/

You can also push both packages to both repositories at the same time:

 	NuGet Push MyPackage.nupkg

If the presence of a `.symbols.nupkg` package is detected it will be automatically pushed to the new SymbolSource.org service hosted at https://nuget.smbsrc.net/.


