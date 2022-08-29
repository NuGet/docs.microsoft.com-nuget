From the folder that contains the *.nupkg* file, run the following command. Specify your unique package ID, and replace the key value with your API key.

```dotnetcli
dotnet nuget push Contoso.08.28.22.001.Test.1.0.0.nupkg --api-key qz2jga8pl3dvn2akksyquwcs9ygggg4exypy3bhxy6w6x6 --source https://api.nuget.org/v3/index.json
```

The output shows the results of the publishing process:

```output
Pushing Contoso.08.28.22.001.Test.1.0.0.nupkg to 'https://www.nuget.org/api/v2/package'...
  PUT https://www.nuget.org/api/v2/package/
warn : All published packages should have license information specified. Learn more: https://aka.ms/nuget/authoring-best-practices#licensing.
  Created https://www.nuget.org/api/v2/package/ 1221ms
Your package was pushed.
```

For more information, see [dotnet nuget push](/dotnet/core/tools/dotnet-nuget-push).