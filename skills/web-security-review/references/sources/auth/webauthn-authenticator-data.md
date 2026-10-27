# WebAuthn authenticator data

> Converted source material; attribution: pilcrow, Pilcrow's auth book. Snapshot processed 2026-10-27.
> Source SHA-256: `9c6207aa34e3b1198ad3a3be1be4703bc14277c7ee38ee99c71c632919197eb1`.
> Conversion: HTML converted to Markdown; text and code preserved. Source opinions and historical examples are preserved, not automatically endorsed.


*Bytes and bits are counted using a 0-based index.*

The authenticator data is binary data that contains information about the authenticator and, during registration, the newly created credential. It must be properly validated during registration and authentication.

The first 32 bytes are the SHA-256 hash of the relying party ID. This must be verified against the expected value.

Byte 32 contains several flags:

	
- Bit 0 (left-most bit): The user presence flag. Set to 1 if the user actively interacted during registration or authentication (e.g., touched a security key or approved a prompt). In most flows, this should be 1.

	
- Bit 2: The user verification flag. Set to 1 if the user's identity was verified (e.g., via PIN or biometrics). Check that the flag is enabled if you require user verification.

	
- Bit 3: The backup eligibility flag. Set to 1 if the authenticator supports credential backup (e.g., cloud sync).

	
- Bit 4: The backed-up state flag. Set to 1 if the credential is currently backed up (e.g., in the cloud).

	
- Bit 6: The attested credential data included flag. Set to 1 if the credential is included in the authenticator data. This should be enabled during registration.

	
- Bit 7: The extensions data included flag.

The next 4 bytes are a 32-bit unsigned integer representing the signature counter.

If the attested credential data included flag is set to 1, the following bytes contain the attested credential data, which has variable length. The first 16 bytes are the AAGUID of the authenticator. An all-zero value indicates it is undefined. The next 2 bytes are a 16-bit unsigned integer indicating the length of the credential ID, followed by the credential ID itself. The maximum size of the ID is set to 1023 bytes (inclusive) by the specification. The remaining bytes contain the credential public key encoded as a COSE key in CBOR format. This is the final element of the attested credential data. See the pages for each algorithm for details on COSE formats: [ECDSA](ecdsa.md), RSA signature scheme, and [EdDSA](eddsa.md). Also see the WebAuthn CBOR encoding page for details on CBOR parsing. Note that the signature algorithm is embedded within the COSE public key.

Finally, if the extensions data included flag is set to 1, the remaining bytes contain a CBOR-encoded map of extensions. See the WebAuthn CBOR encoding page for parsing details. In general, you should reject any extension data that you did not explicitly request or expect.
