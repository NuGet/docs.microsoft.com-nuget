# NuGet.exe CLI Reference

NuGet Command Line Interface (CLI) is used to create, publish, manage and download your packages from the command line instead of from Visual Studio. The [Install Guide](/ndocs/guides/install-nuget) gives an overview of the various ways to install nuget.exe on your box. 

## install

Downloads and extracts a package using the specified sources. If no sources are specified sources from NuGet.Config will be used.

This command does **not** modify the project file, it behaves similar to restore in that it only adds packages to disk. Projects using packages.config may only install new packages using Visual Studio.

### Usage

    nuget install <packageId|pathToPackagesConfig> [options]

Specify the id and optionally the version of the package to install. If a path to a packages.config file is used instead of an id, all the packages it contains will be restored.

### Options

<table>
    <tr>
        <td>configfile</td>
        <td>(v<em>2.5</em>) Specifies the NuGet configuration file. If not specified NuGet.Config
        is used.</td>
    </tr>
    <tr>
        <td>excludeversion</td>
        <td>If set, the destination directory will contain only the package name, not the version number.</td>
    </tr>
    <tr>
        <td>fileconflictaction</td>
        <td>(v<em>2.5</em>) Specifies the action to take, when asked to overwrite or ignore existing files referenced by the project: overwrite, ignore, none.</td>
    </tr>
    <tr>
        <td>help</td>
        <td>Displays help information for the install command.</td>
    </tr>
    <tr>
        <td>nocache</td>
        <td>Disables looking up packages from local machine cache.</td>
    </tr>
    <tr>
        <td>noninteractive</td>
        <td>Do not prompt for user input or confirmations.</td>
    </tr>
    <tr>
        <td>outputdirectory</td>
        <td>Specifies the directory in which packages will be installed. If none are specified, the current directory is used.</td>
    </tr>
    <tr>
        <td>prerelease</td>
        <td>Allows prerelease packages to be installed. This flag is not required when restoring packages with packages.config.</td>
    </tr>
    <tr>
        <td>requireconsent</td>
        <td>Checks if package restore consent is granted before downloading a package.</td>
    </tr>
    <tr>
        <td>solutiondirectory</td>
        <td>Specifies the solution root for package restore.</td>
    </tr>
    <tr>
        <td>source</td>
        <td>A list of package sources to use for the installation.</td>
    </tr>
    <tr>
        <td>verbosity</td>
        <td>Specifies the amount of details displayed in the output: normal, quiet, (v<em>2.5</em>) detailed.</td>
    </tr>
    <tr>
        <td>version</td>
        <td>The version of the package to install.</td>
    </tr>
</table>

### Examples

    nuget install elmah

    nuget install packages.config

    nuget install ninject -o c:\foo

##  update 

Updates packages to the latest available versions. The update command requires a Packages folder. It is recommended to run 'nuget.exe Restore' before running the Update command.

The update command will download and extract all new packages to the packages folders. Assembly references will be updated in the project file, however this is limited to only existing references. If a new package has an added assembly it will **not** be added as part of the update command. New package dependencies will also not have their assembly references added. To perform a complete update use Visual Studio.

This command can also be used to update nuget.exe itself using the *-self* flag.

### Usage
    nuget update [packages.config|solution] [options]

