# High-level implementation plan — proposal v1

Status: **for Astra review and operator acceptance**, not authorization to start consequential implementation. Companion [design v1](../docs/design.md). No runtime code/tests exist yet. Research completion below means bounded source inspection, not integration validation.

## Delivery principles and proposed layout

Deliver tested increments, not a generic orchestration framework. Prefer one TypeScript workspace with small internal boundaries initially: `src/host/` (Durable adapter/app documents/admission), `src/protocol/`, `src/client/` (terminal CLI), `src/policy/`, `src/execution/`, `src/measurement/`, and `test/{unit,integration,fault}/`. Split published packages only when the portable measurement core has a real second consumer. No mandatory service install, global Pi changes, cloud deploy, dashboard, browser app, or plugin ecosystem in the first slice.

Each milestone records exact dependency/source pins, commands/results, unrun cases, runtime capabilities, tracker updates, and rollback. User-facing labels must distinguish prototype/restricted/read-only/mutation-enabled. Fake-provider tests are deterministic; real-provider costs/latency are separate observations. Isolated worktrees and one aggregate documentation maintainer remain the development rule.

## Entry evidence and approval

Completed evidence intake: [official Durable matrix](research/durable-source-findings.md), [ecosystem comparison](research/ecosystem-source-findings.md), full [Argon report](reports/work-setup-insights.md), [qualified intake](findings/2026-10-19-work-report-intake.md), [Astra intake](reviews/astra-work-report-intake-v1.md), and authenticated [Jev smoke](findings/2026-10-19-jev-smoke.md). None establishes crash correctness or sandbox safety.

Before consequential implementation: obtain independent review against a committed proposal, write dispositions separately, resolve blocking design issues, and obtain explicit operator acceptance (including first supported platform and restricted initial capability). Bounded compatibility/fake-provider spikes may be authorized separately; review is not a claim tests already ran.

## Milestones and exit gates

| Milestone | Bounded deliverable | Required exit evidence | Not yet included |
| --- | --- | --- | --- |
| **M0 — Substrate and authority proof** | Select compatible released Pi Durable/Pi-AI/runtime/SQLite versions; lockfile; minimal Session with app docs + native conversation; fake provider; project-store lock adapter; native credential-resolution feasibility check without credential copying | E1/E9/E10-style tests: custom-doc transaction rollback, admission/native-submit crash windows and dedupe retention/conflict behavior, concurrent-open exclusion, close/reopen, missing task definition, failed storage. One existing subscription provider login works through supported host adapter or explicit documented blocker. License/adapter compatibility checked. | General shell, UI polish, automatic startup. If one-store bridge or subscription auth cannot be made sound, stop and revisit design before coding around it. |
| **M1 — Recovery-tested attachable peer slice** | Foreground `ensemble host`; private socket; `create/list/attach/status/send`; two configurable independent agents, explicit task/result messages, snapshot + bounded updates; transcript/context retained after completion; clean detach/reconnect | Fresh/reconnected clients observe committed state; duplicate commands don't duplicate inputs; payload-ID conflict rejected; crash before/after native admission preserves one logical operation; peer FYI/result obligations visible; missing local-prose-only result flagged; no client detach abort. Show real-provider follow-up after completion, with reported usage and safe secrets boundary. | External writes/general bash, arbitrary Pi extensions, rollover automation, unattended spawning, remote listener. This is a restricted usable host/client prototype, not safe autonomous coding yet. |
| **M2 — Steering, control and policy** | Correlated direct steering + driver/human-owner notices; decision requests; scope holds and acknowledgment/reconciliation; pause/stop and persisted control revision; startup offline-update gate; capability checks for all exposed entry paths | E2/E3/E7/E8/E13/E14: driver offline/change, concurrent steering, late messages after stop, blocked owner/timeout, command queue saturation, bounded peer cycles, unauthorized nested calls, offline Markdown/Git drift. Stop includes owned background scope; state accurately reports stopping/uncertain. | Shell privilege isolation not implied by tool allowlist; real mutation remains disabled. |
| **M3 — Safe bounded coding workspace** | Demonstrated sandbox adapter; one writer per worktree/branch; read-only/scratch reviewers; effect broker and revision-bound approvals; fake remote mutation then one explicit low-risk real action if separately authorized | E4/E5/E6 plus adversarial tests: shell/codemode/delegation bypass, symlink/path escape, home/credential/socket access, subprocess environment, network denial, non-cooperative child termination and exit verification, two writers, changed HEAD/approval, crash after effect before receipt. Unsupported target platforms fail closed for mutation. | No arbitrary production action/merge/publication authorization by default. No assertion that hooks/labels isolate same-user shell. |
| **M4 — Context-aware continuation and rollover** | Deterministic common+assigned focus; bounded task envelope; token/growth admission; explicit fresh incarnation; manifest validation/quiescence/CAS cutover; late-result reconciliation; controlled continuation provisioning | E16–E19/E21: small-window budget, large read rejected before exhaustion, handoff omission and changed revision, steering/injection racing cutover, crash each transition, stale process callback/tool, failed successor, task split/reassignment and aggregation. One mutation owner; no transcript cloning or recursion. | No transparent transfer of arbitrary live Durable tasks/tools between conversations. Such cases remain blocked until settled. Compaction optimization separately evaluated. |
| **M5 — Measurement and evaluated autonomy** | Shared event core + native codemode/Durable adapters; meaningful Jev attention-triage rubric and independent outcomes; bounded automatic provisioning; export/recovery handbook | E15/E20: consistent logical records across adapters, actual attempts counted, unknown costs/revisions, errors/outage, counterevidence and blinded baseline, paraphrase/order/polarity sensitivity with independent labels, coverage gaps visible. Same bounded task compared with single agent, current peers and Ensemble; intervention/rework/context metrics reported without causal overclaim. | No compulsory classifier safety authority, hidden paid fallback, global telemetry upload, claimed quality from agreement alone. |

