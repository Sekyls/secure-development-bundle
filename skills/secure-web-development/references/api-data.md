# API inputs, data, and execution boundaries

Use when untrusted data reaches a query, parser, filesystem, process, template, or persistence boundary.

## Runtime contracts and queries

TypeScript types and casts disappear at runtime. Parse external input with the repository's runtime validation, constrain size and shape, and explicitly construct writable objects. Apply limits before expensive parsing or allocation. Validate authorization separately from syntactic correctness.

Bind SQL values through the actual driver or ORM parameter API. Identifiers, sort directions, and query fragments usually require an explicit application-controlled allowlist; interpolating those after validating only values remains unsafe. Check raw query escapes and ORM operators. For NoSQL, reject unexpected operator objects and construct the intended query rather than accepting a client query document.

## Files and interpreters

For paths, use an application-controlled storage root, map opaque IDs to stored objects where possible, and account for decoded traversal, canonicalization, symlinks, and race conditions. A string-prefix check without path-boundary handling is insufficient. For uploads, enforce quotas and size bounds, independently validate the permitted content, generate storage names, authorize retrieval, and control active content serving and execution. Antivirus is a possible extra control, not a substitute for isolation.

For process execution, prefer a direct executable and argument API over a shell string. That still requires preventing dangerous program flags and unintended file targets. For templates, pass data into a fixed template; do not build template source. Use safe serialization formats and avoid deserializing untrusted runtime objects. Disable external entities, external resource fetching, and unwanted DTD behavior in the actual XML parser, checking its version-specific options.

## Verification

Check normal and malicious values at the actual interpreter boundary: parameterized queries with quoted input, unsupported sort keys, nested operator objects, traversal after decoding, symlink escapes, disallowed upload content, shell metacharacters and flags, and external entity references. Demonstrate that data remains data. Match error handling and compatibility expectations; do not silently sanitize identifiers into a different resource.

Read: [input validation](sources/owasp/Input_Validation_Cheat_Sheet.md), [SQL injection](sources/owasp/SQL_Injection_Prevention_Cheat_Sheet.md), [mass assignment](sources/owasp/Mass_Assignment_Cheat_Sheet.md), [uploads](sources/owasp/File_Upload_Cheat_Sheet.md), [command execution](sources/owasp/OS_Command_Injection_Defense_Cheat_Sheet.md), and [XML](sources/owasp/XML_External_Entity_Prevention_Cheat_Sheet.md).
