From the folder that contains the *.nupkg* file, run the following command. Replace `<package-file>` with the name of your *.nupkg* file, and replace `<API-key>` with your API key.

```dotnetcli
dotnet nuget push <package-file> --api-key <API-key> --source https://api.nuget.org/v3/index.json
```

The output shows the results of the publishing process:

```output
Pushing <package-file> to 'https://www.nuget.org/api/v2/package'...
  PUT https://www.nuget.org/api/v2/package/
warn : License missing. See how to include a license within the package: https://aka.ms/nuget/authoring-best-practices#licensing.
  Created https://www.nuget.org/api/v2/package/ 974ms
Your package was pushed.
```

For more information, see [dotnet nuget push](/dotnet/core/tools/dotnet-nuget-push).

> [!NOTE]
> If you want to avoid your test package being live on nuget.org, you can push to the nuget.org test site at [https://int.nugettest.org](https://int.nugettest.org). Note that packages uploaded to int.nugettest.org might not be preserved.