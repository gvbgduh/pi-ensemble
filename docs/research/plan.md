# Research plan

## Goal

Determine whether Pi Durable should replace, supplement, or leave intact the existing interactive multi-agent infrastructure. Preserve the behavior in `../requirements.md`; do not assume fewer dependencies means a better system.

## Workstreams

### 1. Understand the durable harness

- Read the normative spec in full and map the relevant source/tests.
- Inspect background-agent and reviewer examples, task ownership, inbox semantics, cancellation, failure recovery, and schema/task migration.
- Verify provider authentication, coding tools, codemode, and resource-discovery differences from the coding-agent CLI.
- Establish a released package version and compatible dependency set; the initial source snapshot is not a release pin.

### 2. Characterize the existing packages

- Package identities are confirmed as `nicobailon/pi-subagents` and `nicobailon/pi-intercom`; obtain installed versions and pin inspected upstream revisions.
- Inspect peer discovery, addressing, message persistence, lifecycle, delegated child behavior, artifacts, and worktree isolation.
- Build an evidence-backed matrix: existing behavior, Durable primitive, missing application policy, migration cost, and test.
- Separate peer communication from bounded delegation rather than treating both as one feature.

### 3. Choose the product boundary

Compare the options in `../architecture-options.md`, including the cost of losing the ordinary Pi TUI and existing resource ecosystem. Write a proposed design only after the gaps and required UX are understood.

### 4. Recovery and control experiments

Proposed experiments; **none have run yet**. Use a fake provider, temporary isolated repositories, and a local fake remote service first.

| ID | Experiment | Required observation |
| --- | --- | --- |
| E1 | Kill the host after admission, before processing, and during a reply | Message and result identities survive; no silent lost or duplicated application work. |
| E2 | Stop one peer while reporters and queued messages exist | Document late-message behavior; a stopped project cannot be restarted by a callback. |
| E3 | Worker/adviser exchange with orchestrator idle | Peer delivery and bounded callbacks work without central relay or unbounded loops. |
| E4 | Crash after an external effect, before its receipt | The ambiguous result is reconciled or surfaced; unsafe mutation is not blindly repeated. |
| E5 | Two workers request the same checkout | Exactly one obtains write ownership; cancellation, crash, and stale ownership have defined handling. |
| E6 | Change an action after human approval, then restart | Approval cannot authorize a different diff/action and remains auditable. |
| E7 | Exceed role/project budget and concurrency limits | Host blocks new work at defined boundaries; background tasks and retries are included. |
| E8 | Interrupt tracker projection and change remote state offline | Recovery detects and reconciles drift without inventing a distributed transaction. |
| E9 | Try opening one store from two hosts | Chosen host boundary prevents unsafe concurrent ownership; do not assume upstream locking. |
| E10 | Reopen state with changed task/extension definitions | Unsupported migration blocks visibly; upgrade/rollback strategy is explicit. |
| E11 | Finish a worker task, then directly request an explanation and adjustment after reconnect/restart | Same logical agent remains reachable with relevant conversation context and artifact provenance; no orchestrator relay is necessary. |
| E12 | Start two projects with different operator-defined role layouts | No fixed four-role assumptions; role policy and agent lifetime remain explicit. |

### 5. Jev and typed-decision experiments

Follow [the Jev research track](jev.md). Use the provider actively after API/access and a spend envelope are established, without making it a mandatory gate on every action. Compare measured outcomes, not agreement alone. Codemode integration is in scope; local Laya deployment is not an assumed requirement.

### 6. Independent review and implementation gate

**Explicitly tell the operator “Ready for Astra review”** when there is a concrete design, capability matrix, and evidence for the riskiest assumptions. Do not let this gate pass silently. Use `../reviews/astra-brief.md`, record the reviewed revision and findings, then seek operator acceptance before implementing the proposed architecture.

If experiments require a disposable code spike before design review, scope and label it as an experiment, not the product implementation.

## Research done criteria

- Every must-have requirement has a proposed owner and evidence, or a clearly disclosed gap.
- No untested claim of package/API compatibility, external exactly-once effects, or cross-process safety.
- UX constraints and provider access are validated with the operator.
- Highest-risk experiments have results; deferred experiments have explicit rationale.
- The recommendation includes alternatives, migration stages, rollback, risks, and maintenance cost.
