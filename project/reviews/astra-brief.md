# Astra design-review brief

Status: reusable review brief; no review has been requested or performed. Astra is used through the operator's OpenAI subscription inside Pi. Exact model launch ID is not assumed from the nickname.

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
2. Concrete proposal revision and acceptance criteria (to be written).
3. `docs/pi-durable.md`, `docs/architecture-options.md`, `docs/decision-models.md`, `docs/observability.md`.
4. `project/research/sources.md`, relevant findings/experiments, and any received work-setup report.
5. `docs/reference/pi-setup-legacy.md` and sanitized skill contracts as needed.

Do not request raw private sessions or credentials. Missing public-safe evidence is a limitation to disclose, not a reason to export secrets.

## Review questions

### Lifetime, attachment, and reusable substrate

- Do task completion, logical agent identity, conversation/context, provider cache, OS process, and client attachment have distinct lifetimes?
- Can the human reattach to a finished agent, inspect activity/summary, ask why, and request adjustments without reconstructing intent from scratch?
- Is there one replay/admission authority, or a second scheduler/queue duplicating Durable? What did Cloudflare/community integrations teach, and which notes are stale?
- Is the local host portable and future-client-friendly without prematurely requiring cloud networking/sync?
- Can the operator select layouts/counts/roles without runtime code changes? Is a coordinator optional/configurable rather than structurally mandatory?

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

### Evidence and delivery

- What work-session/skill evidence supports the requirements, and where are corpus bias, contradictory cases, or unknown outcomes hidden?
- Does latest `pi-subagents` retained resume satisfy some historical pain without a rebuild? Is a new host still justified for the integrated experience?
- Is the first vertical slice small, usable, and recovery-tested, or is the proposal spending months on generic infrastructure?
- Are current docs and dynamic tracker/history separated with useful role-specific reading/update rules?

## Requested output

Prioritized findings with requirement IDs, concrete failure scenario, evidence, severity/blocking status, and smallest corrective test/change. Separate observed defects from assumptions and preferences. Include what is sound, what is unproven, and what can be deferred without undermining the first slice. Record reviewed revision, unresolved assumptions, and recommendation; leave operator acceptance explicit.
