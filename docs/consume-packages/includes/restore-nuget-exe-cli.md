The NuGet CLI [`restore`](../../reference/cli-reference/cli-ref-restore.md) command downloads and installs any missing packages. The command works on projects that use either [`PackageReference`](../package-references-in-project-files.md) or [packages.config](../../reference/packages-config.md) for package references.

Like `install`, the `restore` command only adds packages to disk. It doesn't modify the project file or *packages.config* file. To add project dependencies, use the Visual Studio Package Manager UI or console.

To restore packages, run the following command:

```cli
nuget restore <project-path>
```

The `restore` command uses a solution file or a *package.config* file in the specified project path.

For example, to restore all packages for *MySolution.slnx* in the current directory, run the following command:

```cli
nuget restore MySolution.slnx
```

> [!NOTE]
> For non-SDK-style projects that use `PackageReference`, the recommended approach is to use [`msbuild -t:restore`](../package-restore.md#restore-using-msbuild) to restore packages instead of the `nuget restore` command. The `msbuild -t:restore` command uses the same project evaluation and resolution logic as the build, which helps to ensure consistent and reliable dependency resolution.
