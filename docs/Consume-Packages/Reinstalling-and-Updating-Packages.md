# Reinstalling and Updating Packages

There are a number of situations, described below under [When to Reinstall a Package](#when-to-reinstall-a-package), where references to a package might get broken within a Visual Studio project. In these cases, uninstalling and then reinstalling the same version of the package will restore those reference to working order.

Being mindful of the [Considerations](#considerations) described later, you can easily reinstall any package using the following command in the Visual Studio Package Manager Console (**Tools** > **NuGet Package Manager** > **Package Manager Console**):

<code class="bash hljs">
	Update-Package –reinstall &lt;package_name&gt;
</code>
	
Using this command is much easier than removing a package and then trying to locate the same package in the NuGet gallery with the same version.

The same command without `-reinstall` will update a package to a newer version, if applicable:

<code class="bash hljs">
	Update-Package &lt;package_name&gt;
</code>

Any updates are subject to version constraints indicated in `packages.config`, as described below in [Constraining upgrade versions](#constraining-upgrade-versions), but do not apply to projects using `project.json`.

For complete usage, refer to the [PowerShell reference for Update-Package](/ndocs/tools/powershell-reference#update-package).

## When to Reinstall a Package

1. **Broken references after package restore**: If you've opened a project and restored NuGet packages, but still see broken references, try reinstalling each of those packages.
2. **Project is broken due to deleted files**: NuGet does not prevent you from removing items added from packages, so it's easy to inadvertently modify contents installed from a package and break your project. To restore the project, reinstall the affected packages.
3. **Package update broke the project**: If an update to a package breaks a project, the failure is generally caused by a dependency package which may have also been. To restore the state of the dependency, reinstall that specific package.
4. **Project Retargeting or Upgrade**: This can be useful when a project has been retargeted or upgraded and if the package requires reinstallation due to the change in target framework. NuGet 2.7 and later shows a build error in such cases immediately after project retargeting, and subsequent build warnings let you know that the package may need to be reinstalled. For project upgrade, NuGet shows an error in the Project Upgrade Log.
5. **Improved NuGet package development cycle**: Package authors often need to reinstall the same version of package they're developing to test the behavior. The `Install-Package` command does not provide an option to force a reinstall, so use `Update-Package -reinstall` instead.

### Considerations

The following may be affected when reinstalling a package:

1. **Reinstalling packages according to project target framework retargeting**
	* In a simple case, just reinstalling a package using `Update-Package –reinstall <package_name>` work. A package that is installed against an old target framework gets uninstalled and the same package gets installed against the current target framework of the project.
	* In some cases, there may be a package that does not support the new target framework.
		- If a package supports portable class libraries (PCLs) and the project is retargeted to a combination of platforms no longer supported by the package, references to the package will be missing after reinstalling. 
		- This can surface for packages you're using directly or for packages installed as dependencies. It's possible for the package you're using directly to support the new target framework while its dependency does not.
		- If reinstalling packages after retargeting your application results in build or runtime errors, you may need to revert your target framework or search for alternative packages that properly support your new target framework.
		
2. **Reinstalling packages with dependencies**
	* `Update-Package –reinstall` reinstalls the same version of the original package, but installs the latest version of dependencies unless specific version constraints are provided. This allows you to update only the dependencies as required to fix an issue. However, if this rolls a dependency back to an earlier version, you can use `Update-Package <dependency_name>` to reinstall that one dependency without affecting the dependent package.
	* `Update-Package –reinstall <packageName> -ignoreDependencies` reinstalls the same version of the original package but does not reinstall dependencies. Use this when updating package dependencies might result in a broken state
	
3. **Reinstalling packages when dependent versions are involved**
	- As explained above, reinstalling a package does not change versions of any other installed packages that depend on it. It's possible, then, that reinstalling a dependency could break the dependent package.


## Constraining upgrade versions 

In NuGet 3.x with projects using `project.json` to list dependencies, installing or updating a package will *always* install the latest version available from the package source. 

In projects using `packages.config`, the same behavior applies unless you specifically constrain the version range. For example, if you know that your application will work only with version 1.x of a package but not 2.0 and above, perhaps due to a major change in the package API, then you'd want to constrain upgrades to 1.x versions. This prevents accidental updates that would break the application.

To set a constraint, open `packages.config` in a text editor, locate the dependency in question, and add the `allowedVersions` attribute with a version range. For example, to constrain updates to version 1.x, set `allowedVersions` to `[1,2]`: 

    <?xml version="1.0" encoding="utf-8"?>
    <packages>
        <package id="ExamplePackage" version="1.1.0" allowedVersions="[1,2)" />

		<!-- ... -->
    </packages>

In all cases, use the notation described in [Dependency versions](/ndocs/create-packages/dependency-versions).

	