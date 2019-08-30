Use the [restore](../../reference/cli-reference/cli-ref-restore.md) command, which downloads and installs any packages missing from the *packages* folder.

For projects migrated to PackageReference, use [msbuild -t:restore](../package-restore.md#restore-using-msbuild) to restore packages instead.

`restore` only adds packages to disk but does not change a project's dependencies. To restore project dependencies, modify `packages.config`, then use the `restore` command.

As with the other `nuget.exe` CLI commands, first open a command line and switch to the directory that contains your project file.

To restore a package using `restore`:

```cli
nuget restore MySolution.sln
```