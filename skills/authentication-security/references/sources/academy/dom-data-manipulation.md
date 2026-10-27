# DOM-data manipulation

> Converted source material; attribution: PortSwigger Web Security Academy. Snapshot processed 2026-10-27.
> Source SHA-256: `cba2e611a55026af7fe39af774f1dead5d478e6d9b5f6fc141d58a4636518d47`.
> Conversion: Local English OCR, all 3 text segments. Source opinions and historical examples are preserved, not automatically endorsed.
> OCR literals, payloads, and complex table alignment are not character-verified throughout. Use the curated guidance and exact-text sources for implementation; do not execute OCR examples without checking the publisher.


DOM-data manipulation

DOM-based vulnerabilities ways to reduce your exposure to DOM-data manipulation vulnerabilities.

## What is DOM-data manipulation?

DOM-data manipulation vulnerabilities arise when a script writes attacker-controllable data to a field within the DOM that is used within the visible Ul or client-side logic. An attacker may be able to use this vulnerability to construct a URL that, if visited by another user, will modify the appearance or behavior of the client-side UI. DOM-data manipulation vulnerabilities can be exploited by both reflected and stored DOM-based attacks.

## What is the impact of DOM-data manipulation?

At the lesser end of the scale, an attacker may be able to leverage this vulnerability to perform virtual defacement of the website, such as changing text or images that are displayed on a particular page. However, attacks can be more severe. For example, if the attacker is able to change the src property of an element, they could potentially induce the user to perform unintended actions by importing a malicious JavaScript file.

## Which sinks can lead to DOM-data manipulation vulnerabilities?

The following are some of the main sinks that can lead to DOM-data manipulation vulnerabilities:

---

script . text script . textContent script . inner Text element. set At t r Ibute ( ) element . search element text element. textContent element . innerText element. outer Text value eI ement . name el ement target method type element . backgroundlmage element. cssText codebase document . title document . implementation . createHTMLDocument ( ) history. pushState ( ) history. replaces tate ( )

## How to prevent DOM-data manipulation vulnerabilities

In addition to the general measures described on the DOM-based vulnerabilities page, you should avoid allowing data from any untrusted source to be dynamically written to DOM-data fields. Note that Burp Suite automatically identifies this issue using static code analysis, which may lead to false positives that are not actually exploitable. The relevant code and execution paths should be reviewed to determine whether this vulnerability is indeed present, or whether mitigations are in place that would prevent exploitation.

---
