# Edwards Curve Digital Signature Algorithm (EdDSA)

> Converted source material; attribution: pilcrow, Pilcrow's auth book. Snapshot processed 2026-10-27.
> Source SHA-256: `4bf14622087930b46c3d98bc94332a0e89a4b3833e4aa68180238831c16f1f78`.
> Conversion: HTML converted to Markdown; text and code preserved. Source opinions and historical examples are preserved, not automatically endorsed.


The Edwards-curve Digital Signature Algorithm (EdDSA) is an asymmetric digital signature scheme based on elliptic curves. It serves as a modern, high-performance alternative to [ECDSA](ecdsa.md).

The private key is a random integer `k` encoded as a bit string, and the corresponding public key is a point on the curve `A` compressed and encoded as a bit string. The signature consists of a pair of integers `(r, s)` which are also compressed and encoded into a single bit string.

Unlike ECDSA, the parameters for EdDSA schemes are typically pre-defined. Ed25519 uses the Curve25519 curve with the SHA-512 hash function and has a key size of 256 bits. Ed448 uses the Curve448 curve with the SHAKE256 hash function and has a key size of 456 bits. These shouldn't be confused with X25519 and X448, which are variants of Elliptic-curve Diffie–Hellman (ECDH).

## Public key formats

### ANSI X9.62

Also known as the X.509, SubjectPublicKeyInfo, or PKIX format. The public key is represented as a DER-encoded ASN.1 `SubjectPublicKeyInfo` sequence. `AlgorithmIdentifier.algorithm` is either `1.3.101.112` for Ed25519 or `1.3.101.113` for Ed448. The `subjectPublicKey` is the raw public key.

```text
SubjectPublicKeyInfo := SEQUENCE {
	algorithm			AlgorithmIdentifier,
	subjectPublicKey	BIT STRING
}

AlgorithmIdentifier := SEQUENCE {
	algorithm	OBJECT IDENTIFIER
}
```

### COSE

Defined in [RFC 8152](https://datatracker.ietf.org/doc/html/rfc8152), the public key is represented as a CBOR-encoded OKP map. Although optional in the COSE specification, the algorithm value (3) is always present in WebAuthn and holds the COSE algorithm identifier registered in the [IANA registry](https://www.iana.org/assignments/cose/cose.xhtml), which is usually -8 (EdDSA). The curve value (-1) is either `6` for Ed25519 or `7` for Ed448, and the x value (-2) contains the raw public key.

```text
{
	1: 4,
	3: -7,
	-1: 6,
	-2: h'fjuLyU4qH41sWp8bPnwtWotOHJ8tajtcfh1Ki5wuXxo',
}
```
