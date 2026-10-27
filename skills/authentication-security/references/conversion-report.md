# Conversion report

Processed 2026-10-27. Unique source documents: **252**. Source copies appear in every independent skill; these copies are intentional and identical before local-link normalization.

| Collection | Documents | Method |
| --- | --- | --- |
| OWASP Cheat Sheet Series | 110 | Exact Markdown text; normalize references, tables and illustrations |
| OWASP Top 10:2025 English sources | 16 | Exact Markdown text; convert embedded tables to Markdown |
| Authentication guide | 21 | Local HTML-to-Markdown conversion preserving text and code |
| Web Security Academy articles | 103 | English OCR of every rendered segment |
| OWASP background books | 2 | Native text extraction |

All 105 supplied print documents were processed across all 637 rendered text segments, including the 120-segment XSS reference. No source was silently skipped. Twenty-seven specialist cheat sheets were deliberately deferred. Fingerprints and per-document processing status are in the [source catalog](source-index.md).

## Visual and exact-text checks

A final quality pass narrowed the navigation filter so research callouts and article headings remain in the source body. The browser-powered request-smuggling article was visually rechecked; its CL.0 and client-side desynchronization sections are retained. The affected source conversions were regenerated before final archive validation.


- XSS prevention: compared the output-encoding section with the rendered source; corrected the HTML entity and Unicode escape examples and transcribed the visible markup example separately.
- XSS reference: compared the event-handler examples for history changes, resize, and animation repeats with the rendered source; provided corrected literal samples as a separate verified transcription.
- Request smuggling: inspected the two front-end/back-end diagrams. Added a Markdown description of normal request boundaries and the attacker's prefix being prepended to another request.
- Race conditions: inspected the sequential and concurrent discount-code diagrams. Added a Markdown description distinguishing the rejected second sequential use from two simultaneous successful checks.
- Authentication guide: code-block extraction preserves the source code text, including WebAuthn and password-format examples. General prose whitespace is normalized; no runtime options were inferred from OCR.
- OWASP category score tables are normalized to Markdown rows. Original code fences are retained. Illustration files were absent from the supplied Cheat Sheet snapshot, so their labels and surrounding text are preserved with explicit visual-detail gaps.

## Remaining limitations

- The OCR article text is a searchable transcription, not a character-perfect source for every payload, code sample, formula, or table. Most literals were not manually compared; the verified samples are explicitly separated. Do not run unverified OCR payloads by default.
- OCR cannot infer every diagram's arrow direction or grouping. Reviewed flow descriptions are included; visual-only details elsewhere remain unverified. The source report identifies illustration labels rather than fabricating missing graphics.
- Some print captures have clipped content, overlays, or start mid-sentence. Lost source text cannot be recovered by conversion. Some small source headings may remain plain paragraphs where typography was ambiguous.
- Source links to omitted attachments or deferred topics are retained as plain labels. Invalid original heading fragments are retargeted to the available Markdown document and recorded in the link-normalization section after validation.
- No full corpus of external links was fetched. Publisher identities and the current primary-source checks are recorded separately; local reference checks do not establish remote availability.

## Per-source conversion notes

### owasp / Abuse_Case_Cheat_Sheet

- Illustration retained as a label; visual-only details unavailable: Overview Schema
- Unavailable cross-reference retained as plain text: Schemas descriptors archive

### owasp / Attack_Surface_Analysis_Cheat_Sheet

- External attachment reference retained as plain text: Relative Attack Surface Quotient (RSQ)
- External attachment reference retained as plain text: Attack Surface Metric
- External attachment reference retained as plain text: Howard, Pincus, and Wing: Measuring Relative Attack Surfaces

### owasp / Authentication_Cheat_Sheet

- Unavailable cross-reference retained as plain text: SAML Security Cheat Sheet

### owasp / Authentication_Patterns_Cheat_Sheet

- Illustration retained as a label; visual-only details unavailable: Service-Level Embedded Authentication
- Illustration retained as a label; visual-only details unavailable: Service-Level Code-Mediated Authentication
- Illustration retained as a label; visual-only details unavailable: Service-Level Proxy-Mediated Authentication
- Illustration retained as a label; visual-only details unavailable: Edge-Level Authentication
- Illustration retained as a label; visual-only details unavailable: Network-Layer (Node-Level) Authentication
- External attachment reference retained as plain text: NIST SP 800-204
- External attachment reference retained as plain text: NIST SP 800-204: Security Strategies for Microservices-Based Application Systems

### owasp / Authorization_Cheat_Sheet

- External attachment reference retained as plain text: Veracode's State of Software Vol. 10
- External attachment reference retained as plain text: NIST SP 800-162
- Unavailable cross-reference retained as plain text: Insecure Direct Object Reference Prevention Cheat Sheet
- External attachment reference retained as plain text: NIST SP 800-162: Guide to Attribute Based Access Control (ABAC) Definition and Considerations
- External attachment reference retained as plain text: Ferraiolo and Kuhn: Role-Based Access Controls (1992)

### owasp / Authorization_Decisions_And_Output_Handling_Cheat_Sheet

