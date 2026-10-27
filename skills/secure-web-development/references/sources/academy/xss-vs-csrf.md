# XSS vs CSRF

> Converted source material; attribution: PortSwigger Web Security Academy. Snapshot processed 2026-10-27.
> Source SHA-256: `505d253428899a909f02871193757be3cd392cf5aeef9248105989bf9785e447`.
> Conversion: Local English OCR, all 2 text segments. Source opinions and historical examples are preserved, not automatically endorsed.
> OCR literals, payloads, and complex table alignment are not character-verified throughout. Use the curated guidance and exact-text sources for implementation; do not execute OCR examples without checking the publisher.


XSS vs CSRF

Cross-site request forgery (CSRF)

## What is the difference between XSS and CSRF?

Cross-site scripting (or XSS) allows an attacker to execute arbitrary JavaScript within the browser of a victim user. Cross-site request forgery (or CSRF) allows an attacker to induce a victim user to perform actions that they do not intend to. The consequences of XSS vulnerabilities are generally more serious than for CSRF vulnerabilities:

- CSRF often only applies to a subset of actions that a user is able to perform. Many applications implement CSRF defenses i

left exposed. Conversely, a successful XSS exploit can normally induce a user to perform any action that the user is able to the vulnerability arises.

- CSRF can be described as a "one-way" vulnerability, in that while an attacker can induce the victim to issue an HTTP reques

request. Conversely, XSS is "two-way", in that the attacker's injected script can issue arbitrary requests, read the responses, attacker's choosing.

## Can CSRF tokens prevent XSS attacks?

Some XSS attacks can indeed be prevented through effective use of CSRF tokens. Consider a simple reflected XSS vulnerability https : / / insecure—website . com/status ?message=&lt;script&gt;/*+Bad+stuff+here. . . + * / &lt; / script &gt; Now, suppose that the vulnerable function includes a CSRF token: https : // insecure—website . com/status ?csrf Assuming that the server properly validates the CSRF token, and rejects requests without a valid token, then the token does prev here is in the name: "cross-site scripting", at least in its reflected form, involves a cross-site request. By preventing an attacker fro prevents trivial exploitation of the XSS vulnerability. Some important caveats arise here:

- If a reflected XSS vulnerability exists anywhere else on the site within a function that is not protected by a CSRF token, then

- If an exploitable XSS vulnerability exists anywhere on a site, then the vulnerability can be leveraged to make a victim user pe

themselves protected by CSRF tokens. In this situation, the attacker's script can request the relevant page to obtain a valid C the protected action.

- CSRF tokens do not protect against stored XSS vulnerabilities. If a page that is protected by a CSRF token is also the output

XSS vulnerability can be exploited in the usual way, and the XSS payload will execute when a user visits the page.

---

Practise exploiting vulnerabilities on realistic targets.

Record your progression from Apprentice to Expert.

See where you rank in our Hall of Fame.

Enter your email

Already got an account? Login here
