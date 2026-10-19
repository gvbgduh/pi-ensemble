# Requirements

Status: current target, updated with operator clarifications on 2026-10-19. These describe the functioning runtime to deliver, not existing implementation guarantees. Work-session evidence will refine them.

## Role and interaction model

| ID | Requirement |
| --- | --- |
| R1 | Long-lived, project-specific ensembles. Worker, orchestrator, manager, and adviser are a starting layout, not mandatory roles. The operator chooses role types, counts, and responsibilities for the problem; each agent has an independent conversation and configurable model/provider. |
| R2 | Role responsibilities and authority are explicit and configurable. In the starting workflow, a driving agent owns coordination/project-scale state, workers implementation, a manager independent coherence/prose review, and an adviser evidence-based investigation. No particular role is mandatory. |
| R3 | Any authorized role can address another directly and return a result without the orchestrator relaying every message. |
| R4 | Human can attach/detach, inspect activity/logs and summaries, ask questions, give feedback, steer, pause, or stop any agent directly, including during active work. No mandatory orchestrator hop. The ordinary Pi TUI is not required; the host, not the client connection, owns agent lifetime. |
| R5 | Completing a work item must not destroy a persistent agent's identity/context: it remains addressable for explanation and adjustments. Bounded disposable child agents may also exist, but are distinct; lifetime, retention, completion, and cancellation semantics must be explicit. |
| R6 | Delegate using artifact pointers and compact outcomes; do not replicate every transcript into every context. |

## Project state and workspace

| ID | Requirement |
| --- | --- |
| R7 | Multi-repository initiatives and dependency/merge topology are first-class; a project is not one checkout. |
| R8 | Shared project knowledge remains human-readable and exportable: goals, progress, decisions, open questions, findings, change requests, and continuation state. |
| R9 | Before routing work after a restart or new milestone, reconcile recorded state with live changes and obtain the human's offline update. |
| R10 | Record milestone outcomes and handoff state promptly; define a consistency/reconciliation protocol between durable state, Git, remote systems, and Markdown. |
| R11 | One writer per worktree, enforced by the host or equivalent boundary; parallel writers need isolated worktrees. Filesystem serialization alone is insufficient. |

## Durability, safety, and control

| ID | Requirement |
| --- | --- |
| R12 | Recover admitted messages, task status, and agent identities after process failure; make ambiguous or interrupted external actions visible. |
| R13 | Stable message IDs, reply/correlation IDs, bounded retries, explicit delivery/result status, and duplicate handling; restart must not silently multiply work. |
| R14 | Enforce capabilities outside prompts. A role without implementation permission must not gain it indirectly through shell, scripts, delegation, or peer requests. |
| R15 | Durable, scoped human approvals for merge, production mutation, and human-facing publication; invalidate approvals if the reviewed action changes. Public repo creation for this scaffold is explicitly authorized by the operator. |
| R16 | Independent stop-one, pause-project, and emergency stop-all semantics, including background tasks and late-arriving callbacks. |
| R17 | Bound concurrency, delegation depth, autonomous turn count, message cycles, retries, and spend. Report overruns and parked work. |
| R18 | Keep private runtime state and credentials outside public Git; treat retrieved material and agent messages as untrusted data. |

## Evaluation

| ID | Requirement |
| --- | --- |
| R19 | Attribute usage, elapsed time, failures, review outcomes, and human interventions to roles/work items. Correlate decisions, predictions, actions, steering, verification, and eventual outcomes using a portable event contract usable from Pi, codemode, and the Durable host. Native totals alone are insufficient; preserve unknowns and cost provenance. |
| R20 | Compare a single-agent baseline, the current multi-session workflow, and the proposed system on the same bounded tasks; more agents is not inherently better. |
| R21 | Evaluate adviser quality using independently checkable evidence and seeded flaws, not agreement among agents or a classifier alone. |
| R37 | Guard classifier question integrity: neutral versioned templates, balanced criteria, observations separated from interpretations, relevant counterevidence/missingness, and no caller expected-answer leakage when seeking an independent signal. Measure framing sensitivity with controlled paraphrase/polarity/label tests and independent outcomes; never reword/cherry-pick until Jev confirms the caller. Unknown evidence cannot become forced certainty. |

## Deployment and decision models

| ID | Requirement |
| --- | --- |
| R22 | First release targets one local setup, portable between personal and work laptops. Keep a host/client separation and stable identities suitable for future remote/multi-device clients, but defer remote deployment, networking, and sync implementation. |
| R23 | Integrate and actively use Jev via direct TypeSafe AI access for appropriate typed decisions, including codemode. Adoption is selected. Measure incremental usefulness, disagreement, latency, usage, cost, and downstream outcomes. Predictions are not authorization. |
| R24 | Keep decision-model use configurable and failure-visible; do not make every action depend on it. Laya may be an alternative, not a required local deployment. Do not assume protocol compatibility or silently switch paid providers. Credentials remain host-owned and out of client messages, prompts, tool environments, and telemetry. |

## Alignment and autonomous decisions

