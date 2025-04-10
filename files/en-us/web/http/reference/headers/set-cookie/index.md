---
title: Set-Cookie
slug: Web/HTTP/Reference/Headers/Set-Cookie
page-type: http-header
browser-compat: http.headers.Set-Cookie
---

{{HTTPSidebar}}

The HTTP **`Set-Cookie`** {{Glossary("response header")}} is used to send a cookie from the server to the user agent, so that the user agent can send it back to the server later.
To send multiple cookies, multiple `Set-Cookie` headers should be sent in the same response.

> [!WARNING]
> Browsers block frontend JavaScript code from accessing the `Set-Cookie` header, as required by the Fetch spec, which defines `Set-Cookie` as a [forbidden response header name](https://fetch.spec.whatwg.org/#forbidden-response-header-name) that [must be filtered out](https://fetch.spec.whatwg.org/#ref-for-forbidden-response-header-name%E2%91%A0) from any response exposed to frontend code.
>
> When a [Fetch API](/en-US/docs/Web/API/Fetch_API/Using_Fetch) or [XMLHttpRequest API](/en-US/docs/Web/API/XMLHttpRequest_API) request [uses CORS](/en-US/docs/Web/HTTP/Guides/CORS#what_requests_use_cors), browsers will ignore `Set-Cookie` headers present in the server's response unless the request includes credentials. Visit [Using the Fetch API - Including credentials](/en-US/docs/Web/API/Fetch_API/Using_Fetch#including_credentials) and the [XMLHttpRequest article](/en-US/docs/Web/API/XMLHttpRequest_API) to learn how to include credentials.

For more information, see the guide on [Using HTTP cookies](/en-US/docs/Web/HTTP/Guides/Cookies).

<table class="properties">
  <tbody>
    <tr>
      <th scope="row">Header type</th>
      <td>{{Glossary("Response header")}}</td>
    </tr>
    <tr>
      <th scope="row">{{Glossary("Forbidden request header")}}</th>
      <td>No</td>
    </tr>
    <tr>
      <th scope="row">{{Glossary("Forbidden response header name")}}</th>
      <td>Yes</td>
    </tr>
  </tbody>
</table>

## Syntax

```http
Set-Cookie: <cookie-name>=<cookie-value>
Set-Cookie: <cookie-name>=<cookie-value>; Domain=<domain-value>
Set-Cookie: <cookie-name>=<cookie-value>; Expires=<date>
Set-Cookie: <cookie-name>=<cookie-value>; HttpOnly
Set-Cookie: <cookie-name>=<cookie-value>; Max-Age=<number>
Set-Cookie: <cookie-name>=<cookie-value>; Partitioned
Set-Cookie: <cookie-name>=<cookie-value>; Path=<path-value>
Set-Cookie: <cookie-name>=<cookie-value>; Secure

Set-Cookie: <cookie-name>=<cookie-value>; SameSite=Strict
Set-Cookie: <cookie-name>=<cookie-value>; SameSite=Lax
Set-Cookie: <cookie-name>=<cookie-value>; SameSite=None; Secure

// Multiple attributes are also possible, for example:
Set-Cookie: <cookie-name>=<cookie-value>; Domain=<domain-value>; Secure; HttpOnly
```

## Attributes

- `<cookie-name>=<cookie-value>`

  - : Defines the cookie name and its value.
    A cookie definition begins with a name-value pair.

    A `<cookie-name>` can contain any US-ASCII characters except for: control characters ({{Glossary("ASCII")}} characters 0 up to 31 and ASCII character 127) or separator characters (space, tab and the characters: `( ) < > @ , ; : \ " / [ ] ? = { }`)

    A `<cookie-value>` can optionally be wrapped in double quotes and include any US-ASCII character excluding control characters (ASCII characters 0 up to 31 and ASCII character 127), {{glossary("Whitespace")}}, double quotes, commas, semicolons, and backslashes.

    **Encoding**: Many implementations perform {{Glossary("Percent-encoding", "percent-encoding")}} on cookie values.
    However, this is not required by the RFC specification.
    The percent-encoding does help to satisfy the requirements of the characters allowed for `<cookie-value>`.

    > [!NOTE]
    > Some `<cookie-name>` have a specific semantic:
    >
    > **`__Secure-` prefix**: Cookies with names starting with `__Secure-` (dash is part of the prefix) must be set with the `secure` flag from a secure page (HTTPS).
    >
    > **`__Host-` prefix**: Cookies with names starting with `__Host-` are sent only to the host subdomain or domain that set them, and not to any other host.
    > They must be set with the `secure` flag, must be from a secure page (HTTPS), must not have a domain specified, and the path must be `/`.

- `Domain=<domain-value>` {{optional_inline}}

  - : Defines the host to which the cookie will be sent.

    Only the current domain can be set as the value, or a domain of a higher order, unless it is a public suffix. Setting the domain will make the cookie available to it, as well as to all its subdomains.

    If omitted, this attribute defaults to the host of the current document URL, not including subdomains.

    Contrary to earlier specifications, leading dots in domain names (`.example.com`) are ignored.

    Multiple host/domain values are _not_ allowed, but if a domain _is_ specified, then subdomains are always included.

- `Expires=<date>` {{optional_inline}}

  - : Indicates the maximum lifetime of the cookie as an HTTP-date timestamp.
    See {{HTTPHeader("Date")}} for the required formatting.

    If unspecified, the cookie becomes a **session cookie**.
    A session finishes when the client shuts down, after which
    the session cookie is removed.

    > [!WARNING]
    > Many web browsers have a _session restore_ feature that will save all tabs and restore them the next time the browser is used. Session cookies will also be restored, as if the browser was never closed.

    The `Expires` attribute is set by the server with a value relative to its own internal clock, which may differ from that of the client browser.
    Firefox and Chromium-based browsers internally use an expiry (max-age) value that is adjusted to compensate for clock difference, storing and expiring cookies based on the time intended by the server.
    The adjustment for clock skew is calculated from the value of the {{httpheader("DATE")}} header.
    Note that the specification explains how the attribute should be parsed, but does not indicate if/how the value should be corrected by the recipient.

- `HttpOnly` {{optional_inline}}

  - : Forbids JavaScript from accessing the cookie, for example, through the {{domxref("Document.cookie")}} property.
    Note that a cookie that has been created with `HttpOnly` will still be sent with JavaScript-initiated requests, for example, when calling {{domxref("XMLHttpRequest.send()")}} or {{domxref("Window/fetch", "fetch()")}}.
    This mitigates attacks against cross-site scripting ({{Glossary("Cross-site_scripting", "XSS")}}).

- `Max-Age=<number>` {{optional_inline}}

  - : Indicates the number of seconds until the cookie expires. A zero or negative number will expire the cookie immediately. If both `Expires` and `Max-Age` are set, `Max-Age` has precedence.

- `Partitioned` {{optional_inline}}

  - : Indicates that the cookie should be stored using partitioned storage.
    Note that if this is set, the [`Secure` directive](#secure) must also be set.
    See [Cookies Having Independent Partitioned State (CHIPS)](/en-US/docs/Web/Privacy/Guides/Privacy_sandbox/Partitioned_cookies) for more details.

- `Path=<path-value>` {{optional_inline}}

  - : Indicates the path that _must_ exist in the requested URL for the browser to send the `Cookie` header.

    The forward slash (`/`) character is interpreted as a directory separator, and subdirectories are matched as well. For example, for `Path=/docs`,

    - the request paths `/docs`, `/docs/`, `/docs/Web/`, and `/docs/Web/HTTP` will all match.
    - the request paths `/`, `/docsets`, `/fr/docs` will not match.

- `SameSite=<samesite-value>` {{optional_inline}}

  - : Controls whether or not a cookie is sent with cross-site requests,
    providing some protection against cross-site request forgery attacks ({{Glossary("CSRF")}}).

    The possible attribute values are:

    - `Strict`

      - : Means that the browser sends the cookie only for same-site requests, that is, requests originating from the same site that set the cookie.
        If a request originates from a different domain or scheme (even with the same domain), no cookies with the `SameSite=Strict` attribute are sent.

    - `Lax`

      - : Means that the cookie is not sent on cross-site requests, such as on requests to load images or frames, but is sent when a user is navigating to the origin site from an external site (for example, when following a link).
        This is the default behavior if the `SameSite` attribute is not specified.

        > [!WARNING]
        > Not all browsers set `SameSite=Lax` by default.
        > See [Browser compatibility](#browser_compatibility) for details.

    - `None`

      - : Means that the browser sends the cookie with both cross-site and same-site requests.
        The `Secure` attribute must also be set when setting this value, like so `SameSite=None; Secure`. If `Secure` is missing an error will be logged:

        ```plain
        Cookie "myCookie" rejected because it has the "SameSite=None" attribute but is missing the "secure" attribute.

        This Set-Cookie was blocked because it had the "SameSite=None" attribute but did not have the "Secure" attribute, which is required in order to use "SameSite=None".
        ```

        > [!NOTE]
        > A [`Secure`](#secure) cookie is only sent to the server with an encrypted request over the HTTPS protocol. Note that insecure sites (`http:`) can't set cookies with the `Secure` directive, and therefore can't use `SameSite=None`.

        > [!WARNING]
        > Cookies with the `SameSite=None; Secure` that do not also have the [`Partitioned`](#partitioned) attribute may be blocked in cross-site contexts on future browser versions. This behavior protects user data from cross-site tracking. See [Cookies Having Independent Partitioned State (CHIPS)](/en-US/docs/Web/Privacy/Guides/Privacy_sandbox/Partitioned_cookies) and [Third-party cookies](/en-US/docs/Web/Privacy/Guides/Third-party_cookies).

- `Secure` {{optional_inline}}

  - : Indicates that the cookie is sent to the server only when a request is made with the `https:` scheme (except on localhost), and therefore, is more resistant to [man-in-the-middle](/en-US/docs/Glossary/MitM) attacks.

    > [!NOTE]
    > Do not assume that `Secure` prevents all access to sensitive information in cookies (session keys, login details, etc.).
    > Cookies with this attribute can still be read/modified either with access to the client's hard disk or from JavaScript if the `HttpOnly` cookie attribute is not set.
    >
    > Insecure sites (`http:`) cannot set cookies with the `Secure` attribute. The `https:` requirements are ignored when the `Secure` attribute is set by localhost.

## Examples

### Session cookie

Session cookies are removed when the client shuts down. Cookies are session cookies if they do not specify the `Expires` or `Max-Age` attribute.

```http
Set-Cookie: sessionId=38afes7a8
```

### Permanent cookie

Permanent cookies are removed at a specific date (`Expires`) or after a specific length of time (`Max-Age`) and not when the client is closed.

```http
Set-Cookie: id=a3fWa; Expires=Wed, 21 Oct 2015 07:28:00 GMT
```

```http
Set-Cookie: id=a3fWa; Max-Age=2592000
```

### Invalid domains

A cookie for a domain that does not include the server that set it [should be rejected by the user agent](https://datatracker.ietf.org/doc/html/rfc6265#section-4.1.2.3).

The following cookie will be rejected if set by a server hosted on `original-company.com`:

```http
Set-Cookie: qwerty=219ffwef9w0f; Domain=some-company.co.uk
```

A cookie for a subdomain of the serving domain will be rejected.

The following cookie will be rejected if set by a server hosted on `example.com`:

```http
Set-Cookie: sessionId=e8bb43229de9; Domain=foo.example.com
```

### Cookie prefixes

Cookie names prefixed with `__Secure-` or `__Host-` can be used only if they are set with the `secure` attribute from a secure (HTTPS) origin.

In addition, cookies with the `__Host-` prefix must have a path of `/` (meaning any path at the host) and must not have a `Domain` attribute.

> [!WARNING]
> For clients that don't implement cookie prefixes, you cannot count on these additional assurances, and prefixed cookies will always be accepted.

```http
// Both accepted when from a secure origin (HTTPS)
Set-Cookie: __Secure-ID=123; Secure; Domain=example.com
Set-Cookie: __Host-ID=123; Secure; Path=/

// Rejected due to missing Secure attribute
Set-Cookie: __Secure-id=1

// Rejected due to the missing Path=/ attribute
Set-Cookie: __Host-id=1; Secure

// Rejected due to setting a Domain
Set-Cookie: __Host-id=1; Secure; Path=/; Domain=example.com
```

### Partitioned cookie

```http
Set-Cookie: __Host-example=34d8g; SameSite=None; Secure; Path=/; Partitioned;
```

> [!NOTE]
> Partitioned cookies must be set with `Secure`. In addition, it is recommended to use the `__Host` prefix when setting partitioned cookies to make them bound to the hostname and not the registrable domain.

## Specifications

{{Specifications}}

## Browser compatibility

{{Compat}}

## See also

- [HTTP cookies](/en-US/docs/Web/HTTP/Guides/Cookies)
- {{HTTPHeader("Cookie")}}
- {{domxref("Document.cookie")}}
- [Samesite cookies explained](https://web.dev/articles/samesite-cookies-explained) (web.dev blog)
