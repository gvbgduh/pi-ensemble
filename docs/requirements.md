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
