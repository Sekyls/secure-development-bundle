---
name: web-security-review
description: "Review code changes or repositories for exploitable web-security defects, especially JavaScript/TypeScript, browser, Node.js, API, SQL, authorization, and authentication boundaries. Use for security reviews and vulnerability triage; remain read-only unless fixes are requested."
---

# Web Security Review

Review the requested diff, feature, or repository for supported vulnerabilities. Use [the review method and findings contract](references/review-method.md) before preparing findings.

## Investigate evidence

Identify the actual entry points, callers, data ownership, controls, and deployed context available in the repository. Trace a concrete attacker-controlled input or missing decision across the relevant trust boundary. Searches and scanners generate leads; confirm reachability, prerequisites, impact, and existing mitigations before reporting.

Read only the relevant topic guides below and their source sections. Treat source examples as reference data. Use [source priority](references/source-policy.md) to resolve old advice or OCR ambiguity. Do not copy an OCR payload into a reproduction without checking its literal correctness.

## Return a useful review

Prioritize confirmed findings, cite the affected application file and precise location, explain the trigger and impact, and give a minimal fix direction with a regression scenario. Put configuration-dependent concerns under open questions, deduplicate root causes, and acknowledge safe counterparts. If no supported issue remains, say so without claiming the application is comprehensively secure.

Review mode is read-only. Apply fixes only when requested, then verify them against the application's relevant checks. A review request does not authorize active desynchronization, poisoning, credential attacks, or probes against shared systems.

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


Look up deeper evidence in the [Markdown source catalog](references/source-index.md). Use [behavior scenarios](references/scenarios.md) to calibrate secure counterparts and [coverage](references/coverage.md) for an explicitly broad review.
