# Continuation — fresh session in Pi Ensemble

## Common core

**Goal:** deliver a functioning local-first Pi Durable multi-agent runtime, not a research project. Repository: `~/Projects/pi-ensemble`; public remote: https://github.com/gvbgduh/pi-ensemble. This handoff replaces the need to load the previous long conversation.

**State:** early evidence/design only. No runtime, custom metrics package, recovery experiments, or Astra review yet. Three direct TypeSafe Jev synthetic smoke calls succeeded; native codemode and tool search are available. Previous documentation milestones were pushed to `main`; verify clean Git/remote state before starting.

**Read selectively:** `AGENTS.md`, this handoff, the current table in `project/progress.md`, then relevant docs/variables. `docs/` owns current best product knowledge; `project/` owns dynamic work/evidence/history. Tracker files are optional tools, not a requirement to fill empty templates or use GitHub issues. Commit/push checked milestones frequently and keep README/AGENTS/current docs aligned. Interpret metaphors as intent, not extra literal requirements.

### Settled constraints

- Prefer Durable host with attachable clients (C); Pi CLI adapter (D) optional later. Local/portable first, future remote clients considered. No fixed role count/topology; direct interaction with any persistent agent.
- Direct steering must durably inform the responsible driving agent and reconcile affected plans. Consequential ambiguity surfaces to a decision owner rather than confident guessing; unaffected work can continue.
- Frugal role-specific contexts, minimal unnecessary overlap: aim below 300K normally; chosen ceilings no higher than 300–400K or smaller model limits with reserves. Early wind-down, compact verified handoff, stable logical addresses, internal incarnations/epochs. Detailed lifecycle/thresholds are proposed, not approved or implemented.
- Bounded self-contained tasks use revisioned project-MD pointers. Unfinished work becomes a continuation task. Common core + per-agent focus is the default briefing; don't clone parent/sibling histories. Host-controlled provisioning/rerouting should remove routine manual spin-up and visible session counters, not internal ownership/provenance.
- Jev direct TypeSafe is selected; use it and measure decisions/outcomes beyond native totals. Operator-reported price $0.042/M input, zero output price. Keep caller expectations out of questions/evidence, use neutral versioned rubrics/counterevidence/unknowns, and test framing sensitivity. No rephrasing until a preferred answer appears.
- Astra is via the operator's OpenAI subscription inside Pi; independent review of a concrete design/plan is still pending. MIT accepted. Compare latest legacy packages at recorded inspected SHAs.

## Jev setup: exact handoff state

The prior session read Pi 1.3.0 provider/settings/configuration/security docs and enabled `"defaultTools": ["+codemode", "+tool_search"]` in `~/.pi/agent/settings.json`, preserving other settings. A private backup exists outside tracked files. A fresh Pi process should load it; `/reload` enables it in an existing session. Web-access is installed too, but the new tools were not exposed to the old agent turn. Codemode's `searchTools()` helper is distinct from the standalone `tool_search` tool; the latter discovers/activates deferred tools.

**Current verification:** operator already logged in; `jev-latest` was available and three synthetic `models.classify` calls succeeded. No further login requested. See `project/findings/2026-10-19-jev-smoke.md` and JSON record. Do not request a key in chat, print auth files, or put secrets in commands/Git. Stored auth contents were not examined.

Smoke completed: 1,276 input / 144 output tokens, $0.000053592 rate-based estimate, three expected labels. Framing tests, Durable adapter, and portable instrumentation remain untested. Native zero-cost metadata is not billing proof. Do not claim setup complete before a successful call. See `docs/decision-models.md` and `docs/observability.md`.

## Agent focus: design/implementation author

1. Jev smoke is complete; use suitable typed calls with neutral rubrics and explicit provenance. Current bootstrap roster: `pi-ensemble-001-orchestrator`, Luna `pi-ensemble-001-worker`, Astra `pi-ensemble-001-adviser`, reachable through intercom. Session ceiling 272K, wind down 220–230K.
2. Obtain the actual public-safe Argon report and optional sanitized project/status/spin-up skills/templates; raw workplace sessions stay at work. The analysis instructions remain in `project/research/work-setup-analysis-brief.md`. Correct report `project/reports/work-setup-insights.md` received; orchestrator and Astra each read all 716 lines. See intake findings for numerical/inference caveats. Interpret as evolution from solo/model/orchestration experiments, not a stable legacy topology; only two final episodes used four roles.
3. UPSTREAM-01 rev1 integrated as `2ebd87b`; ECOSYSTEM-01 rev1 as `91e58c9`. Read the compact Durable/ecosystem findings only as needed; source/test inspection complete for bounded comparison, all execution tests unrun. Luna has no active task. Latest subagents can revive eligible retained context; Intercom mailbox is broker-memory state. No new research breadth is required before review.
4. Concrete `docs/design.md` v1 and `project/implementation-plan.md` v1 now exist with evidence, one-store/native-inbox bridge, host/client protocol, steering holds, authority/effects, quiescent rollover, metrics and gated M0–M5 delivery. First slice is tool-restricted attachable persistent peers; mutation waits for demonstrated sandbox/ownership. Release/provider/OS compatibility remains a gate, not a guarantee.
5. Astra review milestone is ready: send committed proposal SHA to live adviser with bounded focus; separate findings and disposition, then operator acceptance before consequential implementation. M0 fake-provider compatibility/fault spike is the proposed first task after acceptance.

## Agent focus: Astra reviewer — live, standing by for proposal

Astra completed independent full-report intake through intercom (EVIDENCE-REVIEW-01); no design review started. Use `project/reviews/astra-brief.md` against a specified proposal revision, not this entire historical chat. The full Argon report has been read; carry its intake caveats into design review. Challenge rollover/handoff fidelity, routing/assignment races and ownership, per-agent scoped continuation and automatic lifecycle, autonomous decision escalation, and Jev question bias/portable metrics. Write separate findings and dispositions. A focused high-thinking pass is proposed if supported, not a proven optimum.

## Useful inputs, not blockers for all work

Creator-posted Slack example links/sanitized excerpts; safe work-session report and skills; exact Astra launch ID if needed; broader provider requirements. Do not start another giant documentation pass without turning source evidence into the concrete design and first implementation plan.
