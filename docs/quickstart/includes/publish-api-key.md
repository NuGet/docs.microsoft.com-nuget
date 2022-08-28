1. [Sign into your nuget.org account](https://www.nuget.org/users/account/LogOn?returnUrl=%2F) or [create an account](../../nuget-org/individual-accounts.md#add-a-new-individual-account) if you don't have one already.

1. Select your user name at upper right, and then select **API Keys**.

1. Select **Create**, and provide a name for your key.

1. Under **Select Scopes**, select **Push**.

1. Under **Select Packages** > **Glob Pattern**, enter \*.

1. Select **Create**.

1. Select **Copy** to copy the new key.

   ![Screenshot that shows the new API key with the Copy link.](../media/QS_Create-02-APIKey.png)

> [!Important]
> - Always keep your API key a secret. Treat your API key as a password that allows anyone to manage packages on your behalf. Delete or regenerate your API key if it is accidentally revealed.
> - Save your key in a secure location, because you can't copy the key again later on. If you return to the API key page, you need to regenerate the key to copy it. You can also remove the API key if you no longer want to push packages.

*Scoping* lets you create separate API keys for different purposes. Each key has an expiration timeframe, and you can scope the key to specific packages or glob patterns. You also scope each key to specific operations: Push new packages and package versions, pushing only new package versions, or unlisting.

Through scoping, you can create API keys for different people who manage packages for your organization so they have only the permissions they need. For more information, see [scoped API keys](../../nuget-org/scoped-api-keys.md).
