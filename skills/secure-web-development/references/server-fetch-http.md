# Server-side requests and HTTP infrastructure

Use for URL fetching, webhooks, proxies, caches, redirects, host-derived URLs, and requests crossing multiple HTTP parsers.

## SSRF destination enforcement

Prefer a server-controlled destination or a narrow allowlist where the feature permits it. Parse URLs with the actual runtime URL implementation. Constrain scheme, credentials, host, and port; avoid trusting raw substrings. If arbitrary public destinations are required, address IPv4 and IPv6, private and special-use ranges, alternate representations, all DNS answers, and rebinding between validation and connection. Bind the connection to an approved destination with a tested transport mechanism; DNS lookup followed by an unrelated fetch is not a complete defense.

Disable redirects when unnecessary, or repeat destination and credential checks on every hop. Do not forward internal credentials or cookies to untrusted destinations. Bound response size, connection time, overall duration, and content processing. Network egress policy should enforce which internal services and metadata endpoints are reachable. A successful URL parse or TLS check does not establish SSRF safety. Check deployed runtime and transport documentation before writing a dispatcher or DNS-pinning implementation.

## HTTP parser and cache boundaries

Discover the real CDN, load balancer, proxy, protocol conversions, and backend chain. Request smuggling concerns disagreements about message boundaries across hops; changing an application regex is not proof that the infrastructure is safe. Prefer supported patched components, consistent protocol handling, and rejection of ambiguous framing at the responsible layer. Report missing topology evidence rather than claiming a confirmed flaw from the use of a proxy.

For caches, compare cache keys with every input that can alter responses: host, forwarded headers, query parameters, cookies, identity, and tenant. Shared caching must not mix personalized responses. Use an application-configured public origin for password-reset and other sensitive absolute URLs rather than accepting an arbitrary Host or forwarded host. Validate proxy trust settings in the deployment's actual boundary.

## Verification

Use controlled local fixtures for allowed and forbidden destinations, redirect escapes, credential forwarding, oversized responses, and cache isolation. Inspect header handling and cache keys. Active desynchronization or poisoning probes require an explicitly scoped test environment because they can affect other requests; ordinary code-review authorization does not establish one.

Read: [SSRF](sources/owasp/Server_Side_Request_Forgery_Prevention_Cheat_Sheet.md), [webhooks](sources/owasp/Webhook_Security_Cheat_Sheet.md), [cache security](sources/owasp/Web_Cache_Security_Cheat_Sheet.md), and [HTTP headers](sources/owasp/HTTP_Headers_Cheat_Sheet.md). For attack mechanics, use the relevant [source catalog](source-index.md), accounting for OCR limitations.
