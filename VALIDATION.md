# Validation results

Checked 2026-10-27. This report describes the bundle and local fixtures, not the security of an application that later uses the skills.

## Behavioral checks

All eight scenarios passed using Node 26.3.0 and, for XSS, Playwright with a temporary headless Microsoft Edge profile.

| Scenario | Observed outcome |
| --- | --- |
| Tenant isolation | Unscoped query exposed another tenant's row; scoped query denied it and preserved authorized access |
| SQL injection | Concatenation returned both rows for injected input; parameter binding returned none and accepted normal input |
| SSRF redirect | Initial-origin-only check followed a redirect to the forbidden local test server; rejecting redirects prevented that request and allowed the normal endpoint |
| Session revocation | A stolen token remained usable after deleting the client cookie; deleting server state prevented replay and expiry was enforced |
| JWT validation | The intended signed token was accepted; wrong audience, expired/future validity, unsigned algorithm, and altered payload were rejected |
| Concurrent spending | Two synchronized workers on separate SQLite connections spent 160 from a balance of 100 with the unsafe pattern; the conditional transactional update allowed one spend of 80 |
| Browser XSS | Raw HTML insertion executed an event handler; text insertion created no image element, executed no handler, and preserved the literal input |
| TypeScript runtime boundary | A type assertion retained a privilege-bearing input field; runtime checking and explicit field selection preserved the existing role |

The initial sandbox blocked loopback connections and headless browser startup. These checks were rerun successfully outside that sandbox using only temporary local data, local servers, and a fresh browser profile. No production system or existing browser session was tested.

The executable examples are preserved as Markdown code blocks in each skill's [scenario reference](skills/secure-web-development/references/scenarios.md). HMAC verification is a narrow fixture, not production JWT middleware. The database fixture does not prove PostgreSQL or distributed-system behavior. The SSRF fixture does not test general DNS rebinding or arbitrary public destinations.

## Source conversion

The bundle contains 252 unique converted source documents, intentionally duplicated across the three independent skills. All 105 supplied print documents and 637 rendered segments were processed. The [conversion report](skills/secure-web-development/references/conversion-report.md) identifies visual checks, source fingerprints, deferred topics, and remaining OCR limitations.

OCR literals and complex visuals were not all verified character by character. Selected XSS examples and the request-smuggling and race-condition diagrams were compared visually and given corrected Markdown transcriptions or descriptions. Read the exact-text sources and focused guides before implementing security controls.

## Workflow checks

The authoring agent walked through implementation, review-only, and authentication requests against the entrypoints and fixtures. The review entrypoint requires evidence and preserves read-only mode; implementation stays within the requested feature; authentication preserves provider and verifier semantics. Secure counterparts are included to reduce unsupported findings.

These are author checks, not independent model evaluations or guarantees of future invocation behavior.

## Structural and relocation checks

- All three skill directories passed the skill-creator validator.
- The bundle contains 822 files, all with the `.md` extension.
- All 3,191 internal Markdown links resolve, including supplied heading fragments where retained. Unavailable original source anchors or attachments are documented instead of left as broken references.
- Every independent skill contains its own sources, notices, guides, and scenarios. No internal skill reference escapes its skill directory.
- No bundled image dependency, original document attachment, plugin manifest, optional host metadata file, or absolute local reference is required.
- The exact fixture code blocks extracted from the delivered Markdown were rerun: six Node scenarios, one real-browser XSS check, and one TypeScript runtime check passed.

Archive integrity and relocation checks are performed against the matching ZIP and independently copied skill directories.

Results: archive integrity passed; all 822 extracted file hashes match the delivered folder. All internal references resolve after relocating the complete bundle and after copying each skill independently. The archive was rebuilt with this report and verified again.
