# Initial requirements

Status: initial baseline, updated with operator clarifications on 2026-10-19. These describe desired behavior, not existing implementation guarantees.

## Role and interaction model

| ID | Requirement |
| --- | --- |
| R1 | Long-lived, project-specific ensembles. Worker, orchestrator, manager, and adviser are a starting layout, not mandatory roles. The operator chooses role types, counts, and responsibilities for the problem; each agent has an independent conversation and configurable model/provider. |
| R2 | Orchestrator owns coordination and project-scale state; workers own implementation; manager provides independent coherence/prose review; adviser provides evidence-based adversarial investigation. |
| R3 | Any authorized role can address another directly and return a result without the orchestrator relaying every message. |
| R4 | Human can inspect activity/logs and summaries, ask questions, give feedback, steer, pause, or stop any agent directly, including during active work. No mandatory orchestrator hop. The ordinary Pi TUI is not required. |
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
| R19 | Attribute usage, elapsed time, failures, review outcomes, and human interventions to roles and work items, with project-level aggregation. |
| R20 | Compare a single-agent baseline, the current multi-session workflow, and the proposed system on the same bounded tasks; more agents is not inherently better. |
| R21 | Evaluate adviser quality using independently checkable evidence and seeded flaws, not agreement among agents or a classifier alone. |

## Deployment and decision models

| ID | Requirement |
| --- | --- |
| R22 | First release targets one local setup, portable between personal and work laptops. Keep a host/client separation and stable identities suitable for future remote/multi-device clients, but defer remote deployment, networking, and sync implementation. |
| R23 | Actively explore Jev on real bounded typed-decision tasks, including codemode access. Measure usefulness, disagreement, latency, and cost against deterministic rules and agent judgments. Do not treat its predictions as authorization. |
| R24 | Keep decision-model integration optional and configurable. Investigate Laya as an alternative, but do not require local inference hardware, assume API compatibility, or silently fall back to a paid service. Unavailable or uncertain predictions must be surfaced. |

## Not assumed

- Particular models are permanently best at a role.
- Automatic recovery makes arbitrary external effects exactly-once.
- A classifier is a safety authority or a reliable quality evaluator without validation.
- A central runtime requires a rigid central workflow scheduler.
- Markdown and a database can be atomically updated with external services.

## Confirmed operator constraints

- Current packages are `nicobailon/pi-subagents` and `nicobailon/pi-intercom`; installed versions still need to be captured.
- UI choice is flexible. Direct access to a persistent agent's context, activity, summary, and conversation is the requirement, not a particular terminal implementation.
- Start local and portable; account for future remote clients in the design.
- Layouts must vary with project scope/complexity and are selected by the operator.
- Astra review will initially use another Pi session and versioned Markdown artifacts. Frugal, focused reviews matter; no review has happened yet.
- The operator has Jev access and wants to build intuition through active use, including codemode. Work availability is anticipated; local Laya hardware suitability here is unknown.

## Remaining operator questions

1. Which subscription-backed/custom providers must work? Credential compatibility needs verification.
2. Which exact Jev service/API/model is available, what access method does it support, and what spend cap should research use? Do not put credentials in public Git or chat.
3. What is the exact Astra model/provider identifier and available thinking levels?
4. What is the initial budget/concurrency envelope, and which publication/remote mutations require a gate here?
5. Which versions of the two legacy packages are installed at work?
6. Which public redistribution license should this project use?
