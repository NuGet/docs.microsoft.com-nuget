# Local Feeds

Local NuGet package feeds are simply folders on your local network in which you place packages (or even just a folder on your own machine). Local feeds can be either a simple folder of packages, or a hierarchical folder structure that include version numbers. With NuGet 3.3 and above, using the hierarchical structure gives much better performance. 

In these cases, you enable the source by simply adding the pathname, such as `\\myserver\packages` through the [Package Manager UI](/ndocs/tools/package-manager-ui#package-sources) or the command line using [`nuget sources`](/ndocs/tools/nuget.exe-cli-reference#sources).   
 
## Initializing and maintaining hierarchical folders 

With NuGet 3.3 and above, you'll realize much better performance by structuring the feed using a hierarchical versioned folder tree:

	\\myserver\packages
      └─<packageID>
        └─<version>	            
          ├─<packageID>.<version>.nupkg	
          └─<other files>

NuGet will create this structure automatically when you use the [`nuget add`](/ndocs/tools/nuget.exe-cli-reference#add) command to copy packages to the feed: 

    nuget add new_package.1.0.0.nupkg -source \\myserver\packages

You can also use the [`nuget init`](/ndocs/tools/nuget.exe-cli-reference#init) command to copy multiple packages from a single folder to the feed. For example, the following command copues all packages from `c:\packages` to a hierarchical tree on `\\myserver\packages`: 

    nuget init c:\packages \\myserver\packages

Again, this will create a folder for each package identifier, each of which will contain a version number folder, within which will be the appropriate package.

