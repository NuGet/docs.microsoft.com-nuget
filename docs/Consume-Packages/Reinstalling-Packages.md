# Reinstalling Packages

Reinstalling a package is a simple act of uninstalling a package and installing back the same version of the package that was uninstalled. The 2 steps involved are nicely bundled up in the –reinstall switch of the Update-Package command as shown below. But, it is ESSENTIAL that one understands the changes this single command can bring to a project before using it. Please note that this doc will be updated in the near future to cover more scenarios

<code class="bash hljs">
	Update-Package –reinstall &lt;packageName>
</code>
	
## When to Reinstall a Package

1. **Project Retargeting or Upgrade**: This can be useful when a project has been retargeted or upgraded and if the package *REQUIRES* reinstallation due to the change in target framework. In NuGet 2.7, when this happens, we show a build error immediately after project retargeting and subsequently, build warnings to let you know that the package may need to be reinstalled. For project upgrade, we show an error in the Project Upgrade Log.
1. **Project is Broken**: NuGet does not prevent you from removing items added from packages, so it's easy to inadvertently modify contents installed from a package and break your project. If you want to go back to a state where the package contents present in your project match the actual contents of the package, reinstall the package.
1. **Package Update Broke the Project**: If an update to a package has broken your project, the failure is generally caused by a dependency package which may have been directly updated or because another package updated causing a dependency update. If you want is to go back to a state where the older version of the dependency package where things were fine, reinstall the package.
1. **Improved NuGet Package Development Cycle**: Package authors often need to reinstall the same version of package they are developing to test the behavior. The command Install-Package does not provide ‘-force’ option, that can forcefully reinstall a package. As such, `Update-Package -reinstall` was added to meet that requirement.

## Other Considerations

Reinstalling packages can be pretty straightforward. However, it can get very tricky since the following may affect or be affected by a reinstallation:

1. Project retargeting or project upgrade where the target framework of the project gets changed
2. Package dependencies and their versions
3. Dependent packages and their versions

The cases above are covered in detail below:

1. Reinstalling packages according to project target framework retargeting
	* In a simple case, just reinstalling a package using `Update-Package –reinstall &lt;packageName>`
	will do the trick. A package that is installed against an old target framework gets uninstalled and the same package gets installed against the current target framework of the project.
	* In some cases, there may be a package that does not support the new target framework.
		- If a package supports portable class libraries (PCLs) and the project is retargeted to a combination of platforms no longer supported by the package, references to the package will be missing after reinstalling. 
		- This can surface for packages you're using directly or for packages installed as dependencies. It's possible for the package you're using directly to support the new target framework while its dependency does not.
		- If reinstalling packages after retargeting your application yields build or runtime errors, you may need to revert your target framework or search for alternative packages that properly support your new target framework.
2. Reinstalling packages when package dependency versions are involved
	* When a package is reinstalled using ‘Update-Package –reinstall’, the same version of the package is installed. However, this is not true for package dependencies. The version of the package dependency that is installed is determined by package dependency version constraints if provided (or the latest if none is provided).
	* Since the version of the package dependency that worked best is determined by you, use one of the following:
		- `Update-Package –reinstall <packageName>` when updating the package dependencies is required for fixing an issue
		- `Update-Package –reinstall <packageName> -ignoreDependencies` when updating package dependencies might result in a broken state
	* If an `Update-Package -reinstall` command rolled a dependency package back to an older version than was previously installed, you can use `Update-Package <packageName>` to update it to the newer version.
3. Reinstalling packages when dependent versions are involved
	- Not surprisingly, `Update-Package –reinstall` does not change versions of the dependent packages. While this is expected, as explained above, versions of dependent versions need to be taken into account to determine if a package should be reinstalled or not

## Background and Reference

1.	[Package can support multiple .NET Framework Versions and Profiles](../Create/Enforced-Package-Conventions#supporting-multiple-.net-framework-versions-and-profiles)
2.	[Original Codeplex discussion on –reinstall switch](https://nuget.codeplex.com/discussions/256212)
3.	[Codeplex Issue with info on various cases involved with -reinstall switch](http://nuget.codeplex.com/workitem/1779)
4.	[Blog Post: A quick tutorial on the Update-Package command](http://blog.nuget.org/20121231/a-quick-tutorial-on-update-package-command.html)
