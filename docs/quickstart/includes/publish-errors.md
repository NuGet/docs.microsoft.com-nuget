Errors from the `push` command typically indicate the problem. For example, you might have forgotten to update the version number in your project, so you're trying to publish a package that already exists.

You also see errors if you try to publish a package using an identifier that already exists on the host. The name AppLogger, for example, already exists on nuget.org. In this, the `push` command gives the following error:

```output
Response status code does not indicate success: 403 (The specified API key is invalid,
has expired, or does not have permission to access the specified package.).
```

If you're using a valid API key that you just created, this message indicates a naming conflict. The `permission` part of the error doesn't make this clear. To fix the error, change the package identifier, rebuild the project, recreate the *.nupkg* file, and retry the `push` command.
