# DOM-based client-side XPath injection

> Converted source material; attribution: PortSwigger Web Security Academy. Snapshot processed 2026-10-27.
> Source SHA-256: `f7d4f62e739c63af46dec7f6dc0f7354b375df321ae3e2d0209ce935fa1d77ba`.
> Conversion: Local English OCR, all 2 text segments. Source opinions and historical examples are preserved, not automatically endorsed.
> OCR literals, payloads, and complex table alignment are not character-verified throughout. Use the curated guidance and exact-text sources for implementation; do not execute OCR examples without checking the publisher.


> Client-side XPath injection

DOM-based vulnerabilities and suggest ways to reduce your exposure to them.

## What is DOM-based XPath injection?

DOM-based XPath-injection vulnerabilities arise when a script incorporates attacker-controllable data into an XPath query. An attacker may be able to use this behavior to construct a URL that, if visited by another application user, will trigger the execution of an arbitrary XPath query, which could cause different data to be retrieved and processed by the website.

## What is the impact of DOM-based XPath injection?

Depending on the purpose for which the query results are used, it may be possible for the attacker to subvert the website's logic or cause unintended actions on behalf of the user.

## Which sinks can lead to XPath-injection vulnerabilities?

The following are some of the main sinks that can lead to DOM-based XPath-injection vulnerabilities: document . evaluate ( )

## How to prevent DOM-based XPath-injection vulnerabilities

In addition to the general measures described on the DOM-based vulnerabilities page, you should avoid allowing data from any untrusted source to be incorporated into XPath queries.

---

Practise exploiting vulnerabilities on realistic targets.

Record your progression from Apprentice to Expert.

See where you rank in our Hall of Fame.

Enter your email

Already got an account? Login here
