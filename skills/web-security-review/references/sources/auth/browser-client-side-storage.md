# Browser client-side storage

> Converted source material; attribution: pilcrow, Pilcrow's auth book. Snapshot processed 2026-10-27.
> Source SHA-256: `ac6f01a79b35850a5fa8bcbb2833816e5abfd48fbd49e1494b92dfc16975121a`.
> Conversion: HTML converted to Markdown; text and code preserved. Source opinions and historical examples are preserved, not automatically endorsed.


Browsers provide two main mechanisms for client-side data storage: cookies and the Web Storage API (`localStorage` and `sessionStorage`). The Web Storage API is easier to work with but browsers are usually more aggressive in clearing data stored on it over cookies. Cookies are also automatically included in requests, which is essential if your website renders HTML on the server.

However, cookies require a bit of extra care. Under certain conditions, browsers automatically include cookies even when a request is initiated by a third-party website. This means if the user is signed in into your app and the session token is stored in a cookie, a malicious website can make authenticated requests and take actions on behalf of the user. This is called cross-site request forgery (CSRF) and is one of the oldest attacks on the web. Using the `SameSite` cookie attribute can mitigate many cross-site attacks, but it's not a complete solution on its own. See the [CSRF](csrf.md) page to learn how to mitigate it.

You can also control whether client-side JavaScript can access a cookie by using the `HttpOnly` attribute. Including this flag prevents client-side scripts from reading the cookie directly. However, this doesn't stop client-side scripts from making requests that include the cookie. An attacker exploiting a cross-site scripting (XSS) vulnerability can still send authenticated requests. That said, it does prevent malicious scripts from directly stealing and exporting user tokens. It's also a useful defense against supply-chain attacks. For example, using this flag will protect cookies from a malicious package that attempts to read all stored cookies. The same discussion applies to the Web Storage API, since it is also accessible by client-side JavaScript.

Finally, always include the `Secure` attribute in your cookies. This flag ensures that the cookie is only transmitted over "secure channels" as defined by the browser. This usually means HTTPS. Some browsers, like Chrome, also accept localhost but Safari is a notable exception and strictly requires HTTPS.

While cookies offer slight security advantages when the `HttpOnly` attribute is used, both storage options provide a similar overall level of security. In either case, you must still guard against XSS vulnerabilities by using proper sanitization and a Content Security Policy. Additionally, neither method encrypts data, so sensitive personal information should never be stored on the client side.
