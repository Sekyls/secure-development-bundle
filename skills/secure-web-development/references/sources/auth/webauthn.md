# Web Authentication API (WebAuthn)

> Converted source material; attribution: pilcrow, Pilcrow's auth book. Snapshot processed 2026-10-27.
> Source SHA-256: `70c8c0ca547863e1c176ea94295425a6419bbd411704395b768e76fc88255bf2`.
> Conversion: HTML converted to Markdown; text and code preserved. Source opinions and historical examples are preserved, not automatically endorsed.


## Overview

The [Web Authentication API (WebAuthn)](https://www.w3.org/TR/webauthn-3/) is an API for working with public key-based credentials. Typically, the credential's private key is stored on the user's computer, mobile device, external hardware token, or password manager. When the credential is stored on a physical device, the user can prove possession of that device. This is known as user presence. Additionally, credentials can be protected using a shared PIN or biometrics and can be used to verify the user's identity. This is known as user verification.

There are 3 actors involved in the process: the authenticator, the relying party, and the client. The authenticator stores the credential's private key and is also responsible for verifying the user's identity when supported. Each authenticator has a globally unique identifier known as the AAGUID. The relying party is the entity that can request the authenticator to create a credential or provide proof of ownership of an existing credential. It consists of both the client-side component that interacts with the web API and the server that stores and verifies the relevant data. In short, this is your website. Finally, the client is the browser, which acts as the intermediary between the authenticator and the relying party.

During registration, the authenticator generates and stores a private key while sharing the corresponding public key with the relying party, which stores it for future verification. During authentication, the user selects a credential, the authenticator optionally verifies the user, and then signs a message using the credential's private key. The credential ID and signature are sent to the relying party, which verifies the signature using the stored public key.

Credentials that are stored directly on and accessible to the authenticator are called discoverable credentials (formerly known as resident keys). There are also non-discoverable credentials, where the private key is not stored by the authenticator. These are commonly used by devices with limited storage, such as external hardware tokens. These are usually implemented by encrypting the generated private key and exporting it as the credential ID, which is then stored by the relying party. During authentication, the relying party provides the authenticator with a list of credential IDs, and the authenticator attempts to decrypt them and use the ones it recognizes. In this model, the user must first manually identify the account they're attempting to authenticate as.

WebAuthn also optionally supports attestation statements, which can be used to verify the authenticity and provenance of an authenticator. Depending on the attestation format and policy, this may show general information like the authenticator model, or provide more specific information that identifies a particular device. Attestation is most commonly used in enterprise environments, for example to ensure that only company-issued security keys are used.

The relying party is identified by its relying party ID, which is a domain. A website can identify itself as a relying party using either its current domain or any parent domain. For example, `foo.example.com` can use either `foo.example.com` or `example.com`. Similar to the definition of same-site origins, browsers take the Public Suffix List into account. For example, `foo.github.io` cannot use `github.io` as its relying party ID because it is a public suffix.

## Registration

For each attempt, the relying party generates a unique, single-use random nonce as a challenge on the server and return it to the client. The authenticator will use this value to ensure each attestation statement is unique. Note that the challenge is only necessary when attestation is required, and provides no added value if attestation is not being used or enforced.

A credential can be created using `navigator.credentials.create()` by passing the `publicKey` option.

```text
const credential = await navigator.credentials.create({
	publicKey: {
		challenge: challenge,
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
			type: "public-key",
			id: existingCredentialId,
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
	},
});
```

First, it takes the relying party ID as `rp.id`. It also used to accept a human-readable name as `rp.name`, but this has since been deprecated.

Next is the `user` property. The credential will be scoped to the user ID defined by `user.id`. If a new credential is created with the same user ID, it will replace the existing credential. The ID will also be returned by the authenticator during authentication. As such, I recommend matching it to your application's internal user ID, even though it can technically be any arbitrary byte sequence. If you prefer not to expose the actual user ID, use a randomly generated byte string instead. The `user.displayName` should contain a human-readable identifier for the user, such as an email address, user ID, username, or full name. It also previously accepted `user.name`, but this has likewise been deprecated.

The `challenge` option takes the generated challenge. This should be an empty byte array if the challenge is not used.

The `pubKeyCredParams` array defines which credential types and signature algorithms the relying party supports. Since there is only a single credential type, `public-key`, this is used to specify the supported signature algorithms. These algorithms are identified using the [COSE algorithm IDs](https://www.iana.org/assignments/cose/cose.xhtml) defined in the IANA registry. Confusingly, the signature algorithm also determines the public key type, even though multiple public key types can theoretically support the same signature algorithm. Some commonly used algorithms include:

	
- -7 (ES256): ECDSA with SHA-256. The WebAuthn specification requires the authenticator to use the P-256 curve. The authenticator will return an ECDSA public key using P-256. Do not use -9 (ESP256).

	
- -257 (RS256): `RSASSA-PKCS1-v1_5` with SHA-256. The authenticator will return an RSA public key.

	
- -8 (EdDSA): EdDSA. The WebAuthn specification requires the authenticator to use the Ed25519 curve. The authenticator will return an Ed25519 public key and use Ed25519 signatures. Do not use -19 (Ed25519).

The `authenticatorSelection` object specifies the type of credential to create. `residentKey` should be set to `"required"` when creating a discoverable credential and `"discouraged"` otherwise. The `requireResidentKey` option is deprecated, but should still be included for backward compatibility. The `userVerification` option controls whether the authenticator should verify the user and accepts one of `"required"`, `"preferred"`, or `"discouraged"`. In practice, this is mainly used to ensure that the authenticator supports user verification. Requiring user verification during registration does not imply that it'll also be required during authentication.

The `attestation` property specifies the type of attestation the relying party requires. `"none"` indicates that the relying party does not require attestation. Note that the authenticator may omit device-identifying information entirely, including the AAGUID with this option. `"direct"` indicates that the relying party requires attestation from the authenticator itself, which typically reveals information about the authenticator model without identifying the specific device. `"indirect"` is similar to `"direct"`, except the client (browser) may replace the attestation with a more privacy-preserving format, such as one issued by a third-party certificate authority. In this case, detailed device information may not be available, though the attestation can still be used to verify the authenticator's capabilities and trustworthiness. In practice, though, I'm not aware of any modern browsers that behave any differently from `"none"` or `"direct"`. Finally, `"enterprise"` indicates that the relying party is requesting enterprise attestation, which can uniquely identify a specific device. The authenticator must explicitly allow the relying party before this type of attestation can be returned.

Use the `excludeCredentials` option to provide a list of existing credentials and prevent authenticators that already own those credentials from creating new ones.

Finally, the `extensions` property can be used to define extension inputs.

The returned promise will reject with an `Error` with a name `NotAllowedError` if the user cancels the process or `NotSupportedError` if the authenticator cannot create a credential that satisfies the provided requirements (e.g., no supported algorithms, doesn't support user verification).

If a credential is successfully created, the returned promise resolves to a `PublicKeyCredential`, which its `response` property contains an `AuthenticatorAttestationResponse`. This includes the CBOR-encoded attestation object map as `AuthenticatorAttestationResponse.attestationObject` and the UTF-8–serialized, JSON-encoded client data object as `AuthenticatorAttestationResponse.clientDataJSON`.

The attestation object includes the attestation statement, the attestation statement format, and the authenticator data. The attestation statement must be parsed based on the format. The authenticator data is a binary data structure that includes the credential ID, AAGUID, public key, and various other metadata about the authenticator. See the WebAuthn authenticator data page for details. Don't forget to verify that the public key is valid and one of the supported type.

The client data is a JSON object that includes information about the client. See the WebAuthn client data page for details.

If an attestation statement is defined, verify it against the authenticator data and client data. Then invalidate the current challenge. The challenge can be invalidated after a successful attestation or after each attempt. Both approaches are more or less equally secure.

Finally, store the credential ID, public key, and signature algorithm. Ensure the credential ID is unique and queryable, and reject it if it's already in use. Optionally, associate the credential ID with the user ID passed to `navigator.credentials.create()`, since the authenticator returns both during authentication. "Since the challenge can be up to 1023 bytes, you can hash it first if that's too long to store directly.

You can optionally support signature counters, which are incremented after each signing operation. In practice, these are mostly supported by external hardware tokens. A value of 0 indicates that the authenticator does not support signature counters. Although signature counters are intended to help detect potential private key theft, they do not provide definitive proof that compromise has occurred. At best, a counter mismatch should be treated as a signal to log the event and manually review the user's account. It's also worth noting that extracting private keys from external hardware tokens is generally very difficult in practice and unlikely to happen. If you implement this feature, store the latest counter value alongside the credential.

If attestation is not required, you can ignore the client data, as it cannot be verified. The Web API also allows direct access to authenticator data with `AuthenticatorAttestationResponse.getAuthenticatorData()`, letting you skip parsing the CBOR-encoded attestation object.

Alternatively, you can skip parsing the authenticator data on the server entirely. The credential ID is available as `PublicKeyCredential.rawId`, and the public key can be obtained using `AuthenticatorAttestationResponse.getPublicKey()`. The key is returned as a `SubjectPublicKeyInfo` ASN.1 structure encoded in DER format, as defined in [RFC 5280](https://datatracker.ietf.org/doc/html/rfc5280). You must also retrieve the COSE algorithm ID via `AuthenticatorAttestationResponse.getPublicKeyAlgorithm()`, since the public key doesn't include the signing algorithm. Note that Safari only supports algorithm -7 (ES256) with the P-256 curve for `getPublicKey()`, so you may still need to parse out the COSE public key from the authenticator data if you want to support all algorithms equally. The AAGUID is not exposed in JavaScript and must be parsed from the authenticator data. You can then send the credential ID, public key, signature algorithm, and optionally the AAGUID to the server instead.

## Authentication

Similar to registration, you must first generate a challenge. Unlike attestation statements, which are optional, the challenge is required here because it's used to generate and verify signatures.

To verify an authenticator, use `navigator.credentials.get()` with the `publicKey` option.

```text
const credential = await navigator.credentials.get({
	publicKey: {
		challenge: challenge,
		allowCredentials: [{
			type: "public-key",
			id: registeredCredentialId,
		}],
		userVerification: "required",
	},
});
```

The `challenge` option contains the generated challenge.

The `allowCredentials` option can be used to restrict credentials and to list IDs of non-discoverable credentials.

The `userVerification` option controls how strictly the authenticator enforces user verification. It takes one of `"required"`, `"preferred"`, or `"discouraged"`.

The method returns a promise that either rejects if the user cancels the operation or resolves to a `PublicKeyCredential`, which its response property contains an `AuthenticatorAssertionResponse`

Unlike registration, you must send the full binary authenticator data and client data to the server. These are available as `AuthenticatorAssertionResponse.authenticatorData` and `AuthenticatorAssertionResponse.clientDataJSON`. You must also send the credential ID from `PublicKeyCredential.rawId`, along with the signature from `AuthenticatorAssertionResponse.signature`. The user handle for the credential is available via `AuthenticatorAssertionResponse.userHandle`.

After parsing and validating the authenticator data and client data, retrieve the credential using the credential ID. To verify the signature, first compute the SHA-256 hash of the client data JSON. Then build the signed message by concatenating the authenticator data first, followed by the hash of the client data JSON. Verify the signature against this message. If verification fails or the public key is invalid, the process must fail. Note that for ECDSA, the signature is in the ANSI X9.62 (ASN.1) format.

If you support signature counters and the stored credential has a non-zero counter value, check whether the current attempt's signature counter is greater than the stored value, and update the stored value before verifying the signature. If the counter is smaller, simply log the event for future reference. Do not block the authentication attempt, as in the event of private key theft, the legitimate user's authentications are also likely to encounter counter mismatches. I would also discourage disabling the account as well since these can occur naturally.

Finally, invalidate the challenge. As with registration, you can invalidate it only after successful validation or after each attempt.
