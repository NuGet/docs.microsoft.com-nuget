# Project.lock.json

The project.lock.json is a file that is generated in the process of restoring the NuGet packages. It is a snapshot of all the information that is generated as NuGet walks the graph of packages and includes the version, contents, and dependencies of all the packages your application depends on. 

## Overview

In NuGet v3 all packages are downloaded only once to a user level packages directory. The project.lock.json provides the package information to the build system to allow it to only pick packages from the global location that are relevant for the project it is building.  

It stores the list of files and relevant content for compilation and runtime so that the build system only has to read a single file instead of many nuspec files.  

## Checking in the lock file

<div class="block-callout-warning">
    <strong>Deprecated: Locked Property</strong><br>
    When the tooling for ASP.NET Core and .NET Core hits RTM, project.json for portable apps and websites will be deprecated and contents of this file will move into MSBuild project files (.csproj/.vbproj etc..). NuGet clients will continue to support project.json for UWP and other csproj + project.json scenarios.
</div>

In general it is not necessary to check in the lock file. A lock file will be auto generated whenever a restore happens, so you can safely leave it out of source control and avoid accidental merge conflicts. On git source control system and newer versions of TFS a .gitignore/.tfignore can be used to prevent this file from being checked in accidentally. 

Checking in a lock file is possible, and the diff will show the changes in dependencies resolved overtime. 

## Differences from Portable Apps and Web Projects

The project.json file used by NuGet is a subset of that found in ASP.NET Core projects. In ASP.NET Core the project.json file is used for project metadata, compilation information, and dependencies. When used in other project systems those three things are split into separate files so the project.json specifies less information. Notable differences include: 

There can only be one framework in the frameworks section 

The framework should be empty, as shown above. No dependencies, compilation options, etc that you can see in DNX project.json files. Given that there can only be a single framework it doesn't make sense to enter framework specific dependencies. 

Compilation is handled by MSBuild so compilation options, preprocessor defines, etc are all part of the MSBuild project file and not your project.json.  

In NuGet 3 unlike in ASP.NET Core projects the user is not expected to manually edit the file, the UI is responsible for manipulating the content. we're working on unifying the experiences across the project systems for project.json, its not unified yet for this release. 

Note that it is possible to edit the file, the user is responsible to build the project to kick off a package restore.  