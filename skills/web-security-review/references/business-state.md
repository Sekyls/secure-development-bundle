# Business invariants and concurrency

Use for balances, inventory, approval workflows, one-time codes, credits, payments, and retryable side effects.

## State and atomicity

State the invariant before choosing a primitive: an account cannot spend more than its available balance; a reset token can be consumed once; an approval must follow the authorized transition. Recompute security-relevant prices, totals, roles, and workflow state on the server. Client-provided totals and hidden fields cannot establish these facts.

Check-and-update must be atomic across all workers that can act on the same state. Choose the database's conditional update, uniqueness constraint, row lock, transaction isolation, or an existing durable equivalent. Ensure the affected-row result is checked. A transaction does not automatically prevent every race; its isolation and predicate behavior matter. A JavaScript mutex protects only the processes that share it, not a distributed deployment.

## Retries and external effects

Scope idempotency to the actor/tenant and operation. Bind each key to the request's meaningful input, atomically reserve or record the operation, and reject reuse for a different request. Define what retries return while work is pending and after it completes. Avoid duplicate payment, email, or queue effects when a request times out after the effect occurred.

An external API call cannot usually be rolled back by a local database transaction. Use the provider's idempotency mechanism and the application's durable job/outbox or reconciliation pattern where applicable. Preserve auditability and avoid adding an elaborate workflow where a simple atomic database operation is enough.

## Verification

Issue synchronized concurrent requests against the actual persistence layer when testing the application, including separate connections or workers. Verify the invariant, permitted successes, and all side effects. Test replay after success, a timeout after an external effect, mismatched idempotency input, unauthorized actors, and skipped workflow steps. Do not infer distributed correctness from a single in-memory fixture.

Read: [business logic](sources/owasp/Business_Logic_Security_Cheat_Sheet.md), [transaction authorization](sources/owasp/Transaction_Authorization_Cheat_Sheet.md), and [race-condition mechanisms](source-index.md).
