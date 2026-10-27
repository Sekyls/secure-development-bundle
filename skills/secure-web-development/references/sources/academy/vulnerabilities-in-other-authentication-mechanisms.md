# Vulnerabilities in other authentication mechanisms

> Converted source material; attribution: PortSwigger Web Security Academy. Snapshot processed 2026-10-27.
> Source SHA-256: `c7e34c35a5d9efd5416140968153d52b314b2eade0ad1ab82f994613f62bfd47`.
> Conversion: Local English OCR, all 3 text segments. Source opinions and historical examples are preserved, not automatically endorsed.
> OCR literals, payloads, and complex table alignment are not character-verified throughout. Use the curated guidance and exact-text sources for implementation; do not execute OCR examples without checking the publisher.


Other mechanisms

Authentication vulnerabilities typically change their password or reset their password when they forget it. These mechanisms can also introduce vulnerabilities attacker. Websites usually take care to avoid well-known vulnerabilities in their login pages. But it is easy to overlook the fact that you need that related functionality is equally as robust. This is especially important in cases where an attacker is able to create their own ac easy access to study these additional pages.

## Keeping users logged in

A common feature is the option to stay logged in even after closing a browser session. This is usually a simple checkbox labeled or "Keep me logged in" This functionality is often implemented by generating a "remember me" token of some kind, which is then stored in a persistent cc effectively allows you to bypass the entire login process, it is best practice for this cookie to be impractical to guess. However, son cookie based on a predictable concatenation of static values, such as the username and a timestamp. Some even use the passw approach is particularly dangerous if an attacker is able to create their own account because they can study their own cookie and generated. Once they work out the formula, they can try to brute-force other users' cookies to gain access to their accounts. Some websites assume that if the cookie is encrypted in some way it will not be guessable even if it does use static values. While correctly, naively "encrypting" the cookie using a simple two-way encoding like Base64 offers no protection whatsoever. Even usir way hash function is not completely bulletproof. If the attacker is able to easily identify the hashing algorithm, and no salt is used, the cookie by simply hashing their wordlists. This method can be used to bypass login attempt limits if a similar limit isn't applied tl

Brute-forcing a stay-logged-in cookie

Even if the attacker is not able to create their own account, they may still be able to exploit this vulnerability. Using the usual techr attacker could steal another user's "remember me" cookie and deduce how the cookie is constructed from that. If the website was framework, the key details of the cookie construction may even be publicly documented. In some rare cases, it may be possible to obtain a user's actual password in cleartext from a cookie, even if it is hashed. Hashed password lists are available online, so if the user's password appears in one of these lists, decrypting the hash can occasionally b hash into a search engine. This demonstrates the importance of salt in effective encryption.

Offline password cracking

## Resetting user passwords

---

impossible in this scenario, websites have to rely on alternative methods to make sure that the real user is resetting their own pas password reset functionality is inherently dangerous and needs to be implemented securely. There are a few different ways that this feature is commonly implemented, with varying degrees of vulnerability.

Sending passwords by email It should go without saying that sending users their current password should never be possible if a website handles passwords se some websites generate a new password and send this to the user via email. Generally speaking, sending persistent passwords over insecure channels is to be avoided. In this case, the security relies on eith expiring after a very short period, or the user changing their password again immediately. Otherwise, this approach is highly suscc attacks. Email is also generally not considered secure given that inboxes are both persistent and not really designed for secure storage of users also automatically sync their inbox between multiple devices across insecure channels.

Resetting passwords using a URL A more robust method of resetting passwords is to send a unique URL to users that takes them to a password reset page. Less method use a URL with an easily guessable parameter to identify which account is being reset, for example: http: user

In this example, an attacker could change the user parameter to refer to any username they have identified. They would then b they can potentially set a new password for this arbitrary user. A better implementation of this process is to generate a high-entropy, hard-to-guess token and create the reset URL based on tha URL should provide no hints about which user's password is being reset. http : / /vulnerable—webs ite . cb3b 63dI 3822 572 fcffIa2418 95d8 93 f 659

When the user visits this URL, the system should check whether this token exists on the back-end and, if so, which user's passwc token should expire after a short period of time and be destroyed immediately after the password has been reset. However, some websites fail to also validate the token again when the reset form is submitted. In this case, an attacker could sim own account, delete the token, and leverage this page to reset an arbitrary user's password.

Password reset broken logic

If the URL in the reset email is generated dynamically, this may also be vulnerable to password reset poisoning. In this case, an a another user's token and use it change their password.

Password reset poisoning via middleware

Password reset poisoning

## Changing user passwords

Typically, changing your password involves entering your current password and then the new password twice. These pages funde process for checking that usernames and current passwords match as a normal login page does. Therefore, these pages can be techniques.

---

the username is provided in a hidden field, an attacker might be able to edit this value in the request to target arbitrary users. This enumerate usernames and brute-force passwords.

Password brute-force via password change
