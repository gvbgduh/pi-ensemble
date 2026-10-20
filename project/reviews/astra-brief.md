# Astra design-review brief

Status: work-report intake and design v1 review completed independently. See findings and separate author disposition; v1.1 amendments need only unresolved high-risk follow-up, not repeated full review. Operator acceptance pending. Astra is the live `pi-ensemble-001-adviser` session (`gpt-6-astra` per intercom roster), through the operator's subscription. Findings are not operator acceptance.

## Goal

Adversarially review a concrete design for a **functioning Pi Ensemble runtime**, not a research framework. Preferred direction: Pi Durable host with attachable clients; a Pi CLI adapter is an optional client. The operator should be able to attach to any persistent agent as naturally as switching tmux panes, without the terminal owning its lifetime.

## When to review

Prepare a proposal, capability/reuse matrix, evidence for high-risk assumptions, first implementation slice, and explicit unresolved questions. Prioritize experiments that can change the design; do not require a complete product merely to review its architecture. Flag the milestone and the material needed explicitly.

Initially another Pi session reviews shared versioned Markdown. Once Ensemble can safely run an independent reviewer and exchange artifacts, use it. Manual coordination is bootstrap, not a permanent requirement or an excuse to skip eventual self-hosting. Bounded spikes/docs need not wait for the full design gate; consequential architecture needs review and operator acceptance.

## Review substrate and spend

- Record the proposal Git revision. Author proposal, reviewer findings, and author dispositions are separate artifacts; no concurrent editing that erases independent viewpoints.
- Supply a concise decision summary, diagrams/contracts, assumptions, and evidence pointers. Reviewer follows primary evidence for disputed claims, not every historical transcript.
- Start with high thinking if supported for one focused architecture review. This is a heuristic, not a proven optimum; maximum thinking is not the default.
- Inspect actual usage/subscription constraints. Return only unresolved high-risk issues for another deep pass; editorial changes do not require repeated full reviews.
- Review feedback is not human approval. Preserve accepted risks, unresolved blockers, and rationale.

## Read

1. `README.md`, `docs/requirements.md`, and `docs/project-structure.md`.
2. `docs/design.md` and `project/implementation-plan.md` at the requested Git revision, with their acceptance gates.
3. `docs/pi-durable.md`, `docs/architecture-options.md`, `docs/context-lifecycle.md`, `docs/task-contract.md`, `docs/decision-models.md`, `docs/observability.md`.
4. `project/research/sources.md`, `durable-source-findings.md`, `ecosystem-source-findings.md`, and relevant findings. `project/reports/work-setup-insights.md` must be consumed in full (already done in EVIDENCE-REVIEW-01); retain `astra-work-report-intake-v1.md` caveats. No raw-corpus verification or runtime fault experiment has run.
5. `docs/reference/pi-setup-legacy.md` and sanitized skill contracts as needed.

Do not request raw private sessions or credentials. Missing public-safe evidence is a limitation to disclose, not a reason to export secrets.

## Review questions

### Lifetime, attachment, and reusable substrate

- Do task completion, logical agent identity, conversation/context, provider cache, OS process, and client attachment have distinct lifetimes?
- Can the human reattach to a finished agent, inspect activity/summary, ask why, and request adjustments without reconstructing intent from scratch?
- Is there one replay/admission authority, or a second scheduler/queue duplicating Durable? What did Cloudflare/community integrations teach, and which notes are stale?
- Is the local host portable and future-client-friendly without prematurely requiring cloud networking/sync?
- Can the operator select layouts/counts/roles without runtime code changes? Is a coordinator optional/configurable rather than structurally mandatory?

### Context wind-down and replacement — explicit operator-requested review

- Does the budget account for full model input (including cached tokens, prompt/tools, attachments, steering, and tool growth) plus output/control/handoff headroom? Are normal targets below 300K and ceilings no higher than 300–400K or the model's smaller usable limit?
- Are the proposal's illustrative early thresholds sensible given actual work traces? Can a writer checkpoint safely without abrupt interruption or a last-minute giant summarization?
- Is minimal overlap compatible with independent verification? Does the successor receive bounded, authoritative context rather than the entire old transcript or an unverified narrative?
- Should rollover use compaction, reset/handoff, or a new incarnation for each case? Which native primitives are verified, and which routing/ownership policy is application work?
- How is handoff fidelity checked, including hidden assumptions, rejected approaches, human steering, exact workspace state, approvals, children, and unresolved external effects?
- Can stable logical addresses reroute safely with one active owner? Does an epoch fence cover real mutations/in-flight tools, not only a database pointer?
- What happens when messages race cutover, an old handle submits, a late reply arrives, the driver changes, or the host crashes on either side of activation? Are admission, completion, and model compliance kept distinct?
- Can a failed successor or rollback restore an old owner only after reconciling both incarnations' effects? Is the safe fallback a visible blocked state rather than two writers?
- Which deterministic/fake-provider fault tests and post-handoff correction metrics would falsify the design?

Treat [context lifecycle](../../docs/context-lifecycle.md) as a proposal to challenge, not a ratified implementation contract.

