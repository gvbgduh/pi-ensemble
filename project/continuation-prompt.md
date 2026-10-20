# Continuation — Pi Ensemble

## Common core

Goal: deliver a functioning local-first persistent multi-agent host with attachable clients. Repo `~/Projects/pi-ensemble`, public remote `https://github.com/gvbgduh/pi-ensemble`. Read `AGENTS.md`, README/current progress table, then only task-relevant evidence. Do not import the old conversation or whole tracker.

**Current state:** bounded report/source research complete; no runtime or recovery/sandbox experiments. Direct TypeSafe Jev codemode smoke succeeded. Astra reviewed proposal `ab0d906`; findings integrated as `618a4b3`. Design/plan **v1.1** and separate author disposition address AD1–AD4; implementation tests and **operator acceptance remain pending**. No implementation authorization is inferred from review.

### Settled constraints

- C (Durable host with attachable clients) preferred; D optional. Local/portable first, no fixed role count/models/topology. Task completion, attachment, context and agent lifetime differ.
- Direct steering must durably update responsible drivers without a relay/approval hop. Consequential ambiguity pauses affected scope; unaffected authorized work can proceed. Actual executor fencing, not prompts/aliases, governs mutation.
- Frugal common-plus-agent-focus briefing and bounded tasks, not transcript cloning or recursive summaries. Stable logical address with incarnation/epoch and assignment-aware routing. Current bootstrap session ceiling 272K, wind down 220–230K; smaller model windows override large-window policies.
- Jev direct TypeSafe selected; neutral versioned rubrics/counterevidence/unknowns, blinded caller baseline, meaningful action/outcome records. Classifier never authorizes. No hidden paid fallback or credentials in tools/clients/telemetry.
- Keep docs/current knowledge distinct from tracker/history, preserve evidence, commit/push checked coherent milestones, isolate parallel writers. No global settings/service changes as documentation side effects. Metaphors express intent, not literal architecture.

## Evidence map — retrieve selectively

- `project/findings/2026-10-19-jev-smoke.md` + JSON: three successful synthetic native codemode calls, 1,276 input/144 output tokens, $0.000053592 operator-rate estimate ($0.042/M input, zero output). Native zero cost is not invoice proof; framing validation, host adapter and portable measurement untested. No further login requested; credential contents never inspected.
- `project/reports/work-setup-insights.md`: correct Argon report, all 716 lines read by orchestrator/Astra; analysis instructions remain in research/. ONE model analysis, not independent corpus replications. Evolution: solo/bounded helpers → persistent pairs/triads → only two final four-role episodes. Intake and independent caveats are linked from reports index.
- `project/findings/2026-10-19-operator-workflow-experience.md`: separate firsthand observations, earlier Opus ~$120–130/day versus later worker ~$20–30, Argon ~$10–15 (different billing basis), flat-rate Kimi and Fable ~$7–12; lower mental load, roughly twice-faster results. Statistical caveats do not dismiss experience.
- `project/research/durable-source-findings.md` and `ecosystem-source-findings.md`: exact pinned official/latest-package/Cloudflare/community inspection. Source/tests read, not executed. Latest subagents revive eligible retained context; broker mailbox is memory-bound; Durable is not a daemon/auth/lock/exactly-once service. Released compatibility remains M0 gate.

## Agent focus: orchestrator / author

Live bootstrap aliases: `pi-ensemble-001-orchestrator`, Luna `pi-ensemble-001-worker`, Astra `pi-ensemble-001-adviser`; list before reusing session IDs. Luna's source tasks UPSTREAM-01/ECOSYSTEM-01 are complete/integrated; no active implementation task. Last observed Luna context ~183K; do not assign broad reads near wind-down.

1. Read `project/reviews/astra-design-v1-findings.md` and separate `astra-design-v1-disposition.md` as needed. AD1: serialized delivery claim/pin/immutable native draft + wake recovery. AD2: actual startup execution gate—native submit/wait implicitly resume; withholding explicit resume is insufficient. AD3: host-death orphan containment proof before new writer. AD4: revisioned per-steering reconciliation/release, not generic ack flag.
2. `docs/design.md` + `project/implementation-plan.md` v1.1 contain amendments and M0–M5 gates. Get focused Astra check only for unresolved high-risk amendment, then ask operator acceptance/authorization for **M0-01 only**. No claim tests passed from a prose amendment.
3. Proposed M0-01: isolated credential-free fake-provider native/app bridge, control/implicit-resume gate, crash barriers/dedupe and exclusive-lock proof against compatible released pins. Native auth feasibility separate; do not copy tokens. Report unsupported API/platform/provider as blocker rather than invent another scheduler or hidden fallback.
4. Restricted M1 peers need minimum stop/restart and aggregate-next-request budget safety. General shell/mutation waits for M3 demonstrated containment/approval/ownership. M4 readiness binds manifest hash/full revision vector, quiescence and input races. Scope/platform acceptance remains explicit.

## Agent focus: Astra

ASTRA-DESIGN-01 rev1 completed in isolated branch `review/astra-design-v1`; independent commit `51fdb2b9dba716420fbd64db328f00ab1eaae25a`, integrated `618a4b3`. Recommendation: keep C, conditionally accept after four contract clarifications; separately authorized bounded M0 may proceed. Review is not human acceptance. Do not rewrite original findings; any amendment follow-up is separate and narrowly scoped.

## Next decision

Operator acceptance of v1.1 direction and permission for bounded M0 proof; no runtime implementation, mutation-enabled platform promise, external action or service installation authorized yet. Public-safe optional skills/creator links are useful but no further broad research is needed before that decision.
