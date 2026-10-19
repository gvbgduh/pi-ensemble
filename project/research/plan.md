# Research and evidence plan

## Goal

Gather the evidence needed to deliver a functioning Pi Durable-based Ensemble host with attachable clients. C is preferred; D can be an optional client later. Preserve useful behavior in [requirements](../../docs/requirements.md), compare migration/reuse alternatives, and turn research into decisions and tested increments—not a permanent research project.

## Workstreams

### 1. Understand the durable substrate

- Finish normative-spec review and inspect relevant source/tests, not just README examples.
- Map conversations, task ownership, inboxes, steering/follow-ups, cancellation, recovery, definition/schema migrations, and compaction.
- Verify provider/subscription authentication, coding tools, codemode, and resource discovery versus ordinary Pi CLI.
- Select a compatible released dependency set; inspected `main` snapshots are not dependency pins.

### 2. Characterize latest legacy packages

- Use latest `nicobailon/pi-subagents` and `nicobailon/pi-intercom`; record exact inspected revisions.
- Inspect retained-child resume, peer discovery/addressing, delivery receipts, persistence/lifecycle, workspace isolation, and supervisor escalation.
- Build a matrix: desired behavior, existing behavior, Durable primitive, missing application policy, reuse/migration cost, acceptance test.
- Historical work versions explain evidence but do not block comparison. Persistent peers and bounded delegation remain distinct.

### 3. Learn from work and ecosystem implementations

- Use [the work-setup brief](work-setup-analysis-brief.md) to obtain public-safe evidence, including optional sanitized skills/templates.
- Inspect Cloudflare's SDK Pi harness and official example: execution ownership, host wake/lifetime, attach/reconnect, state projection, tools, skills, and tests.
- Request creator-shared example links or sanitized excerpts; private Slack is not accessible from here.
- Inspect selected community applications for persistent-agent identity, client protocol, operator control, and metrics. Distinguish observed code from marketing descriptions.
- Compare older integration notes with current Pi contracts before adopting limitations or workarounds.

### 4. Design the product boundary

Refine [architecture direction](../../docs/architecture-options.md). Specify host/client protocol, logical identity and lifetime, steering-update reconciliation, ambiguity/decision ownership, workspace/capability boundaries, provider access, and portable instrumentation. Scope the smallest useful recovery-tested implementation slice.

### 5. Risk-driven experiments

Proposed; **none have run yet**. Start with a fake provider, isolated temporary repositories, and a fake remote service. Each spike has a bounded question and result, not implied product readiness.

| ID | Experiment | Required observation |
| --- | --- | --- |
| E1 | Kill host after admission, before processing, and during a reply | Stable identities; no silent lost/duplicated application work. |
| E2 | Stop peer/project while reporters and queued messages exist | Late callbacks cannot silently restart stopped work. |
| E3 | Peer exchange while coordinator is idle | Delivery and bounded callbacks without mandatory central relay or loops. |
| E4 | Crash after external effect, before receipt | Ambiguity surfaced/reconciled, unsafe effect not blindly replayed. |
| E5 | Two workers request one checkout | Enforced writer ownership; crash/stale ownership handling. |
| E6 | Change approved action, then restart | Approval cannot authorize another revision/action. |
| E7 | Reach budget/concurrency limits | Defined admission/checkpoint boundaries include retries/background tasks. |
| E8 | Interrupt tracker update; remote state changes offline | Drift detected/reconciled without a fictitious distributed transaction. |
| E9 | Two hosts try one store | Host boundary prevents unsafe concurrent ownership. |
| E10 | Reopen with changed definitions | Unsupported migration is visible; upgrade/rollback policy explicit. |
| E11 | Direct explanation/adjustment after task completion and reconnect | Same logical agent and relevant context remain available. |
| E12 | Different operator-defined layouts | No fixed agent-count/topology assumptions. |
| E13 | Human redirects worker while dependent work is queued/running | Correlated durable driver update, plan reconciliation, stale work visible, no echo loop. |
| E14 | Consequential ambiguity with absent owner/timeout | Durable question blocks affected scope only; timeout is not consent. |
| E15 | Jev through Pi/codemode and Durable, then delayed labels | Comparable decision/action/outcome records; attempts and costs remain accurate. |
| E16 | Near-limit agent drains and briefs a replacement | Bounded verified handoff preserves decisions/work/authority without full-transcript inheritance; smaller model budgets and reserves work. |
| E17 | Messages, human steering, child results, and host crash race routing cutover | One active writer; no silent dropped/duplicated admission; stale handles/results remain attributable; failed activation/rollback blocks safely. |
| E18 | Bounded tasks and continuations distributed across independent workers | Project-MD pointers suffice without parent transcript inheritance; continuation size stays bounded over many passes; driver context stays small. |
| E19 | Split/reassign work while steering/results are in flight | Assignment-aware routes preserve acceptance lineage, aggregation, authority, and one mutation owner; no ambiguous worker broadcast or repeated effects. |
| E20 | Leading and neutral Jev questions, controlled paraphrase/polarity/label variants, labeled counterexamples | Caller expectation is not evidence; framing sensitivity and unknowns are visible; no favorable-result selection or spurious independent-validation claims. |

Prioritize experiments that could change the architecture; defer others explicitly rather than requiring a full product before its design review.

### 6. Jev integration and measurement

Follow [decision models](../../docs/decision-models.md) and [observability](../../docs/observability.md). Direct TypeSafe Jev is selected; use it after secure provisioning without reopening adoption. Native totals are insufficient: design a reusable recording core and adapters for supported paths. Local Laya is not required.

Context distribution, early wind-down, handoff validation, and stable-address rerouting are explicit Astra review topics, not solved by simply enabling compaction. Review the [lifecycle proposal](../../docs/context-lifecycle.md) and use work-session evidence to calibrate budgets and failure tests.

### 7. Independent review and delivery

**Highlight the Astra design-review milestone explicitly** when the proposal and high-risk evidence are ready. Use [the brief](../reviews/astra-brief.md), versioned findings/dispositions, and operator acceptance for consequential architecture. Bootstrap through another Pi session using the OpenAI subscription; use Ensemble itself when safe.

Then deliver the smallest functioning host/client slice, with direct follow-up, persistence, and failure/control tests. Expand through evidence-backed milestones. Review does not freeze ordinary docs or bounded spikes, and exact announcement wording is not a ritual.

## Evidence sufficient for the first design decision

- Each must-have has a proposed owner and evidence, or a disclosed gap.
- No invented compatibility, exactly-once external effects, or cross-process safety.
- Empirical work evidence is incorporated or explicitly pending with assumptions exposed.
- High-risk experiments have results or a reasoned deferral; do not claim they ran.
- Proposal includes alternatives, migration/rollback, risks, maintenance cost, acceptance criteria, and a concrete first implementation slice.
