Use the [dotnet restore](/dotnet/core/tools/dotnet-restore?tabs=netcore2x) command, which restores packages listed in the project file (see [PackageReference](../../consume-packages/package-references-in-project-files.md)). With .NET Core 2.0 and later, restore is done automatically with `dotnet build` and `dotnet run`. As of NuGet 4.0, this runs the same code as `nuget restore`.

As with the other `dotnet` CLI commands, first open a command line and switch to the directory that contains your project file.

To restore a package using `dotnet restore`:

```dotnetcli
dotnet restore 
```