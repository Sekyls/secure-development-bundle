# DOM-based denial of service

> Converted source material; attribution: PortSwigger Web Security Academy. Snapshot processed 2026-10-27.
> Source SHA-256: `1cef67b4bce6a1322219e5b653361ec076aae9ce9dd44c5ddec83f6197843ab2`.
> Conversion: Local English OCR, all 2 text segments. Source opinions and historical examples are preserved, not automatically endorsed.
> OCR literals, payloads, and complex table alignment are not character-verified throughout. Use the curated guidance and exact-text sources for implementation; do not execute OCR examples without checking the publisher.


Denial of service

DOM-based vulnerabilities vulnerability, and discuss ways to reduce your exposure to DOM-based DOS attacks.

## What is DOM-based denial of service?

DOM-based denial-of-service vulnerabilities arise when a script passes attacker-controllable data in an unsafe way to a problematic platform API, such as an API whose invocation can cause the user's computer to consume excessive amounts of CPU or disk space. This may result in side effects if the browser restricts the functionality of the website, for example, by rejecting attempts to store data in local Storage or killing busy scripts.

## Which sinks can lead to DOM-based denial-of-service

## vulnerabilities?

The following are some of the main sinks can lead to DOM-based denial-of-service vulnerabilities: requestFi1eSystem ( ) RegExp ( )

## How to prevent DOM-based denial-of-service vulnerabilities

In addition to the general measures described on the DOM-based vulnerabilities page, you should avoid allowing data from any untrusted source to dynamically pass data into problematic platform APIs.

---

have a more personalized learning experience? (It's free!)

SIGN UP LOGIN

Find DOM-based vulnerabilities using

TRY FOR FREE
