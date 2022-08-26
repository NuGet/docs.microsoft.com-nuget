The NuGet CLI [restore](../../reference/cli-reference/cli-ref-restore.md) command downloads and installs any packages that are missing from the packages directory.

Like `install`, the `restore` command only adds packages to disk, but doesn't change the project's dependencies. To change project dependencies, modify the *packages.config* file manually, then use `restore`.

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
> For projects migrated to `PackageReference`, you can use [msbuild -t:restore](../package-restore.md#restore-using-msbuild) to restore packages instead.

