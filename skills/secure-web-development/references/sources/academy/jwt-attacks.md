# JWT attacks

> Converted source material; attribution: PortSwigger Web Security Academy. Snapshot processed 2026-10-27.
> Source SHA-256: `7c9ae7a8aabc0ea762b8037aa00344472056ca6729ede84d8b92296fe37d0deb`.
> Conversion: Local English OCR, all 9 text segments. Source opinions and historical examples are preserved, not automatically endorsed.
> OCR literals, payloads, and complex table alignment are not character-verified throughout. Use the curated guidance and exact-text sources for implementation; do not execute OCR examples without checking the publisher.


JWT attacks

JWT attacks severity attacks. As JWTs are most commonly used in authentication, session management, and access control mechanisms, the potentially compromise the entire website and its users. Don't worry if you're not familiar with JWTs and how they work - we'll cover all of the relevant details as we go. We've also provide deliberately vulnerable labs so that you can practice exploiting these vulnerabilities safely against realistic targets.

eyJhbGciOiJIUz11Ni1s1nR5cC161kpXVCJ9.eyJ1c2Vy1joid211bmVy1iwiaXNBZG1pbi161nRydWUifQ.

vulnerable-website.com/admin "alg" : "typ". User o "user" "wiener" , true o o

If you're already familiar with the basic concepts behind JWT attacks and just want to practice exploiting them on some realist vulnerable targets, you can access all of the labs in this topic from the link below.

- View all JWT labs

Tip

## What are JWTs?

JSON web tokens (JWTs) are a standardized format for sending cryptographically signed JSON data between systems. They can any kind of data, but are most commonly used to send information ("claims") about users as part of authentication, session handli control mechanisms. Unlike with classic session tokens, all of the data that a server needs is stored client-side within the JWT itself. This mak highly distributed websites where users need to interact seamlessly with multiple back-end servers.

---

A JWT consists of 3 parts: a header, a payload, and a signature. These are each separated by a dot, as shown in the following ex eyJraWQiOi15MTM2ZGRiMy1jYjBhLTRhMTktYTA3ZS11YWRmNWEONGM4YjUiLCJhbGciOiJSUz11NiJ9 eyJpc3MiOiJw: Is ImV4 CCI 6YTYOODAzNzE2NCw ibmFtZS161 kNhcmxvcyBNb2 5 Cb31hZiwi c3ViIj oiY2FybC9z I iwi cm9sZSI 61mJ sb2d WlhaVJwiOiJjYXJsb3NAY2EybG9zLW1vbnRveWEubmV01iwiaWF01joxNTE2MjM5MD1yfO SYZBPIBg2CRiXAJ8vCEROLA P—Hw6GGI zf14JyngsZReIfqRvIAEi5L4HVOq7 9qGhQZvy9ZdxEJbwTxRs 6 Lb— fZTDpW61KYNdMyjw45 alSCZ1fypsMWz 2mTpQzi1010tps5Ei z7mM7M8gCwe AGpT53JxduQOaB5HkT5gVrv9cKu9Csl G5ehoxqm8DL5tFYaW31B50ELxiOKsuTKEbDOt5BCIOaCR2MBJWAbN- 61 f A The header and payload parts of a JWT are just base64urI-encoded JSON objects. The header contains metadata about the toke payload contains the actual "claims" about the user. For example, you can decode the payload from the token above to reveal the

"port swigger" , 1648037164, " name " : "Carlos Montoya" , " sub" : "car los" , " role": "blog author" , " email" • " carlos@carlos-montoya . net" , " i at" : 1516239022

In most cases, this data can be easily read or modified by anyone with access to the token. Therefore, the security of any JWT-be heavily reliant on the cryptographic signature.

JWT signature The server that issues the token typically generates the signature by hashing the header and payload. In some cases, they also e hash. Either way, this process involves a secret signing key. This mechanism provides a way for servers to verify that none of the has been tampered with since it was issued:

- As the signature is directly derived from the rest of the token, changing a single byte of the header or payload results in a mif

Without knowing the server's secret signing key, it shouldn't be possible to generate the correct signature for a given header

Tip

