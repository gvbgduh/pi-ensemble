# Astra design v1 — independent findings

- **Review:** ASTRA-DESIGN-01 rev1, 2026-10-19.
- **Proposal:** `ab0d906c05228fa281b63f68da1205eee7b9fd80`; principally `docs/design.md` and `project/implementation-plan.md`.
- **Reviewer:** live Astra session (`gpt-6-astra`); focused architecture pass. No global settings or thinking-level change was made; no claim that a particular reasoning level was configured.
- **Isolation:** `review/astra-design-v1`, `.local/worktrees/astra-design-v1`; findings only. This review does not modify the proposal or constitute operator acceptance.
- **Method:** read proposal, plan, review brief, requirements and linked lifecycle/task/model/measurement/architecture contracts; read source findings and source register. Previously read all 716 lines of the imported Argon report. Independently inspected selected Durable TypeScript implementation at `1402973a9dbe8c627341e04c9744c54953e285ca`, particularly scheduling and submission admission. Did not independently repeat Luna's full normative/source audit or Argon's private-corpus analysis.
- **Unrun:** all builds, package compatibility/auth integration, tests, fault experiments and sandbox checks. Scenarios below are analytical counterexamples/tests, not reproduced failures of an implemented Ensemble runtime.

## Recommendation

**Keep direction C and proceed with separately authorized, bounded M0 proofs. Conditionally accept the design direction after the four contract clarifications below; do not approve its recovery/fencing guarantees as already established.** There is no evidence here requiring a new substrate or a second scheduler. M0-01 is a useful, appropriately small next task.

Before treating v1 as an accepted implementation contract, resolve **AD1–AD4** in a separate author disposition/proposal amendment. Their tests gate the relevant milestone, not all earlier work. M0 investigation need not wait for a production sandbox. M1 can remain a genuinely useful restricted persistent-peer prototype, provided its scheduling/control and budget claims are explicit and tested. Mutation remains disabled until M3.

| ID | Priority / classification | Required before |
| --- | --- | --- |
| AD1 | High: app/native delivery linearization is underspecified | Contract amendment; M0 bridge proof / M1 delivery claims |
| AD2 | High: startup/control gate must cover implicit scheduler resume | Contract amendment; M0 recovery proof / M1 safe restart |
| AD3 | High: executor ownership must survive host death | Contract amendment; M3 mutation release, M4 rollover |
| AD4 | High: steering hold release lacks a revisioned completion contract | Contract amendment; M2 steering release |
| AG1 | Required implementation gate: aggregate context admission | M1 provider use; expanded M4 tests |
| AG2 | Required implementation gate: immutable handoff readiness and input races | M4 activation |
| AG3 | Required implementation gate: release/auth/storage/platform selection | M0 exit, platform-specific M3 exit |

These are missing mechanical details in a proposal, not assertions that an existing implementation contains these bugs.

## AD1 — Specify the app/native delivery linearization point and recoverable wake obligation

**Requirements:** R12, R13, R16, R31, R36. **Evidence:** design §§3, 6 steps 1–3, 9–10.

The two-step bridge is a reasonable approach. Pinning destination plus a stable request ID before native submission prevents a retry from jumping to another conversation. However, the proposal validates lifecycle/control at initial admission and later says a delivery task claims/pins/submits; it does not state exactly which checks are repeated under the Session line or how that transition excludes drain/stop/cutover.

**Concrete race:** a message is admitted while epoch E is active. Its delivery task reads E, then suspends. Rollover marks E draining and sees no pinned native submission. The task subsequently pins/submits to E, possibly after readiness was calculated. A related race admits work before pause but injects it after pause. A host-side check outside the serialized transition does not close either gap. Stable native request IDs prevent duplicate retries, not stale first delivery.

**Smallest correction:** specify a persisted delivery state machine and its linearization rules:

1. Initial receipt, obligation, immutable payload reference/digest and recoverable wake work commit together, or define a deterministic recovery scan that cannot strand an admitted obligation.
2. Pin/claim is a serialized transition that rechecks current control, lifecycle, assignment and route revisions. It becomes part of the predecessor inventory **before** any external/native submit attempt. Drain/cutover cannot ignore a claimed delivery whose submit outcome is unknown.
3. Define what pause/stop does to a claim made just before fencing: reconcile/withdraw/settle it, never silently move it. If native injection is not atomic with app authorization, document that accepted-in-flight boundary and prevent unintended execution rather than promise impossible cancellation.
4. Store the exact native submission draft, or a fully revisioned immutable construction of it, before the first attempt. A retry must not reconstruct it from changed Markdown or policy.

