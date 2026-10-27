# Authorization and tenant isolation

Use when reading, changing, listing, exporting, caching, subscribing to, or delegating access to protected resources.

## Enforcement

Derive identity from validated authentication. Derive tenant membership and capabilities from server-trusted state. A tenant ID in a URL, header, token claim, or request body is not sufficient until the application's membership and issuer rules have validated it. Decide authorization for the actor, action, resource, tenant, and relevant workflow state.

Constrain database access by authorized tenant and resource together, including bulk operations, nested relations, files, aggregates, exports, background jobs, and WebSocket messages. Parameterization and unpredictable IDs do not provide authorization. Central guards help only if every entry point uses them and the checked object is the one ultimately accessed. A platform or database policy can enforce this boundary; verify it before duplicating controls.

Keep identity-provider administration and application roles distinct. Protect permission changes and recovery paths from self-escalation. Reject privilege-bearing mass-assigned fields; explicitly select writable fields. When permissions can change during a sensitive mutation, ensure the authorization and write are bound to a consistent database operation or equivalent enforced policy.

## Caches and asynchronous work

Include the relevant principal, tenant, resource, and permission context in shared cache boundaries. Isolate client query caches across account or tenant switches; do not solve stale permission state by lengthening cache lifetimes. Sensitive HTTP responses need a deliberate caching policy, including CDN behavior.

Carry authenticated tenant context explicitly into jobs; verify authorization at the time the operation's policy requires. Do not trust a queued caller-provided tenant or infer tenant identity from a process-global variable. Prevent sensitive data from leaking in error responses and audit logs. Preserve existing 403/404 conventions unless changing their disclosure behavior is part of the task.

## Verification

Use two users in one tenant, another tenant, an unauthenticated caller, and a revoked role. Test reads and writes, nested objects, alternate methods, list totals, cache hits, exports, and batch endpoints. A denied operation must not mutate data or trigger a side effect. Verify allowed access still works and avoid reporting intended public data as an IDOR.

Read: [authorization](sources/owasp/Authorization_Cheat_Sheet.md), [access control](sources/owasp/Access_Control_Cheat_Sheet.md), [tenant isolation](sources/owasp/Multi_Tenant_Security_Cheat_Sheet.md), and [IDOR](sources/owasp/Insecure_Direct_Object_Reference_Prevention_Cheat_Sheet.md).