### Options
<table>
    <tr>
        <td>configfile</td>
        <td>(v<em>2.5</em>) Specifies the NuGet configuration file. If not specified NuGet.config
        is used.</td>
    </tr>
    <tr>
        <td>fileconflictactions</td>
        <td>(v<em>2.5</em>) Specifies the action to take, when asked to overwrite or ignore existing files referenced by the project: overwrite, ignore, none.</td>
    </tr>
    <tr>
        <td>help</td>
        <td>Displays help information for the update command.</td>
    </tr>
    <tr>
        <td>id</td>
        <td>A list of package ids to update.</td>
    </tr>
    <tr>
        <td>msbuildversion</td>
        <td>Specifies the version of MSBuild to be used with this command. Supported values are 4, 12, 14, 15. By default the MSBuild in your path is picked, otherwise it defaults to the highest installed version of MSBuild.</td>
    </tr>
    <tr>
        <td>noninteractive</td>
        <td>Do not prompt for user input or confirmations.</td>
    </tr>
    <tr>
        <td>prerelease</td>
        <td>Allows updating to prerelease versions. This flag is not required when updating prerelease packages that are already installed.</td>
    </tr>
    <tr>
        <td>repositorypath</td>
        <td>Path to the local folder where packages are installed.</td>
    </tr>
    <tr>
        <td>safe</td>
        <td>Specifies that only updates with the highest version available within the same major and minor version as the installed package will be installed.</td>
    </tr>
    <tr>
        <td>self</td>
        <td>(v<em>1.4</em>) Updates nuget.exe to the newest version available from the server.</td>
    </tr>
    <tr>
        <td>source</td>
        <td>A list of package sources to update from.</td>
    </tr>
    <tr>
        <td>verbose</td>
        <td>Shows verbose output while updating.</td>
    </tr>
    <tr>
        <td>verbosity</td>
        <td>Specifies the amount of details displayed in the output: normal, quiet, (v<em>2.5</em>) detailed.</td>
    </tr>
    <tr>
        <td>Version</td>
        <td>The version of the package to update. Requires -Id to contain exactly one package id.</td>
    </tr>
</table>

### Examples

    nuget update

    nuget update -safe

    nuget update -self

    # update packages installed in solution.sln, using msbuild version 14.0 to load the solution and its project(s).
    nuget update solution.sln -MSBuildVersion 14

## restore 

(v<em>2.7</em>) Downloads and unzips (restores) any packages missing from the packages folder.

(v<em>3.3.0</em>) Generates a project.lock.json file for projects using project.json.

### Usage
    nuget restore [solution|packages.config|project.json] [options]

### Options
<table>
    <tr>
        <td>configfile</td>
        <td>Specifies the user specific configuration file. If omitted, nuget.config is used instead.</td>
    </tr>
    <tr>
        <td>disableparallelprocessing</td>
        <td>Disable parallel nuget project restores and package downloads.</td>
    </tr>
    <tr>
        <td>help</td>
        <td>Displays help information for the restore command.</td>
    </tr>
    <tr>
        <td>msbuildversion</td>
        <td>Specifies the version of MSBuild to be used with this command. Supported values are 4, 12, 14, 15. By default the MSBuild in your path is picked, otherwise it defaults to the highest installed version of MSBuild.</td>
    </tr>
    <tr>
        <td>nocache</td>
        <td>Disables using the machine cache as the first package source.</td>
    </tr>
    <tr>
        <td>noninteractive</td>
        <td>Do not prompt for user input or confirmations.</td>
    </tr>
    <tr>
        <td>outputdirectory</td>
        <td>Specifies the packages directory. Identical to packagesdirectory.</td>
    </tr>
    <tr>
        <td>packagesdirectory</td>
        <td>Specifies the packages directory. Identical to outputdirectory.</td>
    </tr>
    <tr>
        <td>requireconsent</td>
        <td>Checks if package restore consent is granted before restoring a
        package.</td>
    </tr>
    <tr>
        <td>solutiondirectory</td>
        <td>Specifies the solution directory. Not valid when restoring
        packages for a solution.</td>
    </tr>
    <tr>
        <td>source</td>
        <td>A list of package sources to use for the install.</td>
    </tr>
    <tr>
        <td>verbosity</td>
        <td>Specifies the amount of details displayed in the output: normal, quiet, (v<em>2.5</em>) detailed.</td>
    </tr>
</table>

### Restore  Notes
The restore command is executed in the following steps:

