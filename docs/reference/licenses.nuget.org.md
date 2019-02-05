# licenses.nuget.org

## Rationale

With introduction of the [license expressions](nuspec.md#license) a requirement to have a reliable service that would
provide a reference text for individual license identifiers, exception identifiers or license expression emerged.
The one with a stable URL schema, that is not susceptible to the link rot, so that we can safely use it to provide
backwards compatibility for older clients.

[`licenses.nuget.org`](https://licenses.nuget.org) fulfills that role. Gallery uses it to provide the license
text reference for the pacakages that specify their license using license expression and `nuget.exe pack` operation
generates links to `licenses.nuget.org` for the [`licenseUrl`](nuspec.md#licenseurl) element to provide backwards
compatibility with older clients that don't support the `license` element.

## Protocol

`licenses.nuget.org` is intended to be viewed by people in their browsers, no machine-readable responses are provided.
HTTPS protocol must be used and requests constructed in a certain way. It only responds to `GET` requests. It accepts
license expressions or license exception identifiers as an input in a way specified below.

### License expressions

#### Request

License expressions (including the trivial cases when expression consists of a single license) have to be
[URL-encoded](https://tools.ietf.org/html/rfc3986#section-2.1) and used as a path in the request to
`licenses.nuget.org`.

| License expression | URL to use |
|:---|:---|
MIT                                                | https://licenses.nuget.org/MIT
(MIT)                                              | https://licenses.nuget.org/(MIT)
(LGPL-2.0-only WITH FLTK-exception OR Apache-2.0+) | https://licenses.nuget.org/(LGPL-2.0-only%20WITH%20FLTK-exception%20OR%20Apache-2.0+)

#### Response

`licenses.nuget.org` responds with a web page containing a description of the license expression:
* if supplied license expression contains a single license identifier a web page is returned that contains that
license reference text;
* if supplied license expression is a composite license expression, a web page is returned that contains
the license expression with links to individual license or license exception references.

### License exceptions

#### Request

License exception identifiers have to be URL-encoded and used as a path in the request to `licenses.nuget.org`.
Only single license exception identifier can be supplied in a single request. No additional characters besides
license exception identifier may present in path portion of the URL.

| License exception identifier | URL to use |
|:---|:---|
FLTK-exception            | https://licenses.nuget.org/FLTK-exception
openvpn-openssl-exception | https://licenses.nuget.org/openvpn-openssl-exception

#### Response

`licenses.nuget.org` responds with a web page containing the reference text for the specified
license exception.
