# DOM-based HTML5-storage manipulation

> Converted source material; attribution: PortSwigger Web Security Academy. Snapshot processed 2026-10-27.
> Source SHA-256: `243693dc4174aa5b1d87b4d29453083222837264f336c2902e213e54f41c7ebf`.
> Conversion: Local English OCR, all 2 text segments. Source opinions and historical examples are preserved, not automatically endorsed.
> OCR literals, payloads, and complex table alignment are not character-verified throughout. Use the curated guidance and exact-text sources for implementation; do not execute OCR examples without checking the publisher.


> HTML5-storage manipulation

DOM-based vulnerabilities used as part of this kind of attack, and suggest ways to reduce your exposure to HTML5-storage manipulation.

## What is DOM-based HTML5-storage manipulation?

HTML5-storage manipulation vulnerabilities arise when a script stores attacker-controllable data in the HTML5 storage of the web browser (either local Storage or sessionStorage ). An attacker may be able to use this behavior to construct a URL that, if visited by another user, will cause the user's browser to store attacker-controllable data. This behavior does not in itself constitute a security vulnerability. However, if the application later reads data back from storage and processes it in an unsafe way, an attacker may be able to leverage the storage mechanism to deliver other DOM-based attacks, such as cross-site scripting and JavaScript injection.

## Which sinks can lead to DOM-based HTML5-storage

## manipulation vulnerabilities?

The following are some of the main sinks that can lead to DOM-based HTML5-storage manipulation vulnerabilities: sessionStorage . set Item ( ) local Storage . setltem ( )

## How to prevent DOM-based HTML5-storage manipulation

## vulnerabilities

In addition to the general measures described on the DOM-based vulnerabilities page, you should avoid allowing data from any untrusted source to be placed in HTML5 storage.

---

Practise exploiting vulnerabilities on realistic targets.

Record your progression from Apprentice to Expert.

See where you rank in our Hall of Fame.

Enter your email

Already got an account? Login here
