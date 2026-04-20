The [`dotnet restore`](/dotnet/core/tools/dotnet-restore) command restores packages that are listed in `<PackageReference>` elements in the project file. For more information, see [`PackageReference` in project files](../../consume-packages/Package-References-in-Project-Files.md).

Starting with .NET Core 2.0 and continuing through .NET, the `dotnet build` and `dotnet run` commands restore packages automatically, as do many other dotnet CLI commands. As of NuGet 4.0, `dotnet restore` runs the same code as the `nuget restore` NuGet CLI command.

To restore packages by using `dotnet restore`:

1. Open a command-line window and go to the directory that contains your project file.
1. Run `dotnet restore`.
