# Package Best Practices

The purpose of this page is to present some best practices when creating nuget packages.

## Package Id Best Practices

**Namespace-like names**: Package IDs should follow a pattern similar to namespaces in .NET. For example, 
Ninject.Mvc3 instead of Ninject-Mvc3.

**Sample Packages**: Use the ".Sample" suffix for a package that provides sample code for your package. 
This is useful for helping others get started. For example, if your package name is Clay, then a sample 
of how to use Clay would be Clay.Sample. Within the content folder of sample packages, arrange the samples within a 
root /Samples/PackageID folder structure. For example, the Clay.Sample package would have a folder 
/Samples/Clay.

## Package Contents Best Practices

**App\_Start Folder**: When using the [WebActivator](http://nuget.org/List/Packages/WebActivator) package, 
put all the application startup code in a root **App\_Start** folder within the package's Content folder. 
See [this blog post](http://blog.davidebbo.com/2011/02/appstart-folder-convention-for-nuget.html) for additional information.

**Assemblies**: In general, it makes sense to have one nuget package per assembly. If your package 
contains assemblies that don’t make sense in any other context except within your package, then it’s fine to 
package multiple assemblies within your package. For example, if you have a library Foo.dll that depends on Bar.dll and you think other assemblies may depend on Bar.dll, then make two packages (one for each assembly). But if you have Foo.dll and Foo.resources.dll, then there’s no point in making two packages. Just put both in a single package.

## Package Versioning Conventions
**Versioning Guidelines**: To understand NuGet versioning, the following three part series is a very important 
(and quick!) read: NuGet Versioning [Part 1: Taking on DLL Hell](http://blog.davidebbo.com/2011/01/nuget-versioning-part-1-taking-on-dll.html), 
[Part 2: The core algorithm](http://blog.davidebbo.com/2011/01/nuget-versioning-part-2-core-algorithm.html), 
[Part 3: Unification via Binding Redirects](http://blog.davidebbo.com/2011/01/nuget-versioning-part-3-unification-via.html).

**Choosing a Version**: In general, it makes sense to give the package a version based on the library version, 
but make sure you consider NuGet versioning rules in case your library has a very non-standard versioning 
scheme. While we generally recommend that the version of the package matches the library, it is not required.
