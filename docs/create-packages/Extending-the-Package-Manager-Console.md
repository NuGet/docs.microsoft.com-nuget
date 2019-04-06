---
title: Extending the Package Manager Console
description: A detailed guide to 
author: samrueby
ms.date: 4/5/2019
ms.topic: conceptual
---

# Extending the Package Manager Console

Your package can install new commands for the [Package Manager Console](../tools/Package-Manager-Console.md). This can give users powerful tools for consuming the package, such as code generation. For example, `MvcScaffolding` creates commands like `Scaffold` shown below, which generates ASP.NET MVC controllers and views:

![Installing and using MvcScaffold](../tools/media/PackageManagerConsoleInstall.png)

There are three powershell scripts that can be included in your Tools folder in your nuget package:
- [Init.ps1](#init.ps1)
- [Install.ps1](#install.ps1)
- [Uninstall.ps1](#Uninstall.ps1)

## Init.ps1

*Init.ps1* runs every time a solution is opened that has your package installed. If the same package is installed into additional projects in the solution, it is still only run once. 

Init.ps1 is supported by both packages.config and PackageReference. [Learn more about the difference between packages.config and PackageReference](../reference/migrate-packages-config-to-package-reference.md).

> [!Note] Init.ps1 only gets run when package is used using tooling (e.g.. Visual Studio).

## Install.ps1

*Install.ps1* runs when a package is installed in a project. If the same package is installed in multiple projects in a solution, the script runs each time. In the case of a package that is not installed into a project, the script runs when the package is installed into the solution.

> [!Important] *Install.ps1* is not supported by PackageReference and is deprecated when using packages.config. Thus, this is not recommened for use in new packages.

# Uninstall.ps1
*Uninstall.ps1* runs every time a package is uninstalled.

> [!Important] *Uninstall.ps1* is not supported by PackageReference and is deprecated when using packages.config. Thus, this is not recommened for use in new packages.

### Example Init.ps1
```powershell
#$installPath path to where the project is installed (this is different with package reference)
#$toolsPath path to the extracted tools directory
#$package information about the currently installing package
#$project reference to the EnvDTE project the package is being installed into

param($installPath, $toolsPath, $package, $project)

New-Module -ScriptBlock {
$installPath = $args[0]

function Update-DataLayer {
	[CmdletBinding()]
	Param()
	Process {
		& "$installPath\CommandRunner\CommandRunner.exe" UpdateAllDependentLogic
	}
}

} -ArgumentList $installPath
```

In the example above:
- Four parameters are passed to your script by NuGet that can be used by your script.
- Declare functions which will then appear in the Package Manager Console to be consumed by installers of your package.

Modify your .nuspec to include the file in tools\Init.ps1.
```xml
<?xml version="1.0"?>
<package>
  <metadata>
    ...
  </metadata>
  <files>
    <file src="..\CommandRunner\Package Manager Console\Commands.ps1" target="tools\Init.ps1" />
  </files>
</package>
```