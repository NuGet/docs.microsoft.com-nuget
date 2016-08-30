# Local Feeds


## Creating Local Feeds

Local feeds come in two flavors: a folder of nupkg files and a versioned set of hierarchical folders.  Starting with NuGet 3.3, you can create and install packages [much faster](http://blog.nuget.org/20150922/Accelerate-Package-Source.html) with the hierarchical folder structure.

You can create a local feed on disk or build your own remote feed using NuGet's server components by following the instructions below.

Begin by creating or getting the packages you want to include in the custom feed and then putting them all into a folder. In the following example, a folder has been created in the local *c:* drive. The folder contains a single package (.nupkg file).

![LocalNuGetFeed-folder.png](/images/create/LocalNuGetFeed-folder.png)

Now that you have the folder setup, it is super easy to add this as a package source in Visual Studio. Take a look at this [topic](../tools/package-manager-ui#package-sources) to understand how to add a new package source in Visual Studio.

## Creating Local Feeds v3.3+

Starting with NuGet 3.3, you can create and manage a folder of packages that can be referenced by the NuGet clients in the same way as described in the previous section. To manage this folder, you will need a copy of the [NuGet command-line tool](http://dist.nuget.org/index.html).

Start by creating an empty folder that will contain the new hierarchical NuGet feed.  Let's refer to that folder as `$folder`.  If you have an existing folder of packages at $existing that you would like to add to `$folder`, execute the following command:

    nuget init $existing $folder

As new and updated packages like **freshnizzle.nupkg** are created that you would like to add to the folder, execute the following command:

    nuget add package.nupkg -source $folder

Finally, add a reference to `$folder` in your NuGet configuration as specified above.