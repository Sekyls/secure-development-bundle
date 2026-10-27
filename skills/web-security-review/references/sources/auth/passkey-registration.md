# Passkey registration

> Converted source material; attribution: pilcrow, Pilcrow's auth book. Snapshot processed 2026-10-27.
> Source SHA-256: `54cbc06b735458f64bb8a63b5686dbabb74cf989a449864603f4e2a127cf48e7`.
> Conversion: HTML converted to Markdown; text and code preserved. Source opinions and historical examples are preserved, not automatically endorsed.


Before the user can register a passkey, they should be prompted to verify their identity using any of their existing authentication methods.

Passkeys are discoverable WebAuthn credentials that use user verification and do not require attestation.

Because attestation is not required, providing a challenge is optional. You can simply pass an empty byte array as the challenge.

Algorithms -7 (ES256) and -257 (RS256) must be supported. ES256 is by far the most widely supported algorithm. RS256 is required for compatibility with older Windows devices, which only supported RSA credentials. Even devices running newer versions of Windows may still only suppport RS256 if the user created their passkeys before updating. You can also optionally support algorithm -8 (EdDSA), which is widely supported by security keys.

```text
const credential = await navigator.credentials.create({
	challenge: new Uint8Array(0),
	rp: {
		id: new URL(window.location.href).hostname,
	},
	user: {
		id: new TextEncoder().encode(userId),
		displayName: userEmailAddress,
	},
	pubKeyCredParams: [
		{ type: "public-key", alg: -8 },
		{ type: "public-key", alg: -7 },
		{ type: "public-key", alg: -257 },
	],
	excludeCredentials: [{
		id: existingCredentialId,
		type: "public-key",
	}],
	authenticatorSelection: {
		residentKey: "required",
		requireResidentKey: true,
		userVerification: "required",
	},
	attestation: "none",
	extensions: {
		credentialProtectionPolicy: "userVerificationRequired",
		enforceCredentialProtectionPolicy: false,
	},
})
```

Optionally, set the `credentialProtectionPolicy` extension input to `"userVerificationRequired"`. This prevents credential enumeration on roaming authenticators such as security keys, and also requires user verification even when a credential ID is supplied. If you plan to make user verification optional when the passkey is used as a second factor, use `"userVerificationOptionalWithCredentialIDList"` instead. `enforceCredentialProtectionPolicy` must be set to `false` to avoid blocking devices that do not support the extension, such as software-based authenticators. See my blog [Understanding WebAuthn credential protection policy](https://pilcrowonpaper.com/blog/16) for more details on the credential protection policy.

The public key must be properly validated in the server:

	
- ES256: Ensure that the public key uses the P-256 curve and SHA-256 for signatures. Optionally validate the public key by checking that the point lies on the curve and is not the point at infinity. See SEC1 section 3.2.2.1. If you skip this validation, ensure that the signature verification library you use doesn't panic when given an invalid public key during authentication.

	
- RS256: Ensure that the modulus `N` is a 2048-bit integer with no leading zeroes and that the exponent `e` is 65537. While other values are valid, they are uncommon and allowing larger numbers may increase computation costs. You can optionally support 3072-bit and 4096-bit moduli.

	
- EdDSA: Ensure that the curve is Ed25519. Optionally validate that the public key point lies on the curve. Unlike ECDSA, the EdDSA specification explicitly requires implementations to reject invalid public keys, although many implementations don't enforce this. If you skip validation, ensure that the library you use doesn't panic when given an invalid public key.

The extensions data in the authenticator data is usually empty unless you explicitly pass extension inputs to `navigator.credentials.create()`. However, Chrome automatically includes the `credentialProtectionPolicy` extension input unless it is already defined, so extension data may still be included even if you didn't provide any extensions yourself. If defined, the CBOR map will contain a `credProtect` key with one of three values: integer 1 for `userVerificationOptional`, integer 2 for `userVerificationOptionalWithCredentialIDList`, and integer 3 for `userVerificationRequired`. If you explicitly set the extension input to `"userVerificationRequired"`, you can simply check that the extension data equals:

```text
A16B6372656450726F7465637403
```

Note that even if you explicitly set the extension input, credential protection policy support is limited to roaming authenticators that implement the latest standard. Software-based authenticators, such as password managers, generally don't support it and won't include the extensions data in the authenticator data. As such, you must also allow authenticator data that doesn't include the extensions data.

Don't forget to check the number of passkeys the user already has, and reject the request if they've reached the limit.

After creating a passkey, the user should be prompted to name it. To make things smoother, it's helpful to set the default value to a name inferred from the AAGUID. However, you generally only need to do this for software-based authenticators like password managers. Security keys typically don't return the AAGUID when attestation is disabled, and users often carry multiple hardware keys. Since there is no global registry of AAGUIDs, you'll need to compile the AAGUIDs of commonly used authenticators into a [single list](https://github.com/pilcrowonpaper/passwordless-example.auth.pilcrowonpaper.com/blob/main/server_assets/webauthn_authenticators.json).