### Horizontal allocation and task-shaped continuation

- Does the driver distribute bounded working sets before an agent grows toward a million-token prompt, and keep its own dependency/outcome map compact?
- Are tasks self-contained enough through scope/acceptance and revisioned project-MD pointers without embedding the project history or hundreds of mandatory references?
- Is unfinished work a bounded continuation work item with verified progress, remaining criteria, unresolved effects/decisions, and lineage—not concatenated earlier summaries?
- Do rollover, splitting, and reassignment have distinct identity/routing transitions? Can task assignments and result routes remain unambiguous when several workers share a role?
- Do stale docs, concurrent steering, late replies, failed handoffs, and writer leases prevent unsafe dual ownership or duplicate effects across horizontal workers?
- Is more parallelism justified by task independence, context distribution, and verification capacity, rather than agent count alone? What measured overhead or lost context would falsify the approach?

- Can a shared continuation map offer a small common core plus per-agent focus, with deterministic scoped delivery instead of all agents reading everything or an LLM copying summaries? Who owns aggregate updates, and how do human edits/revision drift reconcile with operational task state?
- Can task-result/continuation-ready/replacement-requested events trigger safe driver-notified provisioning and rerouting without manual spin-up or visible session counters? Are internal identity/epochs, authority limits, and anti-loop controls preserved?

Review [task contract](../../docs/task-contract.md) together with rollover, not as an unrelated future feature.

### Direct steering without drift

- When the operator redirects an agent, how does the driving agent learn changed intent/scope and affected work durably, without becoming a relay or approval bottleneck?
- What revision/correlation/receipt rules prevent lost notifications, echo loops, reordered steering, or a stale driver dispatching contradictory work?
- What happens if the driver is offline, changed, busy, or itself steered? Are multiple workstream drivers and contradictory human inputs handled?
- Which dependent work is paused, invalidated, or reconciled? Can the system distinguish acknowledgment from model compliance?

### Autonomy and uncertainty

- What may an agent resolve from evidence/authority itself, and what requires a decision owner? Does it ask too often, too late, or not at all?
- Can missing requirements, conflicting instructions, and consequential alternatives be surfaced with evidence/options/impact before expensive work relies on an assumption?
- Is a decision request durable and attributable, with blocked scope, owner, resolution, and revision? Do timeout/restart/late reply avoid manufacturing approval?
- Can independent work proceed while affected work waits? Does the design optimize for preserving useful work rather than cache warmth or activity for its own sake?
- How are throwaway work, drift, late clarification, and human correction observed without equating all exploration/waiting with waste?

### Effects, authority, and consistency

- Can shell/scripts/nested calls/delegation/peers bypass capabilities or human approval? Are host credentials isolated from tools/clients/telemetry?
- Where can an external effect succeed without a receipt, and how is that reconciled instead of blindly replayed?
- Are task ownership, conversation lifetime, workspace writer ownership, and decision authority distinct? Are stop/pause/background callback semantics enforceable?
- What is authoritative when durable state, Markdown, Git, and remote services disagree? Are atomicity/replay claims realistic?
- Are concurrency, budgets, upgrades/migrations, parked work, and rollback explicit and testable?

### Jev and portable measurement

- Jev via direct TypeSafe is selected. Does the design use it meaningfully through native/codemode/host paths without a compulsory classifier gate on every action?
- Does a reusable event core correlate question/rubric/input revision, prediction, action, override, later independent outcome, usage/attempts, and cost provenance?
- Can adapters support ordinary Pi and Durable without pretending to observe uninstrumented calls or know ground truth automatically?
- Are unknown cost, absent labels, disagreement, interruptions, and telemetry failure represented honestly? Do retries count charges without duplicate logical events?
- Are safety authorization and deterministic verification kept separate from probabilistic advice?
- Can LLM-authored questions/criteria or selective input evidence encode the desired answer and make Jev mirror the caller? Are neutral versioned rubrics, observation/interpretation separation, counterevidence/unknowns, caller-baseline blinding, and independent labels practical and enforced where needed?
- Do controlled paraphrase/polarity/label tests reveal unjustified framing sensitivity without confusing changed semantics with bias? Are ad hoc prompts marked unvalidated and unfavorable calls preserved rather than reworded/cherry-picked?

### Evidence and delivery

- What work-session/skill evidence supports the requirements, and where are corpus bias, contradictory cases, or unknown outcomes hidden?
- Does latest `pi-subagents` retained resume satisfy some historical pain without a rebuild? Is a new host still justified for the integrated experience?
- Is the first vertical slice small, usable, and recovery-tested, or is the proposal spending months on generic infrastructure?
- Are current docs and dynamic tracker/history separated with useful role-specific reading/update rules?

## Requested output

Prioritized findings with requirement IDs, concrete failure scenario, evidence, severity/blocking status, and smallest corrective test/change. Separate observed defects from assumptions and preferences. Include what is sound, what is unproven, and what can be deferred without undermining the first slice. Record reviewed revision, unresolved assumptions, and recommendation; leave operator acceptance explicit.
