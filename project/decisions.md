# Decision register

## Index

D-001 research-before-selection (refined by D-008); D-002 public/private separation; D-003 name; D-004 initial scope; D-005 bootstrap review gate (superseded by D-010); D-006 documentation/tracker separation; D-007 MIT; D-008 runtime direction; D-009 Jev adoption; D-010 review workflow; D-011 latest baseline and frequent milestones; D-012 bounded/distributed context and explicit rollover review; D-013 GitHub PR terminology.

## Accepted

- **D-001 — Research before runtime selection.** Start with a documentation-only scaffold. Do not choose Pi CLI extension, coding-agent SDK application, Pi Durable host, or hybrid until the relevant behavior is researched and reviewed.
- **D-002 — Public/private separation.** Publish sanitized workflow material only. Keep the original export in ignored local storage. Never publish runtime state, credentials, or global memory.

- **D-003 — Project identity.** The operator selected **Pi Ensemble**, repository name `pi-ensemble`.
- **D-004 — Initial scope.** Local-first and portable between laptops, with a future-compatible host/client boundary. The UI is not prescribed. Persistent direct agent access after task completion and operator-selected role layouts are requirements.
- **D-005 — Explicit independent review gate.** Use a separate Astra Pi session and versioned Markdown artifacts initially. Explicitly announce readiness for review before product implementation; no review or architecture approval has happened yet.

- **D-006 — Current knowledge versus project history.** `docs/` owns the current best product account; `project/` owns dynamic tracking/evidence/history. Both evolve. Keep README/AGENTS/current docs and milestone tracking synchronized; use selective reading. See `docs/project-structure.md`.
- **D-007 — MIT license.** Accepted by the operator; `LICENSE` is authoritative.
- **D-008 — Deliver an attachable runtime.** C (Durable host with attachable clients) is the preferred product direction, with D as an optional client layer. Refines D-001: research serves the functioning runtime; detailed architecture still requires evidence/review. No fixed agent-count/topology.
- **D-009 — Jev selected.** Direct TypeSafe AI access, including codemode; use actively and measure decisions/outcomes through a portable boundary. Operator-reported rate $0.042/M input and zero output price. Adoption is settled; secure provisioning, billing provenance, and instrumentation remain implementation work.
- **D-010 — Bootstrap then self-host review coordination.** Supersedes the procedural wording of D-005, not its independent-review intent. Highlight the design review milestone; initially another Pi session via OpenAI subscription uses versioned artifacts. Later use Ensemble when safe. Review consequential architecture, not every routine documentation change or bounded spike.
- **D-011 — Latest baseline and frequent coherent commits.** Compare latest legacy packages at pinned inspected revisions; historical work versions are evidence context, not a blocker. Commit/push reviewed milestones often and maintain the tracker in the same milestone.

- **D-012 — Bounded, distributed context.** Operator requests frugal contexts with minimal unnecessary overlap, an upper range of 300–400K tokens, early wind-down, successor briefing, and peer rerouting. Context replacement and routing safety must be reviewed explicitly with Astra. Detailed thresholds and the lifecycle proposal are not accepted defaults yet.
- **D-013 — GitHub PR map.** Use `project/prs.md`, not GitLab MR terminology; adapt tracker templates and navigation accordingly.

These establish direction and working contracts, not a reviewed detailed runtime architecture or claims of implementation.

## Architecture decision format

For each later decision record: status, context, requirements, considered alternatives, evidence and experiment results, trade-offs, failure modes, migration/rollback plan, reviewer findings, and the operator's acceptance.
