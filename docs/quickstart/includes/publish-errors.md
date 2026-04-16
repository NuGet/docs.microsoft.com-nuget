When you run the `push` command, you sometimes encounter an error. For instance, you might get an error in the following situations:

- Your API key is invalid or expired.
- You try to publish a package that has an identifier that already exists on the host.
- You make changes to a published package, but you forget to update the version number before you try to publish it again.

The error message typically indicates the source of the problem.

For example, suppose the identifier `Contoso.App.Logger.Test` exists on nuget.org. If you try to publish a package with that identifier, you get the following error:

```output
Response status code does not indicate success: 403 (The specified API key is invalid, has expired, or does not have permission to access the specified package.).
```

To address this situation, check the scope, expiration date, and value of your API key. If the key is valid, the error indicates that the package identifier already exists on the host. To overcome the problem, change the package identifier to be unique, rebuild the project, re-create the *.nupkg* file, and retry the `push` command.
