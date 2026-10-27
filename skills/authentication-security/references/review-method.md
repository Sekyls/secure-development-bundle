# Security review method and findings contract

## Bound the review

Identify whether the request concerns a diff, feature, subsystem, or whole repository. Read applicable repository instructions and inspect entry points, tests, dependencies, and relevant configuration. For a diff, follow enough surrounding code to understand exploitability without turning the assignment into a speculative whole-system audit. State material limits such as unavailable deployment configuration.

## Establish a finding

Trace attacker-controlled input or a missing authorization decision to a reachable protected operation or unsafe sink. Identify the actual trust boundary and the controls that run before it. A grep match, scanner warning, missing middleware name, or attack-family description is a lead, not a confirmed vulnerability. Examine provider, database, and infrastructure enforcement before claiming it is absent.

For each confirmed finding include: a short title and priority using the repository's convention; a precise file and tight line location; the trigger and attacker prerequisites; the concrete impact; why existing controls do not stop it; and the smallest useful remediation and regression scenario. Keep code locations and reproduction separate from the supporting Markdown knowledge citation.

Where a concern depends on unknown configuration, describe the exact assumption under an open question, not as a confirmed exploit. Avoid fabricated numerical risk scores. Deduplicate multiple symptoms of one root cause. A safe counterpart matters: explain why an existing guard, parameter API, escape boundary, or verifier blocks the alleged attack before filing it.

## Output and workflow

Lead with actionable findings in descending practical severity. Follow with unresolved assumptions and the tests or evidence actually examined. If no supported findings exist, say so and name the relevant verification limits. Do not imply a comprehensive assurance result from a narrow review.

Review mode does not edit code. If fixes were requested, preserve unrelated changes, make the scoped correction, and run relevant regression checks. Do not publish comments, run active probes against shared systems, or change deployments merely because a review skill is selected.

Read: [secure review source](sources/owasp/Secure_Code_Review_Cheat_Sheet.md), [attack surface](sources/owasp/Attack_Surface_Analysis_Cheat_Sheet.md), and the topic guide selected in the skill entrypoint.
