# Email address verification codes

> Converted source material; attribution: pilcrow, Pilcrow's auth book. Snapshot processed 2026-10-27.
> Source SHA-256: `6cdc81403ad37789c5169073373365352daef79f6f203331038fae82f86a7db2`.
> Conversion: HTML converted to Markdown; text and code preserved. Source opinions and historical examples are preserved, not automatically endorsed.


Email verification codes are one-time codes sent to an email address. By asking the user to enter the code, you can confirm ownership of the address. This is my preferred option compared to using single-use verification links.

The verification code should be tied to a single session and a single email address. Binding the code to a session links the verification attempt to the device that initiated it. Making each email verification code unique per attempt ensures that a user cannot send a code to an email address they control and then use it to verify a different email address.

Our main goal with email address verification is to discourage the use of email addresses that the user doesn't own. We don't need to make it impossible, just hard enough. With that in mind, I recommend using an 8-digit numeric code with a rate limit of 1 attempt per minute per email address. The code should remain valid for up to an hour, but preferably shorter. Using the email address as the rate-limit key ensures that a malicious actor can't bypass the limit by starting multiple attempts or creating multiple accounts. As an example, you could use a token bucket with a maximum capacity of 5 tokens and a refill rate of 1 token per minute for the rate limit. The code doesn't need to be hashed before storage. You also do not need to keep track of unsuccessful attempts. At this rate limit and expiration, doing so doesn't significantly change the odds of successfully brute-forcing the code.

However, if there are legitimate incentives to verify a particular address, you should use a longer code and hash it using a password hashing algorithm such as Argon2. Also see the Email code authentication page.

To generate a random 8-digit numeric code, generate 4 random bytes, discard 5 bits, and interpret the remaining 27 bits as an integer. If the resulting value is 100,000,000 or greater, repeat the process. Otherwise, left-pad the number with zeroes to produce an 8-digit string. This approach avoids introducing bias, ensuring that all possible codes appear with equal probability. You could also use the modulo operator, although doing so introduces a small amount of statistical bias, but not enough to meaningfully degrade security.