- Illustration retained as a label; visual-only details unavailable: PDP as a filter
- Illustration retained as a label; visual-only details unavailable: Authorized data set
- Illustration retained as a label; visual-only details unavailable: Authorization filter

### owasp / Authorization_Patterns_Cheat_Sheet

- Illustration retained as a label; visual-only details unavailable: Decentralized service-level authorization
- Illustration retained as a label; visual-only details unavailable: Centralized service-level authorization
- Illustration retained as a label; visual-only details unavailable: Edge-level authorization
- Illustration retained as a label; visual-only details unavailable: Gateway authorization with propagated context

### owasp / Authorization_Policy_And_Data_Distribution_Cheat_Sheet

- Illustration retained as a label; visual-only details unavailable: On-demand data lookup
- Illustration retained as a label; visual-only details unavailable: Out-of-band data delivery
- Illustration retained as a label; visual-only details unavailable: Request-time data injection
- Illustration retained as a label; visual-only details unavailable: Embedded data

### owasp / Authorization_Testing_Automation_Cheat_Sheet

- Illustration retained as a label; visual-only details unavailable: RenderingExample

### owasp / Bot_Management_and_Anti-Automation_Cheat_Sheet

- Unavailable cross-reference retained as plain text: Logging Cheat Sheet
- External attachment reference retained as plain text: OWASP Automated Threat Handbook

### owasp / CI_CD_Security_Cheat_Sheet

- External attachment reference retained as plain text: NIST
- External attachment reference retained as plain text: NIST's Security Considerations for Code Signing
- External attachment reference retained as plain text: CISA, NSA, and ODNI: Securing the Software Supply Chain - Recommended Practices for Developers
- External attachment reference retained as plain text: NIST: Security Considerations for Code Signing

### owasp / Clickjacking_Defense_Cheat_Sheet

- Illustration retained as a label; visual-only details unavailable: NestedFrames

### owasp / Credential_Stuffing_Prevention_Cheat_Sheet

- External attachment reference retained as plain text: Sentry MBA

### owasp / Cross-Site_Request_Forgery_Prevention_Cheat_Sheet

- Unavailable cross-reference retained as plain text: ASP.NET
- External attachment reference retained as plain text: This resource
- External attachment reference retained as plain text: paper
- External attachment reference retained as plain text: 4
- External attachment reference retained as plain text: this
- External attachment reference retained as plain text: session fixation attacks
- External attachment reference retained as plain text: Robust Defenses for Cross-Site Request Forgery section 4.1
- External attachment reference retained as plain text: Barth, Jackson, and Mitchell: Robust Defenses for Cross-Site Request Forgery

### owasp / Cryptographic_Storage_Cheat_Sheet

- Unavailable cross-reference retained as plain text: Post-Quantum Cryptography Cheat Sheet
- External attachment reference retained as plain text: NIST SP 800-38D, Sections 5.2.1.1 and 8
- External attachment reference retained as plain text: NIST SP 800-57 Part 1 Rev. 5: Recommendation for Key Management

### owasp / Database_Security_Cheat_Sheet

- Unavailable cross-reference retained as plain text: views

### owasp / Denial_of_Service_Cheat_Sheet

- Illustration retained as a label; visual-only details unavailable: DDOSFlow
- External attachment reference retained as plain text: Assess the provider's mitigation capacity and coverage
- External attachment reference retained as plain text: CERT-EU: DDoS Overview and Incident Response Guide

### owasp / Dependency_Graph_SBOM_Cheat_Sheet

- External attachment reference retained as plain text: CycloneDX SBOM guide
- External attachment reference retained as plain text: CycloneDX guide
- External attachment reference retained as plain text: CycloneDX generation guidance
- External attachment reference retained as plain text: CISA's SBOM consumption guidance
- External attachment reference retained as plain text: CISA's consumption guidance
- External attachment reference retained as plain text: OWASP CycloneDX: Authoritative Guide to SBOM
- External attachment reference retained as plain text: CISA: Recommended Practices for Software Bill of Materials Consumption

### owasp / Deserialization_Cheat_Sheet

- Unavailable cross-reference retained as plain text: Java Deserialization Attacks — German OWASP Day 2016

### owasp / Docker_Security_Cheat_Sheet

- Unavailable cross-reference retained as plain text: Kubernetes Security Cheat Sheet
- External attachment reference retained as plain text: SBOM Generation

### owasp / Error_Handling_Cheat_Sheet

- Illustration retained as a label; visual-only details unavailable: Overview

### owasp / GitHub_Actions_Security_Cheat_Sheet

- External attachment reference retained as plain text: NIST "Strategies for the Integration of Software Supply Chain Security in DevSecOps CI/CD Pipeline"

### owasp / HTTP_Headers_Cheat_Sheet

- Illustration retained as a label; visual-only details unavailable: Illustration

### owasp / Identity_Propagation_Patterns_Cheat_Sheet

- Illustration retained as a label; visual-only details unavailable: External identity propagation
- Illustration retained as a label; visual-only details unavailable: Simple service-level identity forwarding
- Illustration retained as a label; visual-only details unavailable: Token exchange
- Illustration retained as a label; visual-only details unavailable: Protocol-agnostic identity propagation

