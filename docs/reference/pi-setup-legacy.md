# Legacy Pi multi-agent workflow

> Reformatted and condensed from the operator-provided guide. Workplace-specific discussion, third-party comments, and export boilerplate are omitted. This is reference material, not Pi Ensemble's current specification. Historical model rankings, context/cost estimates, and reliability claims are anecdotal, not validated benchmarks. Agent counts and model choices are not requirements. Tracker change-map naming is normalized to `prs.md` for GitHub.

## 1. Principles

### Project-scale coordination

The unit of work is a project or multi-epic initiative, not one task. A coordinating agent holds goals, dependencies, and cross-stream state while implementation agents work at bounded depth. Projects may span repositories and multiple human contributors.

### Separate contexts and scales

Different agents hold different scales of detail: project coordination, implementation, design/prose, and adversarial evidence. Avoid duplicating the same source context into every conversation. Route pointers to artifacts and return compact outcomes with evidence.

The original guide credits this separation with better context/cost efficiency. That is a design motivation to evaluate, not a guarantee that adding agents increases useful capacity linearly.

### Shared tracker as substrate

Human-readable artifacts carry shared knowledge:

- Project README and Related Context links: goals, neighbors, scope.
- `progress.md` and `prs.md`: workstreams, dependencies, change state.
- `variables.md` and findings: task-scale questions and evidence.
- `decisions.md`: inherited and ratified decisions.
- `continuation-prompt.md`: compact restart state.

Agents load the slice appropriate to their role. Knowledge not written to shared artifacts is unavailable to other sessions.

### Independent correction

A manager or adviser can challenge the coordinator's assumptions from an independent context. A worker supplies concrete diffs/tests, not just assertions. Independent perspectives create opportunities to catch drift; they do not establish correctness merely by agreeing.

### Models fit roles

The guide describes model-specific strengths in coordination, implementation, prose, and source disambiguation. Keep that intent—choose models for the work and economics—but do not turn historical vendor/model preferences into permanent role identities.

### Human judgment remains central

The workflow narrows the search and reduces coordination load; it does not remove the need to inspect evidence, consequential decisions, and external actions. Strict role boundaries and current trackers are load-bearing.

## 2. Interactive agents, peer messaging, and delegation

### Example responsibilities

These are example responsibilities, not a mandatory topology.

| Role | Owns | Normally avoids |
| --- | --- | --- |
| Worker | Implementation, isolated worktrees, tests, commits/pushes, change-request creation and descriptions | Owning project architecture or human-facing business communication |
| Coordinator/orchestrator | Goals, dependency topology, tracker state, routing, human interface | Routine implementation and source-heavy investigations |
| Manager | Independent coherence review, architectural/business trade-offs, prose and communication drafts | Code edits and routine orchestration |
| Adviser/oracle | Adversarial review, source/spec/telemetry disambiguation, deep debugging | Routine implementation and coordination |

Workers author change descriptions from implementation context; managers may polish them. Coordinators escalate genuine ambiguities and complex debugging rather than filling their own context with implementation detail.

### Human-visible sessions

The original workflow uses model/role-pinned Pi sessions in Herdr panes, with a shared project/session prefix so peer addresses are recognizable. Layouts expand or shrink with scope. The required behavior for Ensemble is visibility and direct access, not a particular multiplexer or screen layout.

### Peer callbacks

Agents use `pi-intercom` directly. A recipient reports its result to the sender without waiting to be chased. The coordinator routes work but does not relay every message.

The shared tracker records what happened; the intercom transcript is not the sole project authority. Stable addresses, role boundaries, artifact references, and callback discipline keep communication usable.

### Bounded subagents and codemode

- `pi-subagents`: bounded reconnaissance, searches, tests, documentation, and mechanical edits outside the interactive agents' context.
- Codemode: batches tool/API calls and filters large outputs before they enter model context.
- Parallel writers need separate worktrees: one writer per shared tree.
- Delegation should preserve context efficiency without turning the interactive agent into a passive chat relay.

The guide's description of child lifetime reflects its historical usage, not an assertion about the latest package. Current `pi-subagents` documents retained-child resume; inspect actual eligibility and UX before claiming no follow-up is possible.

### Typed decisions

The work setup describes hosted Jev and local Laya for typed questions over structured state: boolean decisions, choices, and ordered scores. Its latency/cost claims are historical and should not be copied as current service guarantees.

