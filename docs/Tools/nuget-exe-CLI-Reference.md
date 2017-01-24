---
# required metadata

title: NuGet Command-Line Interface (CLI) Reference | Microsoft Docs
author: kraigb
ms.author: kraigb
manager: ghogen
ms.date: 1/9/2017
ms.topic: article
ms.prod: nuget
#ms.service:
ms.technology: nuget
ms.assetid: d777c424-0cf3-4bc0-8abd-7ca16c22192b

# optional metadata

description: The complete command-line reference for nuget.exe and information on the use of environment variables.
keywords: nuget.exe reference, nuget.exe command-line interface, nuget.exe CLI, NuGet commands
#ROBOTS:
#audience:
#ms.devlang:
ms.reviewer:
- karann
- unnir
#ms.suite:
#ms.tgt_pltfrm:
#ms.custom:

---

# NuGet CLI Reference

The NuGet Command Line Interface (CLI) provides the full extent of NuGet functionality to install, create, publish, and manage packages. Refer to the [Install Guide](../guides/install-nuget.md) for installation instructions.

Available commands are listed below. Also see the section on [Environment variables](#environment-variables) for how they're used with nuget.exe.

Command | Description | NuGet Version
--- | --- | ---
[add](#add) | Adds a package to a non-HTTP package source using hierarchical layout. For HTTP sources, use *push*. | 3.3+
[config](#config) | Gets or sets NuGet configuration values. | All
[delete](#delete) | Removes or unlists a package from a package source. | All
[help or ?](#help) | Displays help information or help for a command. | All
[init](#init) | Adds packages from a folder to a package source using hierarchical layout. | 3.3+
[install](#install) | Installs a package into the current project but does not modify the project or packages.config. | All
[list](#list) | Displays packages from a given source. | All
[locals](#locals) | Clears or lists packages in various caches or the global packages folder, or identifies those folders. | 3.3+
[mirror](#mirror) | Mirrors a package and its dependencies from a source to a target repository. | Deprecated in 3.2+
[pack](#pack) | Creates a NuGet package from a .nuspec or project file. | 2.7+
[push](#push) | Publishes a package to a package source. | All
[restore](#restore) | Restores all packages referenced by packages.config or project.json. | 2.7+
[setapikey](#setapikey) | Saves an API key for a given package source. | All
[sources](#sources) | Manages package sources in configuration files. | All
[spec](#spec) | Generates a .nuspec file, using tokens if generating the file from a Visual Studio project. | All
[update](#update) | Updates a project's packages to the latest available versions. | All


## add

*Version 3.3+*

Adds a specified package to a non-HTTP package source (a folder or UNC path) in a hierarchical layout, wherein folders are created for the package ID and version number. For example:

    \\myserver\packages
      └─<packageID>
        └─<version>
          ├─<packageID>.<version>.nupkg
          ├─<packageID>.<version>.nupkg.sha512
          └─<packageID>.nuspec

When restoring or updating against the package source, hierarchical layout provides significantly better performance.

To expand all the files in the package to the destination package source, use the `-expand` switch. This typically results in additional subfolders appearing in the destination, such as `tools` and `lib`.

### Usage

```bash
    nuget add <packagePath> -source <sourcePath> [options]
```

where &lt;packagePath&gt; is the pathname to the package to add, and &lt;sourcePath&gt; specifies the folder-based package source to which the package will be added. HTTP sources are not supported.

### Options

| | |
--- | ---
expand |If provided, all the files in the package are added to your package source.
help | Displays help information for the command.
fileconflictaction | *(2.5+)* Specifies the action to take when asked to overwrite or ignore existing files referenced by the project. Values are *overwrite, ignore, none*.
verbosity | Specifies the amount of details displayed in the output: *normal*, *quiet*, *detailed*.

### Examples

```bash
    nuget add foo.nupkg -source c:\bar\

    nuget add foo.nupkg -source \\bar\packages\
```

## config

Gets or sets NuGet config values. For additional usage, see [Configuring NuGet Behavior](../consume-packages/configuring-nuget-behavior.md). For details on allowable key names, refer to the [NuGet config file reference](../Schema/nuget-config-file.md).

### Usage

```bash
    nuget config -set <name>=<value> [<name>=<value> ...] [options]
```

where &lt;name&gt; and &lt;value&gt; specify a key-value pair to be set in the configuration. You can specify as many pairs as desired.

In NuGet 3.4+, &lt;value&gt; can be use environment variables.


### Options

| | |
  --- | ---
configfile | *(2.5+)* The NuGet configuration file to modify. If not specified, *%AppData%\NuGet\NuGet.config* is used.
help | Displays help information for the command.
noninteractive | Suppresses prompts for user input or confirmations.
verbosity | Specifies the amount of details displayed in the output: *normal*, *quiet*, *detailed (2.5+)*.


### Examples

```bash
    nuget config -set repositoryPath=c:\packages -configfile c:\my.config

    nuget config -set repositoryPath=%PACKAGE_REPO% -configfile %ProgramData%\NuGet\NuGetDefaults.Config

    nuget config -set HTTP_PROXY=http://127.0.0.1 -set HTTP_PROXY.USER=domain\user
```

## delete

Deletes or unlists a package from a package source. For nuget.org, the action is to [unlist the package](../policies/Deleting-Packages.md).

### Usage

```bash
    nuget delete <packageID> <packageVersion> [options]
```

where &lt;packageID&gt; and &lt;packageVersion&gt; identify the exact package to delete or unlist. The exact behavior depends on the source. For local folders, for instance, the package is deleted; for nuget.org the package is unlisted.

### Options

|     |     |
| --- | --- |
apikey | The API key for the target repository. If not present,  the one specified in *%AppData%\NuGet\NuGet.config* is used.
configfile | *(2.5+)* The NuGet configuration file to modify. If not specified, *%AppData%\NuGet\NuGet.config* is used.
help | Displays help information for the command.
noninteractive | Suppresses prompts for user input or confirmations.
source | Specifies the server URL. Supported URLs for nuget.org include *https://www.nuget.org, https://www.nuget.org/api/v3, https://www.nuget.org/api/v2/package*. For private feeds, substitute the host name, for example, *%hostname%/api/v3*.
verbosity | Specifies the amount of details displayed in the output: *normal*, *quiet*, *detailed (2.5+)*.

### Examples

```bash
    nuget delete MyPackage 1.0

    nuget delete MyPackage 1.0 -source http://package.contoso.com/source -apikey A1B2C3
```

## help

Displays general help information and help information about specific commands.

### Usage

```bash
    nuget help [command] [options]
    nuget ? [command] [options]
```

where [command] identifies a specific command for which to display help.

> [!Warning]
> With some commands, be mindful to specify *help* first, as with *nuget help install*, because there is a package named "help" on nuget.org. If you give the command *nuget install help*, you'll not get help on the install command but will instead install the help package.

### Options

|     |     |
| --- | --- |
all | Print detailed help for all available commands; ignored if a specific command is given.
help | Displays help information for the help command itself.
markdown | Print detailed help in markdown format when used with -all. Ignored otherwise.
noninteractive | Suppresses prompts for user input or confirmations.
verbosity | Specifies the amount of details displayed in the output: *normal*, *quiet*, *detailed (2.5+)*.

### Examples

```bash
    nuget help
    nuget help push
    nuget ?
    nuget push -?
    nuget help -all -markdown
```

## init

*Version 3.3+*

Copies all the packages from a flat folder to a destination folder using a hierarchical layout as described for the [add command](#add) above. That is, using `init` is equivalent to using the `add` command on each package in the folder.

As with `add`, the destination must be either a local folder or a UNC path; HTTP package repositories such as nuget.org or private servers are not supported.


### Usage

```bash
    nuget init <source> <destination> [options]
```

where &lt;source&gt; is the folder containing packages and &lt;destination&gt; is the local folder or UNC pathname to which the packages will be copied.

### Options

|     |     |
| --- | --- |
expand | Adds the files in the package(s) to destination package source.
help | Displays help information for the command.
verbosity | Specifies the amount of details displayed in the output: *normal*, *quiet*, *detailed (2.5+)*.

### Examples

```bash
    nuget init c:\foo c:\bar
    nuget init \\foo\packages \\bar\packages -expand
```

## install

Downloads and installs a package into a project using the specified package sources. If no sources are specified, those listed in the global configuration file, `%APPDATA%\NuGet\NuGet.Config`, will be used. See [Configuring NuGet Behavior](../consume-packages/configuring-nuget-behavior.md) for additional details.

If no specific packages are specified, `install` installs all packages listed in the project's `packages.config` file, making it similar to [`restore`](#restore). In this case, `install` does not work with projects that use `project.json`.

The `install` command does not modify a project file or `packages.config`; in this way it's similar to `restore` in that it only adds packages to disk but does not change a project's dependencies.

To add a dependency, either add a project through the Package Manager UI or Console in Visual Studio, or modify `packages.config` and then run either `install` or `restore`. For projects using `project.json`, you can modify that file and then run `restore`.


### Usage

```bash
    nuget install <packageID | configFilePath> [options]
```

where &lt;packageID&gt; names the package to install (using the latest version), or &lt;configFilePath&gt; identifies the `packages.config` file that lists the packages to install. You can indicate a specific version with the `-version` option.


### Options

|     |     |
| --- | --- |
configfile | *(2.5+)* The NuGet configuration file to modify. If not specified, *%AppData%\NuGet\NuGet.config* is used.
excludeversion | Excludes the version number from the installation folder.
fileconflictaction | *(2.5+)* Specifies the action to take when asked to overwrite or ignore existing files referenced by the project. Values are *overwrite, ignore, none*.
help | Displays help information for the command.
nocache | Prevents NuGet from using packages from local machine caches.
noninteractive | Suppresses prompts for user input or confirmations.
outputdirectory | Specifies the folder in which packages are installed. If no folder is specified, the current folder is used.
prerelease | Allows prerelease packages to be installed. This flag is not required when restoring packages with *packages.config*.
requireconsent | Verifies that restoring packages is enabled before downloading and installing the packages. For details, see [Package Restore](../consume-packages/package-restore.md).
solutiondirectory | Specifies root folder of the solution for which to restore packages.
source | Specifies a list of package sources to use.
verbosity | Specifies the amount of details displayed in the output: *normal*, *quiet*, *detailed (2.5+)*.
version | Specifies the version of the package to install.

### Examples

```bash
    nuget install elmah

    nuget install packages.config

    nuget install ninject -outputdirectory c:\proj
```

##  list

Displays a list of packages from a given source. If no sources are specified, all sources defined in the global configuration file, `%AppData%\NuGet\NuGet.config`, are used. If `NuGet.config` specifies no sources, then `list` uses the default feed (nuget.org).

### Usage

```bash
    nuget list [search terms] [options]
```

where the optional search terms will filter the displayed list. Search terms are applied to the names of packages, tags, and package descriptions.

### Options
|     |     |
| --- | --- |
allversions | List all versions of a package. By default, only the latest package version is displayed.
configfile | *(2.5+)* The NuGet configuration file to modify. If not specified, *%AppData%\NuGet\NuGet.config* is used.
help | Displays help information for the command.
noninteractive | Suppresses prompts for user input or confirmations.
prerelease | Includes prerelease packages in the list.
source | Specifies a list of packages sources to search.
verbose | Displays a detailed list of information for each package.
verbosity | Specifies the amount of details displayed in the output: *normal*, *quiet*, *detailed (2.5+)*.

### Examples

```bash
    nuget list

    nuget list -verbose -allversions
```

## locals
*Version 3.3+*

Clears or lists local NuGet resources such as the http-request cache, packages cache, and the machine-wide global packages folder. The `locals` command can also be used to display a list of those locations. For more information, see [Managing the NuGet Cache](../consume-packages/managing-the-nuget-cache.md).

### Usage

```bash
    nuget locals <cache> [options]
```

where &lt;cache&gt; is one of `all`, `http-cache`, `packages-cache`, `global-packages`, and `temp` *(3.4+)*.


### Options

|     |     |
| --- | --- |
clear | Clear the specified cache.
help | Displays help information for the command.
list | List the location of the specified cache, or the locations of all caches when used with *all*.
verbosity | Specifies the amount of details displayed in the output: *normal*, *quiet*, *detailed*.

### Examples

```bash
    nuget locals all -list
    nuget locals http-cache -clear
```

## mirror

*Deprecated in 3.2+*

Mirrors a package and its dependencies from the specified source repositories to the target repository.

> [!NOTE]
> To enable this command for NuGet versions before 3.2, go to [https://nuget.codeplex.com/releases](https://nuget.codeplex.com/releases), select the newest stable release, download `NuGet.ServerExtensions.dll` and `Nuget-Signed.exe` to your local disk and rename `Nuget-Signed.exe` to `nuget.exe`.

### Usage

```bash
    nuget mirror <packageID | configFilePath> <listUrlTarget> <publishUrlTarget> [options]
```

where &lt;packageID&gt; is the package to mirror, or &lt;configFilePath&gt; identifies the `packages.config` file that lists the packages to mirror.

The &lt;listUrlTarget&gt; specifies the source repository, and &lt;publishUrlTarget&gt; specifies the target repository.

If your target repository is on https://machine/repo that's running [NuGet.Server](../hosting-packages/NuGet-Server.md), the list and push urls will be *https://machine/repo/nuget* and *https://machine/repo/api/v2/packag*e, respectively.

### Options

|     |     |
| --- | --- |
apikey | The API key for the target repository. If not present,  the one specified in *%AppData%\NuGet\NuGet.config* is used.
help | Displays help information for the command.
nocache | Prevents NuGet from using packages from local machine caches.
noop | Logs what would be done but does not perform the actions; assumes success for push operations.
prerelease | Includes prerelease packages in the mirroring operation.
source | A list of package sources to mirror. If no sources are specified, the ones defined in *%AppData%\NuGet\NuGet.config* are used, defaulting to nuget.org if none are specified.
timeout | Specifies the timeout, in seconds, for pushing to a server. The default is 300 seconds (5 minutes).
version | The version of the package to install. If not specified, the latest version is mirrored.

### Examples

```bash
    nuget mirror packages.config  https://MyRepo/nuget https://MyRepo/api/v2/package -source https://nuget.org/api/v2 -apikey myApiKey -nocache

    nuget mirror Microsoft.AspNet.Mvc https://MyRepo/nuget https://MyRepo/api/v2/package -version 4.0.20505.0

    nuget mirror Microsoft.Net.Http https://MyRepo/nuget https://MyRepo/api/v2/package -prerelease
```

##  pack

*Version 2.7+*

Creates a NuGet package based on the specified nuspec or project file. Note that the `pack` command requires MSBuild and will not work on Linux systems. On Mac OS X, you need to have Mono 4.4.2 or later installed.

With Mono, you also need to adjust non-local paths in the .nuspec file to Unix-style paths, as nuget.exe will not itself convert Windows pathnames.

### Usage

```bash
    nuget pack <nuspecPath | projectPath> [options]
```

where &lt;nuspecPath&gt; and &lt;projectPath&gt; specify the `.nuspec` or project file, respectively.

### Options

|     |     |
| --- | --- |
basepath | Sets the base path of the files defined in the nuspec file.
build | Specifies that the project should be built before building the package.
exclude | Specifies one or more wildcard patterns to exclude when creating a package.
excludeemptydirectories | Prevent inclusion of empty directories when building the package.
help | Displays help information for the command.
includereferencedprojects | Indicates that the built package should include referenced projects either as dependencies or as part of the package. If a referenced project has a corresponding nuspec file that has the same name as the project, then that referenced project is added as a dependency. Otherwise, the referenced project is added as part of the package.
minclientversion | Set the *minClientVersion* attribute for the created package. This value will override the value of the existing *minClientVersion* attribute (if any) in the .nuspec file.
msbuildversion | Specifies the version of MSBuild to be used with this command. Supported values are 4, 12, 14. By default the MSBuild in your path is picked, otherwise it defaults to the highest installed version of MSBuild.
nodefaultexcludes | Prevents default exclusion of NuGet package files and files and folders starting with a dot, such as *.svn*.
nopackageanalysis | Specifies that pack should not run package analysis after building the package.
outputdirectory | Specifies the folder in which the created package is stored. If no folder is specified, the current folder is used.
properties | Specifies a list of token=value pairs, separated by semicolons, where each occurrence of $token$ in the nuspec file will be replaced with the given value. Values can be strings in quotation marks.
suffix | Appends a suffix to the internally generated version number, typically used for appending build or other pre-release identifiers. For example, using `-suffix nightly` will create a package with a version number like `1.2.3.45-nightly`.
symbols | Specifies that the package contains sources and symbols. When used with a with a nuspec file, this creates a regular NuGet package file and the corresponding symbols package.
tool | Specifies that the output files of the project should be placed in the tool folder.
verbosity | Specifies the amount of details displayed in the output: *normal*, *quiet*, *detailed*.
version | Overrides the version number from the nuspec file.

### Excluding development dependencies

Some NuGet packages are useful as development dependencies, which help you author your own library, but aren't necessarily needed as actual package dependencies.

The `pack` command will ignore `package` entries in `packages.config`
that have the `developmentDependency` attribute set to `true`. These entries will not be include as a dependencies in the created package.

For example, consider the following `packages.config` file in the source project:

```xml
    <?xml version="1.0" encoding="utf-8"?>
    <packages>
        <package id="jQuery" version="1.5.2" />
        <package id="netfx-Guard" version="1.3.3.2" developmentDependency="true" />
        <package id="microsoft-web-helpers" version="1.15" />
    </packages>
```

For this project, the package created by `nuget pack` will have a dependency on `jQuery` and `microsoft-web-helpers` but not `netfx-Guard`.

### Examples

```bash
    nuget pack

    nuget pack foo.nuspec

    nuget pack foo.csproj

    nuget pack foo.csproj -Build -symbols -properties owners=janedoe,xiaop;version="1.0.5"

    # create a package from project foo.csproj, using MSBuild version 12 to build the project
    nuget pack foo.csproj -Build -symbols -properties owners=janedoe,xiaop;version="1.0.5" -MSBuildVersion 12

    nuget pack foo.nuspec -version 2.1.0

    nuget pack foo.nuspec -version 1.0.0 -minclientversion 2.5
```

##  push

Pushes a package to a package source and publishes it.

NuGet's default configuration is obtained by loading `%AppData%\NuGet\NuGet.config`, then loading any `nuget.config` or `.nuget\nuget.config` files starting from root of drive and ending in current directory (see [Configuring NuGet Behavior](../consume-packages/configuring-nuget-behavior.md))

### Usage

```bash
    nuget push <packagePath> [options]
```

where &lt;packagePath&gt; identifies the package to push to the server.

### Options

|     |     |
| --- | --- |
apikey | The API key for the target repository. If not present,  the one specified in *%AppData%\NuGet\NuGet.config* is used.
configfile | *(2.5+)* The NuGet configuration file to modify. If not specified, *%AppData%\NuGet\NuGet.config* is used.
help | Displays help information for the command.
noninteractive | Suppresses prompts for user input or confirmations.
source | Specifies the server URL. With NuGet 2.5+, NuGet will identify a UNC or local folder source and simply copy the file there instead of pushing it using HTTP.  Also, starting with NuGet 3.4.2, this is a mandatory parameter unless the NuGet.config file specifies a *DefaultPushSource* value.
timeout | Specifies the timeout, in seconds, for pushing to a server. The default is 300 seconds (5 mintues).
verbosity | Specifies the amount of details displayed in the output: *normal*, *quiet*, *detailed (2.5+)*.

### Examples

```bash
    nuget push foo.nupkg

    nuget push foo.symbols.nupkg

    nuget push foo.nupkg -Timeout 360

    nuget push *.nupkg

    nuget.exe push -source \\mycompany\repo\ mypackage.1.0.0.nupkg

    nuget push foo.nupkg 4003d786-cc37-4004-bfdf-c4f3e8ef9b3a -Source https://www.nuget.org/api/v2/package

    nuget push foo.nupkg 4003d786-cc37-4004-bfdf-c4f3e8ef9b3a

    nuget push foo.nupkg 4003d786-cc37-4004-bfdf-c4f3e8ef9b3a -s https://customsource/
```


## restore

*Version 2.7+*

NuGet 2.7+: Downloads and installs any packages missing from the `packages` folder.

NuGet 3.3+ with projects using `project.json`: Generates a `project.lock.json` file and a `<project>.nuget.props` file, if needed. (Both files can be omitted from source control.)

NuGet 4.0+ with project in which package references are included in the project file directly: Generates a `<project>.nuget.props` file, if needed, in the `obj` folder. (The file can be omitted from source control.)

### Usage

```bash
    nuget restore <projectPath> [options]
```

where &lt;projectPath&gt; specifies the location of a solution, a `packages.config` file, or a `project.json` file. See [Remarks](#remarks) below for behavioral details.

### Options

|     |     |
| --- | --- |
configfile | The NuGet configuration file to modify. If not specified, *%AppData%\NuGet\NuGet.config* is used.
disableparallelprocessing | Disables parallel NuGet project restores and package downloads.
help | Displays help information for the command.
msbuildversion | Specifies the version of MSBuild to be used with this command. Supported values are 4, 12, 14, 15. By default the MSBuild in your path is picked, otherwise it defaults to the highest installed version of MSBuild.
nocache | Prevents NuGet from using packages from local machine caches.
noninteractive | Suppresses prompts for user input or confirmations.
outputdirectory or -packagesdirectory | Specifies the folder in which packages are installed. If no folder is specified, the current folder is used.
requireconsent | Verifies that restoring packages is enabled before downloading and installing the packages. For details, see [Package Restore](../consume-packages/package-restore.md).
solutiondirectory | Specifies the solution folder. Not valid when restoring packages for a solution.
source | Specifies list of package sources to use for the restore.
verbosity | >Specifies the amount of details displayed in the output: *normal*, *quiet*, *detailed (2.5+)*.

### Remarks

The restore command is executed in the following steps:

1. Determine the operation mode of the restore command.
    projectPath file type | Behavior
    | --- | --- |
    Solution (folder) | NuGet looks for a .sln file and uses that if found; otherwise gives an error. $(SolutionDir)\.nuget is used as the starting folder.
    .sln file | Restore packages identified by the solution; gives an error if -solutiondirectory is used. $(SolutionDir)\.nuget is used as the starting folder.
    packages.config, project.json, or project file | Restore packages listed in the file, resolving and installing dependencies.
    Other file type | File is assumed to be a .sln file as above; if it's not a solution, NuGet gives an error.
    (projectPath not specified) | - NuGet looks for solution files in the current folder. If a single file is found, that one is used to restore packages; if multiple solutions are found, NuGet gives an error.
    |- If there are no solution files, NuGet looks for a packages.config or project.json and uses that to restore packages.
    |- If no solution file, packages.config, or project.json is found, NuGet gives an error.

1. Determine the packages folder using the following priority order (NuGet gives an error if none of these folders are found):

    - The `%userprofile%\.nuget\packages` value in `project.json`.
    - The folder specified with `-packagesdirectory`.
    - The `repositoryPath` vale in `nuget.config`
    - The folder specified with `-solutiondirectory`
    - `$(SolutionDir)\packages`


1. When restoring packages for a solution, NuGet does the following:
    - Loads the solution file.
    - Restores solution level packages listed in `$(SolutionDir)\.nuget\packages.config` into the `packages` folder.
    - Restore packages listed in `$(ProjectDir)\packages.config` into the `packages` folder. For each package specified, restore the package in parallel unless `-disableparallelprocessing` is specified.

### Examples

```bash
    # Restore packages for a solution file
    nuget restore a.sln

    # Restore packages for a solution file, using MSBuild version 14.0 to load the solution and its project(s)
    nuget restore a.sln -MSBuildVersion 14

    # Restore packages for a project's packages.config file, with the packages folder at the parent
    nuget restore proj1\packages.config -PackagesDirectory ..\packages

    # Restore packages for the solution in the current folder, specifying package sources
    nuget restore -source "https://www.nuget.org/api/v2;https://www.myget.org/F/nuget"
```

## setapikey

Saves an API key for a given server URL into `NuGet.Config` so that it doesn't need to be entered for subsequent commands.

### Usage

```bash
    nuget setapikey <key> -source <url> [options]
```

where &lt;source&gt; identifies the server and &lt;key&gt; is the key or password to save. If &lt;source&gt; is omitted, nuget.org is assumed.

### Options

|     |     |
| --- | --- |
configfile | *(2.5+)* The NuGet configuration file to modify. If not specified, *%AppData%\NuGet\NuGet.config* is used.
help | Displays help information for the command.
noninteractive | Suppresses prompts for user input or confirmations.
verbosity | Specifies the amount of details displayed in the output: *normal*, *quiet*, *detailed (2.5+)*.

### Examples

```bash
    nuget setapikey 4003d786-cc37-4004-bfdf-c4f3e8ef9b3a

    nuget setapikey 4003d786-cc37-4004-bfdf-c4f3e8ef9b3a -source https://example.com/nugetfeed
```

## sources

Manages the list of sources located in `%AppData%\NuGet\NuGet.config` or the specified configiration file.

### Usage

```bash
    nuget sources <operation> -Name <name> -Source <source>
```

where &lt;operation&gt; is one of *List, Add, Remove, Enable, Disable,* or *Update*, &lt;name&gt; is the name of the source, and &lt;source&gt; is the source's URL.


### Options

|     |     |
| --- | --- |
configfile | *(2.5+)* The NuGet configuration file to modify. If not specified, *%AppData%\NuGet\NuGet.config* is used.
help | Displays help information for the command.
noninteractive | Suppresses prompts for user input or confirmations.
password | Specifies the password for authenticating with the source.
storepasswordincleartext | Indicates to store the password in unencrypted text instead of the default behavior of storing an encrypted form.
username | Specifies the user name for authenticating with the source.
verbosity | Specifies the amount of details displayed in the output: *normal*, *quiet*, *detailed (2.5+)*.

> [!Note]
> Make sure to add the sources' password under the same user context as the NuGet.exe is later used to access the package source. The password will be stored encrypted in the config file and can only be decrypted in the same user context as it was encrypted. So for example when you use a build server to restore NuGet packages the password must be encrypted with the same Windows user under which  the build server task will run.


### Examples

```bash
    nuget sources Add "MyServer" \\myserver\packages

    nuget sources Disable "MyServer"

    nuget source Enable "nuget.org"
```

## spec

Generates a `.nuspec` file for a new package. If run in the same folder as a project file (`.csproj`, `.vbproj`, `.fsproj`), `spec` creates a tokenized `.nuspec` file. For additional information, see [Creating a Package](../create-packages/creating-a-package.md).

### Usage

```bash
    nuget spec [<packageID>] [options]
```

where &lt;packageID&gt; is an optional package identifier to save in the `.nuspec` file.

### Options

|     |     |
| --- | --- |
assemblypath | Specifies the path to the assembly to use for metadata.
configfile | *(2.5+)* The NuGet configuration file to modify. If not specified, *%AppData%\NuGet\NuGet.config* is used.
force | Overwrites any existing .nuspec file.
help | Displays help information for the command.
noninteractive | Suppresses prompts for user input or confirmations.
verbosity | Specifies the amount of details displayed in the output: *normal*, *quiet*, *detailed (2.5+)*.

### Examples

```bash
    nuget spec

    nuget spec MyPackage

    nuget spec -a MyAssembly.dll
```

## update

Updates all packages in a project the latest available versions. It is recommended to run ['restore'](#restore) before running the `update`.

Note: `update` does not work with projects using `project.json`.

The `update` command will also update assembly references in the project file, provided those references already exist. If an updated package has an added assembly, a new reference will *not* be added. New package dependencies will also not have their assembly references added. To include these operations as part of an update, update the package in Visual Studio using the Package Manager UI or the Package Manager Console.

This command can also be used to update nuget.exe itself using the *-self* flag.

### Usage

```bash
    nuget update <configPath> [options]
```

where &lt;configPath&gt; identifies either a `packages.config` or solution file that lists the project's dependencies.

### Options

|     |     |
| --- | --- |
configfile | *(2.5+)* The NuGet configuration file to modify. If not specified, *%AppData%\NuGet\NuGet.config* is used.
fileconflictaction | *(2.5+)* Specifies the action to take when asked to overwrite or ignore existing files referenced by the project. Values are *overwrite, ignore, none*.
help | Displays help information for the command.
id | Specifies a list of package IDs to update.
msbuildversion | Specifies the version of MSBuild to be used with this command. Supported values are 4, 12, 14, 15. By default the MSBuild in your path is picked, otherwise it defaults to the highest installed version of MSBuild.
noninteractive | Suppresses prompts for user input or confirmations.
prerelease | Allows updating to prerelease versions. This flag is not required when updating prerelease packages that are already installed.
repositorypath | Specifies the local folder where packages are installed.
safe | Specifies that only updates with the highest version available within the same major and minor version as the installed package will be installed.
self | *(1.4+)* Updates nuget.exe to the latest version; all other arguments are ignored.
source | Specifies a list of package sources to use for the updates.
verbose | Shows verbose output while updating.
verbosity | Specifies the amount of details displayed in the output: *normal*, *quiet*, *detailed (2.5+)*.
version | When used with one package ID, specifies the version of the package to update.

### Examples

```bash
    nuget update

    # update packages installed in solution.sln, using MSBuild version 14.0 to load the solution and its project(s).
    nuget update solution.sln -MSBuildVersion 14

    nuget update -safe

    nuget update -self
```

## Environment variables

The behavior of nuget.exe can be configured through a number of environment variables, which affect nuget.exe on machine, user, or process levels.

In general, options specified directly on the command line or in the NuGet configuration files have precedence, but there are a few exceptions such as *FORCE_NUGET_EXE_INTERACTIVE*. If you find that nuget.exe behaves differently between machines, an environment variable could be the cause. For example, Azure Web Apps Kudu (used during deployment) has *NUGET_XMLDOC_MODE* set to *skip* to speed up package restore performance and save disk space.

|Variable|Description|Remarks|
| --- | --- | --- |
http_proxy | Http proxy used for NuGet HTTP operations. | This would be specified as *http://&lt;username&gt;:&lt;password&gt;@proxy.com*.
no_proxy | Configures domains to bypass from using proxy. | Specified as domains separated by comma (,).
EnableNuGetPackageRestore | Flag for if NuGet should implicitly grant consent if that's required by package on restore. | Specified flag is specified as *true* or *1*, any other value treated as flag not set.
NUGET_EXE_NO_PROMPT | Prevents the exe for prompting for credentials.| Any value except null or empty string will be treated as this flag set/true.
FORCE_NUGET_EXE_INTERACTIVE | Global environment variable to force interactive mode. | Any value except null or empty string will be treated as this flag set/true.
NUGET_PACKAGES | Path to where packages are stored / cached. | Specified as absolute path.
NUGET_FALLBACK_PACKAGES | Global fallback packages folders. | Absolute folder paths separated by semicolon (;).
NUGET_HTTP_CACHE_PATH | HTTP cache folder. | Specified as absolute path.
NUGET_PERSIST_DG | Flag indicating if dg files (data collected from MSBuild) should be persisted. | Specified as *true* or *false* (default), if NUGET_PERSIST_DG_PATH not set will be stored to temporary directory (NuGetScratch folder in current environment temp directory).
NUGET_PERSIST_DG_PATH | Path to persist dg files. | Specified as absolute path, this option is only used when *NUGET_PERSIST_DG* is set to true.
NUGET_RESTORE_MSBUILD_ARGS | Sets additional MSBuild arguments. |
NUGET_RESTORE_MSBUILD_VERBOSITY | Sets the MSBuild log verbosity. | Default is *quiet* ("/v:q"). Possible values *q[uiet]*, *m[inimal]*, *n[ormal]*, *d[etailed]*, and *diag[nostic]*.
NUGET_SHOW_STACK | Determines whether the full exception (including stack trace) should be displayed to the user. | Specified as *true* or *false* (default).
NUGET_XMLDOC_MODE | Determines how assemblies XML documentation file extraction should be handled. | Supported modes are *skip* (do not extract XML documentation files), *compress* (store XML doc files as a zip archive) or *none* (default, treat XML doc files as regular files).
