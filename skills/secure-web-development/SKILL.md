---
name: secure-web-development
description: "Implement and fix secure web features, especially JavaScript/TypeScript frontends, Node.js APIs, SQL data access, and business workflows. Use during feature design or coding where a concrete security boundary is involved; use a review workflow for review-only requests."
---

# Secure Web Development

Apply security reasoning to the feature being built. Preserve the user's framework, provider, architecture, and requested scope.

## Work from the application

Read the repository's instructions, entry points, runtime versions, data model, existing controls, and relevant tests. Identify the untrusted actor/input, protected asset, and trust boundary. State the feature's security invariant in practical terms. Do not assume a particular hosting topology, ORM, identity provider, or tenant model from these references.

Select only the relevant guide below. Read its linked source sections when details or conflicting advice matter; do not load the full catalog. Follow [source priority and conversion limits](references/source-policy.md) when interpreting historical or OCR material.

## Implement and verify

Reuse maintained project mechanisms. Make the smallest coherent change that enforces the invariant at the trusted boundary, including alternate entry points and relevant failure paths. Keep authentication, authorization, input syntax, and output-context handling distinct.

Add meaningful regression coverage for the concrete threat and an allowed flow. For stateful or concurrent behavior, test the actual persistence/transport boundary when available rather than treating a mock as proof. Run the relevant project checks; report what changed, why, what passed, and remaining assumptions. A security checklist is not a reason to expand an ordinary feature into an unrelated audit.

If the request only asks for review, inspect and report without editing. The selected skill does not authorize deployment changes, external messages, or active testing of shared systems.

## Topic references

| Work being done | Read when applicable |
| --- | --- |
| Rendering, DOM, messages, cookies, CORS, WebSockets | [Browser controls](references/browser.md) |
| Objects, tenants, permissions, cache isolation | [Authorization](references/authorization.md) |
| Queries, validation, files, processes, templates, XML | [API and data](references/api-data.md) |
| Fetching URLs, proxies, hosts, HTTP framing, shared caches | [Server requests and HTTP](references/server-fetch-http.md) |
| Balances, approvals, replay, concurrent operations | [Business state](references/business-state.md) |
| Dependencies, secrets, deployment, logging, exceptions | [Operations](references/operations.md) |
| Sessions, recovery, MFA, sensitive identity changes | [Sessions](references/sessions.md) |
| OAuth/OIDC callbacks or signed tokens | [OAuth and JWT](references/oauth-jwt.md) |
| Password storage/policy or WebAuthn | [Passwords and passkeys](references/passwords-passkeys.md) |


For source lookup use the [Markdown source catalog](references/source-index.md). For observable examples use [behavior scenarios](references/scenarios.md). The [OWASP coverage map](references/coverage.md) is a final gap check for an explicitly broad assessment, not a mandatory checklist for every edit.