JWT vs JWS vs JWE The JWT specification is actually very limited. It only defines a format for representing information ("claims") as a JSON object tha between two parties. In practice, JWTs aren't really used as a standalone entity. The JWT spec is extended by both the JSON We and JSON Web Encryption (JWE) specifications, which define concrete ways of actually implementing JWTs.

JWT

Extends Extends

JWS JWE

---

very similar, except that the actual contents of the token are encrypted rather than just encoded.

Note For simplicity, throughout these materials, "JWT" refers primarily to JWS tokens, although some of the vulnerabilities describe to JWE tokens.

## What are JWT attacks?

JWT attacks involve a user sending modified JWTs to the server in order to achieve a malicious goal. Typically, this goal is to bypæ and access controls by impersonating another user who has already been authenticated.

## What is the impact of JWT attacks?

The impact of JWT attacks is usually severe. If an attacker is able to create their own valid tokens with arbitrary values, they may their own privileges or impersonate other users, taking full control of their accounts.

## How do vulnerabilities to JWT attacks arise?

JWT vulnerabilities typically arise due to flawed JWT handling within the application itself. The various specifications related to JIA flexible by design, allowing website developers to decide many implementation details for themselves. This can result in them acc vulnerabilities even when using battle-hardened libraries. These implementation flaws usually mean that the signature of the JWT is not verified properly. This enables an attacker to tampe passed to the application via the token's payload. Even if the signature is robustly verified, whether it can truly be trusted relies he secret key remaining a secret. If this key is leaked in some way, or can be guessed or brute-forced, an attacker can generate a ve arbitrary token, compromising the entire mechanism

## How to work with JWTs in Burp Suite

In case you haven't worked with JWTs in the past, we recommend familiarizing yourself with the relevant features of Burp Suite be labs in this topic.

Working with JWTs in Burp Suite

## Exploiting flawed JWT signature verification

By design, servers don't usually store any information about the JWTs that they issue. Instead, each token is an entirely self-conte several advantages, but also introduces a fundamental problem - the server doesn't actually know anything about the original con even what the original signature was. Therefore, if the server doesn't verify the signature properly, there's nothing to stop an attac arbitrary changes to the rest of the token. For example, consider a JWT containing the following claims:

" username " : ' car los " " isAdmxn" : false

If the server identifies the session based on this username , modifying its value might enable an attacker to impersonat Similarly, if the isAdmin value is used for access control, this could provide a simple vector for privilege escalation.

---

Accepting arbitrary signatures JWT libraries typically provide one method for verifying tokens and another that just decodes them. For example, the Node.js libre has verify ( ) and decode ( ) Occasionally, developers confuse these two methods and only pass incoming tokens to the decode ( ) method. This effectively application doesn't verify the signature at all.

JWT authentication bypass via unverified signature

Accepting tokens with no signature Among other things, the JWT header contains an alg parameter. This tells the server which algorithm was used to sign the toke which algorithm it needs to use when verifying the signature.

alg": HS256", "typ": "JWT

This is inherently flawed because the server has no option but to implicitly trust user-controllable input from the token which, at thi verified at all. In other words, an attacker can directly influence how the server checks whether the token is trustworthy. JWTs can be signed using a range of different algorithms, but can also be left unsigned. In this case, the alg parameter is set tc indicates a so-called "unsecured JWT". Due to the obvious dangers of this, servers usually reject tokens with no signature. Howe\ filtering relies on string parsing, you can sometimes bypass these filters using classic obfuscation techniques, such as mixed capi unexpected encodings.

Note Even if the token is unsigned, the payload part must still be terminated with a trailing dot.

AppRENTlCE JWT authentication bypass via flawed signature verification

## Brute-forcing secret keys

Some signing algorithms, such as HS256 (HMAC + SHA-256), use an arbitrary, standalone string as the secret key. Just like a pa that this secret can't be easily guessed or brute-forced by an attacker. Otherwise, they may be able to create JWTs with any head values they like, then use the key to re-sign the token with a valid signature. When implementing JWT applications, developers sometimes make mistakes like forgetting to change default or placeholder secr copy and paste code snippets they find online, then forget to change a hardcoded secret that's provided as an example. In this ca for an attacker to brute-force a server's secret using a wordlist of well-known secrets.

