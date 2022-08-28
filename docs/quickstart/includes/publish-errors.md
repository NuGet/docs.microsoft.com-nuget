You receive an error message from the `push` command if the following situations occur:

- You haven't updated the version number in your project and are therefore attempting to publish a package that already exists.

- You attempt to publish a package with an identifier that already exists on the host, causing a naming conflict. For example, if the name *AppLogger* already exists the `push` command produces the following error:

   ```output
   Response status code does not indicate success: 403 (The specified API key is invalid,
   has expired, or does not have permission to access the specified package.).
   ```

   You might also receive this same message if you're using a valid API key that you've recently created when there's a naming conflict.

   To fix the error, change the package identifier, rebuild the project, recreate the `.nupkg` file, and then retry the `push` command.
