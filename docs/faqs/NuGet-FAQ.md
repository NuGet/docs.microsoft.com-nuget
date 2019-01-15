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

### How to change my Microsoft account for NuGet logins?
If you wish to change the associated microsoft account for NuGet login, follow the steps below. Lets say your Microsoft account with email `account1@outlook.com` is associated with your NuGet account with username `MyNuGetAccount`. You wish to change the login to another Microsoft account with email `account2@outlook.com`
- Please sign in using **currently associated microsoft account** i.e `account1@outlook.com` on the [login page](https://www.nuget.org/users/account/LogOn?returnUrl=%2F# "login page") after clicking `Sign in with Microsoft`.
- Once logged in, go to your [account settings](https://www.nuget.org/account "account settings") page.
- Expand the section for `Login Account`. Click on the `Change Account` button.
- You will now be redirected to the microsoft login page. Please sign in with the account that you wish to change the association to i.e `account2@outlook.com`. Note: you might need to click on `Sign out and sign in with different account` during the sign in flow to be able to login with a different microsoft account.
- If you see an error like `Failed to update the Microsoft account with 'account2 <account2@outlook.com>'. This could happen if it is already linked to another NuGet account. Contact support for more information.` Follow the steps for [Microsoft account is linked with another NuGet account](#microsoft-account-is-linked-with-another-nuget-account).
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

### How to transform my NuGet account to an organization?

password recovery and transform url link.

### How to recover account from an unmanaged AAD tenant?

details

### How do I change my NuGet account username?

### How to delete my NuGet account?

details