### owasp / Infrastructure_as_Code_Security_Cheat_Sheet

- External attachment reference retained as plain text: NIST SP 800-218: Secure Software Development Framework

### owasp / Injection_Prevention_Cheat_Sheet

- Unavailable cross-reference retained as plain text: Injection Prevention in Java

### owasp / Key_Management_Cheat_Sheet

- External attachment reference retained as plain text: Commercial National Security Algorithm Suite 2.0
- External attachment reference retained as plain text: CA/Browser Forum Baseline Requirements §6.3.2

### owasp / Legacy_Application_Management_Cheat_Sheet

- External attachment reference retained as plain text: Prioritize remediation

### owasp / Logging_Cheat_Sheet

- Illustration retained as a label; visual-only details unavailable: MIDDLEWARE

### owasp / Logging_Vocabulary_Cheat_Sheet

- Illustration retained as a label; visual-only details unavailable: IBM Cost of Data Breach Report 2025
- External attachment reference retained as plain text: information-retention requirements (NIST SI-12)
- External attachment reference retained as plain text: securely dispose of it and retained copies when no longer needed (SI-12(3))

### owasp / Microservices_based_Security_Arch_Doc_Cheat_Sheet

- External attachment reference retained as plain text: NIST SP 800-204: Security Strategies for Microservices-based Application Systems

### owasp / Microservices_Security_Cheat_Sheet

- Illustration retained as a label; visual-only details unavailable: Signed ID propagation
- Illustration retained as a label; visual-only details unavailable: Logging pattern
- External attachment reference retained as plain text: NIST SP 800-204: Security Strategies for Microservices-Based Application Systems
- External attachment reference retained as plain text: NIST SP 800-204A: Building Secure Microservices-Based Applications Using Service-Mesh Architecture

### owasp / Multifactor_Authentication_Cheat_Sheet

- External attachment reference retained as plain text: CISA's Implementing Phishing-Resistant MFA

### owasp / Network_Segmentation_Cheat_Sheet

- Illustration retained as a label; visual-only details unavailable: Schematic symbols
- Illustration retained as a label; visual-only details unavailable: Traffic passes through two firewalls
- Illustration retained as a label; visual-only details unavailable: Traffic passes through one firewall
- Illustration retained as a label; visual-only details unavailable: FRONTEND
- Illustration retained as a label; visual-only details unavailable: MIDDLEWARE
- Illustration retained as a label; visual-only details unavailable: BACKEND
- Illustration retained as a label; visual-only details unavailable: Interservice interaction
- Illustration retained as a label; visual-only details unavailable: Prohibited Interservice Communication
- Illustration retained as a label; visual-only details unavailable: Interservice Communication with balancing
- Illustration retained as a label; visual-only details unavailable: CI-CD
- Illustration retained as a label; visual-only details unavailable: Logging
- Illustration retained as a label; visual-only details unavailable: Zabbix-Example

### owasp / Nodejs_Security_Cheat_Sheet

- Unavailable cross-reference retained as plain text: A-Z

### owasp / OS_Command_Injection_Defense_Cheat_Sheet

- Illustration retained as a label; visual-only details unavailable: CommandInjection

### owasp / Passkey_Security_Cheat_Sheet

- Illustration retained as a label; visual-only details unavailable: Registration ceremony sequence diagram
- Illustration retained as a label; visual-only details unavailable: Authentication ceremony sequence diagram

### owasp / Password_Storage_Cheat_Sheet

- External attachment reference retained as plain text: scrypt

### owasp / Pinning_Cheat_Sheet

- Unavailable cross-reference retained as plain text: .Net sample program
- Unavailable cross-reference retained as plain text: OpenSSL sample program

### owasp / REST_Security_Cheat_Sheet

- Unavailable cross-reference retained as plain text: FastAPI Security Cheat Sheet
- Unavailable cross-reference retained as plain text: Bypassing Web Authentication and Authorization with HTTP Verb Tampering

### owasp / Secrets_Management_Cheat_Sheet

- External attachment reference retained as plain text: Rowhammer
- Unavailable cross-reference retained as plain text: Kubernetes Security Cheat Sheet
- External attachment reference retained as plain text: Commercial National Security Algorithm Suite 2.0

### owasp / Secure_Cloud_Architecture_Cheat_Sheet

- Illustration retained as a label; visual-only details unavailable: VPC Diagram
- Illustration retained as a label; visual-only details unavailable: Trust Boundaries
- Illustration retained as a label; visual-only details unavailable: No Trust Across Boundaries
- Illustration retained as a label; visual-only details unavailable: Complete Trust Across Boundaries
- Illustration retained as a label; visual-only details unavailable: Some Trust Across Boundaries
- Illustration retained as a label; visual-only details unavailable: Shared Responsibility Model
- External attachment reference retained as plain text: CISA Cyber Risk Assessment
- External attachment reference retained as plain text: CISA's Zero Trust Maturity Model
- External attachment reference retained as plain text: CISA: Cloud Security Technical Reference Architecture
- External attachment reference retained as plain text: CISA: Zero Trust Maturity Model Version 2.0

