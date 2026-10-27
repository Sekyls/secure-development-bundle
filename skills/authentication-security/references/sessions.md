# Authentication sessions and recovery

## Establish the authentication model

Identify the existing identity provider, cookie or bearer-token transport, session store, middleware, and revocation semantics. Extend maintained mechanisms instead of introducing parallel credential stores or home-grown cryptographic protocols. Authentication proves an identity; authorize each application action separately.

## Lifecycle

Generate opaque session secrets with a cryptographic random generator and avoid storing raw recoverable secrets where a verifier can work. Use the framework/provider's established session format and storage model rather than retrofitting an incompatible token scheme. Rotate session identifiers when establishing authentication or increasing privilege as the platform requires. Enforce expiry and invalidation on the server, not only by deleting a client cookie.

Define inactivity and absolute lifetime from application risk and provider support; avoid copying illustrative durations as universal defaults. Invalidate or re-evaluate privilege-bearing sessions when the application's authorization model changes. Distinguish sign-out of one device, sign-out everywhere, ordinary password change, and compromised-account recovery. In a stateless token model, document how revocation actually works and the remaining acceptance window.

Bind sensitive reauthentication to the actor, intended action, and limited validity. Reusing a general session's authenticated flag after a sensitive action can leave stolen sessions with newly elevated authority. Handle MFA enrollment, factor removal, email change, and recovery as high-impact identity changes.

## Recovery and abuse controls

Use unpredictable, expiring, purpose-bound recovery credentials; atomically consume one-time secrets and bind them to the intended account and action. Do not derive callback links from an untrusted Host header. Avoid account enumeration through status, timing, or delivery behavior where the product requires concealment. Apply proportionate rate limits across relevant account, client, and delivery boundaries; attackers should not gain an easy permanent lockout primitive.

Test stale and revoked sessions, fixation, concurrent recovery attempts, replay, wrong-account tokens, rate-limit behavior, and the normal successful flow. Prove that failed verification causes no identity change or unintended notification.

Read: [session management](sources/owasp/Session_Management_Cheat_Sheet.md), [recovery](sources/owasp/Forgot_Password_Cheat_Sheet.md), [MFA](sources/owasp/Multifactor_Authentication_Cheat_Sheet.md), [authentication sessions](sources/auth/auth-sessions.md), and [session concepts](sources/auth/sessions.md).
