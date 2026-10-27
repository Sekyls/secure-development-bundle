# Browser and frontend security

Use for rendering untrusted content, browser storage, cross-origin interactions, embedded pages, and WebSockets.

## Rendering and DOM data flow

Trace the actual source through transformations to the eventual sink. Sources include URLs, server data, messages, persisted content, and storage. In React and similar frameworks, normal text interpolation usually applies context-appropriate escaping; inspect escape hatches such as raw HTML, template compilation, and direct DOM assignment separately.

Use text APIs for plain text. When rich HTML is required, use the project's maintained sanitizer with an explicit policy and test the final insertion context. Do not replace it with regex tag stripping. HTML text escaping does not make values safe inside JavaScript, CSS, event handlers, or arbitrary URLs. Validate URL schemes and application destinations separately. Avoid constructing executable template source from untrusted input. CSP adds defense in depth and does not repair an unsafe sink.

For object merging, inspect nested user-controlled keys, prototype traversal, unsafe getters, and configuration gadgets. Prefer explicit field construction, own-property checks, and suitable map structures. Blocking one spelling of `__proto__` alone does not establish safety.

## CSRF cookies and CORS

First establish whether credentials are attached automatically. For cookie-authenticated mutations, use the framework's CSRF protection, correctly bound tokens and/or verified origin policy as appropriate. Match exact parsed origins; do not accept substring or suffix lookalikes. Safe methods must not change state. Treat SameSite as one layer and account for same-site subdomains, navigation behavior, and legitimate federated callbacks.

Set cookie attributes for the application's deployment and protocol. `HttpOnly` does not stop CSRF or make XSS harmless. CORS controls browser response access, not authorization. Credentialed CORS needs an explicit trusted-origin policy; reflecting arbitrary origins or relying on preflight alone is insufficient. Do not globally disable browser protections to make an integration work.

## Messaging and connections

For `postMessage`, validate the exact origin, expected source window when applicable, and runtime message schema; use a specific outbound target origin. For WebSockets, authenticate the handshake, check permitted origins for cookie-based sessions, authorize each action, and handle revocation and bounded messages. Ordinary HTTP CORS middleware does not establish WebSocket protection.

## Verification

Test stored and reflected content in the real rendering context, allowed rich formatting, wrong-origin messages, forbidden cross-origin mutations, and unauthenticated or revoked socket actions. A suspicious API name alone is not evidence of XSS: demonstrate attacker control reaching an executable context.

Read: [XSS controls](sources/owasp/Cross_Site_Scripting_Prevention_Cheat_Sheet.md), [DOM XSS](sources/owasp/DOM_based_XSS_Prevention_Cheat_Sheet.md), [CSRF](sources/owasp/Cross-Site_Request_Forgery_Prevention_Cheat_Sheet.md), [prototype pollution](sources/owasp/Prototype_Pollution_Prevention_Cheat_Sheet.md), and [WebSockets](sources/owasp/WebSocket_Security_Cheat_Sheet.md).