**Primary-source nuance:** pinned `packages/durable/src/harness/submissions.ts:179–186` deduplicates by conversation/request ID and checks only submission **type** on a collision; it returns the existing submission for different content of the same type. Ensemble's own digest enforcement remains essential. The design already recognizes this as a release test; do not require upstream to supply payload-conflict validation that the host can provide.

**Smallest test:** deterministic barriers after app admission, after pin, before native submit, after native commit, and before app receipt; race pause/drain/cutover and kill/reopen at each barrier. Assert one pinned incarnation/request/draft, no lost obligation, no fresh-prompt replay, and a visible settled/withdrawn/uncertain outcome. Include modified payload under an existing command ID and changed referenced artifact under a native retry.

## AD2 — Startup and pause must gate actual scheduling, not only public admission or explicit resume

**Requirements:** R9, R12, R16, R17. **Evidence:** design §§6, 9; M0/M1/M2 split.

The startup sequence correctly says reconcile before execution and await offline confirmation. The mechanism must account for native APIs that implicitly enable scheduling. This matters before tools are enabled: an unintended recovered model generation can spend money, dispatch peer work, or continue a superseded instruction.

**Independently inspected source:** at the pinned revision, `packages/durable/src/harness/scheduler.ts:348–396` documents and implements recovery on open without dispatch. But `src/harness/submissions.ts:64–69` calls `resume()` **before** committing a submission, and `:85–86` calls it when waiting on a submission. `scheduler.ts:400–404` enables scheduler execution generally. Thus simply omitting `harness.resume()` is not a sufficient startup guard.

**Concrete failure:** reopen a paused project with an unfinished native generation and pending bridge obligation. A reconciliation task waits for the old submission, or a client sends a new question to another agent. This resumes native scheduling and starts recovered work before the human offline update. The effect broker may prevent later mutations but does not undo the model work or peer dispatch. A delivery task that needs the same scheduler to run also needs an explicit bootstrap path; do not accidentally solve this by resuming everything.

**Smallest correction:** define startup states with an execution-disabled recovery phase, explicitly enumerate resume-capable calls, and distinguish metadata inspection from execution. Specify a supported way to gate recovered native generation/tool tasks and app delivery tasks on current control state. It is acceptable initially to expose only status/snapshots until the startup gate opens. If a provider/executor boundary is the guard, show that blocked tasks park without repeated provider calls or hot retries. Do not silently introduce a second task runner.

Also specify the minimal M1 stop/restart contract: M2 can add rich scoped pause semantics, but M1 must not imply safe restart while deferring every mechanism that makes its startup gate effective.

**Smallest test:** reopen with queued input, interrupted generation, delivery task and stopped/paused project. Invoke every permitted inspection/wait/attach path before confirmation. Assert zero provider/tool starts and no new peer work. Explicit confirmation enables only eligible work. Repeat with a pause racing a native submit and with startup storage failure.

## AD3 — Host lock ownership is not executor ownership after a host crash

**Requirements:** R11, R14, R16, R31. **Evidence:** design §§8–10; M3 adversarial tests.

The proposal explicitly rejects same-user shell as confinement and requires real process exit before replacement. This is sound. What remains unspecified is how a replacement host proves exit when the process that launched/registered the executor died.

**Concrete failure:** host H1 holds the store lock and launches a sandbox with a writable workspace mount. H1 is killed; the OS releases its store lock, but the executor or a detached descendant survives. H2 acquires the lock, sees an interrupted task and transfers or reuses the workspace. The old process can still write through its already-open file descriptors/mounts. Checking epoch at tool start cannot intercept each filesystem write, and a stored PID can have been reused.

**Smallest correction:** make executor lifetime ownership a platform-adapter contract, not an implication of the database lease. Require a stable, discoverable containment identity established before mutation, a crash-safe launch/registration handshake, and a recovery rule: discover and terminate/reconcile all prior owned containment scopes, or refuse new writable access. A supervisor/OS containment mechanism may implement this; this review does not mandate a specific platform technology. Unknown orphan status means blocked, not lease takeover. Document whether revocation waits for exit versus revokes writable access; do not claim a shell epoch check mediates every write.

