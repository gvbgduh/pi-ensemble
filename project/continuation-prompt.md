# Continuation — fresh session in Pi Ensemble

## Common core

**Goal:** deliver a functioning local-first Pi Durable multi-agent runtime, not a research project. Repository: `~/Projects/pi-ensemble`; public remote: https://github.com/gvbgduh/pi-ensemble. This handoff replaces the need to load the previous long conversation.

**State:** early evidence/design only. No runtime, custom metrics package, Jev calls, recovery experiments, or Astra review yet. Previous documentation milestones were pushed to `main`; verify clean Git/remote state before starting.

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

**User action:** `/login typesafe`, enter the key only in Pi's private credential UI, then reload or start fresh. Do not request a key in chat, print auth files, or put secrets in commands/Git. `auth.json` permission was 0600. Only environment-variable presence was checked (`TYPESAFE_API_KEY` absent); stored auth was not examined, and authentication is untested.

After login/tools are available: discover the direct TypeSafe classifier, run a few small public/synthetic neutral cases, record outcomes/usage and explicit rate-based cost estimate. Native zero-cost metadata is not billing proof. Do not claim setup complete before a successful call. See `docs/decision-models.md` and `docs/observability.md`.

## Agent focus: design/implementation author

1. Finish Jev smoke test when the user has logged in; otherwise proceed with source research without secrets.
2. Give the work agent `project/research/work-setup-analysis-brief.md`. Await a public-safe report from roughly 70–80 sessions and optional sanitized project/status/spin-up skills/templates; raw workplace sessions stay at work. No report/bundle received yet.
3. Continue upstream/implementation comparison in parallel. `project/research/sources.md` records exact Pi/Cloudflare/latest legacy-package snapshots and what was actually read. Full normative/source/test review and community inspection remain incomplete. Cloudflare has SDK integration as well as a standalone example; notes may be stale. Latest subagents documents retained resume, so do not assume all completed context is irretrievable.
4. Incorporate evidence into a concrete design (`docs/design.md`, to create) and high-level implementation plan (`project/implementation-plan.md`, to create): boundaries, reused components, message/ownership contracts, context/continuation routing, failure cases, metrics/security, acceptance tests, phased first vertical slice and rollback. Prioritize risk-changing spikes rather than demanding a complete runtime before review.
5. Explicitly flag the Astra review milestone once that proposal is coherent. Obtain independent findings and operator acceptance before consequential architecture implementation.

## Agent focus: Astra reviewer — planned, not running

Use `project/reviews/astra-brief.md` against a specified proposal revision, not this entire historical chat. Challenge rollover/handoff fidelity, routing/assignment races and ownership, per-agent scoped continuation and automatic lifecycle, autonomous decision escalation, and Jev question bias/portable metrics. Write separate findings and dispositions. A focused high-thinking pass is proposed if supported, not a proven optimum.

## Useful inputs, not blockers for all work

Creator-posted Slack example links/sanitized excerpts; safe work-session report and skills; exact Astra launch ID if needed; broader provider requirements. Do not start another giant documentation pass without turning source evidence into the concrete design and first implementation plan.
