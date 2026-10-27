# DOM-based client-side JSON injection

> Converted source material; attribution: PortSwigger Web Security Academy. Snapshot processed 2026-10-27.
> Source SHA-256: `1dd1111f5344233076f0acb49758b81c8b627855ae2555fe666f559ae1684590`.
> Conversion: Local English OCR, all 2 text segments. Source opinions and historical examples are preserved, not automatically endorsed.
> OCR literals, payloads, and complex table alignment are not character-verified throughout. Use the curated guidance and exact-text sources for implementation; do not execute OCR examples without checking the publisher.


> Client-side JSON injection

DOM-based vulnerabilities be, and suggest ways to reduce your exposure to this kind of vulnerability.

## What is DOM-based JSON injection?

DOM-based JSON-injection vulnerabilities arise when a script incorporates attacker-controllable data into a string that is parsed as a JSON data structure and then processed by the application. An attacker may be able to use this behavior to construct a URL that, if visited by another user, will cause arbitrary JSON data to be processed.

## What is the impact of a DOM-based JSON-injection attack?

Depending on the purpose for which this data is used, it may be possible for an attacker to subvert the website's logic, or cause unintended actions on behalf of another user.

## Which sinks can lead to DOM-based JSON-injection

## vulnerabilities?

JSON . parse ( ) jQuery .parseJSON ( ) $ . parseJSON ( )

## How to prevent client-side JSON-injection vulnerabilities?

In addition to the general measures described on the DOM-based vulnerabilities page, you should avoid allowing strings containing data from any untrusted source to be parsed as JSON

---

Practise exploiting vulnerabilities on realistic targets.

Record your progression from Apprentice to Expert.

See where you rank in our Hall of Fame.

Enter your email

Already got an account? Login here