**Smallest test:** kill the **host**, not only the child, at launch-before-registration and registration-before-start boundaries. The child repeatedly writes a marker, forks/detaches descendants and retains open descriptors. Restart the host and request a second writer. Assert that the second writer cannot start until the old containment scope is proven unable to mutate. Include stale PID identity and failed termination. Run on every mutation-supported OS; others remain restricted.

This is an M3 implementation blocker, **not** a reason to prevent M0/M1 chat delivery. Linux-first is acceptable if the operator explicitly accepts that capability scope; portable protocol/storage alone must not be marketed as portable coding confinement.

## AD4 — Steering holds need a revisioned release operation, not only acknowledgment and prose reconciliation

**Requirements:** R25–R27, R36. **Evidence:** design §§4–5, 7; task contract routing/reassignment.

Recording steering and holding dependent mutation at admission is a major improvement over an end-of-turn relay. The proposal distinguishes acknowledgment from semantic reconciliation but does not define the durable record/command that releases a hold, its authorized principal, or its preconditions. The protocol lists `decision.resolve` and `work.report`, neither explicitly completes this obligation.

**Concrete race:** S1 creates a scope hold. While the driver reconciles S1, S2 changes the same scope or the work item changes driver. A late acknowledgment/report for S1 clears a generic `steering_pending` flag and resumes mutation under the older plan. Alternatively, no documented operation can clear the hold and every direct question leaves work stuck. Multiple workstream drivers also need an explicit rule about whose unresolved obligation prevents release.

**Smallest correction:** represent outstanding steering obligations individually, bound to work/assignment, driver and input/plan revisions. Define an authorized reconciliation transition (a dedicated command or a clearly specified existing one) that records disposition, affected dependent work, the plan revision and required recipient obligations. Release only the obligations it actually satisfies under compare-and-set checks; unresolved newer/conflicting steering still blocks. Distinguish transport receipt, driver acknowledgment, reconciled plan, and permission to resume. Conservative initial scope may be the complete assigned work item/dependency closure; ambiguous scope stays held for owner clarification.

Notification need not become permission to speak to the worker. The direct conversation remains available while the relevant mutation/dispatch hold is unresolved. Driverless layouts require an actionable human-owner release path, not a dead mailbox.

**Smallest test:** two concurrent steering events, S1 reconciliation arriving after S2, driver reassignment during an outstanding notice, offline driver, two drivers for dependent work, and an explicitly question-only input. Assert no old report clears newer obligations and that a valid disposition can release the appropriate hold without another model-mediated relay.

## Required implementation gates, not additional reasons to redesign

### AG1 — Reserve aggregate model input at generation/injection boundaries

**Requirements:** R17, R29–R30, R34. Design §§10–11 appropriately count cached tokens, include reserves, cap reads and park early. However, per-command admission checks alone cannot bound a batch of queued inputs/tool results. Before enabling real M1 providers, demonstrate reservation or revalidation against the **assembled next request**, including pending injections, tool schemas, system material, attachments, outputs and output/control reserves.

Test many individually small admitted messages, concurrent tool results, an oversized source read, estimation error, and a small-window model. Reject/park/truncate to bounded artifact pointers before the next call exceeds the supported budget; record truncation, never silently omit authority-bearing instructions. The manifest's 8K/16K limits are maxima, not entitlements for a small-window model. The earlier lifecycle document's illustrative thresholds and v1's reference profile should not become two competing defaults.

### AG2 — Readiness is a versioned fact, not a boolean remembered by a model

**Requirements:** R30–R33, R38. The v1 rollover protocol already requires quiescence, checks changed inputs and forward-only recovery. No new architecture is needed. Its M4 test must bind readiness to the exact manifest hash and revision vector: common/per-agent inputs, assignment/driver, policy/control, workspace, accepted delivery inventory, and relevant decisions/approvals. The activation transaction must compare that evidence, not merely observe `ready=true`.

Exercise steering at every preparation boundary, late pin/injection, workspace changes after validation, missing rejected-approach rationale, false predecessor success claims, and a successor that fails after cutover. Verify one owner or a visible blocked state, bounded historical lookup, no transcript cloning, and no pointer-flip rollback. Deterministic checks cannot establish semantic completeness; independent read-only validation must retain an explicit uncertain/needs-decision outcome.

