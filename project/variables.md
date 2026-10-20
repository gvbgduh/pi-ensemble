# Open variables

| ID | Question / watch item | Status / next evidence |
| --- | --- | --- |
| V-001 | Exact Astra model ID and subscription authentication available to an embedded Durable host | Astra works via OpenAI subscription in Pi; verify host reuse without copying secrets or assuming API-key equivalence. |
| V-002 | Runtime/host/client protocol, agent lifetime, ownership, and upgrade boundaries | Preferred C, optional D; design v1 reviewed and v1.1 amends AD1–AD4. Operator acceptance and M0 native execution/admission proofs pending; no recovery experiments run. |
| V-003 | Secure Jev credential provisioning | Resolved for current Pi session: direct TypeSafe available and three synthetic codemode calls succeeded; see smoke findings. Durable-host credential reuse remains untested. No auth contents inspected; never put a key in Git/chat/argv. |
| V-004 | Cross-runtime decision/outcome instrumentation | Need shared schema/core and thin adapters; native usage totals are insufficient. See current observability doc. |
| V-005 | Empirical workflow requirements | Correct report received and read in full by orchestrator/Astra; includes sanitized skill behavior summaries, not executable bundle. See report intake for denominator/causal/ID caveats. Preserve chronology; only two final episodes reflect four-role setup. |
| V-006 | Paused/blocked work, ambiguity ownership, and steering notifications | Design explicit revision/correlation, affected-scope pause, decision requests, reconciliation, and stale-plan prevention. |
| V-007 | Source version drift | Use latest legacy packages as baseline, pin exact inspected revisions, and distinguish historical session behavior. Cloudflare notes may describe older Pi contracts. |
| V-008 | Full production chat-provider set, concurrency and autonomy envelope | Capture before provider/runtime integration; avoid fixed model identities and blanket spend assumptions. |
| V-009 | Context budgets and rollover primitive | Operator wants frugal distributed contexts, normally below 300K and no ceiling above 300–400K/smaller model window. Current bootstrap roster uses 272K ceiling with wind-down at 220–230K per operator. Runtime thresholds and compaction/reset/new-incarnation choice still need work evidence and Astra review. |
| V-010 | Handoff fidelity, routing cutover, pending work ownership | Validate logical address/epoch, mutation fences, in-flight/late messages/effects, successor readiness, failed activation/rollback. `docs/context-lifecycle.md` is a proposal, not implemented guarantees. |
| V-011 | Context-aware horizontal allocation and bounded continuation contract | Explicit work-item/assignment lineage and driver/result routes; estimate working sets before dispatch, minimize unnecessary overlap, validate project-MD pointers, and avoid recursive handoff accumulation. Proposed in `docs/task-contract.md`; Astra review and tests pending. |
| V-012 | Jev question-framing bias and independent signal | Neutral versioned rubrics, expected-answer blinding, evidence provenance/counterevidence, unknowns, controlled framing tests, and independent labels. Core risk is the classifier mirroring caller expectations; validation thresholds/tooling remain to design. |
| V-013 | Shared continuation map, scoped selection, and automatic spin-up | Common-plus-per-agent sections (or linked files) with deterministic host selection; aggregate edit authority/revision reconciliation; task-result/continuation/replacement signals and bounded automatic provisioning. No manual naming counters, but internal epochs/IDs retained. |

Resolved: project name, MIT license, local-first scope, UI flexibility, configurable topology, Jev adoption/provider. Their implementation details remain work, not reasons to reopen the decisions.