Brute-forcing secret keys using hashcat We recommend using hashcat to brute-force secret keys. You can but it also comes pre-installed and Linux.

Note

---

run hashcat.

You just need a valid, signed JWT from the target server and a wordlist of well-known secrets. You can then run the following com JWT and wordlist as arguments: hashcat —a O —m 16500

Hashcat signs the header and payload from the JWT using each secret in the wordlist, then compares the resulting signature with from the server. If any of the signatures match, hashcat outputs the identified secret in the following format, along with various 0th

```text
<jwt>:<identified secret >
```

Note If you run the command more than once, you need to include the --show flag to output the results.

As hashcat runs locally on your machine and doesn't rely on sending requests to the server, this process is extremely quick, even wordlist. Once you have identified the secret key, you can use it to generate a valid signature for any JWT header and payload that you lil« to re-sign a modified JWT in Burp Suite, see Editing JWTs.

JWT authentication bypass via weak signing key

If the server uses an extremely weak secret, it may even be possible to brute-force this character-by-character rather than using

## JWT header parameter injections

According to the JWS specification, only the alg header parameter is mandatory. In practice, however, JWT headers (also knovx often contain several other parameters. The following ones are of particular interest to attackers.

jwk (JSON Web Key) - Provides an embedded JSON object representing the key. j ku (JSON Web Key Set URL) - Provides a URL from which servers can fetch a set of keys containing the correct key. kid (Key ID) - Provides an ID that servers can use to identify the correct key in cases where there are multiple keys to choc on the format of the key, this may have a matching kid parameter.

As you can see, these user-controllable parameters each tell the recipient server which key to use when verifying the signature. Ir learn how to exploit these to inject modified JWTs signed using your own arbitrary key rather than the server's secret.

Injecting self-signed JWTs via the jwk parameter The JSON Web Signature (JWS) specification describes an optional jwk header parameter, which servers can use to embed within the token itself in JWK format.

JWK A JWK (JSON Web Key) is a standardized format for representing keys as a JSON object.

You can see an example of this in the following JWT header:

---

kid": "ed2Nf8sb sD6ngO—scs5390g fFD8sfxG" , "typ": "JWT alg". "RS256", "iwk" : : "RSA",

" kid": , "yyIwpYmffgXBxhAUJzHHocCuJoIwDqqI 7 5ZWuCQ cb33K2vh9m"

Public and private keys In case you're not familiar with the terms "public key" and "private key", we've covered this as part of our materials on algorithl attacks. For more information, see Symmetric vs asymmetric algorithms.

Ideally, servers should only use a limited whitelist of public keys to verify JWT signatures. However, misconfigured servers someti that's embedded in the jwk parameter. You can exploit this behavior by signing a modified JWT using your own RSA private key, then embedding the matching public ke: Although you can manually add or modify the jwk parameter in Burp, the JWT Editor extension provides a useful feature to help vulnerability: 1 With the extension loaded, in Burp's main tab bar, go to the JWT Editor Keys tab. 2. Generate a new RSA key. 3. Send a request containing a JWT to Burp Repeater. 4. In the message editor, switch to the extension-generated JSON Web Token tab and modify the token's payload however yot 5. Click Attack, then select Embedded JWK. When prompted, select your newly generated RSA key. 6. Send the request to test how the server responds You can also perform this attack manually by adding the j wk header yourself. However, you may also need to update the JWT's parameter to match the kid of the embedded key. The extension's built-in attack takes care of this step for you.

JWT authentication bypass via jwk header injection

Injecting self-signed JWTs via the jku parameter Instead of embedding public keys directly using the j wk header parameter, some servers let you use the j ku (JWK Set URL) reference a JWK Set containing the key. When verifying the signature, the server fetches the relevant key from this URL.

JWK Set A JWK Set is a JSON object containing an array of JWKs representing different keys. You can see an example of this below.

---

" keys" :

kty": "RSA " AQAB " , "kid" • "75dOef47 af89 47a9 9061 7c02a610d5ab" 'o-yy1wpYmffgXBxhAUJzHHocCuJ01wDqq175ZWuCQ cb33K2vh9mk6GPM9gNN4Y qTVX67WhsN3,

: "RSA" , " AQAB " , " kid" : 't d8 clAd5abA" , "fc3f yy1wpYmffgXBxhAUJzHq17 9gNNQ cb33HocCuJ01wDqmk6GPM4Y qTVX67WhsN3JvaFYw

JWK Sets like this are sometimes exposed publicly via a standard endpoint, such as / . well-known/ j wks . j son More secure websites will only fetch keys from trusted domains, but you can sometimes take advantage of URL parsing discrepar kind of filtering. We covered some examples of these in our topic on SSRF.

JWT authentication bypass via jku header injection

Injecting self-signed JWTs via the kid parameter Servers may use several cryptographic keys for signing different kinds of data, not just JWTs. For this reason, the header of a JW kid (Key 'D) parameter, which helps the server identify which key to use when verifying the signature. Verification keys are often stored as a JWK Set. In this case, the server may simply look for the JWK with the same kid as the t JWS specification doesn't define a concrete structure for this ID - it's just an arbitrary string of the developer's choosing. For exam the kid parameter to point to a particular entry in a database, or even the name of a file. If this parameter is also vulnerable to directory traversal, an attacker could potentially force the server to use an arbitrary file from verification key.

/ • • / path/ to/ file", " kid": " .. "typ": "JWT "HS256", " asGsADas3 4 21 -dfh 9DGN-AFDFDbas fd8—anfj kvc "

This is especially dangerous if the server also supports JWTs signed using a symmetric algorithm. In this case, an attacker could kid parameter to a predictable, static file, then sign the JWT using a secret that matches the contents of this file. You could theoretically do this with any file, but one of the simplest methods is to use /dev/null , which is present on most Lint an empty file, reading it returns an empty string. Therefore, signing the token with a empty string will result in a valid signature.

JWT authentication bypass via kid header path traversal

If the server stores its verification keys in a database, the kid header parameter is also a potential vector for SQL injection attac

Other interesting JWT header parameters The following header parameters may also be interesting for attackers:

---

the underlying parsing library may support it anyway. If you have found a way to bypass signature verification, you can try inj header to change the content type to text/xml or application/ x java serialized object , which can potentiall for XXE and deserialization attacks.

- x5c (X.509 Certificate Chain) - Sometimes used to pass the X.509 public key certificate or certificate chain of the key used

JWT. This header parameter can be used to inject self-signed certificates, similar to the jwk header injection attacks discus the complexity of the X.509 format and its extensions, parsing these certificates can also introduce vulnerabilities. Details of beyond the scope of these materials, but for more details, check out CVE-2017-2800 and CVE-2018-2633.

## JWT algorithm confusion

Even if a server uses robust secrets that you are unable to brute-force, you may still be able to forge valid JWTs by signing the algorithm that the developers haven't anticipated. This is known as an algorithm confusion attack.

JWT algorithm confusion attacks

## How to prevent JWT attacks

You can protect your own websites against many of the attacks we've covered by taking the following high-level measures:

Use an up-to-date library for handling JWTs and make sure your developers fully understand how it works, along with any se Modern libraries make it more difficult for you to inadvertently implement them insecurely, but this isn't foolproof due to the in the related specifications.

- Make sure that you perform robust signature verification on any JWTs that you receive, and account for edge-cases such as

unexpected algorithms.

- Enforce a strict whitelist of permitted hosts for the j ku header.

- Make sure that you're not vulnerable to path traversal or SQL injection via the kid header parameter.

Additional best practice for JWT handling Although not strictly necessary to avoid introducing vulnerabilities, we recommend adhering to the following best practice when us applications:

- Always set an expiration date for any tokens that you issue.

- Avoid sending tokens in URL parameters where possible.

- Include the aud (audience) claim (or similar) to specify the intended recipient of the token. This prevents it from being used

websites. Enable the issuing server to revoke tokens (on logout, for example).

---

Practise exploiting vulnerabilities on realistic targets.

Record your progression from Apprentice to Expert.

See where you rank in our Hall of Fame.

Enter your email

Already got an account? Login here
