# Progress and continuation

## Current phase

Research scaffold and initial upstream orientation. No runtime implemented, dependencies installed, package compatibility verified, recovery experiments run, or Astra review performed.

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
- Confirmed operator-selected, project-specific role layouts rather than a fixed four-role topology.
- Astra will initially be a separate Pi session using shared Markdown review artifacts. Added an explicit review protocol and a proposed focused high-thinking pass; exact model/levels are unconfirmed.
- Added the active Jev research track, including codemode, labeled evaluation, privacy, and budget requirements. Installed docs describe native Jev classifier support; no integration or calls have occurred. Missing catalog pricing may appear as zero cost and must not be mistaken for free service.

## Next session

1. Read `README.md`, this file, requirements, initial findings, and the source register.
2. Confirm remaining setup details: Jev route/budget, Astra model identifier, required chat providers, package versions, and project license.
3. Finish normative-spec and relevant implementation/test reading; inspect the confirmed legacy packages at pinned versions.
4. Build the behavior/replacement matrix before choosing an architecture. Prefer exploring a dedicated Durable host/client boundary over assuming a Pi CLI adapter is required.
5. Propose tightly scoped fake-provider recovery experiments, including direct follow-up to a finished agent and variable role layouts; record results without claiming product readiness.
6. Set up and exercise native Jev access only after access/configuration/privacy and budget checks; follow `research/jev.md`.
7. Write the concrete design and explicitly tell the operator **“Ready for Astra review”**. Review and operator acceptance precede product implementation.

## Local-only research material

`.local/upstream/` contains cached official README/spec/background-example files for the pinned source revision. It is ignored and is not required to navigate the public project; public pinned links live in `research/sources.md`. The original private export must not be used as public review material.
