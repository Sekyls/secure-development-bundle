# DOM-based WebSocket-URL poisoning

> Converted source material; attribution: PortSwigger Web Security Academy. Snapshot processed 2026-10-27.
> Source SHA-256: `b3a0ca1d95eb089633d338c2199f36eb39629a63bb97364dddcf003a6d6e5e9b`.
> Conversion: Local English OCR, all 2 text segments. Source opinions and historical examples are preserved, not automatically endorsed.
> OCR literals, payloads, and complex table alignment are not character-verified throughout. Use the curated guidance and exact-text sources for implementation; do not execute OCR examples without checking the publisher.


WebSocket-URL poisoning

DOM-based vulnerabilities WebSocket-URL poisoning, and suggest ways you can reduce your exposure to this kind of attack.

## What is DOM-based WebSocket-URL poisoning?

WebSocket-URL poisoning occurs when a script uses controllable data as the target URL of a WebSocket connection. An attacker may be able to use this vulnerability to construct a URL that, if visited by another user, will cause the user's browser to open a WebSocket connection to a URL that is under the attacker's control.

## What is the impact of WebSocket-URL poisoning?

The potential impact of WebSocket-URL poisoning depends on how the website uses WebSockets. If the website transmits sensitive data from the user's browser to the WebSocket server, then the attacker may be able to capture this data. If the application reads data from the WebSocket server and processes it in some way, the attacker may be able to subvert the website's logic or deliver client-side attacks against the user.

## Which sinks can lead to WebSocket-URL poisoning

## vulnerabilities?

The WebSocket constructor can lead to WebSocket-URL poisoning vulnerabilities.

## How to prevent DOM-based WebSocket-URL poisoning

## vulnerabilities

In addition to the general measures described on the DOM-based vulnerabilities page, you should avoid allowing data from any untrusted source to dynamically set the target URL of a WebSocket connection.

---

Practise exploiting vulnerabilities on realistic targets.

Record your progression from Apprentice to Expert.

See where you rank in our Hall of Fame.

Enter your email

Already got an account? Login here
