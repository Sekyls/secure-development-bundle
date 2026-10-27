# DOM-based open redirection

> Converted source material; attribution: PortSwigger Web Security Academy. Snapshot processed 2026-10-27.
> Source SHA-256: `e4e091084bb239e54c48061a595df06600a5ab295223160b18c06124e82c9af4`.
> Conversion: Local English OCR, all 3 text segments. Source opinions and historical examples are preserved, not automatically endorsed.
> OCR literals, payloads, and complex table alignment are not character-verified throughout. Use the curated guidance and exact-text sources for implementation; do not execute OCR examples without checking the publisher.


> Open redirection

DOM-based vulnerabilities suggest ways to reduce your exposure to DOM-based open-redirection vulnerabilities.

## What is DOM-based open redirection?

DOM-based open-redirection vulnerabilities arise when a script writes attacker-controllable data into a sink that can trigger cross-domain navigation. For example, the following code is vulnerable due to the unsafe way it handles the location. hash property:

```text
let url
```

/ . + / . exec (location. hash) ; if (ur I) { location = ur I [0]

An attacker may be able to use this vulnerability to construct a URL that, if visited by another user, will cause a redirection to an arbitrary external domain.

## What is the impact of DOM-based open redirection?

This behavior can be leveraged to facilitate phishing attacks against users of the website, for example. The ability to use an authentic application URL targeting the correct domain and with a valid TLS certificate (if TLS is used) lends credibility to the phishing attack because many users, even if they verify these features, will not notice the subsequent redirection to a different domain. If an attacker is able to control the start of the string that is passed to the redirection API, then it may be possible to escalate this vulnerability into a JavaScript injection attack. An attacker could construct a IJRL with the javas cript : pseudo-protocol to execute arbitrary code when the URL is processed by the browser.

DOM-based open redirection

## Which sinks can lead to DOM-based open-redirection

## vulnerabilities?

The following are some of the main sinks can lead to DOM-based open-redirection vulnerabilities:

---

location . host location . hostname location . href locati on . pathname location. search location . protocol location. assign ( ) location. replace ( ) open ( ) element. srcdoc XMLHttpRequest . open ( ) XMLHttpRequest . send ( ) jQuery. ajax ( ) $.ajax()

## How to prevent DOM-based open-redirection vulnerabilities

In addition to the general measures described in the DOM-vulnerabilities topic, you should avoid dynamically setting redirection targets using data that originated from any untrusted source.

- URL validation bypass cheat sheet

---

vulnerabilities using

## PortSwig(.

Burp su Customers Insights TRY FOR FREE Web vulr 3) Organizations About Burp Sui Testers Careers Blog Y Follow us Release . Developers Contact Research 0 2026 PortSwigger LtC XML external entity injection Directory traversal Privacy Notice Server-side request forgery Modern Slavery Statement
