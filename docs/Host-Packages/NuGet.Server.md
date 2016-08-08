# NuGet.Server and Remote Feeds

You can also host a remote (or internal) feed on a server that runs IIS. There are two alternatives from the NuGet team here

a) NuGet.Server: NuGet.Server is ideal when a simple NuGet server is desired. It is basically a view of a network share or local folder through http(s), and as such is easy to setup. 

b) NuGet Gallery: The [NuGet Gallery Project](https://github.com/NuGet/NuGetGallery/wiki/Hosting-the-NuGet-Gallery-Locally-in-IIS) provides user management and additional features such as an extensive web UI that allows searching and exploring packages from within the browser, similar to nuget.org.

Below are the instruction to host a NuGet.Server, the instructions for setting up NuGet Gallery are in the link above.

## Create a new Empty Web Application in Visual Studio

Go to the **File** | **New** | **Project** menu option (or just hit CTRL + SHIFT + N) which will bring up the new project dialog. Select **ASP.NET Web Application** as in the following screenshot. Please note that the steps here including NuGet.Server package are intended for use with C# project only.

![New Project dialog box](/images/create/New-Project-dialog-box.png)

Next, select the **ASP.NET 4.6 - Empty** template (or higher .NET version) and create the project. This results in a very empty project.

![New project in Solution Explorer](/images/create/New-project-in-Solution-Explorer.png)

## Install the NuGet.Server Package

Now open the [Manage NuGet packages](../tools/package-manager-ui) dialog and search for and install the NuGet.Server package. Alternatively,you can use the Package Manager Console instead and type `Install-Package NuGet.Server`.

![NuGet.Server package](/images/create/NuGet.Server-package.png)

## Configure the Packages folder

Starting with `NuGet.Server` 1.5, you can configure the folder which contains your packages. The web.config file contains a new `appSetting`, named `packagesPath`. When the key is omitted or left blank, the packages folder is the default ~/Packages. You can specify an **absolute path**, or a **virtual path**.

    <appSettings>
        <!-- Set the value here to specify your custom packages folder. -->
        <add key="packagesPath" value="C:\MyPackages" />
    </appSettings>

## Add Packages to the Packages folder

That's it! The **NuGet.Server** package just converted your empty website into a site that's ready to serve up the OData package feed. Just add packages into the Packages folder and they'll show up.

In the following screenshot, you can see that a few packages that have been added manually to the default **Packages** folder.  

![Adding packages to the packages folder](/images/create/Adding-packages-to-the-packages-folder.png)

<p class="info">If you want these packages to be published (such as when selecting Build -> Publish from
the application menu) you'll also need to include the .nupkg files in Solution Explorer
and change the Build Action property to "Content".</p>

You can also add and delete packages to the lightweight feed using 
NuGet.exe. After installing the package, the web.config file will contain a new `appSetting`, named 
`apiKey`. When the key is omitted or left blank, pushing packages to the feed is disabled. Setting the 
apiKey to a value (ideally a strong password) enables pushing packages using NuGet.exe.

    <appSettings>
         <!--
            Determines if an Api Key is required to push\delete packages from the server. 
        -->
        <add key="requireApiKey" value="true" />

        <!-- Set the value here to allow people to push/delete packages from the server.
             NOTE: This is a shared key (password) for all users. -->
        <add key="apiKey" value="" />
    </appSettings>

If however your server is already secured and \ or you do not require an api key to perform this operation, 
set the **requireApiKey** value to false.

## Deploy and run your brand new Package Feed

Hit CTRL + F5 to run the site and it'll provide some instructions on what to do next.

![Package feed home page](/images/create/Package-feed-home-page.png)

Clicking on "here" shows the OData over ATOM feed of packages.

![OData over ATOM package feed](/images/create/OData-over-ATOM-package-feed.png)

<p class="info">
<strong>Note</strong><br />After the first load of the feed, the Packages folder is restructured:<br />
<img src="/images/create/Adding-packages-to-the-packages-folder-expanded.png" title="Adding packages to the packages folder (expanded)" /><br /><br />
NuGet.Server uses the <a href="http://blog.nuget.org/20151118/nuget-3.3.html#folder-based-repository-commands">local storage layout introduced with NuGet 3.3</a> to store packages and will always copy packages that are added to the Packages folder manually to a subfolder that follows this new storage layout. More information about performance improvements added in the latest NuGet.Server package <a href="http://blog.nuget.org/20160113/Accelerate-your-NuGet.Server.html">can be found on our blog</a>.
</p> 

Now all we need to do is deploy this website as we would any other site and then
we can click NuGet's Settings button and add this feed to my set of package sources.

Note that the URL you need to put in is <a href="http://yourdomain/nuget/">http://yourdomain/nuget/</a> depending on how you deploy the site.

Yes, it's that easy! An alternative way of publishing packages to this server is by simply placing the nupkg under the 
the **Packages** folder and they are automatically syndicated.