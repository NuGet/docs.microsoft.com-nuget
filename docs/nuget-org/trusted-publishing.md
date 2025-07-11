---
title: Trusted Publishing
description: Trusted Publishing on nuget.org
author: etvorun
ms.author: evgenyt
ms.date: 07/01/2025
ms.topic: conceptual
---

# Trusted Publishing on nuget.org

Trusted Publishing is a better way to publish NuGet packages. You don’t need to manage long-lived API keys anymore. Instead, you use short-lived credentials issued by your CI/CD system, like GitHub Actions.

This makes your publishing process safer by reducing the risk of leaked credentials. It also makes automation easier because you don’t need to rotate or store secrets. This approach is part of a broader industry shift toward secure, keyless publishing. If you're curious, check out the OpenSSF initiative: https://repos.openssf.org/trusted-publishers-for-all-package-repositories.

> ⚠️ **Heads up:** If you don’t see the **Trusted Publishing** option in your nuget.org account, it might not be available to you yet. We’re rolling it out gradually.

## How it works

Here’s the basic flow:

1. Your CI/CD system (like GitHub Actions) runs a workflow.
2. It issues a short-lived token.
3. That token is sent to nuget.org.
4. NuGet verifies it and returns a temporary API key.
5. Your workflow uses that key to push the package.

NuGet’s temporary API keys are valid for **15 minutes**, so your workflow should request the key shortly before publishing. If you request it too early, it might expire before the push happens.

This setup gives you a secure and automated way to publish packages, without the risks that come with long-lived secrets.


## GitHub Actions Setup

To get started:

1. Log into nuget.org.
2. Click your username and choose **Trusted Publishing**.
3. Add a new trusted publishing policy. You’ll need to provide your GitHub org, repo, workflow file, and few other details.
4. In your GitHub repo, update your workflow to request a short-lived API key and push your package.

Here’s a basic example:

```yaml
jobs:
  build-and-publish:
    permissions:
      id-token: write  # enable GitHub OIDC token issuance for this job
    
    steps:
      # Build your artifacts/my-sdk.nupkg package here
    
      # Get a short-lived NuGet API key
      - name: NuGet login
        id: nuget_login
        uses: nuget/login@v1
        with:
          user: ${{secrets.NUGET_USER}}
          source: https://api.nuget.org/v3/index.json
    
      # Push the package
      - name: NuGet push
        run: dotnet nuget push artifacts/my-sdk.nupkg -k ${{steps.nuget_login.outputs.NUGET_API_KEY}} -s https://api.nuget.org/v3/index.json
```


## Policy Ownership

When you create a Trusted Publishing policy, you need to choose who owns it. The owner can be either:

- **You (an individual user)**
- **An organization you belong to**

The policy will apply to all packages owned by the selected owner. That means it controls who can publish or modify those packages using Trusted Publishing.

If you choose an organization, make sure you're an active member. If you leave the org later, the policy may become disabled until you're added back.

Choosing the right owner helps ensure your publishing setup stays secure and aligned with your team’s structure.


## Temporarily Enabled Policies

Sometimes when you create a Trusted Publishing policy, we can’t get the GitHub repository and owner IDs right away. This usually happens with private repos.

Why does that matter? Because we use those IDs to lock the policy to the original repo and owner. That helps prevent resurrection attacks. Without the IDs, someone could delete a repo, recreate it with the same name, and try to publish as if nothing changed.

If we don’t have the IDs, the policy starts out as **temporarily enabled**. You’ll see this in the UI. It works like a regular policy, but it only lasts for **7 days**.
Once you publish from that repo, we’ll grab the IDs from the GitHub token and upgrade the policy to **permanently enabled**.
If no publish happens in time, the policy is disabled. You can reset the 7-day timer at any point, even if the policy has already been disabled after the initial window expired.

## Policy Ownership Warnings

Trusted Publishing policies are tied to a specific owner - either an individual user or an organization.
If something changes with that ownership, the policy might become disabled. When that happens, you'll see a warning in the UI.

### Common cases

- **User removed from organization**  
  If a policy is owned by an organization and the user who created it is later removed from that org, the policy becomes disabled.  
  If the user is added back to the organization, the policy will be enabled again automatically.

- **Organization is no longer active**  
  If the organization that owns the policy is locked or deleted, the policy becomes disabled.

These warnings help make sure that only enabled, secure policies are used when publishing packages.
