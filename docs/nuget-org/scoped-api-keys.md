---
title: Scoped API keys
description: Take control of API keys that you use to push packages
author: mikejo5000
ms.author: mikejo
ms.date: 06/04/2019
ms.topic: concept-article
---

# Scoped API keys

To make NuGet a more secure environment for package distribution, you can take control of the API keys by adding scopes.

The ability to provide scope to your API keys give you better control on your APIs. You can:

- Create multiple scoped API keys that can be used for different packages with varying expiration timeframes.
- Obtain API keys securely.
- Edit existing API keys to change package applicability.
- Refresh or delete existing API keys without hampering operations using other keys.

## Why do we support scoped API keys?

We support scopes for API keys to allow you to have more fine-grained permissions. Previously, NuGet offered a single API key for an account, and that approach had several drawbacks:

- **One API key to control all packages**. With a single API key that is used to manage all packages, it is difficult to securely share the key when multiple developers are involved with different packages, and when they share a publisher account.
- **All permissions or none**. Anyone with access to the API key has all permissions (publish, push and un-list) on the packages. This is often not desirable in environment with multiple teams.
- **Single point of failure**. A single API key also means a single point of failure. If the key is compromised, all packages associated with the account could potentially be compromised. Refreshing the API key is the only way to plug the leak and avoid an interruption to your CI/CD workflow. In addition, there may be cases when you want to revoke access to the API key for an individual (for example, when an employee leaves the organization). There isn’t a clean way to handle this today.

With scoped API keys, we try to address these problems while making sure that none of the existing workflows break.

## Acquire an API key

[!INCLUDE [publish-api-key](../quickstart/includes/publish-api-key.md)]

## Create scoped API keys

You can create multiple API keys based on your requirements. An API key can apply to one or more packages, have varying scopes that grant specific privileges, and have an expiration date associated with it.

In the following example, you have an API key named `Contoso service CI` that can be used to push packages for specific `Contoso.Service` packages, and is valid for 365 days. This is a typical scenario where different teams within the same organization work on different packages, and the members of the team are provided the key that grants them privileges only for the package they are working on. The expiration serves as a mechanism to prevent stale or forgotten keys.

![Create API keys](media/scoped-api-keys-create-new.png)

## Use glob patterns

If you are working on multiple packages and have a large list of packages to manage, you can choose to use globbing patterns to select multiple packages together. For example, if you wish to grant specific scopes to a key for all packages whose ID starts with `Fabrikam.Service`, you could do this by specifying `fabrikam.service.*` in the **Glob pattern** text box.

![Create API keys - 2](media/scoped-api-keys-glob-pattern.png)

Using glob patterns to determine API key permissions also applies to new packages matching the glob pattern. For example, if you try to push a new package named `Fabrikam.Service.Framework`, you can do that with the key created previously, since the package matches the glob pattern `fabrikam.service.*`.

## Obtain API keys securely

For security, a newly created key is never shown on the screen and is only available using the **Copy** button. Similarly, the key is not accessible after the page is refreshed.

![Create API keys - 3](media/scoped-api-keys-obtain-keys.png)

## Edit existing API keys

You may also want to update the key permissions and scopes without changing the key itself. If you have a key with specific scope(s) for a single package, you can choose to apply the same scope(s) on one or many other packages.

![Create API keys - 4](media/scoped-api-keys-edit.png)

## Refresh or delete existing API keys

The account owner can choose to refresh the key, in which case the permission (on packages), scope, and expiry remain the same, but a new key is issued making the old key unusable. This is helpful in managing stale keys or where there is any potential for an API key leakage.

![Create API keys - 5](media/scoped-api-keys-refresh.png)

You may also choose to delete these keys if they are not needed anymore. Deleting a key removes the key and makes it unusable.

## FAQs

### What happens to my old (legacy) API key?

Your old API key (legacy) continues to work and can work as long as you want it to work. However, these keys will be retired if they have not been used for more than 365 days to push a package. For more details, see the blog post [Changes to expiring API keys](https://blog.nuget.org/20160825/Changes-to-Expiring-API-Keys.html). You can no longer refresh this key. You need to delete the legacy key and create a new scoped key instead.

> [!NOTE]
> This key has all permissions on all the packages and it never expires. You should consider deleting this key and creating new keys with scoped permissions and definite expiry.

### How many API keys can I create?

There is no limit on the number of API keys you can create. However, we advise you to keep it to a manageable count so that you do not end up having many stale keys with no knowledge of where and who is using them.

### Can I delete my legacy API key or discontinue using now?

Yes. You can--and you probably should--delete your legacy API key.

### Can I get back my API key that I deleted by mistake?

No. Once deleted, you can only create new keys. There is no recovery possible for accidentally deleted keys.

### Does the old API key continue to work upon API key refresh?

No. Once you refresh a key, a new key gets generated that has the same scope, permission, and expiry as the old one. The old key ceases to exist.

### Can I give more permissions to an existing API key?

You cannot modify the scope, but you can edit the package list it is applicable to.

### How do I know if any of my keys expired or are getting expired?

If any key expires, we will let you know through a warning message at the top of the page. We also send a warning e-mail to the account holder ten days before the expiration of the key so that you can act on it well in advance.
