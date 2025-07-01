---
title: Trusted publishing
description: Trusted Publishing on nuget.org
author: etvorun
ms.author: evgenyt
ms.date: 07/01/2025
ms.topic: conceptual
---

# Trusted Publishing on nuget.org

Trusted Publishing is a secure and streamlined way to publish NuGet packages without needing to manage long-lived API keys. Instead, it uses short-lived credentials issued by a trusted CI/CD system like GitHub Actions.

This approach improves security by reducing the risk of credential leaks and simplifies automation by eliminating the need to rotate or store API keys.

To learn more about the broader industry effort behind this, check out the [OpenSSF initiative](https://repos.openssf.org/trusted-publishers-for-all-package-repositories).

> ⚠️ **Note:** If you don't see the **Trusted Publishing** option in your nuget.org account, the feature may not be available for your account yet. It will roll out gradually as the feature becomes generally available.



## How it works

Trusted Publishing allows nuget.org to securely integrate with your CI/CD provider.

When your workflow runs, the CI/CD provider (like GitHub Actions) issues a short-lived token.
This token is sent to nuget.org, which verifies it and uses it to generate a temporary API key.
That API key is then used by the workflow to publish your package.
This approach eliminates the need to store long-lived API keys and helps keep your publishing process secure and automated.

Currently, nuget.org supports [GitHub Actions](https://docs.github.com/actions/how-tos) as a trusted publisher.

## GitHub Actions

To use Trusted Publishing with GitHub Actions:

1. Log into nuget.org.
2. Click your username in the top-right corner and select **Trusted Publishing** from the dropdown menu.
3. Add a new Trusted Publisher, specifying your GitHub organization, repository, workflow file, and other required details.
4. In GitHub, configure your GitHub Actions workflow to request a short-lived API key from nuget.org and publish your package.

Here's a basic GitHub Actions workflow YAML example:

```yaml
steps:
    # TODO: steps to produce artifacts/my-sdk.nupkg
    # Get a short-lived NuGet API key to use for package publishing
    - name: NuGet login
      id: nuget_login
      uses: nuget/login@v1
      with:
        user: ${{secrets.NUGET_USER}}
        source: https://api.nuget.org/v3/index.json

    # Use short-lived NuGet API key to publish the package
    - name: NuGet push
      run: dotnet nuget push artifacts/my-sdk.nupkg -k ${{steps.nuget_login.outputs.NUGET_API_KEY}} -s https://api.nuget.org/v3/index.json
```

