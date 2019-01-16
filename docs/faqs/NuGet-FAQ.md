---
title: NuGet Frequently-Asked Questions
description: Common questions and answers for using NuGet on the command line and in Visual Studio, and working with the NuGet gallery.
author: shishirx34
ms.author: shishirh
ms.date: 01/15/2019
ms.topic: conceptual
---

# NuGet frequently-asked questions

## NuGet Account Management

### How to create a new NuGet account?

To create a NuGet account, you need to have a personal Microsoft account (MSA) or an Azure Active Directory (AAD) account. If you do not have one, you can [create](https://www.microsoft.com/en-us/account) one. Follow the following steps if you have a MSA or AAD account.
- Go to the [login page](https://www.nuget.org/users/account/LogOn?returnUrl=%2F).
- Click on `Sign in with Microsoft` button.
- Enter your MSA/AAD account details. 
- Please accept the permissions to be given to the `NuGet.org` application.
- You will be redirected to nuget.org, and asked to register a username.
- Specify the username in the input box. Please note that the username **is** case sensitive and cannot be changed/renamed later.
- Click on `Register` button.

You now have a NuGet account. You can perfrom account management on the [account settings](https://www.nuget.org/account) page.

### How to recover NuGet Password login?

Please note that the [NuGet Password login has been discontinued](https://blog.nuget.org/20180515/NuGet.org-will-only-support-MSA-AAD-starting-June.html "NuGet Password login has been discontinued") and the only way to log in to NuGet will be with a personal Microsoft account (MSA) or Azure Active Directory (AAD) account. However, in cases when you are unable to access your associated MSA/AAD accounts you might need to use password logins for recovering your NuGet account. In such situations follow the steps below.
- **Requirement:** You will need to have access to the email that is associated with the account for which you need to recover the password.
- Go to the [NuGet login page](https://www.nuget.org/users/account/LogOn?returnUrl=%2F "NuGet login page")
- Click on `Sign in using NuGet.org account`
- Click on `Forgot password?` link.
- Enter the `email` address that is associated with the NuGet account you wish to recover.
- Click `Send` button.
- You should get an email in the specified email address account with a link to reset your password. Click on this link and set the new password you wish.
- Once done, you can now login with username/password on NuGet.
- To login with username/password, use the `Sign in using Nuget.org account` link on the  [NuGet login page](https://www.nuget.org/users/account/LogOn?returnUrl=%2F "NuGet login page").

### Which Microsoft account is linked to my NuGet Account?

If you have forgotten which microsoft account is associated with your NuGet account, please follow the steps below to get assistance.
- Go to [NuGet login page](https://www.nuget.org/users/account/LogOn?returnUrl=%2F "NuGet login page") and click on `Need assistance signing in?` link.
- This will show you the pop-up dialog box for assistance. Follow the steps in this dialog box to understand the associated microsoft account(s) for your NuGet account.

### How to change my Microsoft account for NuGet login?
If you wish to change the associated microsoft account for NuGet login, follow the steps below. Lets say your Microsoft account with email `account1@outlook.com` is associated with your NuGet account with username `MyNuGetAccount`. You wish to change the login to another Microsoft account with email `account2@outlook.com`
- Please sign in using **currently associated microsoft account** i.e `account1@outlook.com` on the [login page](https://www.nuget.org/users/account/LogOn?returnUrl=%2F# "login page") after clicking `Sign in with Microsoft`.
- Once logged in, go to your [account settings](https://www.nuget.org/account "account settings") page.
- Expand the section for `Login Account`. Click on the `Change Account` button.
- You will now be redirected to the microsoft login page. Please sign in with the account that you wish to change the association to i.e `account2@outlook.com`. Note: you might need to click on `Sign out and sign in with different account` during the sign in flow to be able to login with a different microsoft account.
- If you see an error like below, see [Microsoft account is linked with another NuGet account](#microsoft-account-is-linked-with-another-nuget-account) for more details.
```
Failed to update the Microsoft account with 'account2 <account2@outlook.com>'. This could happen if it is already linked to another NuGet account. Contact support for more information.
```
- Once you have successfully signed in with your second account, you will be redirected back to your nuget account settings page and you should now see the new Microsoft account associated as the login account. Going forward you should use this account when signing into nuget.org.

### Microsoft account is linked with another NuGet account.

If you tried changing your Microsoft login and saw the error below, this might be due to the fact that you have multiple NuGet accounts associated with different microsoft accounts.

```
Failed to update the Microsoft account with 'account2 <account2@outlook.com>'. This could happen if it is already linked to another NuGet account. Contact support for more information.
```
On NuGet with the [recent change](https://blog.nuget.org/20180515/NuGet.org-will-only-support-MSA-AAD-starting-June.html "recent change")  there can always be a single Microsoft account (MSA or AAD) that is associated with a given NuGet account or profile.
Lets say you were trying to change Microsoft account login from `account1@outlook.com` for NuGet user with username `MyNuGetAccount1` to another microsoft account with email `account2@outlook.com`. And you see the error above.

**What does the error above mean?**
- It means that there is another NuGet account/profile which is associated with the Microsoft account that you are trying to change it i.e. in above example the Microsoft account with email `<account2@outlook.com>` is associated with another NuGet account with, say, username `MyNuGetAccount2`.
- You cannot change the associated login to another Microsoft account which is linked to a different NuGet account. This would otherwise leave the other NuGet account orphaned.

**I forgot I had another NuGet account, how do I find out which NuGet account it is?**
- Login with the second Microsoft account on the [login page](https://www.nuget.org/users/account/LogOn?returnUrl=%2F# "login page")
- This will log you into the NuGet account that is currently associated with the second Microsoft account. You can then view the uploaded packages and perform account management on this account.

**I do not care about this second NuGet account, I want to change my login for first NuGet account with the second microsoft account. What do I do?**
- If you wish to not care about the second NuGet account and still want to re-use the associated Microsoft account with email `account2@outlook.com`. You will first need to get rid of the other account.
- Follow the steps to [delete user](#how-to-delete-my-nuget-account) for the second NuGet account `MyNuGetAccount2`.

**Wait, I care about this second account too. I do not want to lose this account but change my associated account logins for first account**
- You will need to create/use a third Microsoft account, say, with email `account3@outlook.com`. 
- First you should login with your second microsoft account, `account2@outlook.com` on nuget.org. Follow the steps above to change associated logins and associate the third microsoft account with this NuGet account.
- Once done, your second microsoft account with email `account2@outlook.com` is free to be associated to your first NuGet account, `MyNuGetAccount1`. Follow the same steps above to change microsoft logins to the second microsoft account.

### Unable to use microsoft login, how do I recover my NuGet account?

If you tried using the [sign in assistance](#which-microsoft-account-is-linked-to-my-nuget-account) and you do not have access to the microsoft account that is associated with your NuGet account, please follow the steps below to link a new microsoft account to your NuGet account.
1. **Requirement**: You will need an access to a microsoft account(which is not associated with any existing NuGet accounts). If you do not have one, you can [create](https://www.microsoft.com/en-us/account) one.
1. Follow the [steps to recover your password login](#how-to-recover-nuget-password-login), if you have the password login skip this step.
1. [Login](https://www.nuget.org/users/account/LogOnNuGetAccount?returnUrl=%2F) to NuGet using the username/password login.
1. Once logged in, you will see the popup dialog show up like below. This is the password discontinuation dialog box.

![Link MSA Dialog](media/link-msa-dialog.png)

1. **Important**: please ignore the instruction to login with the specified microsoft account. You can now link your NuGet account to any other Microsoft login.
1. Click on the button `Sign in with Microsoft` and login with the microsoft account that you have an access to, as mentioned in step 1.
1. Your account will now be linked to the new Microsoft account, which you can use to log into nuget.org going forward.

### How to transform my NuGet account to an organization?

If you want to transform your account to an organization, and this account is already associated with a Microsoft account login, please follow the steps given in the documentation for [organizations on nuget org](https://docs.microsoft.com/en-us/nuget/reference/organizations-on-nuget-org).

If however, you NuGet account is not associated/linked with a Microsof account, you can follow the steps below to transform this account to an organization.
1. **Requirement**: You need to have an individual account first created on NuGet to be used as an admin on the org account. If you do not have one, please [create a new NuGet account](#how-to-create-a-nuget-account)
1. Follow the [steps to recover your password login](#how-to-recover-nuget-password-login) for your NuGet account if you do not have password login for it, if you do, skip this step.
1. [Login](https://www.nuget.org/users/account/LogOnNuGetAccount?returnUrl=%2F) to NuGet using the username/password login.
1. Once logged in, you will see the popup dialog show up like below. This is the password discontinuation dialog box. **IMPORTANT** Ignore this dialog box, **do not** click on the `Sign in with microsoft` button.

![Link MSA Dialog](media/link-msa-dialog.png)

1. Go to https://www.nuget.org/account/transform. This will allow you to convert the NuGet account to an org without linking to a Microsoft account.
1. Specify the admin username for your personal NuGet account/the account you created in Step 1.
1. Follow the instructions to complete transformation of this account to an organization.

### NuGet login issues with unmanaged AAD tenant?

If you see an error like below during your login flow with your email account domain(@yourdomain.com), see the steps below to recover your NuGet account.

![Unamanaged AAD Tenant Error](media/unmanaged-aad-tenant.png)

- **What is this unmanaged state thing during login? And why is this happening now?** 
  - Your account seems to be previously registered as a personal Microsoft account and it worked fine, however, now it seems like your account has been registered as an "Unmanaged" tenant in the Azure active directory(the identity service which we use to authenticate Microsoft accounts). 
  - This could have happened if you or someone from your organization(with @yourdomain.com email address) registered with one of the AAD integrated services or did a [self-service signup for Azure Active Directory](https://docs.microsoft.com/en-us/azure/active-directory/users-groups-roles/directory-self-service-signup), which creates such an "Unmanaged" tenant for the used Microsoft account domain(@yourdomain.com in your case). 
- **What can I do to recover my account?**
  - At this moment there is not a way for us (NuGet) to authenticate accounts with such "Unmanaged" tenant accounts in Azure Active directory. We are looking in to a better way to authenticate such accounts.
  - If you want to login to nuget.org with your Microsoft account(@yourdomain.com), you(or an administrator at your company) will need to claim the ownership of the AAD by doing a DNS validation to authenticate users with email address "@yourdomain.com". Please follow the steps for [domains admin takeover](https://docs.microsoft.com/en-us/azure/active-directory/users-groups-roles/domains-admin-takeover) documented by the Azure Active directory. 
  - Once this is done, your normal login should start working.
- **I don’t want to do all that, what is the other way to recover my account?**
  - You can create a new Microsoft account(with an email **not** associated with @yourdomain.com).
  - Link this new Microsoft account with your NuGet profile, see steps below.

### How do I change my NuGet account username?

details 

### How to delete my NuGet account?

details