# Target Frameworks

As the .NET ecosystem has grown, so too has the list of frameworks and dependencies that NuGet supports. The frameworks that can be included and referenced by a NuGet package tracks closely with the list of available .NET 
framework versions and flavors that support all of the devices and systems the .NET framework can be used on.

## References
Target frameworks can be referenced from three places:  

[nuspec manifest](/ndocs/schema/nuspec)
You can find a target framework referenced in the dependencies on the group elements to indicate which packages should be referenced when the package is installed into different projects.

[nupkg folder name](/ndocs/create-packages/create-a-package#convention-based-working-directory)
The folders inside of the base package lib folder are named after the target frameworks that the contents support. The DLLs and other content that support these framework version should be placed here.

[project.json](/ndocs/schema/project.json)  
This node specifies the framework versions that the project should be compiled against for project systems that use project.json (ASP.NET 5 and UWP currently).

A framework is typically referenced by a short name, called a "Target Framework Moniker" or TFM.  With the advent of the new Platform Standard, the concept of 'TFM' has been abstracted to 'TxM' since the Platform Standard references multiple frameworks abstractly. 

## Source Reference

The source code from the NuGet clients that is used to calculate these tables can be found at:  

Supported framework names in [FrameworkConstants.cs](https://github.com/NuGet/NuGet.Client/blob/dev/src/NuGet.Core/NuGet.Frameworks/FrameworkConstants.cs)

Framework Precendence and Mapping in [DefaultFrameworkMappings.cs](https://github.com/NuGet/NuGet.Client/blob/dev/src/NuGet.Core/NuGet.Frameworks/DefaultFrameworkMappings.cs)

## Supported Frameworks

The official NuGet clients support the frameworks listed below, with the equivalence indicators provided:

<table class="reference">
  <tbody>
    <tr><th>Name</th><th title="Abbreviation">Abbr</th><th>TFMs/TxMs</th>
    <tr>
		<td>.NET Framework <br/> <br/> Standard .NET BCL on Windows Desktop and Server.</td>
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
		<td>.NET Core <br/> <br/> .NET Core Framework used in Windows Store Development</td>
		<td>netcore</td>
		<td>
		netcore (equivalent to netcore45)<br/>
		netcore45 (equivalent to win8)<br/>
		netcore451 (equivalent to win81)<br/>
		netcore50
		</td>
	</tr>
	<tr>
		<td>.NET MicroFramework <br/> <br/>  Support for [.NET MicroFramework](http://netmf.github.io/) projects</td>
		<td>netmf</td>
		<td>netmf</td>
	</tr>
	<tr>
		<td>Windows <br/> <br/> Frameworks to support Windows Store Development	</td>
		<td>win</td>
		<td>		
				win (equivalent to win8)<br/> 
				win8 (equivalent to netcore45)<br/> 
				win81 (equivalent to netcore451)<br/> 
				win10 -- not supported by Windows 10 Platform
			
		</td>
	</tr>
	<tr>
		<td>Silverlight <br/> <br/> Support for the Silverlight frameworks</td>
		<td>sl</td>
		<td>
				sl4 <br/> 
				sl5 <br/> 
		</td>
	</tr>
	<tr>
		<td>Windows Phone <br/> <br/> Windows Phone application support</td>
		<td>wp</td>
		<td>
				wp (equivalent to wp7)<br/>
				wp7<br/>
				wp75<br/>
				wp8<br/>
				wp81<br/>
				wpa81<br/>
		</td>
	</tr>
	<tr>
    	<td>Universal Windows Platform Support <br/> <br/> for Windows 10 Universal Application Platform</td>
    	<td>uap</td>
    	<td>
			uap (equivalent to uap10)<br/>
			uap10<br/>
    	</td>
    </tr>
    <tr>
    	<td>.NET Standard <br/> <br/> Modern PCL Support</td>
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
    	<td>.NET Core App <br/> <br/> .NET Core Websites, Console Applications</td>
    	<td>netcoreapp</td>
    	<td>
    		netcoreapp1.0<br/>	
		</td>
    </tr>
  </tbody>
</table>

## Deprecated Frameworks
The following frameworks are deprecated. If you have any packages that target these frameworks, we highly recommend that you move towards their replacements.

<div class="block-callout-warning">
    <strong>Deprecated Frameworks</strong><br>
    aspnet50, aspnetcore50, winrt, dotnet50 (equvialent to dotnet), dotnet51, dotnet52, dotnet53, dotnet54, dotnet55, dotnet56, dnx, dnx45, dnx451, dnx452, dnxcore50.
</div>

<div class="block-callout-info">
    <strong>Note:</strong><br>
	dotnet is replaced by netstandard <br/>
	aspnet/dnx is replaced by netcoreapp.
</div>

## Precedence

For a number of these frameworks, they are related to each other.  Not necessarily equivalent, but there is compatibility that can allow binary content to run on other platforms.  The compatibility matrix that NuGet uses is described below.

**Universal Windows Platform (uap)** can use the following content

Win81

WPA81

NetCore50

**Windows Store (win)** can use the following content

winrt

winrt45

## NET Platform Standard 

To simplify the references between binary-compatible frameworks, the [.NET Platform Standard](https://github.com/dotnet/corefx/blob/master/Documentation/project-docs/standard-platform.md) was introduced.  This allows the definition of a single target framework moniker to reference a combination of binary compatible frameworks.  Various versions of the Platform Standard indicate different combinations of frameworks that are compatible.  More information about these frameworks can be found at the [here](https://docs.microsoft.com/en-us/dotnet/articles/standard/index). 

The `dotnet` series of monikers should be used in NuGet 3.3 and the `netstandard` moniker syntax should be used in v3.4 and later.

Take a look at [NuGet Tools - Get Nearest Framework](https://aka.ms/s2m3th) to simulate what NuGet uses to select one framework from many available framework assets in a package based on the project's framework.

## Portable Class Libraries

<div class="block-callout-warning">
    <strong>Not Recommended</strong><br>
    While PCLs are supported, It is our recommendation that package authors move to supporting net standard instead.The .NET Platform Standard version represents binary portability across platforms using a single moniker. They are an evolution of the existing Portable Class Library system. They are "open-ended" in that they aren't tied down to a static list of monikers like portable-a+b+c is.
</div>

To define a target framework that refers to multiple child-target-frameworks, the `portable` keyword shall be used to prefix the list of frameworks that are referenced.  We recommend that you do not artificially include extra frameworks that are not directly compiled against as this could lead to unintended side-effects in those frameworks.

There are additional frameworks that are defined by third parties that provide compatibility with other environments that are accessible in this manner.  Additionally, there are shorthand profile numbers that are available to reference these combinations of related frameworks as `Profile#`, but this is not a recommended practice to use these numbers as it reduces the readiblity of the folders and nuspec. 

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

Additionally, there are optional frameworks provided by Xamarin that are supported by the official NuGet clients.  More details about these frameworks are available from [Xamarin](http://www.xamarin.com).
 
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

Stephen Cleary has a blog post that demonstrates a tool that will mine the list of supported portable class libraries on a workstation and report the features of them: [http://blog.stephencleary.com/2012/05/framework-profiles-in-net.html](http://blog.stephencleary.com/2012/05/framework-profiles-in-net.html)

