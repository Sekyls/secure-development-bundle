# Text descriptions of security flows

These are authored explanations of the security relationships represented in the source topics. They supplement OCR text; they are not a claim that every original diagram was transcribed. All arrows below are ordinary Markdown text, so no image renderer is needed.

## DOM source to sink

URL, storage, message, or saved user content → application transformations → DOM insertion. The decisive boundary is whether the final API parses executable markup/code or inserts text. A filter before later decoding or DOM mutation can lose its intended protection. Read [browser controls](browser.md#rendering-and-dom-data-flow).

## Session binding

Successful authentication → server session bound to an identity → client holds a secret reference → each request validates session state → authorization decides access to a resource. A cookie attributes policy addresses transport and browser behavior; the server still needs expiry, revocation, and authorization. Read [sessions](sessions.md#lifecycle).

## SSRF proxying

Attacker supplies a destination → application server makes a request → internal service or metadata endpoint receives the server's traffic. The attacker gains the server's reach or credentials. Destination enforcement must bind to the connection, including every redirect; network egress controls constrain reach. Read [SSRF enforcement](server-fetch-http.md#ssrf-destination-enforcement).

## Request desynchronization

One byte stream reaches an edge parser → the backend receives the forwarded stream → different framing decisions split that stream at different positions. Bytes interpreted as one request's body by one hop may become another request at the next. Reused connections can then affect unrelated traffic. Read [HTTP boundaries](server-fetch-http.md#http-parser-and-cache-boundaries).

## Cache poisoning and leakage

Request inputs influence response generation → a subset of inputs forms the cache key → the response is reused for another caller. An attacker-controlled input omitted from that key can contaminate later responses; omitted identity or tenant context can leak personalized data. Check actual cache behavior rather than assuming all responses are shared.

## Concurrent consumption

Requests A and B both read an available balance or unused token → both pass a check → both commit an effect. Make the check and state change atomic at the shared persistence boundary. With external effects, also bind retries to durable operation identity. Read [business invariants](business-state.md#state-and-atomicity).

## OAuth callback binding

Browser starts a login transaction → application stores transaction-bound correlation/verifier state → provider authorizes and redirects → application validates callback binding and redeems the code → verified identity is bound to the intended account/session. A token or callback from another transaction must not be substituted. Read [OAuth](oauth-jwt.md#oauth-and-openid-connect).

## Prototype pollution

Untrusted nested object → unsafe property merge or traversal → inherited/shared property changes → a downstream gadget uses that property in a sensitive operation. Pollution and an exploitable gadget are distinct pieces of evidence. Read [browser data flow](browser.md#rendering-and-dom-data-flow).
