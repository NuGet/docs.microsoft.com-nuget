# Package Consumption Overview and Workflow

Between nuget.org and private package galleries that your organization might establish, you can find tens of thousands of highly useful packages to use in your apps and services (including packages that support native C++ projects; see [Native C++ Packages](/ndocs/consume-packages/finding-and-choosing-packages#native-c++-packages)). But regardless of the source, consuming a package follows the same general workflow:

![Flow of going to a package source, finding a package, installing it in a project, then adding a using statement and calls to the package API](/images/Consume/Overview-01-GeneralFlow.png)

For details, see [Finding and Choosing Packages](/ndocs/consume-packages/finding-and-choosing-packages) and the [Use a Package quickstart](/ndocs/quickstart/use-a-package).

NuGet remembers the identity and version number of each installed package, recording it in either `packages.config` (NuGet 2.x) or `project.json` (NuGet 3.x) in your project root. You can look in the appropriate file at any time to see the full list of  dependencies for your project. 

When installing packages, NuGet typically checks if the package is already available from its cache. You can manually clear this cache from the command line, as described on [Managing the NuGet cache](/ndocs/consume-packages/managing-the-nuget-cache).

When adding project code to a source repository, you typically don't include NuGet packages. Those who later clone the repository, which includes build agents on systems like Visual Studio Team Services, must restore the necessary packages prior to running a build:

![Flow of restoring NuGet packages by cloning a repository and using either a restore command](/images/Consume/Overview-02-RestoreFlow.png)

[Package Restore](/ndocs/consume-packages/package-restore) uses the information in `packages.config` or `project.json` to reinstall all dependencies. Note that there are differences in the process between NuGet 2.x and 3.x, which are described in [Dependency Resolution](/ndocs/consume-packages/dependency-resolution). 

Occasionally it's necessary to reinstall packages that are already included in a project, which may also reinstall dependencies. This is easy to do using the `reinstall` command via the NuGet command line or the NuGet Package Manager Console. For details, see [Reinstalling and Updating Packages](/ndocs/consume-packages/reinstalling-and-updating-packages).

Finally, NuGet's behavior is driven by `nuget.config` configuration files. Multiple files can be used to centralize certain settings at different levels, as explained in [Configuring NuGet Behavior](/ndocs/consume-packages/configuring-nuget-behavior). 
 
Enjoy your productive coding with NuGet packages!
  