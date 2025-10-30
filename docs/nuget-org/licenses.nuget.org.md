---
title: licenses.nuget.org
description: Protocol and display information for licenses.nuget.org. Describes the SPDX data source and rationale.
author: agr
ms.author: angrigor
ms.date: 03/02/2023
ms.topic: article
---
# licenses.nuget.org

## Rationale

With the introduction of the [license expressions](../reference/nuspec.md#license), a requirement emerged to have a reliable service
that would provide a reference text for individual license identifiers, exception identifiers or license expressions.
An additional requirement for this service is to have a stable URL schema, that is not susceptible to link rot,
so that we can safely use it to provide backwards compatibility for older clients.

Licenses.nuget.org fulfills that role. Nuget.org uses it to provide the license text reference for packages that
specify their license using a license expression. `nuget pack` or packing with other
[client tools](../install-nuget-client-tools.md) set
the [`licenseUrl`](../reference/nuspec.md#licenseurl) element to point to licenses.nuget.org to provide backwards
compatibility with older clients that don't support the `license` element.

## License and exception text

The license and license exception information displayed on licenses.nuget.org is copied from the SPDX project's [license list data repository](https://github.com/spdx/license-list-data). The format that the information is displayed closely mimics the format used by the SPDX website itself, e.g. see [MIT on licenses.nuget.org](https://licenses.nuget.org/MIT) and [MIT on SPDX.org](https://spdx.org/licenses/MIT.html).

Licenses that are not approved by Open Source Initiative or the Free Software Foundation are not hosted on licenses.nuget.org and are excluded.

Several styles in addition to plain text are used in the display of the license. According to the [SPDX license list data FAQ](https://github.com/spdx/license-list-XML/blob/main/DOCS/faq.md#what-does-the-blue-text-and-red-text-mean-in-the-license-list-entry), red text is considered replaceable and blue text is considered omitable. For more generally information about the SPDX license list data, see their [FAQ](https://github.com/spdx/license-list-XML/blob/main/DOCS/faq.md) and the [SPDX license template specification](https://spdx.github.io/spdx-spec/v2.3/license-matching-guidelines-and-templates/).

Note that the data is copied from SPDX to licenses.nuget.org by the nuget.org on an ad hoc basis. If a license identifier is approved by the Open Source Initiative or the Free Software Foundation but does not appear on licenses.nuget.org, please [report an issue](https://github.com/NuGet/NuGetGallery/issues/new/choose), and the nuget.org team work to update licenses.nuget.org and nuget.org package upload validation with the latest data from SPDX.

If you, as a package author, are not satisfied with the shared license text available on licenses.nuget.org, you can consider using [embedded license text](../reference/nuspec.md#license) (`<license type="file">`) instead of a license expression for your NuGet package. This allows you to fully customize your licensing terms and include the customized text within the package.

## Protocol

Licenses.nuget.org is intended to be viewed by people in their browsers, no machine-readable responses are provided.
HTTPS protocol must be used and requests are expected to be constructed in a certain way. It only supports `GET` requests.
It accepts license expressions or license exception identifiers as an input in a way specified below. Please note, that all
elements of license expressions are case sensitive, and therefore all input to licenses.nuget.org is case sensitive as well.

### License expressions

#### Request

License expressions (including the trivial cases when expression consists of a single license) have to be
[URL-encoded](https://tools.ietf.org/html/rfc3986#section-2.1) and used as a path in the request to
licenses.nuget.org.

| License expression | URL to use |
|:---|:---|
| MIT                                                | <https://licenses.nuget.org/MIT> |
| (MIT)                                              | <https://licenses.nuget.org/(MIT)> |
| (LGPL-2.0-only WITH FLTK-exception OR Apache-2.0+) | <https://licenses.nuget.org/(LGPL-2.0-only%20WITH%20FLTK-exception%20OR%20Apache-2.0+)> |

The service supports only license identifiers and license exception identifiers that are accepted by
nuget.org. Notably, this means only license identifiers that are approved by the Open Source Initiative or the Free Software Foundation will be accepted. All license expressions that contain unsupported license identifiers
or license exception identifiers or that does not conform to license expression syntax are considered
invalid.

#### Response

Licenses.nuget.org responds to requests containing valid license expressions with an HTTP 200 status code and
a web page containing a description of the license expression:

* if supplied license expression contains a single license identifier a web page is returned that contains that
license reference text;
* if supplied license expression is a composite license expression, a web page is returned that contains
the license expression with links to individual license or license exception references.

Any requests that contain an invalid license expression result in an HTTP 404 response.

### License exceptions

#### Request

License exception identifiers must be URL-encoded and used as a path in the request to licenses.nuget.org.
Only a single license exception identifier can be supplied in a single request. No additional characters besides
license exception identifier may present in the path portion of the URL.

| License exception identifier | URL to use |
|:---|:---|
|FLTK-exception            | <https://licenses.nuget.org/FLTK-exception> |
|openvpn-openssl-exception | <https://licenses.nuget.org/openvpn-openssl-exception> |

#### Response

Licenses.nuget.org responds to a request with a known license exception identifier with a HTTP 200 response and
a web page containing the reference text for the specified license exception.

Any request containing an unsupported license exception identifier results in an HTTP 404 response.
