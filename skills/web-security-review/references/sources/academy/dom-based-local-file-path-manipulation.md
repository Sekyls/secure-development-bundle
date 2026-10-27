# DOM-based local file-path manipulation

> Converted source material; attribution: PortSwigger Web Security Academy. Snapshot processed 2026-10-27.
> Source SHA-256: `6edee4822f74c4bfb27bbc342f734b10c357177bec647dbe8cf0c7b0e5362d4c`.
> Conversion: Local English OCR, all 2 text segments. Source opinions and historical examples are preserved, not automatically endorsed.
> OCR literals, payloads, and complex table alignment are not character-verified throughout. Use the curated guidance and exact-text sources for implementation; do not execute OCR examples without checking the publisher.


> Local file-path manipulation

DOM-based vulnerabilities highlight some of the sinks that can lead to this kind of vulnerability, and suggest ways that you can reduce your exposure.

## What is DOM-based local file-path manipulation?

Local file-path manipulation vulnerabilities arise when a script passes attacker-controllable data to a file-handling API as the filename parameter. An attacker may be able to use this vulnerability to construct a URL that, if visited by another user, will cause the user's browser to open an arbitrary local file.

## What is the impact of DOM-based local file-path manipulation?

The potential impact of this vulnerability depends on how the website uses the opened file: If the website reads data from the file, the attacker may be able to retrieve this data. If the website writes specific data to a sensitive file, the attacker may also be able write their own data to the file, which could be the configuration file of the operating system, for example. In both of these cases, the actual exploitability of the potential vulnerability may depend on other suitable functionality being present on the website.

## Which sinks can lead to DOM-based local file-path manipulation

## vulnerabilities?

The following are some of the main sinks can lead to DOM-based local file path manipulation vulnerabilities: readAsArrayBuffer ( ) FileReader . readAsBinaryString ( ) FileReader . readAsDataURL ( ) readAsText ( ) F i I eReader . readAsEiIe ( ) FileReader . root. getFiIe ( )

## How to prevent DOM-based local file-path manipulation

## vulnerabilities

In addition to the general measures described on the DOM-based vulnerabilities page, you should avoid allowing data from any untrusted source to dynamically pass a filename to a file-handling API.

---

Practise exploiting vulnerabilities on realistic targets.

Record your progression from Apprentice to Expert.

See where you rank in our Hall of Fame.

Enter your email

Already got an account? Login here
