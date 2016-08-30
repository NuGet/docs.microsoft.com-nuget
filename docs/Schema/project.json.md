# Project.json
<div class="block-callout-info">
    <strong>Note: </strong><br>
    This page talks about project.json in the context of UWP. For .NET Core take a look <a href="https://docs.microsoft.com/en-us/dotnet/articles/core/tools/project-json">.NET Core Tools - project.json</a>
</div>

Project.json is the file replacing packages.config as the file specifying the packages used by the project. For more information on the changes to package management this change entails, see the topic [here](project.json). This document provides a deeper dive into the content and shape of the file, as well as details on some advanced features not available directly through the UI. 

<div class="block-callout-info">
    <strong>The Future of project.json:</strong><br>
    When the tooling for ASP.NET Core and .NET Core hits RTM, project.json for portable apps and websites will be deprecated and contents of this file will move into MSBuild project files (.csproj/.vbproj etc..). NuGet clients will continue to support project.json for UWP and other csproj + project.json scenarios.
</div>

The project.json used by NuGet 3+ has the following basic shape 

    { 
        "dependencies": { "PackageId" : "1.0.0" }, 
        "frameworks" : { "TxM" : {} }, 
        "runtimes" : { "RID": {}, "RID": {} }, 
        "supports" : { "CompatibilityProfile" : {}, "CompatibilityProfile" : {} }    
    }
   
## Dependencies 

Lists the nuget package dependencies of your project in the form of: 

    PackageId : Version Constraint.  
    
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