1. Determine the operation mode of the restore command.
    * If &lt;solution> is specified, nuget restores packages for the
    solution's projects. In this case, nuget needs to locate the solution
    file.
        * If &lt;solution> is a file, that file is used as the solution
        file.
        * If &lt;solution> is a directory, then nuget searches for a *.sln
        file in that directory. If exactly one file is found, that file is
        used as the solution file. Otherwise, nuget displays an error
        message and exits.
        * If the -solutiondirectory option is specified, nuget will display an error and exit.
    * If &lt;packages.config file> is specified, nuget restores the packages
    listed in the packages.config file.
	* If &lt;project.json file> is specified, nuget restores packages listed in the project.json file, resolves the dependencies of those packages and installs those dependent packages as well.
    * If no argument is provided,
        * nuget first looks for solution files in the current
        directory. If there is just one solution file, nuget will
        restore packages for that solution. If there are multiple
        solution files, an error message is displayed and nuget exits.
        * If there are no solution files, nuget then searches for a
        packages.config file or project.json file in the current directory. If either file
        exists, nuget will restore packages listed in that file.
        * If there are no solution files or packages.config file in
        the current directory, an error message is displayed and
        nuget exits.

1. If nuget is restoring packages for a solution file,
  $(SolutionDir)\.nuget is used as the starting directory. If nuget is
  restoring packages from packages.config file, the directory of that
  file is used as the starting directory.

1. Calculate the packages directory. Nuget will use the following criteria, 
and as soon as one criteria is met, that directory is used.
	* If project.json is specified, %userprofile%\.nuget\packages is used as the packages directory.
    * If -PackagesDirectory &lt;packagesDirectory> is specified,
    &lt;packagesDirectory> is used as the packages directory.
    * If the repositoryPath key is found the nuget.config file, its
    value is used as the packages directory.
    * If -SolutionDirectory &lt;solutionDirectory> is specified,
    &lt;solutionDirectory>\packages is used as the packages directory.
    * If nuget is restoring packages for a solution, the
    $(SolutionDir)\packages is used as the packages directory.
    * If the packages directory cannot be determined, an error message is
    displayed and nuget exits.

1. When nuget is restoring packages for a solution, nuget takes the following steps:
    * Loads the solution file.
    * Restores solution level packages listed in
    $(SolutionDir)\\.nuget\packages.config file.
    * Restoes packages listed in $(ProjectDir)\packages.config file.
        * For each package specified, restore the package in parallel unless -disableparallelprocessing is specified. 
        * Downloads the package from the sources.
        * Unzips the packages to the packages directory.

### Examples

    # Restore packages for a solution file
    nuget restore a.sln

    # Restore packages for a solution file, using msbuild version 14.0 to load the solution and its project(s)
    nuget restore a.sln -MSBuildVersion 14

    # Restore packages for a project's packages.config file, with the packages folder at the parent
    nuget restore proj1\packages.config -PackagesDirectory ..\packages

    # Restore packages for the solution in the current folder, specifying package sources
    nuget restore -source "https://www.nuget.org/api/v2;https://www.myget.org/F/nuget"

##  delete 

Deletes or unlists a package from the server. For NuGet.org, the action is to [unlist the package](../Create/Deleting-Packages).

### Usage
    nuget delete <package Id> <package version> [API Key] [options]

Specify the Id and version of the package to delete or unlist a package from the server. 

This behavior varies depending on the source, nuget.org for example does not allow deletes and will unlist the package instead. For local folders the package will be deleted.

### Options
<table>
    <tr>
        <td>apikey</td>
        <td>The API key for the server.</td>
    </tr>
    <tr>
        <td>configfile</td>
        <td>(v<em>2.5</em>) Specifies the user specific configuration file. 
        If omitted, nuget.config is used instead.</td>
    </tr>
    <tr>
        <td>help</td>
        <td>Displays help information for the delete command.</td>
    </tr>
    <tr>
        <td>noninteractive</td>
        <td>Do not prompt for user input or confirmations.</td>
    </tr>
    <tr>
        <td>source</td>
        <td>Specifies the server URL. Supported URL's for nuget.org include - https://www.nuget.org,
        https://www.nuget.org/api/v3,
        https://www.nuget.org/api/v2/package. For private feeds, substitute the host name (e.g %hostname%/api/v3).</td>
    </tr>
    <tr>
        <td>verbosity</td>
        <td>Specifies the amount of details displayed in the output: 
        normal, quiet, (v<em>2.5</em>) detailed.</td>
    </tr>
</table>

### Examples

    nuget delete MyPackage 1.0

    nuget delete MyPackage 1.0 -NonInteractive

