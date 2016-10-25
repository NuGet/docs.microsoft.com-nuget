# Target Frameworks

NuGet uses target framework references in a variety of places to specifically identify and isolate framework-dependent components of a package:

- [nuspec manifest](/ndocs/schema/nuspec): A package can indicate distinct packages to be included in a project depending on the project's target framework. 
- [nupkg folder name](/ndocs/create-packages/creating-a-package#from-a-convention-based-working-directory): The folders inside a package's `lib` folder can be named according to the target framework, each of which contains the DLLs and other content appropriate to that framework.
- [project.json](/ndocs/schema/project.json): The `frameworks` node specifies the framework versions that the project can be compiled against.

<div class="block-callout-info">
	<strong>Note</strong>
	The NuGet client source code that calculates the tables below is found in the following locations:  
	<ul>
	<li>
		Supported framework names: <a href="https://github.com/NuGet/NuGet.Client/blob/dev/src/NuGet.Core/NuGet.Frameworks/FrameworkConstants.cs">FrameworkConstants.cs</a>
	</li>
	<li>
		Framework precedence and mapping: <a href="https://github.com/NuGet/NuGet.Client/blob/dev/src/NuGet.Core/NuGet.Frameworks/DefaultFrameworkMappings.cs">DefaultFrameworkMappings.cs</a>
	</li>
	</ul>
</div>


## Supported Frameworks

A framework is typically referenced by a short target framework moniker or TFM. In .NET Standard this is also is generalized to *TxM* to allow a single reference to multiple frameworks.

The NuGet clients support the following frameworks. Equivalents are shown within brackets [].

<table class="reference">
  <tbody>
    <tr><th>Name</th><th>Abbreviation</th><th>TFMs/TxMs</th>
    <tr>
		<td>.NET Framework</td>
		<td>net</td>
		<td>
			net11<br/>
			net20<br/>
			net35<br/>
			net40<br/>
			net403<br/>
			net45<br/>
			net451<br/>
			net452<br/>
			net46<br/>
			net461<br/>
			net462
		</td>
	</tr>
	<tr>
		<td>.NET Core</td>
		<td>netcore</td>
		<td>
			netcore [netcore45]<br/>
			netcore45 [win, win8]<br/>
			netcore451 [win81]<br/>
			netcore50
		</td>
	</tr>
	<tr>
		<td>.NET MicroFramework</td>
		<td>netmf</td>
		<td>netmf</td>
	</tr>
	<tr>
		<td>Windows</td>
		<td>win</td>
		<td>		
			win [win8, netcore45]<br/> 
			win8 [netcore45, win]<br/> 
			win81 [netcore451]<br/> 
			win10 (not supported by Windows 10 Platform)			
		</td>
	</tr>
	<tr>
		<td>Silverlight</td>
		<td>sl</td>
		<td>
			sl4 <br/> 
			sl5 <br/> 
		</td>
	</tr>
	<tr>
		<td>Windows Phone</td>
		<td>wp</td>
		<td>
			wp [wp7]<br/>
			wp7<br/>
			wp75<br/>
			wp8<br/>
			wp81<br/>
			wpa81<br/>
		</td>
	</tr>
	<tr>
    	<td>Universal Windows Platform</td>
    	<td>uap</td>
    	<td>
			uap [uap10]<br/>
			uap10 [uap]<br/>
    	</td>
    </tr>
    <tr>
    	<td>.NET Standard</td>
    	<td>netstandard</td>
    	<td>                
			netstandard1.0<br/>
			netstandard1.1<br/>
			netstandard1.2<br/>
			netstandard1.3<br/>
			netstandard1.4<br/>
			netstandard1.5<br/>
			netstandard1.6<br/>
    	</td>
    </tr>
    <tr>
    	<td>.NET Core App</td>
    	<td>netcoreapp</td>
    	<td>
    		netcoreapp1.0<br/>	
		</td>
    </tr>
  </tbody>
</table>

## Deprecated Frameworks
The following frameworks are deprecated. Packages targeting these frameworks should migrate to the indicated replacements.

<table>
	<tr>
		<th>Deprecated framework</th>
		<th>Replacement</th>
	</tr>
	<tr>
		<td>
			aspnet50<br>
			aspnetcore50<br>
			dnxcore50<br>
			dnx<br>
			dnx45<br>
			dnx451<br>
			dnx452<br>
		</td>		
		<td>netcoreapp</td>
	</tr>
	<tr>
		<td>
			dotnet<br>
			dotnet50<br>
			dotnet51<br>
			dotnet52<br>
			dotnet53<br>
			dotnet54<br>
			dotnet55<br>
			dotnet56<br>
		</td>
		<td>netstandard</td>
	</tr>
	<tr>
		<td>winrt</td>		
		<td>win</td>
	</tr>
</table>

## Precedence

A number of frameworks are related to and compatible with one another, but not necessarily equivalent: 

<table>
	<tr>
		<th>Framework</th>
		<th>Can use</th>
	</tr>
	<tr>
		<td>
			uap (Universal Windows Platform)
		</td>		
		<td>
			win81<br>
			wpa81<br>
			netcore50
		</td>
	</tr>
	<tr>
		<td>
			win (Windows Store)
		</td>
		<td>
			winrt<br>
			winrt45
		</td>
	</tr>

</table>

## NET Platform Standard 

The [.NET  Platform Standard](https://github.com/dotnet/corefx/blob/master/Documentation/architecture/net-platform-standard.md) simplifies references between binary-compatible frameworks, allowing a single target framework to reference a combination of others. (For background, see the [.NET Primer](https://docs.microsoft.com/en-us/dotnet/articles/standard/index).) 

The [NuGet Get Nearest Framework Tool](https://aka.ms/s2m3th) simulates what NuGet uses to select one framework from many available framework assets in a package based on the project's framework.

The `dotnet` series of monikers should be used in NuGet 3.3 an earlier; the `netstandard` moniker syntax should be used in v3.4 and later.


## Portable Class Libraries

<div class="block-callout-warning">
    <strong>Not Recommended</strong><br>
    ALthough PCLs are supported, package authors should support netstandard instead. The .NET Platform Standard is an evolution of PCLs and represents binary portability across platforms using a single moniker that isn't tied to a static like like <em>portable-a+b+c</em> monikers.
</div>

To define a target framework that refers to multiple child-target-frameworks, the `portable` keyword use used to prefix the list of referenced frameworks. Avoid artificially including extra frameworks that are not directly compiled against because it can lead to unintended side-effects in those frameworks.

Additional frameworks defined by third parties provide compatibility with other environments that are accessible in this manner. Additionally, there are shorthand profile numbers that are available to reference these combinations of related frameworks as `Profile#`, but this is not a recommended practice to use these numbers as it reduces the readability of the folders and nuspec. 

<table class="reference">
  <tr>
	<th>Profile #</th>
	<th>Frameworks</th>
	<th>Full name</th>
	<th>.NET Standard</th>
  </tr>
<tr>
<td>Profile2</td>
<td>
.NETFramework 4.0<br/>
Windows 8.0<br/>
Silverlight 4.0<br/>
WindowsPhone 7.0</td>
<td>portable-net40+win8+sl4+wp7</td>
<td></td>
</tr>
<tr>
<td>Profile3</td>
<td>
.NETFramework 4.0<br/>
Silverlight 4.0</td>
<td>portable-net40+sl4</td>
<td></td>
</tr>
<tr>
<td>Profile4</td>
<td>
.NETFramework 4.5<br/>
Silverlight 4.0<br/>
Windows 8.0<br/>
WindowsPhone 7.0</td>
<td>portable-net45+sl4+win8+wp7</td>
<td></td>
</tr>
<tr>
<td>Profile5</td>
<td>
.NETFramework 4.0<br/>
Windows 8.0</td>
<td>portable-net40+win8</td>
<td></td>
</tr>
<tr>
<td>Profile6</td>
<td>
.NETFramework 4.0.3<br/>
Windows 8.0</td>
<td>portable-net403+win8</td>
<td></td>
</tr>
<tr>
<td>Profile7</td>
<td>
.NETFramework 4.5<br/>
Windows 8.0</td>
<td>portable-net45+win8</td>
<td>netstandard1.1</td>
</tr>
<tr>
<td>Profile14</td>
<td>
.NETFramework 4.0<br/>
Silverlight 5.0</td>
<td>portable-net40+sl5</td>
<td></td>
</tr>
<tr>
<td>Profile18</td>
<td>
.NETFramework 4.0.3<br/>
Silverlight 4.0</td>
<td>portable-net403+sl4</td>
<td></td>
</tr>
<tr>
<td>Profile19</td>
<td>
.NETFramework 4.0.3<br/>
Silverlight 5.0</td>
<td>portable-net403+sl5</td>
<td></td>
</tr>
<tr>
<td>Profile23</td>
<td>
.NETFramework 4.5<br/>
Silverlight 4.0</td>
<td>portable-net45+sl4</td>
<td></td>
</tr>
<tr>
<td>Profile24</td>
<td>
.NETFramework 4.5<br/>
Silverlight 5.0</td>
<td>portable-net45+sl5</td>
<td></td>
</tr>
<tr>
<td>Profile31</td>
<td>
Windows 8.1<br/>
WindowsPhone 8.1</td>
<td>portable-win81+wp81</td>
<td>netstandard1.0</td>
</tr>
<tr>
<td>Profile32</td>
<td>
Windows 8.1<br/>
WindowsPhone 8.1</td>
<td>portable-win81+wpa81</td>
<td>netstandard1.2</td>
</tr>
<tr>
<td>Profile36</td>
<td>
.NETFramework 4.0<br/>
Silverlight 4.0<br/>
Windows 8.0<br/>
WindowsPhone 8.0</td>
<td>portable-net40+sl4+win8+wp8</td>
<td></td>
</tr>
<tr>
<td>Profile37</td>
<td>
.NETFramework 4.0<br/>
Silverlight 5.0<br/>
Windows 8.0</td>
<td>portable-net40+sl5+win8</td>
<td></td>
</tr>
<tr>
<td>Profile41</td>
<td>
.NETFramework 4.0.3<br/>
Silverlight 4.0<br/>
Windows 8.0</td>
<td>portable-net403+sl4+win8</td>
<td></td>
</tr>
<tr>
<td>Profile42</td>
<td>
.NETFramework 4.0.3<br/>
Silverlight 5.0<br/>
Windows 8.0</td>
<td>portable-net403+sl5+win8</td>
<td></td>
</tr>
<tr>
<td>Profile44</td>
<td>
.NETFramework 4.5.1<br/>
Windows 8.1</td>
<td>portable-net451+win81</td>
<td>netstandard1.2</td>
</tr>
<tr>
<td>Profile46</td>
<td>
.NETFramework 4.5<br/>
Silverlight 4.0<br/>
Windows 8.0</td>
<td>portable-net45+sl4+win8</td>
<td></td>
</tr>
<tr>
<td>Profile47</td>
<td>
.NETFramework 4.5<br/>
Silverlight 5.0<br/>
Windows 8.0</td>
<td>portable-net45+sl5+win8</td>
<td></td>
</tr>
<tr>
<td>Profile49</td>
<td>
.NETFramework 4.5<br/>
WindowsPhone 8.0</td>
<td>portable-net45+wp8</td>
<td>netstandard1.0</td>
</tr>
<tr>
<td>Profile78</td>
<td>
.NETFramework 4.5<br/>
Windows 8.0<br/>
WindowsPhone 8.0</td>
<td>portable-net45+win8+wp8</td>
<td>netstandard1.0</td>
</tr>
<tr>
<td>Profile84</td>
<td>
WindowsPhone 8.1<br/>
WindowsPhone 8.1</td>
<td>portable-wp81+wpa81</td>
<td>netstandard1.0</td>
</tr>
<tr>
<td>Profile88</td>
<td>
.NETFramework 4.0<br/>
Silverlight 4.0<br/>
Windows 8.0<br/>
WindowsPhone 7.5</td>
<td>portable-net40+sl4+win8+wp75</td>
<td></td>
</tr>
<tr>
<td>Profile92</td>
<td>
.NETFramework 4.0<br/>
Windows 8.0<br/>
WindowsPhone 8.1</td>
<td>portable-net40+win8+wpa81</td>
<td></td>
</tr>
<tr>
<td>Profile95</td>
<td>
.NETFramework 4.0.3<br/>
Silverlight 4.0<br/>
Windows 8.0<br/>
WindowsPhone 7.0</td>
<td>portable-net403+sl4+win8+wp7</td>
<td></td>
</tr>
<tr>
<td>Profile96</td>
<td>
.NETFramework 4.0.3<br/>
Silverlight 4.0<br/>
Windows 8.0<br/>
WindowsPhone 7.5</td>
<td>portable-net403+sl4+win8+wp75</td>
<td></td>
</tr>
<tr>
<td>Profile102</td>
<td>
.NETFramework 4.0.3<br/>
Windows 8.0<br/>
WindowsPhone 8.1</td>
<td>portable-net403+win8+wpa81</td>
<td></td>
</tr>
<tr>
<td>Profile104</td>
<td>
.NETFramework 4.5<br/>
Silverlight 4.0<br/>
Windows 8.0<br/>
WindowsPhone 7.5</td>
<td>portable-net45+sl4+win8+wp75</td>
<td></td>
</tr>
<tr>
<td>Profile111</td>
<td>
.NETFramework 4.5<br/>
Windows 8.0<br/>
WindowsPhone 8.1</td>
<td>portable-net45+win8+wpa81</td>
<td>netstandard1.1</td>
</tr>
<tr>
<td>Profile136</td>
<td>
.NETFramework 4.0<br/>
Silverlight 5.0<br/>
Windows 8.0<br/>
WindowsPhone 8.0</td>
<td>portable-net40+sl5+win8+wp8</td>
<td></td>
</tr>
<tr>
<td>Profile143</td>
<td>
.NETFramework 4.0.3<br/>
Silverlight 4.0<br/>
Windows 8.0<br/>
WindowsPhone 8.0</td>
<td>portable-net403+sl4+win8+wp8</td>
<td></td>
</tr>
<tr>
<td>Profile147</td>
<td>
.NETFramework 4.0.3<br/>
Silverlight 5.0<br/>
Windows 8.0<br/>
WindowsPhone 8.0</td>
<td>portable-net403+sl5+win8+wp8</td>
<td></td>
</tr>
<tr>
<td>Profile151</td>
<td>
.NETFramework 4.5.1<br/>
Windows 8.1<br/>
WindowsPhone 8.1</td>
<td>portable-net451+win81+wpa81</td>
<td>netstandard1.2</td>
</tr>
<tr>
<td>Profile154</td>
<td>
.NETFramework 4.5<br/>
Silverlight 4.0<br/>
Windows 8.0<br/>
WindowsPhone 8.0</td>
<td>portable-net45+sl4+win8+wp8</td>
<td></td>
</tr>
<tr>
<td>Profile157</td>
<td>
Windows 8.1<br/>
WindowsPhone 8.1<br/>
WindowsPhone 8.1</td>
<td>portable-win81+wp81+wpa81</td>
<td>netstandard1.0</td>
</tr>
<tr>
<td>Profile158</td>
<td>
.NETFramework 4.5<br/>
Silverlight 5.0<br/>
Windows 8.0<br/>
WindowsPhone 8.0</td>
<td>portable-net45+sl5+win8+wp8</td>
<td></td>
</tr>
<tr>
<td>Profile225</td>
<td>
.NETFramework 4.0<br/>
Silverlight 5.0<br/>
Windows 8.0<br/>
WindowsPhone 8.1</td>
<td>portable-net40+sl5+win8+wpa81</td>
<td></td>
</tr>
<tr>
<td>Profile240</td>
<td>
.NETFramework 4.0.3<br/>
Silverlight 5.0<br/>
Windows 8.0<br/>
WindowsPhone 8.1</td>
<td>portable-net403+sl5+win8+wpa81</td>
<td></td>
</tr>
<tr>
<td>Profile255</td>
<td>
.NETFramework 4.5<br/>
Silverlight 5.0<br/>
Windows 8.0<br/>
WindowsPhone 8.1</td>
<td>portable-net45+sl5+win8+wpa81</td>
<td></td>
</tr>
<tr>
<td>Profile259</td>
<td>
.NETFramework 4.5<br/>
Windows 8.0<br/>
WindowsPhone 8.1<br/>
WindowsPhone 8.0</td>
<td>portable-net45+win8+wpa81+wp8</td>
<td>netstandard1.0</td>
</tr>
<tr>
<td>Profile328</td>
<td>
.NETFramework 4.0<br/>
Silverlight 5.0<br/>
Windows 8.0<br/>
WindowsPhone 8.1<br/>
WindowsPhone 8.0</td>
<td>portable-net40+sl5+win8+wpa81+wp8</td>
<td></td>
</tr>
<tr>
<td>Profile336</td>
<td>
.NETFramework 4.0.3<br/>
Silverlight 5.0<br/>
Windows 8.0<br/>
WindowsPhone 8.1<br/>
WindowsPhone 8.0</td>
<td>portable-net403+sl5+win8+wpa81+wp8</td>
<td></td>
</tr>
<tr>
<td>Profile344</td>
<td>
.NETFramework 4.5<br/>
Silverlight 5.0<br/>
Windows 8.0<br/>
WindowsPhone 8.1<br/>
WindowsPhone 8.0</td>
<td>portable-net45+sl5+win8+wpa81+wp8</td>
<td></td>
</tr>
</table>

Additionally, NuGet packages targeting Xamarin can use additional Xamarin-defined frameworks. See [Creating NuGet packages for Xamarin](https://developer.xamarin.com/guides/cross-platform/advanced/nuget/).
 
<table class="reference">
  <tr>
    <th>Name</th>
    <th>Description</th>
    <th>.NET Standard</th>
  </tr>
  <tr>
    <td>monoandroid</td>
    <td>Mono Support for Android OS</td>
    <td>netstandard1.4</td>
  </tr>
  <tr>
    <td>monotouch</td>
    <td>Mono Support for iOS</td>
    <td>netstandard1.4</td>
  </tr>
  <tr>
    <td>monomac</td>
    <td>Mono Support for OSX</td>
    <td>netstandard1.4</td>
  </tr>
  <tr>
    <td>xamarinios</td>
    <td>Support for Xamarin for iOS</td>
    <td>netstandard1.4</td>
  </tr>
  <tr>
    <td>xamarinmac</td>
    <td>Supports for Xamarin for Mac</td>
    <td>netstandard1.4</td>
  </tr>
  <tr>
    <td>xamarinpsthree</td>
    <td>Support for Xamarin on Playstation 3</td>
    <td>netstandard1.4</td>
  </tr>
  <tr>
    <td>xamarinpsfour</td>
    <td>Support for Xamarin on Playstation 4</td>
    <td>netstandard1.4</td>
  </tr>
  <tr>
    <td>xamarinpsvita</td>
    <td>Support for Xamarin on PS Vita</td>
    <td>netstandard1.4</td>
  </tr>
  <tr>
    <td>xamarinwatchos</td>
    <td>Xamarin for Watch OS</td>
    <td>netstandard1.4</td>
  </tr>
  <tr>
    <td>xamarintvos</td>
    <td>Xamarin for TV OS</td>
    <td>netstandard1.4</td>
  </tr>
  <tr>
    <td>xamarinxboxthreesixty</td>
    <td>Xamarin for XBox 360</td>
    <td>netstandard1.4</td>
  </tr>
  <tr>
    <td>xamarinxboxone</td>
    <td>Xamarin for XBox One</td>
    <td>netstandard1.4</td>
  </tr>
</table>

<div class="block-callout-info">
	<strong>Note</strong>
	Stephen Cleary has created a tool that lists the supported PCLs, which you can find on his post, <a href="http://blog.stephencleary.com/2012/05/framework-profiles-in-net.html">Framework profiles in .NET</a>.
</div>



