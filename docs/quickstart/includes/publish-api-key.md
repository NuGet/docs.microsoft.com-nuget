1. [Sign into your nuget.org account](https://www.nuget.org/users/account/LogOn?returnUrl=%2F) or create an account if you don't have one already.

   For more information about creating your account, see [Individual accounts on NuGet.org](../../nuget-org/individual-accounts.md).

1. Select your user name on the upper right of the page, and then select **API Keys** from the drop-down menu.

1. Expand **Create** and enter a **Key Name** for your key.

1. Under **Select Scopes**, select **Push**, and then enter _*_ for **Glob Pattern**.

1. Select **Create** to create the key.

1. After the key is created, select **Copy** to retrieve the access key you need in the CLI.

    :::image type="content" source="../media/qs-create-api-key.png" alt-text="Screenshot showing a NuGet API key with the Copy button highlighted.":::

> [!WARNING]
> **Always keep your API key a secret**. Treat your API key as a password that allows anyone to manage packages on your behalf. You should delete or regenerate your API key if it's ever accidentally revealed.
>
> Because you can't copy your key again later, be sure to save it in a secure location. If you return to the API key page, regenerate the key to copy it. You can remove the API key if you no longer want to push packages.

Scoping allows you to create separate API keys for different purposes. Each key has an expiration timeframe and can be scoped to specific packages (or glob patterns). Each key is also scoped to specific operations: push new packages and package versions, push only new package versions, or unlist packages. Through scoping, you can create API keys for different people who manage packages for your organization such that they have only the permissions they need. For more information, see [Scoped API keys](../../nuget-org/scoped-api-keys.md).