##  list 

Displays a list of packages from a given source. If no sources are specified, all sources defined in %AppData%\NuGet\NuGet.config are used. If NuGet.config specifies no sources, uses the default NuGet feed.

### Usage
    nuget list [search terms] [options]

Specify optional search terms.

### Options
<table>
    <tr>
        <td>allversions</td>
        <td>List all versions of a package. By default, only the latest package version is displayed.</td>
    </tr>
    <tr>
        <td>configfile</td>
        <td>(v<em>2.5</em>) Specifies the user specific configuration 
        file. If omitted, nuget.config is used instead.</td>
    </tr>
    <tr>
        <td>help</td>
        <td>Displays help information for the list command.</td>
    </tr>
    <tr>
        <td>noninteractive</td>
        <td>Do not prompt for user input or confirmations.</td>
    </tr>
    <tr>
        <td>prerelease</td>
        <td>Allow prerelease packages to be shown.</td>
    </tr>
    <tr>
        <td>source</td>
        <td>A list of packages sources to search.</td>
    </tr>
    <tr>
        <td>verbose</td>
        <td>Displays a detailed list of information for each package.</td>
    </tr>
    <tr>
        <td>verbosity</td>
        <td>Specifies the amount of details displayed in the output: 
        normal, quiet, (v<em>2.5</em>) detailed.</td>    </tr>
</table>

### Examples

    nuget list

    nuget list -verbose -allversions

##  sources 

Provides the ability to manage list of sources located in %AppData%\NuGet\NuGet.config or the specified config file.

### Usage
    nuget sources <List|Add|Remove|Enable|Disable|Update> -Name <name> -Source <source>

### Options
<table>
    <tr>
        <td>configfile</td>
        <td>(v<em>2.5</em>) Specifies the user specific configuration 
        file. If omitted, %appdata%\NuGet\nuget.config is used 
        instead.</td>
    </tr>
    <tr>
        <td>help</td>
        <td>Displays help information for the sources command.</td>
    </tr>
    <tr>
        <td>name</td>
        <td>Name of the source.</td>
    </tr>
    <tr>
        <td>noninteractive</td>
        <td>Do not prompt for user input or confirmations.</td>
    </tr>
    <tr>
        <td>password</td>
        <td>Password to be used when connecting to an authenticated source.</td>
    </tr>
    <tr>
        <td>source</td>
        <td>Path to the package(s) source.</td>
    </tr>
	<tr>
        <td>storepasswordincleartext</td>
        <td>If present, store the password in unencrypted text instead of 
        the default behavior of storing an encrypted form.</td>
    </tr>
    <tr>
        <td>username</td>
        <td>UserName to be used when connecting to an authenticated source.</td>
    </tr>
    <tr>
        <td>verbosity</td>
        <td>Specifies the amount of details displayed in the output: normal, quiet, (v<em>2.5</em>) detailed.</td>
    </tr>
</table>

##  spec 

Generates a nuspec for a new package. If this command is run in the same folder as a project file (.csproj, .vbproj, .fsproj), it will create a tokenized nuspec file.

### Usage
    nuget spec [package id]

### Options
<table>
    <tr>
        <td>assemblypath</td>
        <td>Assembly to use for metadata.</td>
    </tr>
    <tr>
        <td>configfile</td>
        <td>(v<em>2.5</em>) Specifies the user specific configuration file. 
        If omitted, %appdata%\NuGet\nuget.config is used instead.</td>
    </tr>
    <tr>
        <td>force</td>
        <td>Overwrite nuspec file if it exists.</td>
    </tr>
    <tr>
        <td>help</td>
        <td>Displays help information for the spec command.</td>
    </tr>
    <tr>
        <td>noninteractive</td>
        <td>Do not prompt for user input or confirmations.</td>
    </tr>
    <tr>
        <td>verbosity</td>
        <td>Display this amount of details in the output: normal, quiet, (v<em>2.5</em>) detailed.</td>
    </tr>
</table>

### Examples

    nuget spec

    nuget spec MyPackage

    nuget spec -a MyAssembly.dll

##  pack

