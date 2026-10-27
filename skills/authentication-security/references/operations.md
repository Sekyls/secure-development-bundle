# Dependencies, secrets, configuration, and failures

Use when changing dependencies, build pipelines, deployment settings, cryptographic storage, logs, or exception paths.

## Supply chain and secrets

Inspect manifests, lockfiles, package sources, lifecycle scripts, and CI trust boundaries. Review the smallest relevant dependency change and its supported versions; do not run broad upgrades solely because a security skill is active. A scanner finding needs reachability and version evidence, and a clean scan does not prove an application safe.

Keep credentials out of source, browser bundles, build logs, test fixtures, and error output. Use the deployment's secret manager or protected configuration. Rotate an exposed secret with its owner; removing a string from the latest commit does not revoke it. Avoid sharing privileged CI credentials with untrusted pull-request code, including workflows that check out that code after entering a privileged context. Pin automation dependencies according to the project's supply-chain policy.

## Configuration and cryptography

Use maintained cryptographic libraries and provider mechanisms. Distinguish password hashing, encryption, message authentication, and signatures. Identify the data, key lifecycle, access paths, and recovery requirement before choosing a control. Avoid universal algorithm parameters detached from deployment capacity, library semantics, or assurance needs.

Validate TLS, proxy trust, debugging, security headers, storage permissions, and default credentials where the change affects them. Do not weaken certificate verification or globally relax browser policy to resolve local integration issues. Treat infrastructure changes as deployment-specific work supported by evidence.

## Exceptional conditions and observability

Authorization and validation failures must stop the protected operation. Test timeouts, malformed input, missing configuration, upstream failures, and partial writes. Return the application's public error contract without stack traces or secrets. Log actionable security events with safe identifiers and correlation data; exclude tokens, passwords, and unnecessary personal data. Check CR/LF handling and cardinality in user-controlled log fields.

Read: [supply chain](sources/owasp/Software_Supply_Chain_Security_Cheat_Sheet.md), [NPM](sources/owasp/NPM_Security_Cheat_Sheet.md), [CI](sources/owasp/CI_CD_Security_Cheat_Sheet.md), [secrets](sources/owasp/Secrets_Management_Cheat_Sheet.md), [logging](sources/owasp/Logging_Cheat_Sheet.md), and [errors](sources/owasp/Error_Handling_Cheat_Sheet.md).
