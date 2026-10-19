# Progress and continuation

## Current phase and workstreams

Deliver a functioning runtime; current phase is evidence intake and concrete design. Codemode and tool search have been enabled in local Pi settings; secure TypeSafe login confirmation and a smoke test are pending. No runtime, portable instrumentation, recovery experiments, Jev calls, or Astra review is implemented/performed yet.

| Stream | State | Next |
| --- | --- | --- |
| Product/tracker foundation | Updated with operator review | Keep README, AGENTS, docs and tracker aligned |
| Work-setup evidence | Analysis brief prepared; report not received | Operator shares brief with work agent; optional sanitized skills/templates |
| Substrate/ecosystem | Official Pi and latest legacy/Cloudflare docs partially read | Complete normative/source/tests comparison; inspect community implementations |
| Host/client design | C preferred; D optional; context-rollover proposal added | Concrete ownership, attachment, steering/ambiguity and context-replacement contracts |
| Jev and measurement | Direct TypeSafe selected; codemode/tool search enabled; login/test pending | User `/login typesafe`, then reload/fresh Pi; neutral smoke test and usage/cost recording |
| Independent review | Brief expanded; no review yet | Highlight the Astra milestone when concrete design/evidence are ready |

## Milestone log

### 2026-10-19 — Initial scaffold

- Created the documentation-first scaffold under a provisional name; final project path is `~/Projects/pi-ensemble`.
- Moved the original Downloads legacy export to ignored `.local/imports/pi-setup-legacy.txt`.
- Prepared a public sanitized reference excluding third-party discussion, workplace-specific details, and export boilerplate.
- Extracted initial role, multi-repository, tracker, recovery, safety, and evaluation requirements.
- Verified GitHub source access; read the official Durable README and persistent-background-agent example at commit `1402973a9dbe8c627341e04c9744c54953e285ca`.
- Added sources, research plan, architecture alternatives, and a draft Astra review brief.
- Confirmed that the installed `pi-web-access` package is configured, but its tools were not available in the active research turn. No global settings changed.
- Operator reiterated that the original legacy file must stay out of public Git and requested explicit clarification questions and a highlighted Astra review gate.

### 2026-10-19 — Operator clarifications and final name

- Operator selected **Pi Ensemble**; renamed the local project to `~/Projects/pi-ensemble`.
- Confirmed UI flexibility: agent activity/logs, summaries, and direct interactive access matter, not a particular TUI. Persistent agents must remain available with context after task completion for explanations and adjustments.
- Confirmed local-first, laptop-portable target with future remote clients considered in design, not implemented in the MVP.
- Confirmed `nicobailon/pi-subagents` and `nicobailon/pi-intercom`; installed versions remain unknown.
- Confirmed operator-selected, project-specific role layouts rather than a fixed agent-count topology.
- Astra will initially be a separate Pi session using shared Markdown review artifacts. Added an explicit review protocol and a proposed focused high-thinking pass; exact model/levels are unconfirmed.
- Added the active Jev research track, including codemode, labeled evaluation, privacy, and budget requirements. Installed docs describe native Jev classifier support; no integration or calls have occurred. Missing catalog pricing may appear as zero cost and must not be mistaken for free service.

### 2026-10-19 — Public scaffold published

- Created and pushed the public repository: https://github.com/gvbgduh/pi-ensemble (`main`).
- Verified local Markdown links, clean diff whitespace, and exclusion of private imports; a limited credential-pattern scan found no matches. This is not a comprehensive security audit.
- Original legacy export and upstream research cache remain ignored and were not committed. Only the sanitized workflow reference was published.
- No license selected yet, no application/runtime code shipped, and no global Pi settings changed.

### 2026-10-19 — Operator documentation review and tracking split

- Reframed the objective as delivery of a functioning runtime, not research for its own sake. C is preferred; D is an optional client extension.
- Separated current product knowledge (`docs/`) from project state/evidence/history (`project/`) and documented selective reading/update conventions. Added tracker backbone and Markdown legacy reference.
- Removed public/private implementation details from the product README while retaining ignore and publication safeguards.
- Accepted MIT. Adopt latest legacy-package baseline, pinning inspected revisions rather than blocking on work installation versions.
- Added work-session analysis brief, including corpus/episode coverage, counterexamples, public-safe evidence and optional skill/template intake.
- Confirmed direct TypeSafe Jev access and operator-reported $0.042/M input, zero output price; usage is selected, secure key provisioning remains open. Added portable decision/outcome instrumentation requirements beyond native metrics.
- Confirmed Astra via OpenAI subscription inside Pi. Manual review coordination is bootstrap-only, not a ritual or a permanent dependency on the human relay.
- Added direct-steering notification/reconciliation and consequential-ambiguity/decision-owner requirements, including affected-scope blocking and prevention of throwaway work.
- Read Cloudflare's Pi harness guide, example README/notes, and official Cloudflare example README. Notes include older limitations; version comparison is required. Latest subagents docs describe retained-child resume; no runtime behavior was tested.

