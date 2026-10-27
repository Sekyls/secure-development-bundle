# WebAuthn client data

> Converted source material; attribution: pilcrow, Pilcrow's auth book. Snapshot processed 2026-10-27.
> Source SHA-256: `a3136dcb647e5b06469f7efece1968c54c8dd6e2b41845f0e101173062955765`.
> Conversion: HTML converted to Markdown; text and code preserved. Source opinions and historical examples are preserved, not automatically endorsed.


The client data is a JSON object containing information about the client and the context of the WebAuthn operation. The `type` key indicates the operation type, using `"webauthn.create"` for registration and `"webauthn.get"` for authentication. The `challenge` value is the Base64url-encoded challenge string issued by the relying party. Next, the `origin` key specifies the URL origin that initiated the credential request and must always be validated against trusted origins. Finally, the optional `crossOrigin` value is set to `true` when the request is performed within an iframe, and is omitted when the request is not cross-origin. All values should be validated and match the expected value.

```text
{
	"type": "webauthn.get",
	"challenge": "q83vEjR9mK2Y7pT4cW1BQg",
	"origin": "https://example.com",
	"crossOrigin": false
}
```