### owasp / Secure_Code_Review_Cheat_Sheet

- Unavailable cross-reference retained as plain text: Password Storage Cheat Sheet
- Unavailable cross-reference retained as plain text: Multifactor Authentication Cheat Sheet
- Unavailable cross-reference retained as plain text: Forgot Password Cheat Sheet
- Unavailable cross-reference retained as plain text: Insecure Direct Object Reference Prevention Cheat Sheet
- Unavailable cross-reference retained as plain text: Key Management Cheat Sheet
- External attachment reference retained as plain text: cipher block chaining (CBC) requires unpredictable IVs
- External attachment reference retained as plain text: Galois/Counter Mode (GCM) requires IV uniqueness under each key
- Unavailable cross-reference retained as plain text: Denial of Service Cheat Sheet
- Unavailable cross-reference retained as plain text: Secrets Management Cheat Sheet
- Unavailable cross-reference retained as plain text: Error Handling Cheat Sheet
- Unavailable cross-reference retained as plain text: Logging Cheat Sheet
- Unavailable cross-reference retained as plain text: HTTP Headers Cheat Sheet
- Unavailable cross-reference retained as plain text: Transport Layer Security Cheat Sheet
- Unavailable cross-reference retained as plain text: Vulnerable Dependency Management Cheat Sheet
- External attachment reference retained as plain text: OWASP Code Review Guide, Version 2
- External attachment reference retained as plain text: NIST SP 800-218: Secure Software Development Framework (SSDF) Version 1.1

### owasp / Secure_Coding_with_AI_Cheat_Sheet

- External attachment reference retained as plain text: NIST SP 800-218: Secure Software Development Framework

### owasp / Secure_Product_Design_Cheat_Sheet

- External attachment reference retained as plain text: NIST SP 800-218: Secure Software Development Framework (SSDF) Version 1.1

### owasp / Security_Terminology_Cheat_Sheet

- External attachment reference retained as plain text: message-processing rules

### owasp / Server_Side_Request_Forgery_Prevention_Cheat_Sheet

- Illustration retained as a label; visual-only details unavailable: SSRF Common Flow
- Illustration retained as a label; visual-only details unavailable: Case 1 for Network layer protection about flows that we want to prevent
- Unavailable cross-reference retained as plain text: talk
- Unavailable cross-reference retained as plain text: document
- Unavailable cross-reference retained as plain text: case 1 for network layer protection about flows that we want to prevent

### owasp / Session_Management_Cheat_Sheet

- Illustration retained as a label; visual-only details unavailable: SessionDiagram
- External attachment reference retained as plain text: session fixation attacks
- External attachment reference retained as plain text: here

### owasp / Software_Supply_Chain_Security_Cheat_Sheet

- External attachment reference retained as plain text: NIST
- External attachment reference retained as plain text: NIST SP 800-204D: Strategies for the Integration of Software Supply Chain Security in DevSecOps CI/CD Pipelines

### owasp / Third_Party_Javascript_Management_Cheat_Sheet

- External attachment reference retained as plain text: RAST

### owasp / Third_Party_Payment_Gateway_Integration_Cheat_Sheet

- Illustration retained as a label; visual-only details unavailable: Payment integration sequence flow

### owasp / Threat_Modeling_Cheat_Sheet

- External attachment reference retained as plain text: NIST SP 800-218, practice PW.1.2
- External attachment reference retained as plain text: NIST SP 800-218: Secure Software Development Framework

### owasp / Transaction_Authorization_Cheat_Sheet

- External attachment reference retained as plain text: here
- External attachment reference retained as plain text: Drimer, Murdoch and Anderson: Optimised to Fail — Card Readers for Online Banking
- External attachment reference retained as plain text: Peotta et al.: A Formal Classification of Internet Banking Attacks and Vulnerabilities
- External attachment reference retained as plain text: Blom et al.: Designed to Fail — A USB-Connected Reader for Online Banking

### owasp / Transport_Layer_Security_Cheat_Sheet

- External attachment reference retained as plain text: PCI DSS
- External attachment reference retained as plain text: NIST SP 800-57
- External attachment reference retained as plain text: SP 800-52 Rev. 2 Guidelines for the Selection, Configuration, and Use of Transport Layer Security (TLS) Implementations

### owasp / User_Privacy_Protection_Cheat_Sheet

- External attachment reference retained as plain text: Securing the SSL channel against man-in-the-middle attacks: Future technologies - HTTP Strict Transport Security and Pinning of Certs, by Tobias Gondrom

### owasp / Vulnerability_Disclosure_Cheat_Sheet

- External attachment reference retained as plain text: CERT: Guide to Coordinated Vulnerability Disclosure

### owasp / Webhook_Security_Cheat_Sheet

- Unavailable cross-reference retained as plain text: Query Parameterization Cheat Sheet
- Unavailable cross-reference retained as plain text: Cross Site Scripting Prevention Cheat Sheet

### owasp / XML_External_Entity_Prevention_Cheat_Sheet