<div class="block-callout-info">
	<strong>Note:</strong> Applies to v<em>2.7</em>+
</div>

Creates a NuGet package based on the specified nuspec or project file.

### Usage
    nuget pack <nuspec | project> [options]

### Options
<table>
    <tr>
    </tr>
    <tr>
        <td>basepath</td>
        <td>The base path of the files defined in the nuspec file.</td>
    </tr>
    <tr>
        <td>build</td>
        <td>Determines if the project should be built before building the package.</td>
    </tr>
    <tr>
        <td>exclude</td>
        <td>Specifies one or more wildcard patterns to exclude when creating a package.</td>
    </tr>
    <tr>
        <td>excludeemptydirectories</td>
        <td>Prevent inclusion of empty directories when building the package.</td>
    </tr>
    <tr>
        <td>help</td>
        <td>Displays help information for the pack command.</td>
    </tr>
    <tr>
        <td>includereferenceprojects</td>
        <td>Include referenced projects either as dependencies or as part of the package. If a referenced
        project has a corresponding nuspec file that has the same name as the project, then that
        referenced project is added as a dependency. Otherwise, the referenced project is added as part
        of the package.</td>
    </tr>
    <tr>
        <td>minclientversion</td>
        <td>Set the <strong>minClientVersion</strong> attribute for the created package. This value will override the value of the existing minClientVersion attribute (if any) in the .nuspec file.</td>
    </tr>
    <tr>
        <td>msbuildversion</td>
        <td>Specifies the version of MSBuild to be used with this command. Supported values are 4, 12, 14. By default the MSBuild in your path is picked, otherwise it defaults to the highest installed version of MSBuild.</td>
    </tr>
    <tr>
        <td>noninteractive</td>
        <td>Do not prompt for user input or confirmations.</td>
    </tr>
    <tr>
        <td>nodefaultexcludes</td>
        <td>Prevent default exclusion of NuGet package files and files and folders starting with a dot e.g. .svn.</td>
    </tr>
    <tr>
        <td>nopackageanalysis</td>
        <td>Specify if the command should not run package analysis after building the package.</td>
    </tr>
    <td>outputdirectory</td>
        <td>Specifies the directory for the created NuGet package file. 
        If not specified, the current directory is used.</td>
    </tr>
    <tr>
        <td>properties</td>
        <td>Provides the ability to specify a semicolon ";" delimited list of properties when creating a package.</td>
    </tr>
    <tr>
        <td>symbols</td>
        <td>Determines if a package containing sources and symbols should be created. When specified with a nuspec, creates a regular NuGet package file and the corresponding symbols package.</td>
    </tr>
    <tr>
        <td>tool</td>
        <td>Determines if the output files of the project should be in the tool folder. </td>
    </tr>
    <tr>
        <td>verbose</td>
        <td>Shows verbose output for package building.</td>
    </tr>
    <tr>
        <td>verbosity</td>
        <td>Display this amount of details in the output: normal, quiet, 
        detailed.</td>
    </tr>
    <tr>
        <td>version</td>
        <td>Overrides the version number from the nuspec file.</td>
    </tr>
</table>

### Excluding development dependencies

Some NuGet packages are useful as development dependencies, which help you author your own library, but aren't necessarily needed as actual package dependencies.

The pack command will ignore `package` entries in the packages.config 
file which have the **developmentDependency** attribute set to **true**. 
These entries will not be include as a dependencies in the created package. 
For example, consider the following packages.config file in the source 
project:

	<?xml version="1.0" encoding="utf-8"?>
	<packages>
		<package id="jQuery" version="1.5.2" />
		<package id="netfx-Guard" version="1.3.3.2" developmentDependency="true" />
		<package id="microsoft-web-helpers" version="1.15" />
	</packages>

When running the `nuget pack` on this project, the created package will 
have a dependency on **jQuery** and **microsoft-web-helpers**, but will 
*not* have a dependency on **netfx-Guard**.

