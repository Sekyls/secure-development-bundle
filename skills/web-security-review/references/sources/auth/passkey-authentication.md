# Passkey authentication

> Converted source material; attribution: pilcrow, Pilcrow's auth book. Snapshot processed 2026-10-27.
> Source SHA-256: `6ad42a0ab2f7c4e29e88842d71b13607711bf6a345a3c44a7cf47e577f54329c`.
> Conversion: HTML converted to Markdown; text and code preserved. Source opinions and historical examples are preserved, not automatically endorsed.


Generate a challenge and call `navigator.credentials.get()` with user verification set to required.

```text
const credential = await navigator.credentials.get({
	publicKey: {
		challenge: challenge,
		userVerification: "required",
	},
});
```

On the server, verify that both the user presence and user verification flag in the authenticator data is enabled. Then, verify the signature using the public key.

If you also support authentication methods that require the user to enter their email address (such as password-based login or email code sign-in), consider adding passkey autofill. This is a feature where the browser displays a dropdown of the user's available passkeys when they focus the input field, similar to password managers. This significantly reduces friction since the user doesn't need to understand what passkeys are or manually look for a “Sign in with passkey” button.

First, set the `autocomplete` attribute of the email address input to `"webauthn"`.

```text
<input name="email_address" type="email" autocomplete="webauthn">
```

Next, after the page loads, call `navigator.credentials.get()` with `mediation` set to `"conditional"`.

```text
const credential = await navigator.credentials.get({
	mediation: "conditional",
	publicKey: {
		...
	},
});
```

The promise resolves only when the user selects a passkey from the dropdown, and rejects if the user cancels the authentication flow. Note that the promise may remain pending indefinitely until the user takes an action. As such, this should either be called at the end of the main JavaScript script or by using floating promises.

If your challenges expire, ensure that the request is aborted when the challenge expires, or automatically refresh the challenge periodically. Users may leave the page open for an extended period of time before interacting with the dropdown UI.