- External attachment reference retained as plain text: Morgan and Al Ibrahim: XML Schema, DTD, and Entity Attacks

### owasp / XS_Leaks_Cheat_Sheet

- Illustration retained as a label; visual-only details unavailable: XS Leaks Attack Vector
- Illustration retained as a label; visual-only details unavailable: XS Leaks eTLD explanation
- Illustration retained as a label; visual-only details unavailable: XS-Leaks-ID
- Illustration retained as a label; visual-only details unavailable: XS Leaks Sec-Fetch-Dest
- Illustration retained as a label; visual-only details unavailable: XS-Leaks-Frame-Counting
- Illustration retained as a label; visual-only details unavailable: XS Leaks Cache Attack

### owasp / XSS_Filter_Evasion_Cheat_Sheet

- Unavailable cross-reference retained as plain text: 'al\u0065rt'
- Unavailable cross-reference retained as plain text: "al"+"ert"
- Unavailable cross-reference retained as plain text: /al/.source+/ert/.source
- Unavailable cross-reference retained as plain text: 'al\145rt'
- Unavailable cross-reference retained as plain text: 'al\x65rt'
- Unavailable cross-reference retained as plain text: 8680439..toString(30)
- Illustration retained as a label; visual-only details unavailable: Illustration

### owasp / Zero_Trust_Architecture_Cheat_Sheet

- External attachment reference retained as plain text: NIST SP 800-207, Section 2.1
- External attachment reference retained as plain text: NIST SP 800-207, Section 6.2
- External attachment reference retained as plain text: OMB M-22-09
- External attachment reference retained as plain text: NIST SP 800-207, Section 7
- External attachment reference retained as plain text: CISA Zero Trust Maturity Model v2.0
- External attachment reference retained as plain text: NIST SP 800-207, Section 7.3
- Unavailable cross-reference retained as plain text: Kubernetes policy management
- External attachment reference retained as plain text: NIST SP 800-207: Zero Trust Architecture
- External attachment reference retained as plain text: CISA: Zero Trust Maturity Model, Version 2.0

### top10 / 0x00_2025-Introduction

- Illustration retained as a label; visual-only details unavailable: Mapping

### top10 / 0x01_2025-About_OWASP

- Illustration retained as a label; visual-only details unavailable: License

### top10 / 0x02_2025-What_are_Application_Security_Risks

- Illustration retained as a label; visual-only details unavailable: Calculation diagram

### top10 / A02_2025-Security_Misconfiguration

- Unavailable cross-reference retained as plain text: A03 Software Supply Chain Failures

### top10 / A03_2025-Software_Supply_Chain_Failures

- External attachment reference retained as plain text: SAFECode Software Integrity Controls (PDF)

### top10 / A06_2025-Insecure_Design

- Unavailable cross-reference retained as plain text: Establish a Modern AppSec Program: Planning and Design Phase

### top10 / A08_2025-Software_or_Data_Integrity_Failures

- External attachment reference retained as plain text: SAFECode Software Integrity Controls

### top10 / A09_2025-Security_Logging_and_Alerting_Failures

- Unavailable cross-reference retained as plain text: A01:2025-Broken Access Control

### auth / email-address-verification-codes

- A reference outside the included Markdown collection was retained as plain text: Email code authentication

### auth / sessions

- A reference outside the included Markdown collection was retained as plain text: browser client-side storage

### auth / webauthn

- A reference outside the included Markdown collection was retained as plain text: WebAuthn authenticator data
- A reference outside the included Markdown collection was retained as plain text: WebAuthn client data

### auth / webauthn-authenticator-data

- A reference outside the included Markdown collection was retained as plain text: RSA signature scheme
- A reference outside the included Markdown collection was retained as plain text: WebAuthn CBOR encoding

### academy / access-control-security-models

- OCR prose complete by segment count; literal code, diagrams and table relationships require contextual review.

### academy / access-control-vulnerabilities-and-privilege-escalation

- OCR prose complete by segment count; literal code, diagrams and table relationships require contextual review.

### academy / advanced-request-smuggling

- OCR prose complete by segment count; literal code, diagrams and table relationships require contextual review.

### academy / algorithm-confusion-attacks

- An empty text segment requires review: 5
- OCR prose complete by segment count; literal code, diagrams and table relationships require contextual review.

### academy / authentication-vulnerabilities

- An empty text segment requires review: 4
- OCR prose complete by segment count; literal code, diagrams and table relationships require contextual review.

### academy / blind-sql-injection

- An empty text segment requires review: 5
- OCR prose complete by segment count; literal code, diagrams and table relationships require contextual review.

### academy / blind-ssrf-vulnerabilities

- An empty text segment requires review: 3
- OCR prose complete by segment count; literal code, diagrams and table relationships require contextual review.

### academy / browser-powered-request-smuggling

- An empty text segment requires review: 3
- OCR prose complete by segment count; literal code, diagrams and table relationships require contextual review.

### academy / business-logic-vulnerabilities

- An empty text segment requires review: 4
- OCR prose complete by segment count; literal code, diagrams and table relationships require contextual review.

### academy / bypassing-csrf-token-validation

