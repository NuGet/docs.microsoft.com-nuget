The NuGet CLI [restore](../../reference/cli-reference/cli-ref-restore.md) command downloads and installs any packages that are missing from project package directories. The command works on projects that use either `PackageReference` or *packages.config* for package references.

Like `install`, the `restore` command only adds packages to disk, but doesn't modify the project file or *packages.config*. To add project dependencies, use the Visual Studio Package Manager UI or Console, then run `install` or `restore`.

To restore packages, run the following command:

```cli
nuget restore <projectPath>
```

The `restore` command uses a solution file or a *package.config* file in the specified project path.

For example, to restore all packages for *MySolution.sln* in the current directory, run:

```cli
nuget restore MySolution.sln
```

> [!NOTE]
> For non-SDK-style projects that use `PackageReference`, use [msbuild -t:restore](../package-restore.md#restore-using-msbuild) to restore packages instead.

