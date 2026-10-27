# Secure Development Skills

Three portable, independent skills for practical web development with deeper JavaScript and TypeScript guidance. Every bundled file is Markdown; using the skills requires no conversion tools, original document files, account connection, or network access to read their knowledge. Current version-specific implementation questions may still require checking official documentation.

## Choose a skill

- [Secure web development](skills/secure-web-development/SKILL.md): build and fix features at their actual security boundaries.
- [Web security review](skills/web-security-review/SKILL.md): inspect code and report supported findings. Read-only unless fixes are requested.
- [Authentication security](skills/authentication-security/SKILL.md): implement or review identity flows while preserving the existing provider.

## Install or use elsewhere

Copy the desired skill directory, including its `references` directory, into your agent's skill location. For repository-scoped Codex use, the conventional location is `.agents/skills/`. Each skill carries its own references and can be copied independently. Identical source copies across skills are intentional for that independence. No install or publication was performed by creating this bundle.

In tools that accept Agent Skills, retain `SKILL.md` and its frontmatter. In other tools, read the entrypoint and follow its relative Markdown references manually. Automatic invocation remains available through the skill descriptions where the host supports it. The bundle has no plugin manifest or host-specific metadata files.

## Example prompts

```text
Use $secure-web-development to add a tenant-scoped document export. Inspect our existing authorization and storage patterns and test access by another tenant.

Use $web-security-review to review this diff. Report confirmed security findings with file locations; do not modify code.

Use $authentication-security to fix our password-reset flow using the current identity provider. Check expiry, account binding, replay, and concurrent use.

Use $web-security-review to assess this raw HTML rendering. Check whether our existing sanitization actually protects the final sink before reporting XSS.
```

## Source quality and verification

The entrypoints load focused guides; converted source documents remain optional. Source text and workflow synthesis are clearly separated. Read [validation results](VALIDATION.md) before treating the fixture results as application assurance. Each skill's source catalog includes fingerprints, attribution, deferred topics, and known conversion limitations.
