# What is path traversal, and how to prevent it_

> Converted source material; attribution: PortSwigger Web Security Academy. Snapshot processed 2026-10-27.
> Source SHA-256: `a55c9b11cb22543264e677f2461abfd33a1158b0704a1c343b0b533e95fd7c08`.
> Conversion: Local English OCR, all 5 text segments. Source opinions and historical examples are preserved, not automatically endorsed.
> OCR literals, payloads, and complex table alignment are not character-verified throughout. Use the curated guidance and exact-text sources for implementation; do not execute OCR examples without checking the publisher.


Path traversal

Path traversal

## What path traversal is.

## How to carry out path traversal attacks and circumvent common obstacles.

## • How to prevent path traversal vulnerabilities.

## Iloadlmage?filename=gift.png

root : x : O : O : root: / root : /bin/bash daemon : x: 1 : 1 : daemon : /usr/sbin : /bin/sh bin : x: 2 : 2 : bin : /bin : /bin/ sh SYS : X: 3 : 3: SYS : /deV: / bin/sh sync : x 4 : 65534 : sync : / bin : /bin/ sync games : x : 5 : 60 : games: /usr/games : / Ein/ sh man : x : 6 : 12 : man : /var/cache/man : /bin/sh Ip:x: 7 : 7 : Ip: / vat/ spool/ Ipd: /bin/ sh mail : x: g : 8 :mail : /vaz/mail : /bin/sh news x : 9 : 9 : news : / vat/ : / bin/sh uucp : x : IO : IO : : / Var/ : /bin/ sh proxy : x : 13 : 13 : proxy : / bin : /bin/ sh www-data : x: 33 : 33 : www-data : Ivar /www : /bin/sh ckup : x : 34 : 34 :backup : /var/backups : /bin/ sh

## If you're familiar with the basic concepts behind path traversal and want to practice exploiting them on some realistic,

## deliberately vulnerable targets, you can access labs in this topic from the link below.

## View all path traversal labs

## What is path traversal?

## Path traversal is also known as directory traversal. These vulnerabilities enable an attacker to read arbitrary files on the

## server that is running an application. This might include:

## • Application code and data.

## • Credentials for back-end systems.

## • Sensitive operating system files.

---

or behavior, and ultimately take full control of the server. What is directory traversal? (file path traversal) - Web Security Academy

---

## Common obstacles to exploiting path traversal vulnerabilities

Many applications that place user input into file paths implement defenses against path traversal attacks. These can often be bypassed. If an application strips or blocks directory traversal sequences from the user-supplied filename, it might be possible to bypass the defense using a variety of techniques. You might be able to use an absolute path from the filesystem root, such as filename—/etc/passwd , to directly reference a file without using any traversal sequences.

File path traversal, traversal sequences blocked with absolute path bypass

You might be able to use nested traversal sequences, such as .... \ / . These revert to simple traversal .... // or sequences when the inner sequence is stripped.

File path traversal, traversal sequences stripped non-recursively

In some contexts, such as in a URL path or the filename parameter of a multipart/ form data request, web servers may strip any directory traversal sequences before passing your input to the application. You can sometimes bypass this kind of sanitization by URL encoding, or even double URL encoding, the .. / characters. This results in and respectively. Various non-standard encodings, such as . or . , may also work. For Burp Suite Professional users, Burp Intruder provides the predefined payload list Fuzzing - path traversal. This contains some encoded path traversal sequences that you can try.

File path traversal, traversal sequences stripped with superfluous URL-decode

An application may require the user-supplied filename to start with the expected base folder, such as / var/www/images . In this case, it might be possible to include the required base folder followed by suitable traversal sequences. For example: filename—/var/www/images/ . . / . . / .. / etc/ passwd

File path traversal, validation of start of path

An application may require the user-supplied filename to end with an expected file extension, such as .png . In this case, it might be possible to use a null byte to effectively terminate the file path before the required extension. For example: / / / etc/ passwd%OO .png filename=. .

File path traversal, validation of file extension with null byte bypass

## How to prevent a path traversal attack

The most effective way to prevent path traversal vulnerabilities is to avoid passing user-supplied input to filesystem APIs altogether. Many application functions that do this can be rewritten to deliver the same behavior in a safer way.

---

attacks:

- Validate the user input before processing it. Ideally, compare the user input with a whitelist of permitted values. If that

isn't possible, verify that the input contains only permitted content, such as alphanumeric characters only.

- After validating the supplied input, append the input to the base directory and use a platform filesystem API to

canonicalize the path. Verify that the canonicalized path starts with the expected base directory. Below is an example of some simple Java code to validate the canonical path of a file based on user input: new File (BASE DIRECTORY, userlnput) ; if (file . getCanonicaIPath ( ) . startsWith (BASE DIRECTORY) ) // process file

- Find directory traversal vulnerabilities using Burp Suite's web vulnerability scanner

---
