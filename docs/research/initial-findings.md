# Initial findings: Pi Durable

Status: preliminary documentation/source reading, **not a completed feasibility study**. Sources refer to IDs in [the source register](sources.md). No experiments or paid model runs have been performed.

## What it is

`@earendil-works/pi-durable` is an experimental TypeScript durable agent harness built on Pi AI and Chord. It is not described as a drop-in replacement plugin for the coding-agent CLI. Its README warns that APIs can change without notice [S1].

The host opens storage and a `Harness`, installs a registry of definitions, configures conversations, and supplies execution environments. Conversations, task checkpoints, inboxes, and application documents can be persisted [S1].

## Relevant primitives

| Existing need | Documented primitive | Application behavior still needed |
| --- | --- | --- |
| Independent long-lived roles | Separate conversations, each with model, tools, instructions, extensions, and cwd | Ensemble membership, stable role addressing, capability policy, operator UX. |
| Peer messages and callbacks | Persisted input submissions, busy-mode steer/follow-up/reject, request IDs; background reporters demonstrated | Peer authorization/routing, correlations, loop suppression, stop-state admission, delivery/result receipts. |
| Bounded delegation | Task-owned conversations, owned cancellation, foreground/background examples | Worktree allocation, role defaults, output/artifact contracts, delegation limits. |
| Crash recovery | Stored task checkpoints and resumable scheduling | External-effect reconciliation, lifecycle supervision, upgrade policy. |
| Shared coordination state | Typed documents committed with entries/tasks | Project schema, tracker projection, provenance, reconciliation. |
| Human controls | Submission, abort, configuration, watched views/task graph | Interactive clients, durable scoped approvals, pause semantics, emergency stop. |
| Usage | Per-conversation/model/tool totals and harness aggregation | Work-item attribution, budget admission/enforcement, quality and intervention metrics. |

This makes replacement plausible, but does not establish feature parity.

## The most relevant example

The persistent-background-subagent example [S3] contains:

- Named persistent child conversations recorded in a document.
- A tool exposing spawn, send, stop, and status.
- Background anchor tasks so children outlive ordinary parent idle/abort boundaries.
- Durable reporter tasks with delivery and report phases.
- Stable request IDs for submissions and callback reports.
- A demonstration that closes/reopens the harness while a reply is pending.

It is parent/child oriented, not a general peer ensemble. It also deliberately allows a report arriving after an ordinary parent abort to start another turn. That is a major reason to design explicit project pause/stop semantics rather than equating Esc with stop-all.

The example is illustrative source, not a library contract or proof: it has not been run, audited for all failure windows, or adapted here.

## Boundaries that matter

1. **Two extension APIs.** Coding-agent extensions register through `ExtensionAPI`; Durable extensions use registry definitions such as `defineExtension`, tools, hooks, and durable tasks [S1 and installed Pi docs]. Do not assume existing packages load unchanged. Whether an adapter is worthwhile remains open.
2. **Recovery is not external exactly-once.** A task phase can rerun after restart. Tools rerun only when declared replay-safe; other interrupted calls produce an interrupted result. Even replay-safe tools need their own external idempotency/reconciliation [S1].
3. **One host per store.** The README states one process owns storage at a time and no cross-process locking is provided [S1]. Multiple clients must not independently open one shared database as competing hosts.
4. **Durability has levels.** SQLite's documented default uses WAL with `synchronous = NORMAL`: process-crash survival is not the same as newest-commit survival through power/host loss [S1].
5. **Tool selection is not a complete sandbox.** Configuring fewer tools is useful, but shell access, nested calls, environment access, and delegation still need a coherent capability boundary. Prompt-only role restrictions are not enforcement.
6. **Observability is partial.** Usage totals, committed views, and a task graph exist. The documented telemetry adapter boundary currently emits no spans or metrics; richer tracing and evaluation remain host work [S1].
7. **Persisted chat is not project memory.** Markdown trackers carry human-readable decisions and handoffs; storing conversations does not replace that function. Authority and projections need deliberate design.

## Preliminary answer

Pi Durable may supply much of the execution substrate previously assembled from sessions and packages. It does not by itself supply the full ensemble workflow, ordinary Pi CLI integration, workspace isolation policy, approval system, or evaluation framework.

A durable host with attachable clients is a reasonable **hypothesis**, not an accepted recommendation. The operator has confirmed that a native Pi CLI UI is not mandatory; a Pi adapter should therefore be optional, not assumed MVP work. Compare a new host with incremental improvement to the existing setup before committing to a rebuild.

## Still unknown

Normative-spec details, installed legacy package versions and behavior, provider/subscription compatibility, concrete client UX, released package compatibility, migration and state export, performance with a multi-day ensemble, and actual recovery behavior. See [the research plan](plan.md).
