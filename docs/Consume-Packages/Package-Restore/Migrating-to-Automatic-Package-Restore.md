# Migrating MSBuild-Integrated solutions to use Automatic Package Restore

Solutions currently using MSBuild-Integrated package restore can be migrated to [Automatic Package Restore](/consume/package-restore). Prior to migrating, it can help to understand some of the differences between the two approaches.

## MSBuild-Integrated package restore vs. Automatic Package Restore
Projects that use MSBuild-Integrated package restore typically contain a .nuget folder with three files:

1. NuGet.Config
1. NuGet.exe
1. NuGet.targets

Since the presence of a NuGet.targets file determines whether NuGet will continue to use the MSBuild-Integrated approach, this file must be removed. Also, as the .nuget\NuGet.exe file is not used by Automatic Package Restore, it likewise can be removed.

By default, the NuGet.Config file instructs NuGet to bypass adding package binaries to source control. Automatic Package Restore will honor this as long as you leave this file in place. Note that NuGet.Config only has an effect when using Visual Studio to integrate with Team Foundation Server (TFS).

In addition to these files, NuGet modifies the project files in the solution to reference the NuGet.targets file so it can participate in the build process. When migrating to Automatic Package Restore, these references must also be removed.

## Performing the migration

With these details in mind, follow the steps below for your particular setup.

### Close down Visual Studio
If the solution you are trying to migrate is open in Visual Studio, then changes may be lost. Visual Studio may overwrite/ignore your changes in some cases and the NuGet extension will also try to re-enable Package Restore when it sees some projects in the solution are missing it.

### If you are using TFS

1. Remove the NuGet.exe and NuGet.targets files from the solution's .nuget folder. Make sure the files themselves are also removed from the solution workspace.
2. Retain the NuGet.Config file to continue to bypass adding packages to source control.
3. Edit each project file (e.g., .csproj, .vbproj) in the solution and remove any references to the NuGet.targets file. Open the project file(s) in the editor of  your choice and remove the following settings:

        <RestorePackages>true</RestorePackages>  
        ...
        <Import Project="$(SolutionDir)\.nuget\nuget.targets" />  
        ...
        <Target Name="EnsureNuGetPackageBuildImports" BeforeTargets="PrepareForBuild">  
            <PropertyGroup>
                <ErrorText>This project references NuGet package(s) that are missing on this computer. Enable NuGet Package Restore to download them.  For more information, see http://go.microsoft.com/fwlink/?LinkID=322105. The missing file is {0}.</ErrorText>
            </PropertyGroup>
            <Error Condition="!Exists('$(SolutionDir)\.nuget\NuGet.targets')" Text="$([System.String]::Format('$(ErrorText)', '$(SolutionDir)\.nuget\NuGet.targets'))" />
        </Target>


### If you are not using TFS

1. Remove the .nuget folder from your solution. Make sure the folder itself is also removed from the solution workspace.
2. Edit each project file (e.g., .csproj, .vbproj) in the solution and remove any references to the NuGet.targets file. Open the project file(s) in the editor of  your choice and remove the following settings:

        <RestorePackages>true</RestorePackages>  
        ...
        <Import Project="$(SolutionDir)\.nuget\nuget.targets" />  
        ...
        <Target Name="EnsureNuGetPackageBuildImports" BeforeTargets="PrepareForBuild">  
            <PropertyGroup>
                <ErrorText>This project references NuGet package(s) that are missing on this computer. Enable NuGet Package Restore to download them.  For more information, see http://go.microsoft.com/fwlink/?LinkID=322105. The missing file is {0}.</ErrorText>
            </PropertyGroup>
            <Error Condition="!Exists('$(SolutionDir)\.nuget\NuGet.targets')" Text="$([System.String]::Format('$(ErrorText)', '$(SolutionDir)\.nuget\NuGet.targets'))" />
        </Target>

### Migration Script

Many users have requested a migration tool to disable the MSBuild-based package restore and convert to Automatic Package Restore. The NuGet team has decided not to provide a supported tool for this because of the high probability of edge cases that would be unhandled. However, [Owen Johnson](https://github.com/owen2) has authored a [PowerShell script](https://github.com/owen2/AutomaticPackageRestoreMigrationScript) that can work in many cases. It's available on GitHub and can be used at your own risk. In other words, be sure to commit to source control before running it, just in case it doesn't work in your scenario.

## Testing the migration

1. Save the solution and close Visual Studio.
1. Remove the packages folder located under the solution root.
1. Reopen the solution in Visual Studio.
1. Rebuild the solution. Automatic Package Restore should:  
    a. Download and unzip each package  
    b. Ignore adding the packages to source control
