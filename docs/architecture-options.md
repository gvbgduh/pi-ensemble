# Architecture options

Status: hypotheses only. No product architecture has been selected.

## A. Keep Pi CLI peers; improve the current packages

Retain separate ordinary Pi CLI sessions, intercom, and bounded delegation. Add only the missing persistence and control features where justified.

- Preserves familiar terminal UI and existing extension/resource behavior.
- Potentially smallest migration and maintenance cost.
- Must inspect real package behavior before identifying durability gaps.
- Recovery, project-wide limits, and ownership may be harder across processes.

## B. Standalone coding-agent SDK supervisor

Host or supervise SDK sessions and introduce application-level coordination persistence.

- Preserves coding-agent resource/extension concepts better than assuming Durable compatibility.
- Requires defining restart semantics across the coordination store and session files.
- Risk of rebuilding capabilities already provided by Durable.

## C. Pi Durable ensemble host with dedicated clients

One host owns storage and exposes independent persistent role conversations to attachable clients. Host scheduling executes tasks; agents can still choose work and communicate as peers rather than following a rigid workflow DAG.

- Strong fit with documented conversation/task/inbox primitives.
- Requires ensemble policy, peer routing, resource/provider integration, UI, approvals, workspace ownership, and budgets.
- Existing CLI packages are not assumed portable.
- Experimental upstream API implies version pinning and migration work.

## D. Durable host plus thin Pi CLI adapter

Option C owns durable execution and coordination. A coding-agent CLI extension provides controls, views, or handoffs to that host where this is useful.

- Could keep a familiar operator entry point without making the CLI process the lifetime owner of background work.
- Does **not** automatically make every durable role a native Pi CLI session.
- Needs a clear protocol and failure semantics for reconnect, authentication, duplicate commands, and cancellation.
- Must avoid running two independent agent loops for one role or making a UI adapter another source of truth.

## Working hypothesis

Investigate C first because the background-agent example directly addresses persistent conversation and callback mechanics, and the operator does not require native Pi CLI UI. D is an optional later adapter if it earns its maintenance cost. Keep A as a serious baseline; package replacement is not a success metric.

The local MVP should distinguish the agent host from its attachable clients, without building remote deployment or synchronization yet. Persistent agent identity/conversation must outlive an individual work item; logical completion must not terminate the agent's usefulness for feedback and adjustments.

## Questions that select among them

- UI flexibility is confirmed: compare the least expensive client options that expose activity, summaries, and direct conversation.
- Which existing extensions, skills, provider logins, and tools must be reused unchanged?
- One local host is the first target. What protocol/ownership decisions must remain compatible with future authenticated remote clients?
- Which missing guarantees actually cause pain in the existing workflow?
- How much custom UI/runtime/protocol maintenance is acceptable?

## Required design boundaries, whichever option wins

Separate project/ensemble identity, role identity, conversation lifetime, execution-task ownership, worktree write ownership, and human approval authority. Define authoritative state and how external artifacts are reconciled. Define peer message contracts and limits. Define pause/stop semantics before callback automation. Choose a recoverable migration path that preserves readable project knowledge.

**Next gate:** prepare a concrete proposal, evidence-backed capability matrix, and high-risk experiment results; then explicitly announce readiness for Astra review before product implementation.
