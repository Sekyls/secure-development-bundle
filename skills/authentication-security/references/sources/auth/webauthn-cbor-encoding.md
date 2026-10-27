# WebAuthn CBOR encoding

> Converted source material; attribution: pilcrow, Pilcrow's auth book. Snapshot processed 2026-10-27.
> Source SHA-256: `7b0c8cbe0a4d66a5b9bd5f64b4d142e4083629e767b83b0d2e319bd87967e167`.
> Conversion: HTML converted to Markdown; text and code preserved. Source opinions and historical examples are preserved, not automatically endorsed.


You are likely to encounter CBOR in two places: the COSE public key and the extensions data in the authenticator data. You may also encounter it if you choose to manually parse the attestation object during registration.

CBOR is a binary format with data types similar to JSON. It is defined in [RFC 8949](https://datatracker.ietf.org/doc/html/rfc8949). It supports maps (similar to JSON objects), arrays, text strings, byte strings, integers, floats, and simple values (including true, false, undefined, and null).

All CBOR encoding in WebAuthn follows the [CTAP2 canonical CBOR encoding form](https://fidoalliance.org/specs/fido-v2.2-ps-20250714/fido-client-to-authenticator-protocol-v2.2-ps-20250714.html#message-encoding), which defines strict rules that ensure each data structure has a single encoding. This makes parsing CBOR maps significantly easier. Most notably, indefinite-length items are forbidden and map keys must be sorted. As such, you can parse a map expecting a specific key order, and when you expect a specific CBOR value (including maps), it can just be verified using a direct byte-level comparison.
