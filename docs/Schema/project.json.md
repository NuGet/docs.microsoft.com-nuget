# Project.json Reference

*NuGet 3.x+; also see the end of this topic for project.lock.json*

The `project.json` file replaces `packages.config` in UWP, ASP.NET 5, and .NET desktop projects (WPF and WinForms) as a means to maintain the list of packages used in a project. (For .NET Core, refer to [NET Core Tools - project.json](https://docs.microsoft.com/dotnet/articles/core/tools/project-json)).

The [`project.lock.json`](#project-lock-json) file (described below) is also used in projects with a `project.json` file.

<div class="block-callout-info">
    <strong>Note</strong><br>
	When tooling for .NET Core and ASP.NET Core is complete, the function of project.json will be incorporated directly into project files (.csproj, .vsproj, etc.). NuGet clients will continue to support project.json for existing scenarios.
</div>

A project.json file has the following basic structure, where each of the four top-level objects can have any number of child objects:

    { 
        "dependencies": {
			"PackageID" : "{version_constraint}"
		}, 
        "frameworks" : {
			"TxM" : {}
		}, 
        "runtimes" : {
			"RID": {}
		}, 
        "supports" : {
			"CompatibilityProfile" : {}
		}    
    }
   
## Dependencies 

Lists the nuget package dependencies of your project in the form of: 

    "PackageID" : "version_constraint"
    
For example: 

    "dependencies": {     
       "Microsoft.NETCore": "5.0.0", 
       "System.Runtime.Serialization.Primitives": "4.0.10"     
    } 

The dependencies section is where the NuGet Package Manager dialog will add package dependencies to your project. 

The Package id corresponds to the id of the package on nuget.org , the same as the id used in the package manager console: `Install-Package Microsoft.NETCore` 

The version constraint of **"5.0.0"** corresponds to the **>= 5.0.0** constraint. This means that if for some reason 5.0.0 is not available on the server and 5.0.1 is, the restore will pick 5.0.1 and warn you about the upgrade. Otherwise restore will pick the lowest possible version on the server matching the constraint which will be 5.0.0. 

See [dependency resolution document]() for more details on resolution rules. 

## Frameworks

This lists the frameworks that your project will run on. E.g. net45, netcoreapp, netstandard. 

    "frameworks": { 
        "netcore50": {}  
     } 

Unlike the project.json used by ASP.NET Core a project.json that is being used with other project types can only have a single entry in the frameworks section. This is because the build system, MSBuild, only ever builds for a single target in contrast to DNX where the build is run once for each of the targets. 

## Runtimes 

The Operating System and Architectures that your application will be running on. For example, win7-x64, win8-x64, win8-x86. 

If you are a portable class library that can run on any runtime, you don't need to specify a runtime. Of course any dependencies of your package have to run on any runtime as well. 

	"runtimes": { 
        "win10-arm": { }, 
	    "win10-arm-aot": { }, 
	    "win10-x86": { }, 
	    "win10-x86-aot": { }, 
	    "win10-x64": { }, 
	    "win10-x64-aot": { } 
	} 


## Supports 

Defines a set of checks for package dependencies. You can define where you expect the portable library/application to run, it is not restrictive, you may be able to run elsewhere but specifying things here will make NuGet check that all dependencies are able to be satisfied on the listed TxMs. Examples of the values for this are: net46.app,uwp.10.0.app etc...

This section should be populated automatically when you select an entry in the Portable Class Library targets dialog. 

	supports": { 
	    "net46.app": {}, 
	    "uwp.10.0.app": {} 
	} 

## Imports

Imports are designed to allow packages that use the dotnet TxM to operate with packages that don't declare a dotnet TxM. If your project is using the dotnet TxM then all the packages you depend on must also have a dotnet TxM, unless you add the following to your project.json in order to allow non dotnet platforms to be compatible with dotnet. If you are using the dotnet TxM then the PCL project system will add the appropriate imports statement based on the supported targets. 

	"frameworks": { 
    	"dotnet": { "imports" : "portable-net45+win81" } 
	} 



## Differences from Portable Apps and Web Projects

The project.json file used by NuGet is a subset of that found in ASP.NET Core projects. In ASP.NET Core the project.json file is used for project metadata, compilation information, and dependencies. When used in other project systems those three things are split into separate files so the project.json specifies less information. Notable differences include: 

There can only be one framework in the frameworks section 

The framework should be empty, as shown above. No dependencies, compilation options, etc that you can see in DNX project.json files. Given that there can only be a single framework it doesn't make sense to enter framework specific dependencies. 

Compilation is handled by MSBuild so compilation options, preprocessor defines, etc are all part of the MSBuild project file and not your project.json.  

In NuGet 3 unlike in ASP.NET Core projects the user is not expected to manually edit the file, the UI is responsible for manipulating the content. we're working on unifying the experiences across the project systems for project.json, its not unified yet for this release. 

Note that it is possible to edit the file, the user is responsible to build the project to kick off a package restore.


# project.lock.json

The `project.lock.json` file is generated in the process of restoring the NuGet packages in projects that use `project.json`. It holds a snapshot of all the information that is generated as NuGet walks the graph of packages and includes the version, contents, and dependencies of all the packages in your project. The build system uses this to choose packages from a global location that are relevant when building the project instead of depending on a local packages folder in the project itself. This results in faster build performance because it's necessary to read only `project.lock.json` instead of many separate `.nuspec` files.

The `project.lock.json` is automatically generated on package restore, so it can be omitted from source control by adding it to `.gitignore` and `.tfignore` files. However, if you include it in source control, the change history will show changes in dependencies resolved over time. 
  