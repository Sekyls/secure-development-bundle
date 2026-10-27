# How to prevent OAuth authentication vulnerabilities

> Converted source material; attribution: PortSwigger Web Security Academy. Snapshot processed 2026-10-27.
> Source SHA-256: `304cc40cc290e041f7b01032b7ba7f9f14d19a9cba4d2d22907c9ed6fa0f6147`.
> Conversion: Local English OCR, all 2 text segments. Source opinions and historical examples are preserved, not automatically endorsed.
> OCR literals, payloads, and complex table alignment are not character-verified throughout. Use the curated guidance and exact-text sources for implementation; do not execute OCR examples without checking the publisher.


OAuth 2.0 authentication vulnerabilities implement robust validation of the key inputs, especially the redirect uri parameter. There is very little built-in protection in the OAuth specification, so it's up to developers themselves to make the OAuth flow as secure as possible. It is important to note that vulnerabilities can arise both on the side of the client application and the OAuth service itself. Even if your own implementation is rock solid, you're still ultimately reliant on the application at the other end being equally robust.

## For OAuth service providers

- Require client applications to register a whitelist of valid redirect uris . Wherever possible, use strict byte-for-

byte comparison to validate the URI in any incoming requests. Only allow complete and exact matches rather than using pattern matching. This prevents attackers from accessing other pages on the whitelisted domains.

- Enforce use of the state parameter. Its value should also be bound to the user's session by including some

unguessable, session-specific data, such as a hash containing the session cookie. This helps protect users against CSRF-like attacks. It also makes it much more difficult for an attacker to use any stolen authorization codes.

- On the resource server, make sure you verify that the access token was issued to the same client id that is

making the request. You should also check the scope being requested to make sure that this matches the scope for which the token was originally granted.

## For OAuth client applications

- Make sure you fully understand the details of how OAuth works before implementing it. Many vulnerabilities are

caused by a simple lack of understanding of what exactly is happening at each stage and how this can potentially be exploited.

- Use the state parameter even though it is not mandatory.

- Send a redirect_uri parameter not only to the / authorization endpoint, but also to the / token endpoint.

When developing mobile or native desktop OAuth client applications, it is often not possible to keep the c lent secret private. In these situations, the PKCE ( RFC 7636 ) mechanism may be used to provide additional protection against access code interception or leakage. If you use the OpenID Connect id token , make sure it is properly validated according to the JSON Web Signature, JSON Web Encryption, and OpenID specifications.

- Be careful with authorization codes - they may be leaked via Referer headers when external images, scripts, or

CSS content is loaded. It is also important to not include them in the dynamically generated JavaScript files as they may be executed from external domains via tags.

---

Practise exploiting vulnerabilities on realistic targets.

Record your progression from Apprentice to Expert.

See where you rank in our Hall of Fame.

Enter your email

Already got an account? Login here