| ID | Requirement |
| --- | --- |
| R25 | Direct human steering creates a durable correlated update to the responsible driving agent: changed intent/scope, current state, affected work, and any decision needed. Reconcile dependent plans/trackers and stale work. Receipt/acknowledgment and failures must be visible; notification is not an orchestrator approval hop. Support different driving agents per workstream and avoid echo loops. |
| R26 | Work autonomously within evidence and granted authority. When consequential ambiguity, conflicting instructions, missing requirements, or a decision outside authority remains unresolved, persist and surface a decision request to the right owner rather than assume. Pause affected work, allow independent work to continue, and record resolution/revision. |
| R27 | Surface the question, evidence, options, impact, and blocked scope with the decision request. Timeouts must not manufacture consent. Favor preventing throwaway work over uninterrupted execution or cache retention; measure late clarification, drift, and avoidable rework. |
| R28 | Development and runtime-supported project workflows distinguish current product/project knowledge from dynamic tracking/history. Keep milestone state and handoff current, preserve evidence/decision history, and let agents load only relevant slices. The repository convention is a template, not a mandatory layout for all user projects. |

## Context distribution and lifecycle

| ID | Requirement |
| --- | --- |
| R29 | Keep working context distributed by responsibility with minimal unnecessary overlap. Aim below 300K normally; select an upper budget within the operator's 300–400K range and never above 400K or a smaller model's usable window. Count system/tools, history, cached tokens, attachments/steering, bounded tool growth, and output/control/handoff reserves. |
| R30 | Predict context growth and start wind-down before exhaustion. Stop admitting new substantive work to a draining incarnation, reach a safe checkpoint, and create/validate a compact successor briefing. Do not abruptly interrupt mutation or silently discard uncertain effects to meet a token threshold. |
| R31 | Preserve stable logical agent addressing across conversation/incarnation replacement, with explicit routing epochs and one active mutation owner. Reconcile accepted/in-flight messages, tasks/children, decisions/approvals, direct human steering, and late results; notify drivers/clients and reject or redirect stale handles. |
| R32 | Verify handoff fidelity against authoritative artifacts/current workspace and operation state before activation. Keep old context inspectable but do not load the entire transcript into the successor by default. Measure peak context, overlap signals, rollover failures, and post-handoff correction/rework. |
| R33 | Subject context thresholds, rollover primitives, handoff validation, ownership fencing, and routing-cutover failure modes to explicit Astra review and acceptance tests. The operator's budget/continuity constraints are requirements; the detailed [lifecycle proposal](context-lifecycle.md) is not yet settled or implemented. |
| R34 | Allocate bounded tasks using the relevant working set, current worker context, expected growth/reserves, dependencies, and workspace/budget capacity. Split scope or use suitable fresh/additional workers before contexts inflate; do not copy giant parent/sibling histories. The coordinator's own context also stays bounded. |
| R35 | Task briefs are sufficiently self-contained through concise intent/scope/acceptance, explicit authority/ownership, and revisioned project-MD/artifact pointers. Unfinished work is an explicit bounded continuation work item with verified progress, remaining criteria, pending effects/decisions, and predecessor lineage—not accumulated instructions or recursively concatenated summaries. |
| R36 | Horizontal splits/reassignment and incarnation rollover have distinct routing transitions. Track work-item/assignment revision, current logical assignee, driver/result destination, child aggregation, and late-result provenance. Reassignment must not duplicate effects, create multiple writers, erase authority limits, or lose steering/acceptance lineage. See [task contract](task-contract.md). |

## Scoped continuation and automatic lifecycle

| ID | Requirement |
| --- | --- |
| R38 | Support a continuation map with a small common core and per-logical-agent focus/next-task sections, or linked per-agent files when needed. Default worker briefing is scoped to common-plus-assigned focus and revisioned artifact pointers. The driver maintains allocation; the host can select a view without an LLM relay. Define freshness and aggregate-update authority rather than allow concurrent divergent edits. |
| R39 | Distinguish task result, continuation-ready, and replacement-requested signals. Notify the responsible driver, then allow host-controlled provisioning/briefing/rerouting within configured policy without routine manual spin-up or user-managed incremental session suffixes. Preserve internal IDs/epochs, authority, ownership, receipts, and lineage; do not turn completion into unlimited automatic spawning. |

## Not assumed

- Particular models are permanently best at a role.
- Automatic recovery makes arbitrary external effects exactly-once.
- A classifier is a safety authority or a reliable quality evaluator without validation.
- A central runtime requires a rigid central workflow scheduler.
- Markdown and a database can be atomically updated with external services.

## Confirmed operator constraints

- Use latest `nicobailon/pi-subagents` and `nicobailon/pi-intercom` as the comparison baseline; record exact inspected revisions for reproducibility. Historical session versions are context, not a prerequisite.
- UI choice is flexible. Direct access to a persistent agent's context, activity, summary, and conversation is the requirement, not a particular terminal implementation.
- Start local and portable; account for future remote clients in the design.
- Layouts must vary with project scope/complexity and are selected by the operator.
- Astra is used via an OpenAI subscription inside Pi. Review initially uses another session and versioned Markdown; later use the runtime when it can safely support the exchange. Frugal, focused review matters; none has happened yet.
- Jev access is direct TypeSafe API key access, with operator-reported pricing $0.042/M input tokens and zero output price. Use it and measure it; secure provisioning remains necessary. Local Laya deployment is not required.
- MIT license accepted. Commit and push coherent checked milestones frequently; keep README, AGENTS, current docs, and tracking aligned.

## Evidence and open design work

See [open variables](../project/variables.md), [work-setup evidence brief](../project/research/work-setup-analysis-brief.md), and [progress](../project/progress.md). Provider reuse, exact launch IDs, runtime limits, message/revision semantics, and instrumentation packaging remain design/implementation work; name, license, UI flexibility, and Jev adoption are settled.
