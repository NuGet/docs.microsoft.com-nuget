# licenses.nuget.org

## Why

With introduction of [license expressions](nuspec.md#license) a requirement to have a reliable service that would
provide a reference license text for individual license IDs, exception IDs or license expression arose. The one that
is not be susceptible to the link rot and the one that we can safely link to provide backwards compatibility for
older clients.

[Licenses.nuget.org](https://licenses.nuget.org) fulfulls that role. Gallery web site uses it to provide the license
text reference for the pacakages that specify their license using license expression and `nuget.exe pack` operation
generates links to `licenses.nuget.org` for the [`licenseUrl`](nuspec.md#licenseurl) element to provide backwards
compatibility with the older clients that don't know how to handle the `license` element.

## Protocol

`licenses.nuget.org` is intended to be viewed by people in their browsers. HTTPS protocol must be used and requests
constructed in a certain way.

### License expressions

#### Request

License expression (including the trivial cases when expression consists of a single license) have to be
[URL-encoded](https://tools.ietf.org/html/rfc3986#section-2.1) and used as a path in the request to
`licenses.nuget.org`.

| License expression | Url to use |
|:---|:---|
MIT               | https://licenses.nuget.org/MIT
(MIT)             | https://licenses.nuget.org/(MIT)
MIT OR Apache-2.0 | https://licenses.nuget.org/MIT%20OR%20Apache-2.0

#### Response

`licenses.nuget.org` will respond with two kinds of responses:
* if supplied license expression contains a single license ID a web page is returned that contains that
license's reference text;
* if supplied license expression is a composite license expression, a web page is returned that contains
the license expression with links to individual license or exception references.

### License exceptions

#### Request

License exception IDs have to be URL-encoded and used as a path in the request to `licenses.nuget.org`.
Only one license exception ID can be supplied in a single request. No additional characters besides
exception ID may present in path portion of the URL.

| License exception ID | Url to use |
|:---|:---|
FLTK-exception            | https://licenses.nuget.org/FLTK-exception
openvpn-openssl-exception | https://licenses.nuget.org/openvpn-openssl-exception

#### Response

`licenses.nuget.org` will respond with a web page containing the reference text for the specified
license exception.
