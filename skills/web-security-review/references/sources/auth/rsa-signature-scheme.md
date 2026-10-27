# RSA signature scheme

> Converted source material; attribution: pilcrow, Pilcrow's auth book. Snapshot processed 2026-10-27.
> Source SHA-256: `9529a8c32fdafa4b4d3d349ce9361e8a4251a49e5286487b067151af3862cee0`.
> Conversion: HTML converted to Markdown; text and code preserved. Source opinions and historical examples are preserved, not automatically endorsed.


RSA is a widely used family of public-key cryptosystems based on the math of modular exponentiation. The two common signature schemes are `RSASSA-PKCS1-v1_5`, originally defined in PKCS #1 v1.5, and `RSASSA-PSS`, originally defined in PKCS #1 v2.1. Both are currently defined in [PKCS #1 v2.2 (RFC 8017)](https://datatracker.ietf.org/doc/html/rfc8017). They use different padding algorithms but share the same RSA public key structure. `RSASSA-PSS` is newer and more complex, while the older `RSASSA-PKCS1-v1_5` is still commonly used.

The private exponent `d` is an integer derived from two large primes. The size of these primes determines the size and security of the key. For example, choosing two 1024-bit prime numbers will result in a 2048-bit private key. During key generation, the public key is created as a pair consisting of the modulus `n` and the public exponent `e`. The modulus has the same bit length as the private key and the public exponent is commonly set to 65537.

When generating or verifying a signature, a cryptographic hash function is used to hash the message. A common choice is SHA-256.

## Public key formats

### PKCS#1

In this format, the signature is represented as a DER-encoded ASN.1 sequence containing the modulus and public key exponent.

```text
RSAPublicKey ::= SEQUENCE {
	modulus			INTEGER,
	publicExponent	INTEGER
}
```

### ANSI X9.6

Also known as the X.509, SubjectPublicKeyInfo, or PKIX format. The public key is represented as a DER-encoded ASN.1 `SubjectPublicKeyInfo` sequence. `AlgorithmIdentifier.algorithm` is `1.2.840.113549.1.1.1` for RSA. The `subjectPublicKey` is the public key in the PKCS#1 format.

```text
SubjectPublicKeyInfo := SEQUENCE {
	algorithm			AlgorithmIdentifier,
	subjectPublicKey	BIT STRING
}

AlgorithmIdentifier := SEQUENCE {
	algorithm	OBJECT IDENTIFIER
	parameter	NULL
}
```

### COSE

Defined in [RFC 8152](https://datatracker.ietf.org/doc/html/rfc8230), the public key is represented as a CBOR-encoded RSA key map. The algorithm value is a COSE identifier registered in the [IANA registry](https://www.iana.org/assignments/cose/cose.xhtml).

```text
{
	1: 3,
	3: -257,
	-1: h'...',
	-2: h'010001'
}
```