### Examples

    nuget pack

    nuget pack foo.nuspec

    nuget pack foo.csproj

    nuget pack foo.csproj -Build -symbols -properties Configuration=Release

    # create a package from project foo.csproj, using msbuild version 12 to build the project
    nuget pack foo.csproj -Build -symbols -properties Configuration=Release -MSBuildVersion 12

    nuget pack foo.nuspec -version 2.1.0

    nuget pack foo.nuspec -version 1.0.0 -minclientversion 2.5

##  push 

Pushes a package to the server and publishes it. NuGet's default configuration is obtained by loading %AppData%\NuGet\NuGet.config, then loading any nuget.config or .nuget\nuget.config starting from root of drive and ending in current directory.

### Usage
    nuget push <package path> [API key] [options]

Specify the path to the package and your API key to push the package to the server.

### Options
<table>
    <tr>
        <td>apikey</td>
        <td>The API key for the server.</td>
    </tr>
    <tr>
        <td>configfile</td>
        <td>(v<em>2.5</em>) Specifies the user specific configuration 
        file. If omitted, nuget.config is used 
        instead. </td>
    </tr>
    <tr>
        <td>help</td>
        <td>Displays help information for the push command.</td>
    </tr>
    <tr>
        <td>noninteractive</td>
        <td>Do not prompt for user input or confirmations.</td>
    </tr>
    <tr>
        <td>source</td>
        <td>Specifies the server URL. Starting with NuGet 3.4.2, this is 
        a mandatory parameter unless the NuGet.config file specifies a 
        DefaultPushSource value.
        <br />
        Starting with NuGet 2.5, if nuget.exe identifies a UNC/folder 
        source, it will copy the file to the source.
        </td>
    </tr>
    <tr>
        <td>timeout</td>
        <td>Specifies the timeout for pushing to a server in seconds. 
        Defaults to 300 seconds (5 minutes).</td>
    </tr>
    <tr>
        <td>verbosity</td>
        <td>Specifies the amount of details displayed in the output: normal, 
        quiet, (v<em>2.5</em>) detailed.</td>
    </tr>
</table>

### Examples

    Starting with NuGet 3.4.2, -Source is a mandatory parameter unless DefaultPushSource config value is set in the NuGet config file.
    nuget push foo.nupkg 4003d786-cc37-4004-bfdf-c4f3e8ef9b3a -Source https://www.nuget.org/api/v2/package

    nuget push foo.nupkg 4003d786-cc37-4004-bfdf-c4f3e8ef9b3a

    nuget push foo.nupkg 4003d786-cc37-4004-bfdf-c4f3e8ef9b3a -s https://customsource/

    nuget push foo.nupkg

    nuget push foo.symbols.nupkg

    nuget push foo.nupkg -Timeout 360

    nuget push *.nupkg

    Starting with NuGet 2.5 push supports a UNC/Folder source
    nuget.exe push -source \\mycompany\repo\ mypackage.1.0.0.nupkg

##  setapikey 

Saves an API key for a given server URL. When no URL is provided API key 
is saved for the NuGet gallery.

### Usage
    nuget setapikey <API key> [options]

### Options
<table>
    <tr>
        <td>configfile</td>
        <td>(v<em>2.5</em>) The NuGet configuration file. If not specified, file %AppData%\NuGet\NuGet.config
        is used as configuration file.</td>
    </tr>
    <tr>
        <td>help</td>
        <td>Displays help information for the setapikey command.</td>
    </tr>
    <tr>
        <td>noninteractive</td>
        <td>Do not prompt for user input or confirmations.</td>
    </tr>
    <tr>
        <td>source</td>
        <td>Server URL where the API key is valid.</td>
    </tr>
    <tr>
        <td>verbosity</td>
        <td>Specifies the amount of details displayed in the output: normal, 
        quiet, (v<em>2.5</em>) detailed.</td>
    </tr>
</table>

### Examples

    nuget setapikey 4003d786-cc37-4004-bfdf-c4f3e8ef9b3a

    nuget setapikey 4003d786-cc37-4004-bfdf-c4f3e8ef9b3a -source https://example.com/nugetfeed

## init 

