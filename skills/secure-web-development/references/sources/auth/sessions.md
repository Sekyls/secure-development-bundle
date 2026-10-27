# Sessions

> Converted source material; attribution: pilcrow, Pilcrow's auth book. Snapshot processed 2026-10-27.
> Source SHA-256: `c93c547a4bb93cbcea270a982946f4b790f0f607593acdf6ef4a72bba3fc5c43`.
> Conversion: HTML converted to Markdown; text and code preserved. Source opinions and historical examples are preserved, not automatically endorsed.


When a user visits your website, the browser will make multiple HTTP requests to your server. However, these requests are independent to each other on the protocol level as HTTP is stateless. The server can't tell if 2 requests are from the same client.

A simple workaround is to store the data on the client and have the client send it with future requests. However, this approach doesn't work for data that the client shouldn't be able to see or modify. For example, storing the authenticated user ID on the client and having the server trust whatever user ID the client sends would be a complete breakdown in security. Instead, any relevant data should be stored as a record on the server, and the client can reference it in subsequent requests to continue the user's activity.

In this book, a session refers to a server-side record that represents a client's ongoing activity within an application. To prevent malicious actors from referencing arbitrary sessions, the server issues a session token to the client. The client includes this token on subsequent requests. Because the token is known only to client that started the session, an outside actor cannot hijack the session.

Note that I'll be specifically referring to sessions that keep track of authentication state as "auth session." Like some of my examples, a server can have multiple types of sessions. For example, I often use a sign-up session to keep track of the user's email address and the email address verification process.

If the ID is random and unguessable, a token isn't technically required. The client can just send the ID and this is a common implementation. However, I'd discourage going this approach. Using the identifier as a secret is a footgun and there are many times you'd want to reference the session without leaking sensitive credentials. Instead, I would recommend generating both an ID and a secret for your sessions. Similar to user authentication, ID identifies the session and the secret acts as the proof. The ID and secret is combined to a single token.

The session secret must be generated using a cryptographically-secure random source. Do not use a fast, predictable random generator often found in the language's standard math library. It should also be hashed before storage. This ensures that even if your database is leaked, a malicious actor can't derive a valid session credential. To verify a secret, hash the input value and compare it against the stored hash using a constant-time comparison. If the secret has enough entropy, a fast hashing algorithm can be used unlike user-defined passwords. I recommend using a 32-byte secret and hashing it with SHA-256. Note that with SHA-256, there is little security benefit in making the secret larger than 32 bytes as the output hash is also 32 bytes.

To create the token, encode the secret into a string and join it with the ID, such as by using a period as the delimiter. There are various encoding schemes, but hex encoding, base64, and base64url are common options.

```text
u9qerabnmqwfjrig.oIGZG+9w+flpwCIb5azPgXLdmmS+86KYkQbIag/wXB8=
```

Your main threats against sessions are session hijacking. The session token could be stolen in many different ways, including malware and stolen devices. As such, it's important to put boundaries on sessions. A malicious actor should not be able to take over everything just by stealing a single token. First, this means putting a reasonable expiration. This could be a fixed expiration or a dynamic one. Second, issue a new session and a session token after certain actions such as user authentication. You can also add further restrictions to sessions, such as binding them to an IP address. However, for most consumer-facing websites, I'd recommend against this since IP addresses can change frequently, especially on mobile networks or when using a VPN. A more practical alternative is to bind the session to the country or region inferred from the IP address.

As for storage of the session token, I personally don't have a strong opinion on it. Just store it at the safest option provided the platform. For browsers, all methods provide more or less the same level of security and risks. See the browser client-side storage page for details.
