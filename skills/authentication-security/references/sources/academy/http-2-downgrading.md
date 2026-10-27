# HTTP_2 downgrading

> Converted source material; attribution: PortSwigger Web Security Academy. Snapshot processed 2026-10-27.
> Source SHA-256: `1de6c97f887c3dda775a129b1f203094a49d34ba9f7707d0c31f03f48ce23995`.
> Conversion: Local English OCR, all 2 text segments. Source opinions and historical examples are preserved, not automatically endorsed.
> OCR literals, payloads, and complex table alignment are not character-verified throughout. Use the curated guidance and exact-text sources for implementation; do not execute OCR examples without checking the publisher.


> HTTP/2 downgrading

HTTP request smuggling infrastructure that only speaks HTTP/I . As a result, it has become common practice for front-end servers rewrite each incoming HTTP/2 request using HTTP/I syntax, effectively generating its HTTP/I equivalent. This "downgraded" request is then forwarded to the relevant back-end server instead.

Front—end Back—end

```text
HTTP/2
```

HTTP/I.I

When the HTTP/1 -speaking back-end issues a response, the front-end server reverses this process to generate the

```text
HTTP/2 response that it returns to the client.
```

This works because each version of the protocol is fundamentally just a different way of representing the same information. Each item in an HTTP/I message has an approximate equivalent in HTTP/2.

```text
HTTP/2
```

HTTP/I.I

:meth0d POST

```text
POST / login
```

ERP/ 1.1 :path /109in Bost t Content—type: : authority Content—Lengeh: 32 content-type application/ x-www-form-urlencoded u r Ios&pas sword—nontoya user namea:ca rlos&pa sswordzmontoya

As a result, it's relatively straightforward for servers to convert these requests and responses between the two protocols. In fact, this is how Burp is able to display HTTP/2 messages in the message editor using HTTP/I syntax.

```text
HTTP/2 downgrading is extremely widespread and is even the default behavior for a number of popular reverse proxy
```

services. In some cases, there isn't even an option to disable it.

## What risks are associated with HTTP/2 downgrading?

```text
HTTP/2 downgrading can expose websites to request smuggling attacks, even though HTTP/2 itself is generally
```

considered immune when used end to end.

---

ways to specify the length of the same request, which is the basis of all request smuggling attacks.

- HTTP/2 request smuggling
