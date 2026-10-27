# Passwords, passkeys, and cryptographic boundaries

## Password policy

Prefer the existing provider's supported password policy and migration mechanisms. Do not copy the supplied book's 8–10 character and ASCII-only preferences into new policy. NIST SP 800-63B-4 specifies 15 characters for single-factor passwords and permits 8 when used with MFA; it supports a maximum of at least 64 and broader character input. Applicability depends on the required assurance context.

Avoid silent trimming, truncation, or case changes. Use compromised-password screening and suitable abuse controls rather than arbitrary composition rules. Check the exact current standard and provider constraints before changing validation; preserve existing users' ability to authenticate during a policy migration.

For applications that manage password verifiers, use a maintained password-hashing implementation and verify its actual parameter units, input limits, salt handling, and deployment resource cost. Benchmark before choosing cost settings; migrate with the library's supported rehash process. Do not store plaintext, reversible equivalents, or a fast general-purpose digest as the password verifier. Keep peppers separate from the password database if the chosen design uses one, and define rotation/recovery behavior.

## Passkeys and WebAuthn

Use a maintained WebAuthn verifier matched to the application and deployed browser/runtime versions. Generate unpredictable server challenges, bind them to the operation and account context, expire them, and consume them once. Verify the expected origin, RP ID binding, challenge, credential identity, signature, and the required user-presence/user-verification policy.

Registration requires authorized account binding and careful duplicate-credential handling. Authentication must look up the actual credential owner, not trust a client-provided user ID. Factor enrollment, deletion, fallback, and recovery must not bypass the intended assurance. Counter behavior varies across authenticator types, including synced credentials; apply library and provider guidance rather than requiring a simplistic universally increasing counter.

Test wrong origin/RP, challenge replay or mismatch, incorrect account binding, missing required verification, revoked credentials, and recovery fallback. Do not implement signature primitives or WebAuthn decoding from an abbreviated tutorial.

Read: [password storage](sources/owasp/Password_Storage_Cheat_Sheet.md), [passkey controls](sources/owasp/Passkey_Security_Cheat_Sheet.md), [book password discussion](sources/auth/passwords.md), [passkey authentication](sources/auth/passkey-authentication.md), and [current checks](source-policy.md#current-primary-source-checks).
