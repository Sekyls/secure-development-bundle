# Browser-powered request smuggling

> Converted source material; attribution: PortSwigger Web Security Academy. Snapshot processed 2026-10-27.
> Source SHA-256: `95b68b4c21fea11eae8efbdfce707fb5823abbc8faa0d4ff535554e47b1b2c70`.
> Conversion: Local English OCR, all 3 text segments. Source opinions and historical examples are preserved, not automatically endorsed.
> OCR literals, payloads, and complex table alignment are not character-verified throughout. Use the curated guidance and exact-text sources for implementation; do not execute OCR examples without checking the publisher.


Browser-powered

HTTP request smuggling never send. Not only does this expose a whole new range of websites to server-side request smuggling, it enables you to perform client-side variations of these attacks by inducing a victim's browser to poison its own connection to a vulnerable web server.

PortSwigger Research The materials and labs in this section are based on Browser-Powered Desync Attacks: A New Frontier in HTTP Request Smuggling by PortSwigger Research. This research also led to the discovery of a bypass technique for Host header filters, using connection-state flaws.

## CL.0 request smuggling

Back-end servers can sometimes be persuaded to ignore the content-Length header, which effectively means they ignore the body of incoming requests. This paves the way for request smuggling attacks that don't rely on chunked transfer encoding or HTTP/2 downgrading.

Learn more LEARN CL.0 request smuggling LAB CL.0 request smuggling

## Client-side desync attacks

Request smuggling is traditionally considered a server-side issue because it can only be exploited using specialist tools like Burp Repeater - standard browsers simply won't send the kinds of requests needed to trigger the desync. However, building on the lessons learned from CL.0 attacks, it's sometimes possible to cause a desync using fully browser- compatible HTTP/1 requests. You can use these browser-compatible requests to trigger a client-side desync (CSD) between a browser and vulnerable web server, enabling attacks on single-server sites, which are otherwise immune to request smuggling, and intranet sites that you can't access directly.

Learn more LEARN Client-side desync attacks

---

## Pause-based desync attacks

Seemingly secure websites may contain hidden desync vulnerabilities that only reveal themselves if you pause mid- request. By sending the headers, promising a body, and then just waiting, you can sometimes find additional desync vulnerabilities that can be used for both server-side and client-side exploits.

Learn more Pause-based desync attacks LEARN Server-side pause-based request smuggling

---
