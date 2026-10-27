# Password reset poisoning

> Converted source material; attribution: PortSwigger Web Security Academy. Snapshot processed 2026-10-27.
> Source SHA-256: `b5dffa519949b2efd23cc9f3ee702c553a99dff707eec713620f8a28f8f88f56`.
> Conversion: Local English OCR, all 3 text segments. Source opinions and historical examples are preserved, not automatically endorsed.
> OCR literals, payloads, and complex table alignment are not character-verified throughout. Use the curated guidance and exact-text sources for implementation; do not execute OCR examples without checking the publisher.


Password reset poisoning

HTTP Host header attacks password reset link pointing to a domain under their control. This behavior can be leveraged to steal the secret tokens required to reset arbitrary users' passwords and, ultimately, compromise their accounts.

Forgot your password? /password/reset HTTP 1.1 Enter your email address to reset POST your password:

```text
Host:
```

evil - user . net carlos@normal-user.com RESET YOUR PASSWORD VULNERABLE WEBSITE

Hello Carlos, to reset your password, click the following link: https : / / evil -user . net/ token— -aObaOd1Cb3b6 3d13822572fcff1a24 pa g sword/ reget ? token= Oba0d1cb3b63d13822572f 1895d893f659164d4c c550b421ebdd48a8 cEf1a241895d893E659164 d4CC550b421ebdd48aa

Research This technique was first documented in 2013 by our Director of Research, James Kettle. Check out our Research page for full write-ups and video presentations of more innovative techniques discovered by James and the rest of the team.

## How does a password reset work?

Virtually all websites that require a login also implement functionality that allows users to reset their password if they forget it. There are several ways of doing this, with varying degrees of security and practicality. One of the most common approaches goes something like this: 1. The user enters their username or email address and submits a password reset request. 2. The website checks that this user exists and then generates a temporary, unique, high-entropy token, which it associates with the user's account on the back-end. 3. The website sends an email to the user that contains a link for resetting their password. The user's unique reset token is included as a query parameter in the corresponding URL: https : / / normal -website . com/reset?token=0a1b2c3d4e5f6g7h8i9j

---

account is being reset. If everything is as expected, the user is given the option to enter a new password. Finally, the token is destroyed

This process is simple enough and relatively secure in comparison to some other approaches. However, its security relies on the principle that only the intended user has access to their email inbox and, therefore, to their unique token. Password reset poisoning is a method of stealing this token in order to change another user's password.

## How to construct a password reset poisoning attack

If the URL that is sent to the user is dynamically generated based on controllable input, such as the Host header, it may be possible to construct a password reset poisoning attack as follows:

1. The attacker obtains the victim's email address or username, as required, and submits a password reset request on their behalf. When submitting the form, they intercept the resulting HTTP request and modify the Host header so that it points to a domain that they control. For this example, we'll use evil-user . net 2. The victim receives a genuine password reset email directly from the website. This seems to contain an ordinary link to reset their password and, crucially, contains a valid password reset token that is associated with their account. However, the domain name in the URL points to the attacker's server: https : / / evil—user . net/ reset? token=0a1b2c3d4e5f6g7h8i9j 3. If the victim clicks this link (or it is fetched in some other way, for example, by an antivirus scanner) the password reset token will be delivered to the attacker's server. 4. The attacker can now visit the real URL for the vulnerable website and supply the victim's stolen token via the corresponding parameter. They will then be able to reset the user's password to whatever they like and subsequently log in to their account.

In a real attack, the attacker may seek to increase the probability of the victim clicking the link by first warming them up with a fake breach notification, for example. Even if you can't control the password reset link, you can sometimes use the Host header to inject HTML into sensitive emails. Note that email clients typically don't execute JavaScript, but other HTML injection techniques like dangling markup attacks may still apply.

Basic password reset poisoning

Password reset poisoning via middleware

Password reset poisoning via dangling markup

---

Practise exploiting vulnerabilities on realistic targets.

Record your progression from Apprentice to Expert.

See where you rank in our Hall of Fame.

Enter your email

Already got an account? Login here