(v<em>3.3</em>) Adds all the packages from a flat folder of 
nupkgs to the destination package source in a hierarchical layout as 
described below. When restoring or updating against your package source, 
this layout has significant performance benefits. For this command, both 
the source package source and the destination package source must be a folder
or a UNC share. In order to expand all the files in the package to the 
destination package source, use the `-expand` switch.  

    \\destinationpackagesource\
        yourpackage\
            0.0.1-beta\
                yourpackage.0.0.1-beta.nupkg
                yourpackage.nuspec
                yourpackage.0.0.1-beta.nupkg.sha512

### Usage
    nuget init <srcPackageSourcePath> <destPackageSourcePath> options]

### Options
<table>
    <tr>
        <td>expand</td>
        <td>If provided, all the files in the package(s) are added to destination package source.</td>
    </tr>
    <tr>
        <td>help</td>
        <td>Displays help information for the init command.</td>
    </tr>
    <tr>
        <td>verbosity</td>
        <td>Specifies the amount of details displayed in the output: normal, quiet, (v<em>2.5</em>) detailed.</td>
    </tr>
</table>

### Examples

    nuget init c:\foo c:\bar

    nuget init \\foo\packages \\bar\packages

## add

<div class="block-callout-info">
	<strong>Note:</strong> Applies to v<em>3.3</em>+
</div>

Adds a specified package to your package source
in a hierarchical layout as described below. When restoring or updating against your package source, 
this layout has significant performance benefits. For 
this command, your package source must be a folder or a UNC share. In 
order to expand all the files in the package to the destination package 
source, use the `-expand` switch.

    \\yourpackagesource\
        yourpackage\
            0.0.1-beta\
                yourpackage.0.0.1-beta.nupkg
                yourpackage.nuspec
                yourpackage.0.0.1-beta.nupkg.sha512

### Usage
    nuget add <packagePath> -Source <folderBasedPackageSource> [options]

<div class="block-callout-info">
    <strong>Note: </strong><br>
    Http sources are not supported.
</div>

### Options
<table>
    <tr>
        <td>expand</td>
        <td>If provided, all the files in the package are added to your package source.</td>
    </tr>
    <tr>
        <td>help</td>
        <td>Displays help information for the add command.</td>
    </tr>
    <tr>
        <td>source</td>
        <td>Specifies the folderBasedPackageSource to which the nupkg will be added. Http sources are not supported.</td>
    </tr>
    <tr>
        <td>verbosity</td>
        <td>Specifies the amount of details displayed in the output: normal, quiet, detailed.</td>
    </tr>
</table>

### Examples

    nuget add foo.nupkg -source c:\bar\

    nuget add foo.nupkg -source \\bar\packages\

##  config 

Gets or sets NuGet config values.

### Usage
    nuget config -set name=value

### Options
<table>
    <tr>
        <td>configfile</td>
        <td>The NuGet configuration file. If not specified, file
        %AppData%\NuGet\NuGet.config is used as configuration file.</td>
    </tr>
    <tr>
        <td>help</td>
        <td>Displays help information for the config command.</td>
    </tr>
    <tr>
        <td>noninteractive</td>
        <td>Do not prompt for user input or confirmations.</td>
    </tr>
    <tr>
        <td>set</td>
        <td>One or more key-value pairs to be set in the configuration.</td>
    </tr>
    <tr>
        <td>verbosity</td>
        <td>Specifies the amount of details displayed in the output: 
        normal, quiet, (v<em>2.5</em>) detailed.</td>
    </tr>
</table>

### Examples

    nuget config -set HTTP_PROXY=http://127.0.0.1 -set HTTP_PROXY.USER=domain\user

## locals

<div class="block-callout-info">
	<strong>Note:</strong> Applies to v<em>3.3</em>+
</div>

Clears or lists local NuGet resources such as http request cache, packages cache, or machine-wide global packages folder.

	nuget locals <target> -clear

Where &lt;target> is one of the following options: all, http-cache, 
packages-cache, global-packages, (v<em>3.4</em>) temp.
### Options

