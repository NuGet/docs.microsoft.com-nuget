1. Open a command prompt and change to the folder containing your NuGet package file.

1. Run the following command. Replace \<package filename> with the file name of your package and replace \<api key value> with your API key. The package filename is a concatenation of your package ID and version number. For example, *AppLogger.1.0.0.nupkg*:

    ```dotnetcli
    dotnet nuget push <package filename> --api-key <api key value> --source https://api.nuget.org/v3/index.json
    ```

    The result of the publishing process is displayed as follows:

    ```output
    info : Pushing <package filename> to 'https://www.nuget.org/api/v2/package'...
    info :   PUT https://www.nuget.org/api/v2/package/
    info :   Created https://www.nuget.org/api/v2/package/ 12620ms
    info : Your package was pushed.
    ```

For more information, see [dotnet nuget push](/dotnet/core/tools/dotnet-nuget-push).