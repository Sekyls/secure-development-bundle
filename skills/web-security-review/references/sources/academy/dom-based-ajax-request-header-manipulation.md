# DOM-based Ajax request-header manipulation

> Converted source material; attribution: PortSwigger Web Security Academy. Snapshot processed 2026-10-27.
> Source SHA-256: `f480716621184b2fdd809416c7be1d4e47775887c5257dba71a3542d6b93d421`.
> Conversion: Local English OCR, all 2 text segments. Source opinions and historical examples are preserved, not automatically endorsed.
> OCR literals, payloads, and complex table alignment are not character-verified throughout. Use the curated guidance and exact-text sources for implementation; do not execute OCR examples without checking the publisher.


> Ajax request-header manipulation

DOM-based vulnerabilities kind of attack, and suggest ways to reduce your exposure to Ajax request-header manipulation vulnerabilities.

## What is DOM-based Ajax request-header manipulation?

Using Ajax enables a website to make asynchronous requests to the server so that web applications can dynamically change content on the page without the need to reload the entire page. However, Ajax request-header manipulation vulnerabilities arise when a script writes attacker-controllable data into the request header of an Ajax request that is issued using an XmlHttpRequest object. An attacker may be able to use this vulnerability to construct a URL that, if visited by another user, will set an arbitrary header in the subsequent Ajax request. This can then be used as a starting point to chain together other kinds of attack, thereby increasing the potential severity of this vulnerability.

## What is the impact of DOM-based Ajax request-header

## manipulation?

The potential impact of the vulnerability depends on the role of specific HTTP headers in the server-side processing of the Ajax request. If the header is used to control the behavior that results from the Ajax request, the attacker may be able to cause the user to perform unintended actions by manipulating the header. The impact also depends on what exactly the attacker is able to inject into the headers.

## Which sinks can lead to DOM-based Ajax request-header

## manipulation vulnerabilities?

The following are some of the main sinks can lead to DOM-based Ajax request-header vulnerabilities: XMLHttpRequest . setRequestHeader ( ) XMLHttpRequest . open ( ) XMLHttpRequest . send ( ) jQuery . global Eval ( )

## How to prevent DOM-based Ajax request-header manipulation

In addition to the general measures described on the DOM-based vulnerabilities page, you should avoid allowing data from any untrusted source dynamically set Ajax request headers.

---

Practise exploiting vulnerabilities on realistic targets.

Record your progression from Apprentice to Expert.

See where you rank in our Hall of Fame.

Enter your email

Already got an account? Login here