### AG3 — Release, auth and durability feasibility are correctly gated; select a failure model

**Requirements:** R12, R18, R22–R24. M0 correctly refuses to invent package compatibility from source SHAs, separates a credential-free fake-provider task from real auth, and permits an explicit blocker. Existing coding-agent login support is not proof that a Durable host can reuse its resource/auth stack safely. Test supported credential resolution and refresh without token copying or dumping errors/environment; unsupported provider support is a product-scope decision for the operator, not permission to silently select a paid fallback.

Record a supported OS/runtime/filesystem and storage durability configuration. Distinguish process kill/restart from power/host loss: a committed receipt's survival claim depends on SQLite/backend settings and filesystem behavior. Test exclusive open, rollback, torn/failed storage, reopen and unavailable task definitions against the **released dependency pin**, not only the research revision. Keep custom app docs and native submission state within one authoritative Session; use public supported transaction/task APIs or explicitly revisit the boundary.

## What is sound and should be retained

1. **Useful first product slice:** two independent persistent peers, real attachment, post-completion follow-up, observable delivery/results and restart are meaningful without shell, auto-spawning or UI breadth.
2. **Single replay owner:** Durable owns native scheduling/transcripts; app records own routing, authority and effect uncertainty. The outbox bridge is not inherently a second scheduler.
3. **Honest receipts and effects:** transport, durable admission, model response, accepted result and remote completion are distinct. Unknown remote effects block destructive replay; approvals bind exact revisions.
4. **Flexible identities:** logical address, incarnation, work assignment, driver, client attachment and role profile are separate. No fixed four-role topology or model ranking is encoded.
5. **Control at admission:** steering is recorded before model interpretation; direct human access is not routed through a coordinator. Late messages retain original provenance and cannot resurrect archived writers.
6. **Fresh rollover with evidence:** immutable history, bounded scoped handoffs, one prepared successor, settled ownership and forward epochs are easier to audit than opaque context transfer. Compaction can remain an optimization.
7. **Security honesty:** no arbitrary CLI extensions in the trusted host, no assumption that tool allowlists isolate shell, private credentials, network-denied executors and mutation-disabled unsupported platforms.
8. **Delivery and measurement discipline:** audit IDs start early; Jev is advisory, failure-visible and evaluated with neutral rubrics/independent outcomes. Native unknown pricing does not become zero cost.

## Operator experience and evidence provenance

The operator clarified during this review, after the pinned proposal, that the imported report is **one Argon model's analysis** of the broad brief. My independent intake checks are not a second raw-corpus analysis.

Separately, the operator reports carefully observed longitudinal experience: earlier single Opus 5 sessions cost approximately **$120–130/day**; the evolved setup costs approximately **$20–30/day for the Opus 5.5 worker**, **$10–15/day for Argon on its different billing basis**, **flat-rate Kimi**, and **$7–12/day for Fable**, with lower mental load and roughly twice-faster results. These are valuable firsthand product observations, not disproven by incomplete telemetry. Preserve them separately from Argon's extracted statistics. Different models, subscriptions, tasks and evolving workflow prevent a controlled decomposition of the gains, not recognition that the operator experienced them.

This clarification was correlated to the orchestrator by intercom; acknowledgment `57249f6b-9d05-467c-be88-9506444a04ba` confirms separate treatment. The orchestrator subsequently reports it recorded in `project/findings/2026-10-19-operator-workflow-experience.md` at main `f95d16b`, outside the pinned proposal; that later artifact was not used to change this review's proposal baseline. The design should preserve direct specialist access and distributed responsibility while reducing manual relay/replacement work. Evaluation should include operator intervention/mental-load feedback and end-to-end throughput, not just token totals or agent count.

## Deferrable work and disposition boundary

Defer remote transport, dashboards, service installation, generic plugin APIs, automatic compaction, adaptive role optimization, complete historical corpus recounting and universal cross-platform mutation support. Do not defer the minimum control/budget boundary needed by the capability actually shipped. Do not require all M5 features to demonstrate M1.

The author should disposition each AD finding separately with the amended contract and corresponding test gate. Subsequent review should focus on unresolved high-risk changes and actual M0/M3/M4 evidence rather than repeating the entire document review. Operator acceptance of scope/platform/provider remains explicit. This artifact does not authorize implementation, publication, service changes, credentials access or runtime mutation.
