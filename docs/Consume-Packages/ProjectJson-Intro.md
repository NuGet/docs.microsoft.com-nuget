# NuGet 3.0, .NET and project.json 

NuGet 3.0 introduces two new client concepts 

1. Transitive package restore 

2. New package format 

These changes bring a lot of flexibility, performance enhancements and support for new scenarios, as well as a few breaking changes. 

These two new concepts enabled .NET to ship in the form of NuGet packages instead of globally installed. 

Starting from Visual Studio 2015, several project types are utilizing this technology, with more to follow in the future. 

1. Universal Windows Platform managed apps (UWP). 

2. Portable class libraries (PCL). 

3. ASP.NET Core applications. 

These changes enable .NET to span multiple platforms with a common surface area as well as evolve rapidly and ship updates directly on NuGet.org.  

## Transitive Package Restore  ##

In projects using NuGet we are going to have two types of configuration files. 

1. Packages.config 

2. Project.json – Added in NuGet 3.0 

Packages.config is the format utilized up until NuGet 3 with any project consuming NuGet packages. It is an XML file with a flat list of packages. When the user installs a package into the project the NuGet UI will resolve the dependent packages add them them to the file, and modify the project files. 

In a C# project, that will mean changing the .csproj file, and sometimes the app.config/web.config file as well as other files within the project. 


In contrast if the project is using project.json file, installing a package simply means adding the package id and version to the project.json file. There is no dependency resolution at this point nor a change to the project file. 

When packages are restored (or simply said downloaded) from the NuGet.org server, a restore process kicks in and downloads these packages to the user’s computer. 

At this point the project system picks up the assembly references and updates them in memory, without modifying any of the files on disk. The underlying mechanism for picking up these references is an addition to the common msbuild targets. 

The build system can choose to pick up other conventions in the package. This is how msbuild targets are supported as well as Roslyn analyzers. It is possible to build packages that supports targets and analyzers for packages.config and project.json scenarios. 

## How does msbuild task know what to pick up?  ##

The restore process runs ahead of the build itself, it then writes out a new file called project.lock.json. The file includes the package graph as well as other useful information about the packages restored. MSBuild reads the file and translates it into a set of folders where potential references can be found, and then adds them to the project tree in memory. 

**Where is this file dropped** – Right next to the project.json 

**Should I check it in** – Typically no, the file is included in the default .gitignore in Visual Studio normally not included in source control either through .gitignore or .tfignore.

For a deeper dive on dependency resolution and restore decision making refer to the Dependency Resolution document. 

project.json is mandatory for UWP apps and ASP.NET 5 apps, is optional for PCLs, and currently is not fully supported on other project systems. 


## Advantages of Transitive Restore  ##

1. Ability of the developer to declare explicitly what packages he depends on, without worrying about what dependencies they bring in with them. 

2. Ability for the developer to pick version of dependencies directly. 

3. Ability for the developer to easily change dependent packages without having to worry about the side effects of editing the rest of the files in the project. 

4. Project files are not modified, avoiding merge conflicts and file churn on commits. 

5. Hint paths are not being burned into the project files, fixing the ability to move projects around on disk, and retargeting problems. 

6. Enabling the project system to evolve without being dependent on NuGet features. 

7. Direct support for Native or special dependencies that can be determined directly by the project system – This also relates to the new package format. 

8. The ability to use a floating version range e.g. 2.8.*, and avoids the expensive and error prone calls to NuGet update on the client machines and CI servers. 


## Changes affecting existing packages usage  ##

Traditional NuGet packages support a set of features that are not carried over to the transitive world. 

### Install/Uninstall scripts 

These scripts are not supported and will be ignored. In case they exist in the package a project using transitive restore. 

The main reason we removed support for this is that in the transitive model, is that there is no concept of package install time. A package is either present or not present, but there is no consistent process that occurs when a package is installed. Install scripts only worked in Visual Studio and other IDE had to mock Visual Studio extensibility API in an attempt to support this scenario. But common editors have no support, and similarly command line tools have no support for running these scripts. 


### Content Transforms 

Similarly to install scripts, transforms run on package install and are typically not idempotent. Since there is no install time anymore, XDT Transform and similar features are not supported, and will be ignored if such a package is used in a transitive scenario. 


### Content 

Traditional NuGet packages are shipping content files such as source code and configuration files. There are used typically in two scenarios 

1. Initial files dropped into the project so the user can edit them at a later time. The common example is default configuration files. 

2. Content files used as companions to the assemblies installed in the project. The example here would be a logo image used by an assembly. 

Support for content is currently disabled for similar reasons for scripts and transforms, but we are in the process of designing support for content. 

Content files can still be carried inside the packages, and will be ignored currently, however the end user can still copy them into the right spot. 

You can see one of the proposals for bringing back content files, and follow its progress, here: [https://github.com/NuGet/Home/issues/627](https://github.com/NuGet/Home/issues/627)   

## Impact for Package Authors  ##

Packages using the above features would have to use a different mechanism. The most commonly useful mechanism for this would be the MSBUILD targets/props that continues to get fully supported. 

Packages that attempt to modify the project to ease startup, typically work in a very limited set of scenarios, and will instead provide a readme, or guidance on how to use the package. 

Most existing packages should not need to use the new package format described below. 

The new format enables native content as a first class scenario. This means that managed assemblies depending on close to hardware implementations to ship binary implementations alongside the managed assemblies based on the target platform. For example System.IO.Compression package is utilizing this technology.  [https://www.nuget.org/packages/System.IO.Compression](https://www.nuget.org/packages/System.IO.Compression)  


In summary if the functionality above is not absolutely necessary, we recommend sticking with the existing package format. The new format will be supported only by NuGet 3.0. 

It would be possible to build packages to work for both packages.config and project.json scenarios through shimming, however it is often simpler to just structure the packages the traditional way, without the deprecated features mentioned above. 


## New Package Format  ##

The new package format allows for several new features 

1. Defining a reference assembly used for compilation and a set of implementation assemblies used for runtime on different platforms/devices. Which allows you to take advantage of platform specific APIs while providing a common surface area for your consumers. Specifically this makes writing intermediate portable libraries easier. 

2. Allows packages to pivot on platforms e.g. operating systems or CPU architecture. 

3. Allows for separation of platform specific implementations to companion packages. 

4. Support Native dependencies as a first class citizen. 
