When you run the `push` command, you sometimes encounter an error. For instance, you might get an error in the following situations:

- Your API key is invalid or expired.
- You try to publish a package that has an identifier that already exists on the host.
- You make changes to a published package, but you forget to update the version number before you try to publish it again.

The error message typically indicates the source of the problem.

For example, suppose the identifier `Contoso.App.Logger.Test` exists on nuget.org. If you try to publish a package with that identifier, you get the following error:

```output
Response status code does not indicate success: 409 (A package with ID 'Contoso.App.Logger.Test' and version '1.0.0' already exists and cannot be modified.).
```

To address this problem, increment the version, rebuild the project, re-create the *.nupkg* file, and retry the `push` command.

If you use an expired or invalid API key when you run the `push` command, you get the following error:

```output
Response status code does not indicate success: 403 (The specified API key is invalid, has expired, or does not have permission to access the specified package.).
```

If you get this error, check the scope, expiration date, and value of your API key.