The guide explicitly leaves incremental value open: strong agents and deterministic filters often agree with the classifier. Ensemble will use Jev and measure where it changes useful decisions, including misses and disagreements, rather than treat agreement as validation.

## 3. Project tracking convention

### Hierarchical tracker tree

Project trackers can live independently of code checkouts and be shared across harnesses. A project can contain subprojects and narrower feature trackers. Link related projects instead of copying their knowledge.

```text
<tracker-root>/
└── <project>/
    ├── index.md
    ├── README.md
    ├── tickets.md
    ├── prs.md
    ├── decisions.md
    ├── variables.md
    ├── progress.md
    ├── continuation-prompt.md
    ├── findings/
    ├── reports/
    └── <subproject>/
        ├── index.md
        ├── README.md
        ├── progress.md
        ├── variables.md
        └── spec.md
```

Use the full backbone for substantial trackers. Small leaves may start lean and grow. Each directory has an `index.md` map; its README explains purpose, scope, and Related Context. The current application of this convention is documented in [project structure](../project-structure.md).

### Artifact contracts

| Artifact | Purpose | Update convention |
| --- | --- | --- |
| `index.md` | Directory navigation | Update when contents change |
| `README.md` | Goal, strategy, scope, related context | Current, not copied from parent |
| `progress.md` | Current workstreams and session log | Update tables; append dated milestones |
| `prs.md` | Pull-request map, including dependencies and relevant teammates' work | Keep live state current |
| `tickets.md` | Issue/epic map, owners, status, priority | Reconcile with source system |
| `decisions.md` | Inherited and accepted decisions | Index IDs; supersede rather than erase |
| `variables.md` | Open questions, trade-offs, watch items | Record when they surface |
| `continuation-prompt.md` | Compact restart handoff | Refresh at milestones and wrap-up |
| Findings/reports | Evidence, investigations, status snapshots | Dated, linked, preserved |
| Publishable design/spec/runbook | Local source for an external document | Edit source first; publish deliberately |

### Memory versus trackers

- Trackers own project knowledge and decisions.
- Global memory owns durable cross-project preferences and pointers.
- Scratchpad holds temporary working reminders; surviving items move to the appropriate durable store.
- A day log can record chronology across projects without duplicating full project state.

## 4. Progress and session lifecycle

### Preflight before resuming work

Reconcile the tracker with cheap live checks:

1. Referenced changes: merged/closed state and new review activity.
2. Untracked related changes: branches or requests mentioning known work.
3. Issue status/owner/priority deltas.
4. Tracker changes since the previous session.
5. Previous agents still running.

Present the delta and ask for the operator's offline update before routing significant new work. Trackers mirror the world; they are not the world.

### Same-milestone bookkeeping

Update workstream state, change map, decisions, variables, and compact handoff when the milestone occurs. Do not wait for session end: sessions can terminate abruptly.

### Team and multi-repository tracking

Track relevant human-authored work and external dependencies to detect conflicts and merge topology, not to duplicate every team activity. A resource-producing change may block a downstream configuration change even across repositories.

### Automation and human gates

The original guide permits bounded implementation and routine bookkeeping while reserving merge, production promotion, and human-facing communication for explicit human approval. Exact policy belongs to the current project/operator; importing this guide does not grant permission.

Classify actions by authority and effect. Draft consequential messages and changes before seeking approval. When a consequential decision is genuinely unresolved, ask rather than silently assume.

## 5. Skills worth preserving

### New project creation

Create the tracker from templates, link parents/siblings, seed real issue/change state, inherit applicable decisions explicitly, and register a global pointer. Do not invent state just to fill a template.

### Project status tracking

Read predictable artifacts, verify against cheap live checks, lead with drift, and render a concise per-workstream view with critical path and blockers.

### Session spin-up

Identify the next project iteration, run preflight, validate models/roles, create the interactive layout, and provide a kickoff referencing the handoff and deltas. Include dry-run/preflight-only modes. Work starts after the operator supplies relevant offline updates.

Sanitized skill files and their templates/scripts will be stronger evidence than this summary. They are not required merely to maintain the initial scaffold.

## 6. Original tool categories

Pi harness, terminal/multiplexer, `pi-intercom`, `pi-subagents`, codemode, Git-host CLI, issue/document/telemetry integrations, and optional typed-decision extensions. Temporal is mentioned in some workflows but is not a requirement of this tracker convention or a selected Ensemble dependency.
