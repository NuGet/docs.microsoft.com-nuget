# Creating Native Packages

A native package contains native C++ code instead of managed code, allowing it to be used within C++ projects. (See [Native C++ Packages](/ndocs/consume-packages/finding-and-choosing-packages#native-c++-packages) in the Consume section.)

To be consumable in a C++ project, a package must target the `native` framework. At present there are not any version numbers associated with this framework as NuGet treats all C++ projects the same.

<div class="block-callout-info">
	<strong>Note</strong><br>
	Be sure to include <em>native</em> in the &lt;tags&gt; section of your .nuspec to help other developers find your package by searching on that tag.
</div>

Native NuGet packages targeting `native` then provide files in `\build`, `\content`, and `\tools` folders; `\lib` is not used in this case (NuGet cannot directly add references to a C++ project). A package may also include targets and props files in `\build` that NuGet will automatically import into projects that consume the package. Those files must be named the same as the package ID with the `.targets` and/or `.props` extensions. For example, the [cpprestsdk](https://nuget.org/packages/cpprestsdk/) package includes a `cpprestsdk.targets` file in its `\build` folder.

The `\build` folder can be used for all NuGet packages and not just native packages. The `\build` folder respects target frameworks just like the `\content`, `\lib`, and `\tools` folders. This means you can create a `\build\net40` folder and a `\build\net45` folder and NuGet will import the appropriate props and targets files into the project. (Use of PowerShell scripts to import MSBuild targets is no longer needed.)