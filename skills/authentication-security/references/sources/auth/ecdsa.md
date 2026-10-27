# Elliptic Curve Digital Signature Algorithm (ECDSA)

> Converted source material; attribution: pilcrow, Pilcrow's auth book. Snapshot processed 2026-10-27.
> Source SHA-256: `47e02b625aec6b453a49a3536b7761d50e4d103a4fa0b018493e29a2bd29577a`.
> Conversion: HTML converted to Markdown; text and code preserved. Source opinions and historical examples are preserved, not automatically endorsed.


Elliptic Curve Digital Signature Algorithm (ECDSA) is an asymmetric digital signature scheme defined in SEC 1 that uses elliptic-curve cryptography. It achieves security comparable to RSA-based schemes while using significantly smaller keys and signatures.

The elliptic curve used with ECDSA is configurable. NIST-recommended curves are often used, with P-256 (secp256r1) being by far the most common.

The private key is a randomly generated integer `d`. The corresponding public key is a point on the elliptic curve derived from this value, represented as a pair of integers `(x, y)`. The signature consists of two integers, `r` and `s`, whose values depend on both the message and the private key.

When generating and verifying a signature, a hash function is used to hash the message. A common choice is using the SHA-256 hash function with the P-256 curve. However, the hash function is independent of the curve and any hash algorithm can be used with any key pair.

## Public key formats

### SEC 1

Originally defined in the SEC 1 specification, this public key is a concatenation of the integer pair with a header byte. For the P-256 curve, both the `x` and `y` coordinates are 32 bytes each.

```text
0x04 || x || y
```

The public key can also be represented in a compressed format, which stores only the `x` coordinate. The header byte is `0x02` if `y` is even or `0x03` if it is odd.

```text
0x02 || x
0x03 || x
```

### ANSI X9.62

Also known as the X.509, SubjectPublicKeyInfo, or PKIX format. The public key is represented as a DER-encoded ASN.1 `SubjectPublicKeyInfo` sequence. `AlgorithmIdentifier.algorithm` is `1.2.840.10045.2.1`. The `subjectPublicKey` is either the uncompressed or compressed SEC1 format public key.

For the P-256, the object identifier of `AlgorithmIdentifier.namedCurve` is `1.2.840.10045.3.1.7`

```text
SubjectPublicKeyInfo := SEQUENCE {
	algorithm			AlgorithmIdentifier,
	subjectPublicKey	BIT STRING
}

AlgorithmIdentifier := SEQUENCE {
	algorithm	OBJECT IDENTIFIER
	namedCurve	OBJECT IDENTIFIER
}
```

### COSE

Defined in [RFC 8152](https://datatracker.ietf.org/doc/html/rfc8152), the public key is represented as a CBOR-encoded EC2 key map. Although optional in the COSE specification, the algorithm value (3) will always be defined in WebAuthn. The algorithm value is a COSE algorithm identifier registered in the [IANA registry](https://www.iana.org/assignments/cose/cose.xhtml) (usually ECDSA with the hash function). The curve value (-1) is one of the curve identifier also registered in the IANA registry. The x and y values (-2, -3) are integers encoded as binary strings in WebAuthn.

For the P-256 curve, the curve identifier is 1, and the x and y values are exactly 32 bytes each.

```text
{
	1: 2,
	3: -7,
	-1: 1,
	-2: h'0000000000000000000000000000000000000000000000000000000000000000',
	-3: h'0000000000000000000000000000000000000000000000000000000000000000'
}
```

## Signature formats

### IEEE P1363

In this format, the signature is represented as a simple concatenation of the integer pair `(r, s)`, with each encoded as a big-endian binary string. For the P-256 curve, these are 32 bytes each.

```text
r || s
```

### ANSI X9.62

Also referred to simply as the ASN.1 format. In this format, the signature is represented as a DER-encoded ASN.1 sequence containing the integer pair `(r, s)`.

```text
SEQUENCE {
	r	INTEGER,
	s	INTEGER
}
```
