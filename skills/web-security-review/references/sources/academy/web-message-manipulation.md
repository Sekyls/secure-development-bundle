# Web message manipulation

> Converted source material; attribution: PortSwigger Web Security Academy. Snapshot processed 2026-10-27.
> Source SHA-256: `b2ba23f6b8e84b494d50b383d5ef746104b712c2d3510a89e60d53448debbc27`.
> Conversion: Local English OCR, all 2 text segments. Source opinions and historical examples are preserved, not automatically endorsed.
> OCR literals, payloads, and complex table alignment are not character-verified throughout. Use the curated guidance and exact-text sources for implementation; do not execute OCR examples without checking the publisher.


Web message manipulation

DOM-based vulnerabilities to them.

## What is DOM-based web message manipulation?

Web message vulnerabilities arise when a script sends attacker-controllable data as a web message to another document within the browser. An attacker may be able to use the web message data as a source by constructing a web page that, if visited by a user, will cause the user's browser to send a web message containing data that is under the attacker's control. For more information about the using web messages as a source, please refer to the Controlling the web-message source page.

## Which sinks can lead to DOM-based web-message manipulation

## vulnerabilities?

The postMessage ( ) method for sending web messages can lead to vulnerabilities if the event listener for receiving messages handles the incoming data in an unsafe way.

## How to prevent DOM-based web message manipulation

In addition to the general measures described on the DOM-based vulnerabilities page, you should avoid sending web messages that contain data that originated from any untrusted source. When sending cross-origin messages, you should always explicitly specify the target window. Most importantly, make sure to implement robust measures to verify the origin of any incoming messages.

---
