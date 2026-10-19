# Architecture options

Status: C is the preferred direction, supported by the operator; D is a useful optional client on top. The detailed architecture has not been reviewed or implemented. A/B remain comparison and migration baselines, not equal-priority product destinations.

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

## Preferred direction

Develop C toward a functioning attachable runtime: the host owns persistent agents, clients inspect and interact with them. Think tmux-style attachment, not disposable per-task agent sessions. D can expose the same host through Pi later. Use A as a serious behavior/migration baseline; package replacement alone is not a success metric.

The local MVP should distinguish the agent host from its attachable clients, without building remote deployment or synchronization yet. Persistent agent identity/conversation must outlive an individual work item; logical completion must not terminate the agent's usefulness for feedback and adjustments.

## Evidence and design questions

- UI flexibility is confirmed: compare the least expensive client options that expose activity, summaries, and direct conversation.
- Which existing extensions, skills, provider logins, and tools must be reused unchanged?
- One local host is the first target. What protocol/ownership decisions must remain compatible with future authenticated remote clients?
- Analyze real work sessions and sanitized skills to identify friction, successful recovery, steering/drift, ambiguity, and throwaway work rather than only hypothetical gaps.
- Inspect Cloudflare's Agents SDK Pi harness, the official Cloudflare example, creator-shared examples, and community applications for reusable boundaries. Record stale notes/version differences.
- How much custom UI/runtime/protocol maintenance is justified, and what existing component can be reused?

## Required design boundaries, whichever option wins

Separate project/ensemble identity, role identity, conversation lifetime, execution-task ownership, worktree write ownership, and human approval authority. Define authoritative state and how external artifacts are reconciled. Define peer message contracts and limits, including direct-steering updates to the driving agent and dependent-plan reconciliation. Define decision requests and affected-scope blocking for unresolved consequential ambiguity. Define pause/stop semantics before callback automation. Choose a recoverable migration path that preserves readable project knowledge.

**Review milestone:** prepare the concrete proposal, capability matrix, and high-risk evidence, then explicitly highlight the Astra handoff. Manual Pi/Markdown coordination is the bootstrap path; later the runtime should support the exchange. Review consequential architecture before committing to it; do not freeze ordinary documentation or bounded evidence-gathering spikes. See [Astra brief](../project/reviews/astra-brief.md).