### 2026-10-19 — GitHub tracker terminology and bounded context lifecycle

- Used `project/prs.md` for GitHub, removing the legacy MR filename from current conventions. Expanded `docs/project-structure.md` with per-file outlines, layering, selective reads, and maintenance rules.
- Committed/pushed the documentation/tracker split and operator feedback as `ee44b33`; current product/runtime implementation status remains explicit.
- Added `docs/context-lifecycle.md`: proposed early wind-down, bounded verified successor briefing, logical-address/incarnation separation, routing epochs, mutation ownership, pending/late-work reconciliation, failure/rollback cases, and metrics.
- Captured the operator's frugal distributed-context constraint: normal target below 300K; ceilings no higher than the 300–400K upper range or smaller model limits with reserves. Example thresholds are unreviewed, not implemented defaults.
- Explicitly added rollover/handoff/routing to the Astra brief, work-setup analysis brief, requirements, open variables, and planned fault experiments. No Astra review or rollover experiment has run.
- Operator connected rollover with horizontal worker allocation. Added `docs/task-contract.md`: bounded self-contained tasks using project Markdown, explicit continuation items, pre-dispatch context-fit checks, compact coordinator state, and assignment-aware rerouting distinct from incarnation replacement. Added corresponding Astra questions and tests; no implementation is claimed.

### 2026-10-19 — Classifier question-integrity constraint

- Operator highlighted the risk that Jev merely confirms bias already encoded in LLM-authored questions/criteria/state.
- Added neutral versioned rubrics, observation/interpretation separation, expected-answer leakage prevention, counterevidence/unknown handling, controlled framing tests, and independently labeled outcomes to integration/metrics/requirements and Astra review.
- Added an explicit experiment and work-session analysis question. No claim of validated neutrality or performed classifier testing.

### 2026-10-19 — Scoped continuation map and automatic lifecycle

- Operator proposed per-agent continuation focus and automatic driver-notified replacement instead of manual incremental sessions/suffixes.
- Added the proposed small common core plus per-logical-agent sections/linked files, scoped deterministic host delivery, aggregate-update authority, task-result/continuation/replacement signals, and bounded automatic provisioning/rerouting.
- Preserved internal incarnation IDs/epochs, ownership and provenance; no claim that underlying conversations or failure bookkeeping disappear. Added Astra questions, requirements, and a fault experiment.
- Committed/pushed context/task lifecycle and Jev question-integrity refinements as `f462d22`; source docs/tracker split was pushed as `ee44b33`.

### 2026-10-19 — Jev prerequisite setup and fresh-session handoff

- User confirmed next priorities: Jev setup, work-session insights/upstream evidence, then concrete design and high-level implementation plan for Astra review.
- Read installed Pi provider/configuration/settings/security/session docs. Enabled `defaultTools: ["+codemode", "+tool_search"]` in the user's local Pi settings, preserving existing tools/settings; private backup retained. This setting is not committed to the public repository.
- Did not read/print auth-file contents or receive a key. Auth-file permission is 0600; `TYPESAFE_API_KEY` was absent from this process, which does not establish whether a stored credential exists. User should use `/login typesafe` privately, then reload/start fresh. No Jev call or provider authentication test has run.
- Clarified that tracker files are optional, especially issue/ticket maps. Existing placeholder can remain; no need to create GitHub issues merely for scaffolding. Treat metaphors as intent rather than literal architecture requirements.
- Enabled standalone `tool_search` alongside codemode at the user's request. Codemode's internal `searchTools()` is separate; the standalone tool helps discover/activate deferred tools. Fresh process/reload and actual availability remain to verify.
- User requested winding down this growing conversation and starting a fresh Pi session from the repository directory. Rewrote the compact continuation with setup status, scoped author/reviewer focus, next actions, and honest pending checks. Do not import the full prior transcript into the successor.

## Handoff

Use [continuation-prompt.md](continuation-prompt.md) for next actions and [variables.md](variables.md) for unresolved details. Preserve dated observations; promote current conclusions to product docs.
