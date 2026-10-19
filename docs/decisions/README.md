# Decision register

## Accepted

- **D-001 — Research before runtime selection.** Start with a documentation-only scaffold. Do not choose Pi CLI extension, coding-agent SDK application, Pi Durable host, or hybrid until the relevant behavior is researched and reviewed.
- **D-002 — Public/private separation.** Publish sanitized workflow material only. Keep the original export in ignored local storage. Never publish runtime state, credentials, or global memory.

- **D-003 — Project identity.** The operator selected **Pi Ensemble**, repository name `pi-ensemble`.
- **D-004 — Initial scope.** Local-first and portable between laptops, with a future-compatible host/client boundary. The UI is not prescribed. Persistent direct agent access after task completion and operator-selected role layouts are requirements.
- **D-005 — Explicit independent review gate.** Use a separate Astra Pi session and versioned Markdown artifacts initially. Explicitly announce readiness for review before product implementation; no review or architecture approval has happened yet.

These decisions establish name, scope, and process; no runtime architecture has been accepted.

## Architecture decision format

For each later decision record: status, context, requirements, considered alternatives, evidence and experiment results, trade-offs, failure modes, migration/rollback plan, reviewer findings, and the operator's acceptance.
