# What is stored XSS (cross-site scriptin...rial & Examples

> Converted source material; attribution: PortSwigger Web Security Academy. Snapshot processed 2026-10-27.
> Source SHA-256: `07a8180996e7061c5e45577b152fdd64ea619c3083a71169d5e6b97011f35c9c`.
> Conversion: Local English OCR, all 3 text segments. Source opinions and historical examples are preserved, not automatically endorsed.
> OCR literals, payloads, and complex table alignment are not character-verified throughout. Use the curated guidance and exact-text sources for implementation; do not execute OCR examples without checking the publisher.


Stored

Cross-site scripting (XSS) vulnerabilities.

## What is stored cross-site scripting?

Stored cross-site scripting (also known as second-order or persistent XSS) arises when an application receives data from an untri source and includes that data within its later HTTP responses in an unsafe way. Suppose a website allows users to submit comments on blog posts, which are displayed to other users. Users submit comments I HTTP request like the following:

```text
POST /post/comment HTTP/ 1.1
```

```text
Host: vulnerable—website . corn
```

Content Length: 100

pos t ld—3 &commen t—Thi s +pos t +wa s -I-extreme Iy+he 1 pfui . & name—Car 1 os +Mon toya &ernai I—car los % 4 0 normal —u

After this comment has been submitted, any user who visits the blog post will receive the following within the application's resporu post was extremely helpful .

Assuming the application doesn't perform any other processing of the data, an attacker can submit a malicious comment like this: Bad stuff here... * / &lt; / script &gt;

Within the attacker's request, this comment would be URL-encoded as:

Any user who visits the blog post will now receive the following within the application's response: Bad stuff here. . .

The script supplied by the attacker will then execute in the victim user's browser, in the context of their session with the applicatior

Stored XSS into HTML context with nothing encoded

## Impact of stored XSS attacks

If an attacker can control a script that is executed in the victim's browser, then they can typically fully compromise that user. The a carry out any of the actions that are applicable to the impact of reflected XSS vulnerabilities. In terms of exploitability, the key difference between reflected and stored XSS is that a stored XSS vulnerability enables contained within the application itself. The attacker does not need to find an external way of inducing other users to mak

---

The self-contained nature of stored cross-site scripting exploits is particularly relevant in situations where an XSS vulnerability onl users who are currently logged in to the application. If the XSS is reflected, then the attack must be fortuitously timed: a user who to make the attacker's request at a time when they are not logged in will not be compromised. In contrast, if the XSS is stored, the is guaranteed to be logged in at the time they encounter the exploit.

Exploiting cross-site scripting vulnerabilities

## Stored XSS in different contexts

There are many different varieties of stored cross-site scripting. The location of the stored data within the application's response d what type of payload is required to exploit it and might also affect the impact of the vulnerability. In addition, if the application performs any validation or other processing on the data before it is stored, or at the point when the st is incorporated into responses, this will generally affect what kind of XSS payload is needed.

- Cross-site scripting contexts

## How to find and test for stored XSS vulnerabilities

Many stored XSS vulnerabilities can be found using Burp Suite's web vulnerability scanner. Testing for stored XSS vulnerabilities manually can be challenging. You need to test all relevant "entry points" via which attacker-c data can enter the application's processing, and all "exit points" at which that data might appear in the application's responses. Entry points into the application's processing include:

- Parameters or other data within the URL query string and message body.

- The URL file path.

- HTTP request headers that might not be exploitable in relation to reflected XSS.

- Any out-of-band routes via which an attacker can deliver data into the application. The routes that exist depend entirely on th

functionality implemented by the application: a webmail application will process data received in emails; an application disple Twitter feed might process data contained in third-party tweets; and a news aggregator will include data originating on other

The exit points for stored XSS attacks are all possible HTTP responses that are returned to any kind of application user in any sitt The first step in testing for stored XSS vulnerabilities is to locate the links between entry and exit points, whereby data submitted t point is emitted from an exit point. The reasons why this can be challenging are that:

- Data submitted to any entry point could in principle be emitted from any exit point. For example, user-supplied display names

appear within an obscure audit log that is only visible to some application users.

- Data that is currently stored by the application is often vulnerable to being overwritten due to other actions performed within t

application. For example, a search function might display a list of recent searches, which are quickly replaced as users perfo searches. To comprehensively identify links between entry and exit points would involve testing each permutation separately, submitting a value into the entry point, navigating directly to the exit point, and determining whether the value appears there. However, this apg not practical in an application with more than a few pages. Instead, a more realistic approach is to work systematically through the data entry points, submitting a specific value intoeachon monitoring the application's responses to detect cases where the submitted value appears. Particular attention can be p id to rele

---

the data is indeed being stored across different requests, as opposed to being simply reflected in the immediate response. When you have identified links between entry and exit points in the application's processing, each link needs to be specifically tes detect if a stored XSS vulnerability is present. This involves determining the context within the response where the stored data ap testing suitable candidate XSS payloads that are applicable to that context. At this point, the testing methodology is broadly the sz finding reflected XSS vulnerabilities.
