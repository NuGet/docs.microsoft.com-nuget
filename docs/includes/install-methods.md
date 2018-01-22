You can install a NuGet package in three different ways:

| Method | Description | Reference |
| --- | --- | --- |
| nuget.exe CLI: `nuget install <package_name>` | Downloads the package identified by \<package_name\> and expands its contents into a folder in the current directory. If no packages are specified, installs all packages listed in the project's `packages.config` file. No changes are made to any project files. Dependencies are also downloaded and expanded. | [CLI reference](../tools/nuget-exe-CLI-Reference.md) |
| Package Manager Console (Visual Studio): `Install-Package <package_name>` | Downloads and installs the package into the current project, then update the project file to list the package as a dependency. | [Package Manager Console Guide](../tools/Package-Manager-Console.md) |
| Package Manager UI (Visual Studio) | Provides a UI through which you can browse, select, and install packages into a project. Updates the project file to list the package as a dependency. | [Package Manager UI Reference](../tools/Package-Manager-UI.md) |
