#Overview and Workflow

Creating a package starts with the code you want to package and share with others, either through the public nuget.org gallery or a private gallery within your organization. The package can also include additional files such as a readme that is displayed when the package is installed, and can include transformations to certain project files.

A package can also serve only to pull in a number of other dependencies and not contain any code of its own, which is a convenient way to create a single package for an SDK that's composed of multiple independent packages. In other cases, a package may contain only symbol (.pdb) files to aid debugging.

To learn and understand the creation process, start with [Creating a package](/ndocs/create-packages/creating-a-package) which will guide you through the core processes common to all packages. This includes deciding which assemblies to package, creating the `.nuspec` (manifest) file, choosing a package identity and version number, setting a package type, adding a readme, and including MSBuild props and targets. The topic ends with creating the package itself using the `nuget pack` command.

From there, you can consider a number of other options for your package:

-  [Dependency Versions](/ndocs/create-packages/dependency-versions) discusses how to identify the exact versions that you allow for your dependencies (other packages that you consume from your package).
-  [Supporting Multiple Target Frameworks](/ndocs/create-packages/supporting-multiple-target-frameworks) describes how to create a package with multiple variants for different .NET Frameworks.
-  [Source and Config File Transformations](/ndocs/create-packages/source-and-config-file-transformations) describes how you can do both one-way token replacements in files that are added to a project, and modify `web.config` and `app.config` with settings that are also backed out when the package is uninstalled.
-  [Creating Localized Packages](/ndocs/create-packages/creating-localized-packages) describes how to structure a package with multiple language resources and how to use separate localized satellite packages.
-  [Pre-release Packages](/ndocs/create-packages/prerelease-packages) demonstrates how to release alpha, beta, and rc packages to those customers who are interested.
-  [Native Packages](/ndocs/create-packages/native-packages) describes the process for creating a package for C++ consumers.
-  [Symbol Packages](/ndocs/create-packages/symbol-packages) offers guidance for supplying symbols for your library that allow consumers to step into your code while debugging.


When you're then ready to publish a package to nuget.org, follow the simple process in [Publish a package](/ndocs/create-packages/publish-a-package).

If you want to use a private feed instead of nuget.org, see the [Hosting Packages Overview](/ndocs/hosting-packages/overview)