- An empty text segment requires review: 4
- OCR prose complete by segment count; literal code, diagrams and table relationships require contextual review.

### academy / bypassing-referer-based-csrf-defenses

- An empty text segment requires review: 3
- OCR prose complete by segment count; literal code, diagrams and table relationships require contextual review.

### academy / bypassing-samesite-cookie-restrictions

- OCR prose complete by segment count; literal code, diagrams and table relationships require contextual review.

### academy / cl-0-request-smuggling

- OCR prose complete by segment count; literal code, diagrams and table relationships require contextual review.

### academy / clickjacking-ui-redressing

- OCR prose complete by segment count; literal code, diagrams and table relationships require contextual review.

### academy / client-side-desync-attacks

- An empty text segment requires review: 7
- OCR prose complete by segment count; literal code, diagrams and table relationships require contextual review.

### academy / client-side-template-injection

- OCR prose complete by segment count; literal code, diagrams and table relationships require contextual review.

### academy / client-side-prototype-pollution-vulnerabilities

- An empty text segment requires review: 4
- OCR prose complete by segment count; literal code, diagrams and table relationships require contextual review.

### academy / content-security-policy

- OCR prose complete by segment count; literal code, diagrams and table relationships require contextual review.

### academy / controlling-the-web-message-source

- OCR prose complete by segment count; literal code, diagrams and table relationships require contextual review.

### academy / cors-and-the-access-control-allow-origin-response-header

- OCR prose complete by segment count; literal code, diagrams and table relationships require contextual review.

### academy / cross-origin-resource-sharing-cors

- OCR prose complete by segment count; literal code, diagrams and table relationships require contextual review.

### academy / cross-site-request-forgery-csrf

- An empty text segment requires review: 5
- OCR prose complete by segment count; literal code, diagrams and table relationships require contextual review.

### academy / cross-site-scripting-xss-cheat-sheet

- OCR prose complete by segment count; literal code, diagrams and table relationships require contextual review.

### academy / cross-site-scripting-contexts

- OCR prose complete by segment count; literal code, diagrams and table relationships require contextual review.

### academy / cross-site-scripting

- OCR prose complete by segment count; literal code, diagrams and table relationships require contextual review.

### academy / cross-site-websocket-hijacking

- OCR prose complete by segment count; literal code, diagrams and table relationships require contextual review.

### academy / dangling-markup-injection

- An empty text segment requires review: 3
- OCR prose complete by segment count; literal code, diagrams and table relationships require contextual review.

### academy / dom-clobbering

- OCR prose complete by segment count; literal code, diagrams and table relationships require contextual review.

### academy / dom-based-ajax-request-header-manipulation

- OCR prose complete by segment count; literal code, diagrams and table relationships require contextual review.

### academy / dom-based-client-side-json-injection

- OCR prose complete by segment count; literal code, diagrams and table relationships require contextual review.

### academy / dom-based-client-side-sql-injection

- OCR prose complete by segment count; literal code, diagrams and table relationships require contextual review.

### academy / dom-based-client-side-xpath-injection

- OCR prose complete by segment count; literal code, diagrams and table relationships require contextual review.

### academy / dom-based-cookie-manipulation

- OCR prose complete by segment count; literal code, diagrams and table relationships require contextual review.

### academy / dom-based-denial-of-service

- OCR prose complete by segment count; literal code, diagrams and table relationships require contextual review.

### academy / dom-based-document-domain-manipulation

- OCR prose complete by segment count; literal code, diagrams and table relationships require contextual review.

### academy / dom-based-html5-storage-manipulation

- OCR prose complete by segment count; literal code, diagrams and table relationships require contextual review.

### academy / dom-based-javascript-injection

- OCR prose complete by segment count; literal code, diagrams and table relationships require contextual review.

### academy / dom-based-link-manipulation

- OCR prose complete by segment count; literal code, diagrams and table relationships require contextual review.

### academy / dom-based-local-file-path-manipulation

- OCR prose complete by segment count; literal code, diagrams and table relationships require contextual review.

### academy / dom-based-open-redirection

- OCR prose complete by segment count; literal code, diagrams and table relationships require contextual review.

### academy / dom-based-vulnerabilities

- OCR prose complete by segment count; literal code, diagrams and table relationships require contextual review.

### academy / dom-based-websocket-url-poisoning

- OCR prose complete by segment count; literal code, diagrams and table relationships require contextual review.

### academy / dom-based-xss

- An empty text segment requires review: 6
- OCR prose complete by segment count; literal code, diagrams and table relationships require contextual review.

### academy / dom-data-manipulation

- An empty text segment requires review: 3
- OCR prose complete by segment count; literal code, diagrams and table relationships require contextual review.

### academy / examining-the-database-in-sql-injection-attacks

- An empty text segment requires review: 3
- OCR prose complete by segment count; literal code, diagrams and table relationships require contextual review.

### academy / examples-of-business-logic-vulnerabilities

- OCR prose complete by segment count; literal code, diagrams and table relationships require contextual review.

### academy / exploiting-cache-design-flaws

- An empty text segment requires review: 5
- OCR prose complete by segment count; literal code, diagrams and table relationships require contextual review.

