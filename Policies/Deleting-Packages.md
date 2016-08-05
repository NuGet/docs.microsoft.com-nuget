# Deleting packages

NuGet.org does not support permanent deletion of packages, because that would break anyone who is depending on it
remaining available. This is particularly true when using the workflow that restores packages on build.

Instead, NuGet.org supports a way to 'unlist' a package, which can be done in the package management page on the
web site. When a package is unlisted, it no longer shows up in search and in any package listing, both on NuGet.org
and from the NuGet Visual Studio extension (or nuget.exe). However, it remains downloadable by specifying its exact
version, which is what allows the Restore workflow to continue working.


## Exceptions

If you run into an exceptional situation where you think one of your packages must be deleted, this can be handled
manually by the NuGet team. e.g. if there is a copyright infringement issue, or potentially harmful content, that could be a valid reason to delete it. You should submit a support request through [NuGet Gallery] (http://www.nuget.org) in that case.