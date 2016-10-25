# Finding and Choosing Packages

When starting any .NET project, or whenever you identify a functional need for your app or service, you can save yourself lots of time and trouble by using existing NuGet packages that fulfill that need. These packages can come from the public collection on [nuget.org](http://www.nuget.org/packages/), or a private source that's provided by your organization or another third party.

On this page:

- [Finding packages](#finding-packages)
- [Evaluating packages](#evaluating-packages)
- [Search syntax](#search-syntax)

## Finding packages

When you visit nuget.org or open the Package Manager UI in Visual Studio, you'll see a list of packages sorted by total downloads. This immediately shows you the most widely-used packages across the millions of .NET projects. There's a good chance, then, that at least some of the packages listed on the first few pages will be useful in your projects.

![Default view of nuget.org/packages showing the most popular packages](/images/Consume/Finding-01-Popularity.png) 

For specific needs, searching by tags (within Visual Studio's Package Manager or on a portal like nuget.org) is the most common means of discovering a suitable package. For example, searching on "json" will list all NuGet packages that are tagged with that keyword and thus have some relationship to the JSON data format. 

![Search results for 'json' on nuget.org](/images/Consume/Finding-02-SearchResults.png) 

You can also search using the package ID, if you know it. See [Search Syntax](#search-syntax) below.

At this time, search results are sorted only by relevance, so you generally want to look through at least the first few pages of results for packages that suit your needs, or refine your search terms to be more specific.


### Native C++ packages

As of [version 2.5](../Release-Notes/NuGet-2.5), NuGet supports native C++ packages can that can be used in C++ projects in Visual Studio. This enables the **Manage NuGet Packages** context-menu command for projects, introduces a `native` target framework, and provides MSBuild integration.

To find native packages on [nuget.org](https://www.nuget.org/packages), search using `tag:native`. Such packages typically provide `.targets` and `.props` files, which NuGet imports automatically when the package is added to a project. 

For information on creating native packages, see [Native packages](/ndocs/create-packages/native-packages).


## Evaluating packages

The best way to evaluate the usefulness of a package is, of course, to download it and try it out. After all, every highly popular package got started with only a few developers using it, and you might be one of the early adopters!

At the same time, using a NuGet package means taking a dependency on it, so you'll want to make sure it's robust and reliable. Because installing and directly testing a package is very time-consuming, you can also learn a lot about a package's quality by using the information on a package's listing page: 

- *Downloads statistics*: a the package page you'll see total downloads, downloads of the most recent version, and average downloads per day. Larger numbers obviously indicates that many other developers have taken a dependency on the package, which means that it has proven itself.
	![Download statistics on a package's listing page](/images/Consume/Finding-03-Downloads.png)

- *Version history*: on the package page, look at the date of the most recent update, and scroll down to see the version history. A well-maintained package will have recent updates, and will have a rich version history. Neglected packages will have few updates and often haven't been updated in some time. 

	![Version history on a package's listing page](/images/Consume/Finding-04-VersionHistory.png)

- *Recent installs*: on the package page, click **Package Statistics** on the left side. This will show you the package installs for the last six weeks. A package with more current use is obviously a better choice than one without.
- *Restore vs. new install*: on the same package statistics page, uncheck the **Version** checkbox and check **Operation** to see a chart of package restores vs. new installs (below).
 
	![Chart of operations for a NuGet package on nuget.org package statistics](/images/Consume/Finding-05-OperationChart.png)

- *Dependency installs*: the same **Operation** chart above has an "Install-Dependency" bar that indicates how many other NuGet packages have taken a dependency on this one. This is an even stronger indication that other developers find the package reliable.
-  *Support*: on the package page, click **Project Site** if available to see what support opions are available. A project with a dedicated site is generally better supported.
- *Developer history*: on the package page, click the listed **Owners** to see what other packages they've published. Those with multiple packages are more likely to continue supporting their work in the future.
- *Open source contributions*: many packages are maintained in an open-source repository, making it possible for developers depending on it to directly contribute bug fixes and feature improvements. The contribution history of any given package is also a good indicator of how many developers are actively involved.
- *Interview the owners*: all this is not to say that new developers are not equally committed to producing great packages for you to use, and it's certainly good to give them a chance to bring something new to the NuGet ecosystem. With this in mind, you can always talk directly to the package developers through the **Contact Owners** option on the listing page. Chances are, they'll be happy to work directly with you to serve your needs!

<div class="block-callout-info">
	<strong>Note</strong><br>
	Always be mindful of a package's licence terms, which you can see by clicking <strong>License</strong> on a package's listing page on nuget.org.
</div>

<div class="block-callout-info">
	<strong>Note</strong><br>
	Every package that is uploaded to nuget.org is scanned for viruses. All packages listed on nuget.org are also scanned periodically.
</div>


## Search Syntax

NuGet package search works the same on nuget.org, from the NuGet CLI, and within the NuGet Package Manager extension in Visual Studio. In general, search is applied to keywords as well as package descriptions.

- **Keywords**: Search will look for relevant packages that contain with all of the provided keywords. Example: 

	    modern UI javascript

- **Phrases**: Entering terms within quotation marks will look for exact matches to those terms (case-insensitive). Example:

    	"modern UI" package

- **Filtering**: You can apply a search term to a specific property by using the syntax `<property>:<term>` where <property> (case-insensitive) can be `id`, `packageid`, `version`, `title`, `tags`, `author`, `description`, `summary`, and `owner`. Terms can be contained in quotes if needed, and you can search for multiple properties at the same time. Also, searches on the `id` property are substring matches, whereas `packageid` uses an exact match. Examples:
           
	    id:NuGet.Core                //Match any part of the id property
		Id:"Nuget.Core"
		ID:jQuery
        title:jquery                 //Searches title as shown on the package listing
        PackageId:jquery             //Match the package id exactly
		id:jquery id:ui              //Search for multiple terms in the id
        id:jquery tags:validation    //Search multiple properties
        id:"jquery.ui"               //Phrase search
        invalid:jquery ui            //Unsupported properties are ignored, so this
                                     //is the same as searching on jquery ui
