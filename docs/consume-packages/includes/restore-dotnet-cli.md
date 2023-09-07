The [dotnet restore](/dotnet/core/tools/dotnet-restore) command restores packages that the project file lists with `<PackageReference>`. For more information, see [PackageReference in project files](../../consume-packages/package-references-in-project-files.md).

.NET Core 2.0 and later `dotnet build` and `dotnet run` commands restore packages automatically. As of NuGet 4.0, `dotnet restore` runs the same code as `nuget restore`.

To restore a package with `dotnet restore`:

1. Open a command line and switch to the directory that contains your project file.
1. Run `dotnet restore`.

