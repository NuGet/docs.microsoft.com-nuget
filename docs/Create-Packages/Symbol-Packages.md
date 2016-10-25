# Symbol Packages

In addition to building packages for nuget.org or other sources, NuGet also supports creating associated symbol packages and publishing them to the [SymbolSource repository](http://www.symbolsource.org/Public).

Package consumers can then add https://nuget.smbsrc.net/ to their symbol sources in Visual Studio. This allows consumers to step into your package code in the Visual Studio debugger.

## Creating a symbol package

To create a symbol package, follow these conventions:
- Name the primary package (with your code) `{identifier}.nupkg` and include all your files except `.pdb` files.
- Name the symbol package `{identifier}.symbols.nupkg` and include your assembly DLL, `.pdb` files, XMLDOC files, source files (see the sections that follow).

You can create both packages with the `-Symbols` option, either from a nuspec file or a project file:

<code class="bash hljs">
	nuget pack MyPackage.nuspec -Symbols
</code>

<code class="bash hljs">
	nuget pack MyProject.csproj -Symbols
</code>

## Symbol package structure

A symbol package can target multiple target frameworks in the same way that a library package does, so the structure of the `lib` folder should be exactly the same as the primary package, only including `.pdb` files alongside the DLL.

For example, a symbol package that targets .NET 4.0 and Silverlight 4 would have this layout:
	
	\lib
		\net40
			\MyAssembly.dll
			\MyAssembly.pdb
		\sl40
			\MyAssembly.dll
			\MyAssembly.pdb

Source files are then placed in a separate special folder named `src`, which must follow the relative structure of your source repository. This is because PDBs contain absolute paths to source files used to compile the matching DLL, and they need to be found during the publishing process. A base path (common path prefix) can be stripped out. For example, consider a library built from these files:

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

## Referring to files in the nuspec

A symbol package can be built by conventions, from a folder structure as described in the previous section, or by specifying its contents in the `files` section of the manifest. For example, to build the package shown in the previous section, use the following in the `.nuspec` file:

    <files>
      <file src="Full\bin\Debug\*.dll" target="lib\net40" /> 
	  <file src="Full\bin\Debug\*.pdb" target="lib\net40" /> 
      <file src="Silverlight\bin\Debug\*.dll" target="lib\sl40" /> 
	  <file src="Silverlight\bin\Debug\*.pdb" target="lib\sl40" /> 
      <file src="**\*.cs" target="src" />
    </files>

## Publishing a symbol package

1. For convenience, first save your API key with NuGet (see [publish a package](/ndocs/create-packages/publish-a-package), which will apply to both nuget.org and symbolsource.org, because symbolsource.org will check with nuget.org to verify that you are the package owner.

	<code class="bash hljs>   
		nuget SetApiKey Your-API-Key
	</code>

2. After publishing your primary package to nuget.org, push the symbol package as follows, which will automatically use symbolsource.org as the target because of the `.symbols` in the filename:
	
	<code class="bash hljs>
 		nuget push MyPackage.symbols.nupkg
	</code>


3. To publish to a different symbol repository, or to push a symbol package that doesn't follow the naming convention, use the `-Source` option:

	<code class="bash hljs>
 		nuget push MyPackage.symbols.nupkg -source https://nuget.smbsrc.net/
	</code>


4. You can also push both primary and symbol packages to both repositories at the same time using the following:
	
	<code class="bash hljs>
	 	nuget push MyPackage.nupkg
	</code>

	In this case, NuGet will publish `MyPackage.symbols.nupkg`, if present, to symbolsource.org (https://nuget.smbsrc.net/), after it publishes the primary package to nuget.org.