<table>
	<tr>
		<td>clear</td>
		<td>Clear the resources in the specified cache location.</td>
	</tr>
	<tr>
		<td>help</td>
		<td>Displays help information for the config command.</td>
	</tr>
	<tr>
		<td>list</td>
		<td>List the resources of the cache locations.</td> 
	</tr>
	<tr>
		<td>verbosity</td>
		<td>Specifies the amount of details displayed in the output: 
        normal, quiet, detailed.</td>
	</tr>
</table>

##  mirror

Mirrors a package and its dependencies from the specified source repositories to the target repository. 

<div class="block-callout-info">
    <strong>Note: Deprecated from v3.2+</strong><br>
    To enable this command for versions of NuGet before v<em>3.2</em>, navigate to <a href="https://nuget.codeplex.com/releases">https://nuget.codeplex.com/releases</a>, select newest stable release, download NuGet.ServerExtensions.dll and Nuget-Signed.exe to your local disk and rename the Nuget-Signed.Exe to nuget.exe..  
</div>

### Usage
    nuget mirror <packageId|pathToPackagesConfig> <listUrlTarget> <publishUrlTarget> [options]

Specify the id of the package to mirror, the url to query the target repository (list ) and the url to push packages to the target repository. If a path to a packages.config file is used instead of a package id, all the packages it contains are mirrored to the given version (if specified) or latest otherwise.

Assuming you're targeting a private repository under [https://machine/repo](https://machine/repo) installed using NuGet.Server, the list and push urls will be [https://machine/repo/nuget](https://machine/repo/nuget) and [https://machine/repo/api/v2/package](https://machine/repo/api/v2/package) respectively.

### Options
<table>
    <tr>
        <td>apikey</td>
        <td>The API key for pushing to the target repository. If not 
        specified, the one specified in %AppData%\NuGet\NuGet.config 
        is used.</td>
    </tr>
    <tr>
        <td>help</td>
        <td>Displays help information for the mirror command.</td>
    </tr>
    <tr>
        <td>nocache</td>
        <td>Disables the default behavior of using a local cache when a 
        package or a package dependency is not found in the specified 
        sources. Instead, only the specified sources will be used.
    </tr>
    <tr>
        <td>noop</td>
        <td>Log what would be done without actually doing it. Assumes success for push operations.</td>
    </tr>
    <tr>
        <td>prerelease</td>
        <td>When set, prereleased packages will be mirrored.</td>
    </tr>
    <tr>
        <td>source</td>
        <td>A list of package sources to mirror.
        If no sources are specified, the ones defined in %AppData%\NuGet\NuGet.config are used.
        If the default NuGet config file specifies no sources, the default NuGet feed will be used.</td>
    </tr>
    <tr>
        <td>timeout</td>
        <td>Specifies the timeout for pushing to a server in seconds. 
        Defaults to 300 seconds (5 minutes).</td>
    </tr>
    <tr>
        <td>version</td>
        <td>The version of the package to install. If not specified, 
        latest version is mirrored.</td>
    </tr>
</table>

### Examples

    nuget mirror packages.config  https://MyRepo/nuget https://MyRepo/api/v2/package -source https://nuget.org/api/v2 -apikey myApiKey -NoCache

    nuget mirror Microsoft.AspNet.Mvc https://MyRepo/nuget https://MyRepo/api/v2/package -version 4.0.20505.0

    nuget mirror Microsoft.Net.Http https://MyRepo/nuget https://MyRepo/api/v2/package -prerelease

##  help 

Displays general help information and help information about other commands.

### Usage
    nuget help [command]

Pass a command name to display help information for that command.
### Options
<table>
    <tr>
        <td>all</td>
        <td>Print detailed help for all available commands.</td>
    </tr>
    <tr>
        <td>help</td>
        <td>Displays help information for the help command.</td>
    </tr>
    <tr>
        <td>markdown</td>
        <td>Print detailed all help in markdown format.</td>
    </tr>
    <tr>
        <td>noninteractive</td>
        <td>Do not prompt for user input or confirmations.</td>
    </tr>
    <tr>
        <td>verbosity</td>
        <td>Specifies the amount of details displayed in the 
        output: normal, quiet, (v<em>2.5</em>) detailed.</td>
    </tr>
</table>

### Examples

    nuget help

    nuget help push

    nuget ?

    nuget push -?
