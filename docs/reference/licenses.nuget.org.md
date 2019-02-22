# licenses.nuget.org

## Rationale

With the introduction of the [license expressions](nuspec.md#license) a requirement emerged to have a reliable service
that would provide a reference text for individual license identifiers, exception identifiers or license expressions.
An additional requirement for this service is to have a stable URL schema, that is not susceptible to link rot,
so that we can safely use it to provide backwards compatibility for older clients.

Licenses.nuget.org fulfills that role. Nuget.org uses it to provide the license text reference for packages that
specify their license using a license expression. `nuget pack` or packing with other
[client tools](https://docs.microsoft.com/en-us/nuget/install-nuget-client-tools) set
the [`licenseUrl`](nuspec.md#licenseurl) element to point to licenses.nuget.org to provide backwards
compatibility with older clients that don't support the `license` element.

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
MIT                                                | https://licenses.nuget.org/MIT
(MIT)                                              | https://licenses.nuget.org/(MIT)
(LGPL-2.0-only WITH FLTK-exception OR Apache-2.0+) | https://licenses.nuget.org/(LGPL-2.0-only%20WITH%20FLTK-exception%20OR%20Apache-2.0+)

The service supports only license identifiers and license exception identifiers that are accepted by
nuget.org. All license expressions that contain unsupported license identifiers
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
FLTK-exception            | https://licenses.nuget.org/FLTK-exception
openvpn-openssl-exception | https://licenses.nuget.org/openvpn-openssl-exception

#### Response

Licenses.nuget.org responds to a request with a known license exception identifier with a HTTP 200 response and
a web page containing the reference text for the specified license exception.

Any request containing an unsupported license exception identifier results in an HTTP 404 response.