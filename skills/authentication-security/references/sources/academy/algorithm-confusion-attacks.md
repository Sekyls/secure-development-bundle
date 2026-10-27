# Algorithm confusion attacks

> Converted source material; attribution: PortSwigger Web Security Academy. Snapshot processed 2026-10-27.
> Source SHA-256: `65d64a81cdee3806bb9829ca9deae2e4a443fa5ec7158e91c634a4bd045bf78f`.
> Conversion: Local English OCR, all 5 text segments. Source opinions and historical examples are preserved, not automatically endorsed.
> OCR literals, payloads, and complex table alignment are not character-verified throughout. Use the curated guidance and exact-text sources for implementation; do not execute OCR examples without checking the publisher.


Algorithm confusion attacks

JWT attacks web token (JWT) using a different algorithm than is intended by the website's developers. If this case isn't handled properly, this n to forge valid JWTs containing arbitrary values without needing to know the server's secret signing key.

## Symmetric vs asymmetric algorithms

JWTs can be signed using a range of different algorithms. Some of these, such as HS256 (HMAC + SHA-256) use a "symmetric" the server uses a single key to both sign and verify the token. Clearly, this needs to be kept secret, just like a password.

Server Client (Browser) Sign Secret JWT Verify

Other algorithms, such as RS256 (RSA + SHA-256) use an "asymmetric" key pair. This consists of a private key, which the server token, and a mathematically related public key that can be used to verify the signature.

Server Client (Browser)

Sign private key JWT public key Verify

As the names suggest, the private key must be kept secret, but the public key is often shared so that anybody can verify the signæ issued by the server.

## How do algorithm confusion vulnerabilities arise?

Algorithm confusion vulnerabilities typically arise due to flawed implementation of JWT libraries. Although the actual verification pr depending on the algorithm used, many libraries provide a single, algorithm-agnostic method for verifying signatures. These meth parameter in the token's header to determine the type of verification they should perform. The following pseudo-code shows a simplified example of what the declaration for this generic verify ( ) method mig

---

algorithm = token. getAIgHeader ( ) ; if (algorithm "RS256") { // Use the provided key as an RSA public key } else if (a gorithm — "HS256") { // Use the provided key as an HMAC secret key

Problems arise when website developers who subsequently use this method assume that it will exclusively handle JWTs signed u algorithm like RS256. Due to this flawed assumption, they may always pass a fixed public key to the method as follows: publicKey — of server > ; token = request. getCookie ("session") ; verify (token, publicKey) ;

In this case, if the server receives a token signed using a symmetric algorithm like HS256, the library's generic verify ( ) meth( key as an HMAC secret. This means that an attacker could sign the token using HS256 and the public key, and the server will use to verify the signature.

Note The public key you use to sign the token must be absolutely identical to the public key stored on the server. This includes usin (such as X.509 PEM) and preserving any non-printing characters like newlines. In practice, you may need to experiment with in order for this attack to work.

## Performing an algorithm confusion attack

An algorithm confusion attack generally involves the following high-level steps:

1. Obtain the server's public key 2. Convert the public key to a suitable format 3. Create a malicious JWT with a modified payload and the alg header set to HS256 4. Sign the token with HS256, using the public key as the secret.

In this section, we'll walk through this process in more detail, demonstrating how you can perform this kind of attack using Burp Sl

Step 1 - Obtain the server's public key Servers sometimes expose their public keys as JSON Web Key (JWK) objects via a standard endpoint mapped to / j wks . j son known/jwks . ison , for example. These may be stored in an array of JWKs called keys . This is known as a JWK Set.

---

" keys" :

"kty": "RSA" ,

"kid" • "75dOef47 af89 47a9 9061-7c02a610d5ab" 'o-yy1wpYmffgXBxhAUJzHHocCuJ01wDqq175ZWuCQ cb33K2vh9mk6GPM9gNN4Y qTVX67WhsN3,

: "RSA" , " AQAB " , " kid" : 't d8 clAd5abA" , "fc3f yy1wpYmffgXBxhAUJzHq17 9gNNQ cb33HocCuJ01wDqmk6GPM4Y qTVX67WhsN3JvaFYw

Even if the key isn't exposed publicly, you may be able to extract it from a pair of existing JWTs.

Step 2 - Convert the public key to a suitable format Although the server may expose their public key in JWK format, when verifying the signature of a token, it will use its own copy of filesystem or database. This may be stored in a different format. In order for the attack to work, the version of the key that you use to sign the JWT must be identical to the server's local copy. In a the same format, every single byte must match, including any non-printing characters. For the purpose of this example, let's assume that we need the key in X.509 PEM format. You can convert a JWK to a PEM using extension in Burp as follows:

1. With the extension loaded, in Burp's main tab bar, go to the JWT Editor Keys tab. 2. Click New RSA Key. In the dialog, paste the JWK that you obtained earlier. 3. Select the PEM radio button and copy the resulting PEM key. 4. Go to the Decoder tab and Base64-encode the PEM. 5. Go back to the JWT Editor Keys tab and click New Symmetric Key. 6. In the dialog, click Generate to generate a new key in JWK format. 7. Replace the generated value for the k parameter with a Base64-encoded PEM key that you just copied. 8. Save the key.

Step 3 • Modify your JWT Once you have the public key in a suitable format, you can modify the JWT however you like. Just make sure that the alg headt

Step 4 Sign the JWT using the public key Sign the token using the HS256 algorithm with the RSA public key as the secret.

JWT authentication bypass via algorithm confusion

## Deriving public keys from existing tokens

In cases where the public key isn't readily available, you may still be able to test for algorithm confusion by deriving the key from JWTs. This process is relatively simple using tools such as jwt forgery . py . You can find this, along with several other useful

We have also created a simplified version of this tool, which you can run as a single command: ——rm —it port swigger/sig2n docker run &lt; token &lt; token2&gt;

---

You need the Docker CLI to run either version of the tool. The first time you run this command, it will automatically pull the irne Hub, which may take a few minutes.

This uses the JWTs that you provide to calculate one or more potential values of n . Don't worry too much about what this means know is that only one of these matches the value of n used by the server's key. For each potential value, our script outputs:

- A Base64-encoded PEM key in both X.509 and PKCSI format.

- A forged JWT signed using each of these keys.

To identify the correct key, use Burp Repeater to send a request containing each of the forged JWTs. Only one of these will be acc You can then use the matching key to construct an algorithm confusion attack.

JWT authentication bypass via algorithm confusion with no exposed key

For more information on how this process works, and details of how to use the standard jwt forgery .py tool, please refer to provided in the

For more labs, check out the rest of our topic on JWT attacks. JWT attacks

---
