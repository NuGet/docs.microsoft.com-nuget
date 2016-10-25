# NuGet.Server

NuGet.Server is a package provided by the .NET Foundation that creates an ASP.NET application that can host a package feed on any server that runs IIS. Simply said, NuGet.Server basically makes a folder on the server available through HTTP(S) (specifically OData). As such it's best for simple scenarios and is easy to set up.

The process is as follows:

1. Create an empty ASP.NET Web application in Visual Studio and add the NuGet.Server package to it.
2. Configure the `Packages` folder in the application and add packages.
3. Deploy the application to a suitable server.

The following sections walk through this process in detail, using C#.

## Create and deploy an ASP.NET Web application with NuGet.Server

1. In Visual Studio, select **File > New > Project**, set the target framework for .NET Framework 4.6 (see below), search for "ASP.NET", and select the **ASP.NET Web Application** template for C#.

	![Setting .NET Framework target to 4.6](/images/hosting/Hosting_01-NuGet.Server-Set4.6.png)

<div class="block-callout-warning">
	<strong>Note</strong><br>
	The web application must target .NET Framework 4.6 or above to use the latest version of NuGet.Server (2.11.2 as of this writing). If you target .NET Framework 4.5.2, be sure to install NuGet Server 2.10.3 instead below in step 3.
</div>

2. Give the application a suitable name, click OK, and in the next dialog select the **ASP.NET 4.6 - Empty** template (or a higher version) and click OK.

3. Right-click the project, select **Manage NuGet Packages**, and in the Package Manager UI search and install the latest version of the NuGet.Server package. (You can also install it from the Package Manager Console with `Install-Package NuGet.Server`.)

	![Installing the NuGet.Server package](/images/hosting/Hosting_02-NuGet.Server-Package.png)

4. Installing NuGet.Server converts the empty Web application into a package source. It creates a `Packages` folder in the application and overwrites `web.config` to include additional settings (see the comments in that file for details).
5. To make packages available in the feed when you publish the application to a server, add their `.nupkg` files to the `Packages` folder in Visual Studio, then set their **Build Action** to **Content** and **Copy to Output Directory** to **Copy always**:

	![Copying packages to the Packages folder in the project](/images/hosting/Hosting_03-NuGet.Server-Package-Folder.png)

6. Run the site locally in Visual Studio (without debugging, that is Ctrl+F5). The home page provides the package feed URLs:

	![Default home page for an application with NuGet.Server](/images/hosting/Hosting_04-NuGet.Server-FeedHomePage.png)

7. Click on **here** in the area outlined above to see the OData feed of packages.

8. By running the application the first time, the `Packages` folder gets restructured to contain a folder for each package. This matches the [local storage layout](http://blog.nuget.org/20151118/nuget-3.3.html#folder-based-repository-commands) introduced with NuGet 3.3 to improve performance. When adding more packages, continue to follow this structure.

9. Once you've tested your local deployment, you can deploy the application to any other internal or external site as needed.
10. Once deployed to `http://<domain>`, the URL that you use for the package source will be `http://<domain>/nuget`.


##Configuring the Packages folder

With `NuGet.Server` 1.5 and later, you can more specifically configure the package folder using the `appSetting/packagesPath` value in `web.config`:

    <appSettings>
        <!-- Set the value here to specify your custom packages folder. -->
        <add key="packagesPath" value="C:\MyPackages" />
    </appSettings>

`packagesPath` can be an absolute or virtual path.

When `packagesPath` is omitted or left blank, the packages folder is the default `~/Packages`. 

## Adding packages to the feed externally

Once a NuGet.Server site is running, you can add or delete packages using nuget.exe provided that you set an API key value in `web.config`. 

After installing the NuGet.Server package, `web.config` will contain an empty `appSetting/apiKey` value:

    <appSettings>         
        <add key="apiKey" value="" />
    </appSettings>
  
When `apiKey` is omitted or blank, pushing packages to the feed is disabled.

To enable this capability, set the `apiKey` to a value (ideally a strong password) and add a key called `appSettings/requireApiKey` with the value of `true`:

    <appSettings>
         <!-- Sets whether an API Key is required to push/delete packages -->
        <add key="requireApiKey" value="true" />

        <!-- Set a shared password (for all users) to push/delete packages -->
        <add key="apiKey" value="" />
    </appSettings>

If your server is already secured or you do not otherwise require an API key (for example, when using a private server on a local team network), you can set `requireApiKey` to `false`. All users with access to the server can then push or delete packages.

