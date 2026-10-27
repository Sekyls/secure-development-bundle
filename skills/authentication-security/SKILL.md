---
name: authentication-security
description: "Design, implement, fix, or review web authentication: login, sessions, password recovery, MFA, passkeys/WebAuthn, OAuth/OIDC, and JWT validation. Use for identity-specific flows while preserving the existing provider; general object/tenant authorization uses the authorization reference."
---

# Authentication Security

Support the requested identity flow using the application's existing provider, framework, and session model. Do not replace them simply because the reference collection contains a custom implementation.

## Establish the flow and mode

Determine whether the request asks for design, implementation/fixes, or review only. Review mode inspects and reports without edits. Inspect the actual provider configuration, middleware, browser transport, token verifier, account linkage, persistence, and tests. Identify which party asserts identity, how the request proves it, which audience/action is intended, and how expiry and revocation are enforced.

Read [source priority and known disagreements](references/source-policy.md). Old personal preferences, illustrative durations, and OCR examples must not silently become policy. Check current provider/library documentation against the deployed version when options, password parameters, cookie behavior, or protocol support affect correctness.

## Load the relevant reference

- Login, sign-out, expiry, recovery, MFA, email change, or reauthentication: [sessions](references/sessions.md).
- OAuth/OIDC callbacks, linking identities, JWTs, or signing-key selection: [OAuth and JWT](references/oauth-jwt.md).
- Password policy/storage, passkeys, or WebAuthn verification: [passwords and passkeys](references/passwords-passkeys.md).
- Cookies, CSRF, CORS, or browser credential exposure: [browser controls](references/browser.md).
- Object ownership, tenant membership, or roles after authentication: [authorization](references/authorization.md).
- Single-use tokens and racing identity changes: [business invariants](references/business-state.md).

## Deliver and verify

Preserve account compatibility and provider semantics. Bind credentials and one-time operations to the correct account, transaction, purpose, and validity. Keep sensitive actions from bypassing required reauthentication or recovery assurance. Avoid exposing credentials in logs, public errors, browser bundles, or verification evidence.

Test a permitted flow and relevant negative cases: wrong account, replay, expiry, revoked state, wrong issuer/audience/origin, failed verification, and concurrent consumption. When fixing a flow, run the appropriate application regression checks and report provider/deployment assumptions. For review, use [the evidence contract](references/review-method.md) and report only supported findings.

Use the [Markdown catalog](references/source-index.md) for deeper sources and [behavior scenarios](references/scenarios.md) for verifier and lifecycle examples. The skill is self-contained and does not require another skill or the original document collection.
