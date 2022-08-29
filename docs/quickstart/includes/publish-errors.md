Errors from the `push` command typically indicate the problem. For example, you might have forgotten to update the version number in your project, so you're trying to publish a package that already exists.

You also see errors if your API key is invalid or expired, or if you try to publish a package using an identifier that already exists on the host. The identifier `AppLogger-test`, for example, already exists on nuget.org. If you try to publish a package with that identifier, the `push` command gives the following error:

```output
Response status code does not indicate success: 403 (The specified API key is invalid,
has expired, or does not have permission to access the specified package.).
```

If you get this error, check that you're using a valid API key that hasn't expired. If you are, the error indicates a naming conflict, which the message doesn't make clear. To fix the error, change the package identifier to be unique, rebuild the project, recreate the *.nupkg* file, and retry the `push` command.
