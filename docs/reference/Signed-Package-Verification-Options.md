---
title: NuGet Signed-Package Verification Options
description: Options for NuGet signed-package verification
author: dtivel
ms.author: dtivel
ms.date: 09/01/2023
ms.topic: reference
---

# NuGet signed-package verification options

## Retry untrusted root failures

> [!Note]
> This issue only applies to Windows for root certificates in the [Microsoft Trusted Root Program](https://aka.ms/RootCert).

During certificate chain building, Windows fetches relevant 3rd party root certificates on first use and adds them as locally trusted root certificates.  Internally, Windows initiates this network fetch with an RPC call, and if the system is sufficiently busy, this RPC call may fail.  This failure results in the root certificate not being locally trusted.  This issue may occur the first time a root certificate is observed, but once the root certificate has been locally trusted, the issue will not recur for that certificate.  Typically, chain building will succeed with retries.

For NuGet users, symptoms of this issue are that the NuGet operation will typically succeed on retry and either of the following:

* [NU3028](errors-and-warnings/NU3028.md) with a message like "A certification chain processed correctly but terminated in a root certificate that is not trusted by the trust provider."
* [NU3037](errors-and-warnings/NU3037.md) with a message like "The repository primary signature validity period has expired."

> [!Note]
> This option is available starting from NuGet 6.0.0 and only applies to the Windows-specific failure described above.  The option does not apply to any other scenario and has no effect on Linux or macOS.

You can opt-in to an experimental, automatic retry for untrusted root failures on Windows by setting an environment variable named `NUGET_EXPERIMENTAL_CHAIN_BUILD_RETRY_POLICY` with a value consisting of 2 comma-delimited positive integers representing retry count and sleep interval in milliseconds, respectively. There are no default values; you need to pick retry values that are sensible for you.

For example, setting the environment variable to a value of `3,1000` like so:

<pre>set NUGET_EXPERIMENTAL_CHAIN_BUILD_RETRY_POLICY=3,1000</pre>

...would try up to 4 times (initial try plus 3 retries) with 1 second (1,000 ms) between each try.
