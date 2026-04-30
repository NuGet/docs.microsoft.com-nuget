From the folder that contains the *.nupkg* file, run the following command. Replace `<package-file>` with the name of your *.nupkg* file, and replace `<API-key>` with your API key.

```dotnetcli
dotnet nuget push <package-file> --api-key <API-key> --source https://api.nuget.org/v3/index.json
```

> [!TIP]
> Starting with NuGet 7.6 (.NET SDK 10.0.300), you can set the `NUGET_API_KEY` environment variable instead of passing `--api-key` on the command line.
> This is useful in CI/CD scenarios where you don't want to pass the key as a command-line argument.

The output shows the results of the publishing process:

```output
Pushing <package-file> to 'https://www.nuget.org/api/v2/package'...
  PUT https://www.nuget.org/api/v2/package/
  Created https://www.nuget.org/api/v2/package/ 2891ms
Your package was pushed.
```

For more information, see [dotnet nuget push](/dotnet/core/tools/dotnet-nuget-push).
