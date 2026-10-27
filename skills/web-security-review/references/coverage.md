# OWASP Top 10:2025 coverage map

Use this map to check topic coverage during an explicitly broad assessment. It is not an instruction to apply every control to every feature, and not a compliance certificate.

| Category | Relevant guide | Converted category source |
| --- | --- | --- |
| A01 Broken Access Control | [Authorization](authorization.md), [SSRF and HTTP](server-fetch-http.md), [CSRF](browser.md) | [A01](sources/top10/A01_2025-Broken_Access_Control.md) |
| A02 Security Misconfiguration | [Operations](operations.md), [HTTP](server-fetch-http.md) | [A02](sources/top10/A02_2025-Security_Misconfiguration.md) |
| A03 Software Supply Chain Failures | [Dependencies and CI](operations.md#supply-chain-and-secrets) | [A03](sources/top10/A03_2025-Software_Supply_Chain_Failures.md) |
| A04 Cryptographic Failures | [Cryptographic boundaries](passwords-passkeys.md), [Configuration](operations.md#configuration-and-cryptography) | [A04](sources/top10/A04_2025-Cryptographic_Failures.md) |
| A05 Injection | [API and data](api-data.md), [Browser](browser.md) | [A05](sources/top10/A05_2025-Injection.md) |
| A06 Insecure Design | [Business invariants](business-state.md), [Flow descriptions](flows.md) | [A06](sources/top10/A06_2025-Insecure_Design.md) |
| A07 Authentication Failures | [Sessions](sessions.md), [OAuth and JWT](oauth-jwt.md), [Passwords and passkeys](passwords-passkeys.md) | [A07](sources/top10/A07_2025-Authentication_Failures.md) |
| A08 Software or Data Integrity Failures | [Operations](operations.md), [Interpreter boundaries](api-data.md#files-and-interpreters) | [A08](sources/top10/A08_2025-Software_or_Data_Integrity_Failures.md) |
| A09 Security Logging and Alerting Failures | [Observability](operations.md#exceptional-conditions-and-observability) | [A09](sources/top10/A09_2025-Security_Logging_and_Alerting_Failures.md) |
| A10 Mishandling of Exceptional Conditions | [Failures](operations.md#exceptional-conditions-and-observability), [Atomic state](business-state.md) | [A10](sources/top10/A10_2025-Mishandling_of_Exceptional_Conditions.md) |

[Deferred specialist topics](deferred.md) include mobile, automotive, Kubernetes, AI-agent-specific security, and framework-specific material outside the selected JavaScript/TypeScript focus. The catalog includes the relevant neutral web and server topics.
