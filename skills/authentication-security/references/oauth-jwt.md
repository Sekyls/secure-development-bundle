# OAuth, OpenID Connect, and JWT boundaries

## OAuth and OpenID Connect

Establish whether the flow delegates API access or authenticates a user; OAuth alone is not an identity assertion format. Use the provider's maintained SDK and the client's actual deployment model. Keep client secrets out of public browser or mobile clients.

For a new integration, use an authorization-code flow with transaction-bound PKCE and the supported `S256` method. Bind callbacks to the initiating browser and transaction using the provider's supported state/nonce protections; validate redirect destinations exactly. A constant state, unused nonce, or verification on a different session does not provide the intended binding. Verify issuer and token response context; do not link accounts solely because an untrusted identity claim supplies a familiar email address.

Test replayed or missing state, verifier mismatch, reused codes, wrong issuer, malicious post-login destinations, and account-linking collisions. Preserve legitimate callback cookie behavior when adjusting SameSite.

## JWT verification

Decoding a token is not verification. Configure allowed algorithms, trusted issuer/key source, intended audience, time constraints, and token purpose through the actual library/provider verifier. Required claims depend on the token profile; do not treat a missing expiry as acceptable for an expiring access-token profile. Distinguish ID tokens, access tokens, refresh tokens, and application sessions.

Do not choose verification behavior from an attacker-controlled algorithm or fetch an arbitrary key URL from a token header. Bind key lookup to the trusted issuer; handle legitimate key rotation without accepting arbitrary issuers or silently skipping signature checks. Redact tokens from logs. Test a valid signature with wrong audience, forged signature, disallowed algorithm, expired token, future not-before, unknown issuer/key, and wrong token type.

Sources: [OAuth cheat sheet](sources/owasp/OAuth2_Cheat_Sheet.md), [JWT cheat sheet](sources/owasp/JSON_Web_Token_Cheat_Sheet.md), and the brief [current primary-source checks](source-policy.md#current-primary-source-checks).
