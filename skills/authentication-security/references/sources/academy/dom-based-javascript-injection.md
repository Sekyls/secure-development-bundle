# DOM-based JavaScript injection

> Converted source material; attribution: PortSwigger Web Security Academy. Snapshot processed 2026-10-27.
> Source SHA-256: `45fe309ce637c5b6531a7fe50fd14ee5381cdb80dd0321f34cbcf447acbe7370`.
> Conversion: Local English OCR, all 2 text segments. Source opinions and historical examples are preserved, not automatically endorsed.
> OCR literals, payloads, and complex table alignment are not character-verified throughout. Use the curated guidance and exact-text sources for implementation; do not execute OCR examples without checking the publisher.


> JavaScript injection

DOM-based vulnerabilities suggest ways to reduce your exposure to JavaScript-injection vulnerabilities.

## What is DOM-based JavaScript injection?

DOM-based JavaScript-injection vulnerabilities arise when a script executes attacker-controllable data as JavaScript. An attacker may be able to use the vulnerability to construct a URL that, if visited by another user, will cause arbitrary JavaScript supplied by the attacker to execute in the context of the user's browser session. Users can be induced to visit the attacker's malicious URL in various ways, similar to the usual attack-delivery vectors for reflected cross-site scripting vulnerabilities. For more information, please refer to our page on DOM XSS.

## What is the impact of a DOM-based JavaScript-injection attack?

The attacker-supplied code can perform a wide variety of actions, such as stealing the victim's session token or login credentials, performing arbitrary actions on the victim's behalf, or even logging their keystrokes.

## Which sinks can lead to DOM-based JavaScript-injection

## vulnerabilities?

The following are some of the main sinks that can lead to DOM-based JavaScript-injection vulnerabilities: eval ( ) Function ( ) set Timeout ( ) setlnterval ( ) set Immediate ( ) execCommand ( ) execScript ( ) msSetImmedxate ( ) range . createContextuaIFragment ( ) crypto . generateCRMFRequest ( )

## How to prevent DOM-based JavaScript-injection vulnerabilities

In addition to the general measures described on the DOM-based vulnerabilities page, you should avoid allowing data from any untrusted source to be executed as JavaScript.

---

Practise exploiting vulnerabilities on realistic targets.

Record your progression from Apprentice to Expert.

See where you rank in our Hall of Fame.

Enter your email

Already got an account? Login here