### academy / exploiting-cache-implementation-flaws

- An empty text segment requires review: 10
- OCR prose complete by segment count; literal code, diagrams and table relationships require contextual review.

### academy / exploiting-cross-site-scripting-vulnerabilities

- An empty text segment requires review: 3
- OCR prose complete by segment count; literal code, diagrams and table relationships require contextual review.

### academy / exploiting-http-request-smuggling-vulnerabilities

- OCR prose complete by segment count; literal code, diagrams and table relationships require contextual review.

### academy / exploiting-insecure-deserialization-vulnerabilities

- OCR prose complete by segment count; literal code, diagrams and table relationships require contextual review.

### academy / exploiting-server-side-template-injection-vulnerabilities

- OCR prose complete by segment count; literal code, diagrams and table relationships require contextual review.

### academy / file-upload-vulnerabilities

- OCR prose complete by segment count; literal code, diagrams and table relationships require contextual review.

### academy / finding-and-exploiting-blind-xxe-vulnerabilities

- An empty text segment requires review: 5
- OCR prose complete by segment count; literal code, diagrams and table relationships require contextual review.

### academy / finding-http-request-smuggling-vulnerabilities

- OCR prose complete by segment count; literal code, diagrams and table relationships require contextual review.

### academy / how-to-find-and-exploit-information-disclosure-vulnerabilities

- An empty text segment requires review: 6
- OCR prose complete by segment count; literal code, diagrams and table relationships require contextual review.

### academy / how-to-identify-and-exploit-http-host-header-vulnerabilities

- An empty text segment requires review: 8
- OCR prose complete by segment count; literal code, diagrams and table relationships require contextual review.

### academy / how-to-prevent-csrf-vulnerabilities

- OCR prose complete by segment count; literal code, diagrams and table relationships require contextual review.

### academy / how-to-prevent-oauth-authentication-vulnerabilities

- OCR prose complete by segment count; literal code, diagrams and table relationships require contextual review.

### academy / how-to-prevent-xss

- OCR prose complete by segment count; literal code, diagrams and table relationships require contextual review.

### academy / how-to-secure-your-authentication-mechanisms

- OCR prose complete by segment count; literal code, diagrams and table relationships require contextual review.

### academy / http-host-header-attacks

- OCR prose complete by segment count; literal code, diagrams and table relationships require contextual review.

### academy / http-request-smuggling

- OCR prose complete by segment count; literal code, diagrams and table relationships require contextual review.

### academy / http-request-tunnelling

- OCR prose complete by segment count; literal code, diagrams and table relationships require contextual review.

### academy / http-2-downgrading

- OCR prose complete by segment count; literal code, diagrams and table relationships require contextual review.

### academy / http-2-exclusive-vectors

- OCR prose complete by segment count; literal code, diagrams and table relationships require contextual review.

### academy / information-disclosure-vulnerabilities

- OCR prose complete by segment count; literal code, diagrams and table relationships require contextual review.

### academy / insecure-deserialization

- OCR prose complete by segment count; literal code, diagrams and table relationships require contextual review.

### academy / insecure-direct-object-references-idor

- An empty text segment requires review: 2
- OCR prose complete by segment count; literal code, diagrams and table relationships require contextual review.

### academy / javascript-prototypes-and-inheritance

- An empty text segment requires review: 5
- OCR prose complete by segment count; literal code, diagrams and table relationships require contextual review.

### academy / jwt-attacks

- OCR prose complete by segment count; literal code, diagrams and table relationships require contextual review.

### academy / oauth-2-0-authentication-vulnerabilities

- OCR prose complete by segment count; literal code, diagrams and table relationships require contextual review.

### academy / oauth-grant-types

- An empty text segment requires review: 6
- OCR prose complete by segment count; literal code, diagrams and table relationships require contextual review.

### academy / obfuscating-attacks-using-encodings

- OCR prose complete by segment count; literal code, diagrams and table relationships require contextual review.

### academy / openid-connect

- OCR prose complete by segment count; literal code, diagrams and table relationships require contextual review.

### academy / os-command-injection

- An empty text segment requires review: 2
- An empty text segment requires review: 6
- OCR prose complete by segment count; literal code, diagrams and table relationships require contextual review.

### academy / password-reset-poisoning

- OCR prose complete by segment count; literal code, diagrams and table relationships require contextual review.

### academy / path-traversal

- An empty text segment requires review: 5
- OCR prose complete by segment count; literal code, diagrams and table relationships require contextual review.

### academy / pause-based-desync-attacks

- OCR prose complete by segment count; literal code, diagrams and table relationships require contextual review.

### academy / preventing-prototype-pollution-vulnerabilities

- OCR prose complete by segment count; literal code, diagrams and table relationships require contextual review.

### academy / prototype-pollution

- OCR prose complete by segment count; literal code, diagrams and table relationships require contextual review.

### academy / prototype-pollution-via-browser-apis

- OCR prose complete by segment count; literal code, diagrams and table relationships require contextual review.

### academy / race-conditions

- OCR prose complete by segment count; literal code, diagrams and table relationships require contextual review.