Measurement IDs and audit-critical events begin at M0/M1, not retrofitted at M5. M5 completes reusable adapters/evaluation and autonomous policy; Jev may be used appropriately before then with explicit evidence/recording limits. Likewise bounded context admission starts in M1 even though automatic rollover arrives in M4: early prototype parks work before limits instead of overflowing.

## First implementation task after acceptance

**M0-01:** In a temporary isolated project, create the smallest fake-provider Durable Session + custom app-doc admission bridge. Pin a released dependency set. Prove a command receipt, pinned native request ID, injected crash before receipt, reopen and deduplicated settlement; attempt the same command ID with changed content. Prove exclusive host lock by concurrent process test. Output a tiny runnable test harness and findings naming every crash window. No model/provider calls, credentials, production files or service installation for this task. The separate real-auth feasibility test must use the native private resolver, redact failures and never print credential contents.

Acceptance owner: orchestrator with independent review of assertions; operator decides consequential scope changes. A failure is a useful result and blocks M1's relevant guarantees. Do not turn an unproven bridge into a runtime abstraction without tests.

## Test strategy and release gates

- Unit tests: canonical command IDs/digests, revisions/state transitions, routing and authorization, lease identities, handoff selection/caps, budget math, rubric/version records.
- Integration: actual selected Durable store/provider adapter and socket, two clients, native inbox settlement, snapshot resync, clean close/reopen.
- Fault harness: process kill at explicit injected hooks, torn/missing outputs, delayed/duplicate replies, fake service effect-before-receipt, blocked tools. Validate authoritative documents and observable client receipts, not just successful process exit.
- Adversarial execution: prove sandbox/credential boundaries on every supported OS; run mutation tests only in temporary repos. Test callbacks after cancellation and stale epochs at *effect* admission, not only model dispatch.
- Human acceptance: attach to any completed specialist and ask why/adjust; directly steer a worker and observe driver notification/hold/reconciliation; replace a draining worker without manual suffix bookkeeping or two writers.

The current research-plan E1–E21 list remains the comprehensive scenario catalog. Tests may combine scenarios, but each requirement release claim links a result or disclosed gap. Do not require all M5 features before demonstrating M1. Do not enable mutation before M3 gates merely to make a demo impressive.

## Migration, rollback and operational limits

Keep existing Pi sessions/intercom usable as the baseline. Import reviewed project pointers and a bounded summary into new logical agents, not full transcripts or raw workplace data. Do not share their active worktree/remote branch with Ensemble. No silent global settings change, legacy prompt cleanup, key migration or package replacement.

Private host directory holds the SQLite store, transport metadata, logs and backups; tracked repository contains only code/docs/tests/synthetic fixtures. Start/stop is foreground CLI until lifecycle semantics are proven; optional user service is a separate explicit action. Define shutdown/backup, supported filesystem assumptions and schema compatibility before release.

For an upgrade: stop/fence work, reconcile uncertain effects, make a tested backup, validate migrations/task definitions against a clone, then open. An old binary may not open a new schema. Reverting the database after external actions is unsafe; use forward reconciliation or park. Preserve export/read-only inspection even when execution is blocked.

## Explicit risks and decision owners

| Risk / open decision | Owner and next evidence |
| --- | --- |
| Released package/API drift versus pinned main research | Implementer + Astra; M0 release pin and tests, no assumed compatibility |
| Native subscription auth/resource reuse differs from coding-agent CLI | Host adapter spike; operator resolves acceptable provider/platform scope if unsupported |
| App outbox/native inbox atomicity, dedupe horizon, routing cutover | Astra review then M0/M4 fault tests; park uncertain admission rather than re-route blindly |
| Portable OS sandbox and real stale-process fencing | Execution spike + independent adversarial review; operator accepts supported platforms/capabilities |
| Steering scope cannot always be inferred | Conservatively hold assigned scope; operator/decision owner resolves ambiguity; no hidden LLM authorization |
| Handoff correctness and context estimates | M4 tests/metrics plus semantic independent checks; arbitrary exact token proof not claimed |
| Complexity overwhelms smallest useful product | M1 usability demo and phase gates; keep UI/remote/plugin breadth deferred |
| Empirical report numbers/causality remain unverified | Preserve original + caveats; do not block concrete scenario testing on retrospective statistical cleanup |

## Astra review request

Assess design v1 and this plan at a specific Git revision. Prioritize first-slice viability, replay/admission race windows, effective permission/fencing boundaries, context/handoff fidelity, steering reconciliation, portable auth/sandbox feasibility, and honest evidence/release gates. Return blocking findings with smallest corrective test/change and what is sound/deferable. Review is not human acceptance. Record independent findings and author disposition in separate versioned files.
