1. [Sign in to your nuget.org account](https://www.nuget.org/users/account/LogOn?returnUrl=%2F) or [create an account](../../nuget-org/individual-accounts.md#add-a-new-individual-account) if you don't have one already.

1. In the upper-right corner, select your user name, and then select **API Keys**.

1. Select **Create**, and then enter a name for your key.

1. Under **Select Scopes**, select **Push**.

1. Under **Select Packages**, for **Glob Pattern**, enter an asterisk (**\***).

1. Select **Create**.

1. Select **Copy** to copy the new key.

   :::image type="content" source="../media/qs-create-api-key.png" alt-text="Screenshot of a nuget.org page that shows the new API key, a message about copying the key now, and the Copy button, which is highlighted.":::

> [!IMPORTANT]
>
> - Always keep your API key a secret. The API key is like a password that anyone can use to manage packages on your behalf. Delete or regenerate your API key if it's accidentally revealed.
> - Save your key in a secure location, because you can't copy the key again later. If you return to the API key page, you need to regenerate the key to copy it. You can also remove the API key if you no longer want to push packages.

*Scoping* provides a way to create separate API keys for different purposes. Each key has an expiration time frame, and you can scope the key to specific packages or glob patterns. You also scope each key to specific operations: Push new packages and package versions, push only new package versions, or unlist.

Through scoping, you can create API keys for different people who manage packages for your organization so they have only the permissions they need.