### academy / reflected-xss

- An empty text segment requires review: 4
- OCR prose complete by segment count; literal code, diagrams and table relationships require contextual review.

### academy / response-queue-poisoning

- OCR prose complete by segment count; literal code, diagrams and table relationships require contextual review.

### academy / same-origin-policy-sop

- An empty text segment requires review: 3
- OCR prose complete by segment count; literal code, diagrams and table relationships require contextual review.

### academy / server-side-prototype-pollution

- An empty text segment requires review: 10
- OCR prose complete by segment count; literal code, diagrams and table relationships require contextual review.

### academy / server-side-request-forgery-ssrf

- An empty text segment requires review: 6
- OCR prose complete by segment count; literal code, diagrams and table relationships require contextual review.

### academy / server-side-template-injection

- OCR prose complete by segment count; literal code, diagrams and table relationships require contextual review.

### academy / sql-injection-cheat-sheet

- OCR prose complete by segment count; literal code, diagrams and table relationships require contextual review.

### academy / sql-injection-union-attacks

- OCR prose complete by segment count; literal code, diagrams and table relationships require contextual review.

### academy / sql-injection

- An empty text segment requires review: 2
- An empty text segment requires review: 8
- OCR prose complete by segment count; literal code, diagrams and table relationships require contextual review.

### academy / stored-xss

- OCR prose complete by segment count; literal code, diagrams and table relationships require contextual review.

### academy / testing-for-websockets-security-vulnerabilities

- An empty text segment requires review: 5
- OCR prose complete by segment count; literal code, diagrams and table relationships require contextual review.

### academy / vulnerabilities-in-multi-factor-authentication

- OCR prose complete by segment count; literal code, diagrams and table relationships require contextual review.

### academy / vulnerabilities-in-other-authentication-mechanisms

- OCR prose complete by segment count; literal code, diagrams and table relationships require contextual review.

### academy / vulnerabilities-in-password-based-login

- OCR prose complete by segment count; literal code, diagrams and table relationships require contextual review.

### academy / web-cache-poisoning

- An empty text segment requires review: 6
- OCR prose complete by segment count; literal code, diagrams and table relationships require contextual review.

### academy / web-message-manipulation

- An empty text segment requires review: 2
- OCR prose complete by segment count; literal code, diagrams and table relationships require contextual review.

### academy / what-are-websockets

- OCR prose complete by segment count; literal code, diagrams and table relationships require contextual review.

### academy / xml-entities

- OCR prose complete by segment count; literal code, diagrams and table relationships require contextual review.

### academy / xml-external-entity-xxe-injection

- OCR prose complete by segment count; literal code, diagrams and table relationships require contextual review.

### academy / xss-vs-csrf

- OCR prose complete by segment count; literal code, diagrams and table relationships require contextual review.

## Link normalization

10 original source links were normalized to an existing Markdown document or retained as plain labels when their target was unavailable. Authored guide links were validated without silently retargeting them.

| Source document | Original target | Outcome |
| --- | --- | --- |
| `auth/ecdsa.md` | `https://www.secg.org/sec1-v2.pdf` | Plain label; target not included |
| `auth/passkey-registration.md` | `https://www.secg.org/sec1-v2.pdf` | Plain label; target not included |
| `owasp/Authorization_Cheat_Sheet.md` | `https://nvlpubs.nist.gov/nistpubs/SpecialPublications/NIST.SP.800-162.pdf` | Plain label; target not included |
| `owasp/Authorization_Cheat_Sheet.md` | `https://raw.githubusercontent.com/OWASP/ASVS/v4.0.3/4.0/OWASP%20Application%20Security%20Verification%20Standard%204.0.3-en.pdf` | Plain label; target not included |
| `owasp/CI_CD_Security_Cheat_Sheet.md` | `https://www.cisa.gov/sites/default/files/publications/ESF_SECURING_THE_SOFTWARE_SUPPLY_CHAIN_DEVELOPERS.PDF` | Plain label; target not included |
| `owasp/Cross-Site_Request_Forgery_Prevention_Cheat_Sheet.md` | `http://www.acrossecurity.com/papers/session_fixation.pdf` | Plain label; target not included |
| `owasp/Microservices_based_Security_Arch_Doc_Cheat_Sheet.md` | `https://nvlpubs.nist.gov/nistpubs/SpecialPublications/NIST.SP.800-204.pdf` | Plain label; target not included |
| `owasp/Session_Management_Cheat_Sheet.md` | `https://media.blackhat.com/bh-eu-11/Raul_Siles/BlackHat_EU_2011_Siles_SAP_Session-WP.pdf` | Plain label; target not included |
| `owasp/GraphQL_Cheat_Sheet.md` | `Docker_Security_Cheat_Sheet.md#rule-7-limit-resources-memory-cpu-file-descriptors-processes-restarts` | Available document; unavailable fragment removed |
| `owasp/NodeJS_Docker_Cheat_Sheet.md` | `Docker_Security_Cheat_Sheet.md#rule-9-integrate-container-scanning-tools-into-your-cicd-pipeline` | Available document; unavailable fragment removed |
