# Continuation

## Objective and state

Build Pi Ensemble: a functioning durable local agent runtime with attachable clients, persistent independent contexts, project-specific layouts, and direct human access. Research serves implementation. No runtime or instrumentation is implemented yet; no recovery tests, Jev calls, or Astra review have run.

## Read selectively

Start with `AGENTS.md`, `docs/project-structure.md`, the current table in `project/progress.md`, relevant requirements, and open variables. Product knowledge lives in `docs/`; evidence/plans/history live in `project/`. Update README/AGENTS as conventions or behavior change. Commit and push coherent checked milestones often.

## Confirmed direction

- Prefer a Pi Durable host with attachable clients (C); Pi CLI adapter (D) is optional later.
- Local and portable first, future remote clients considered. No fixed agent count/topology.
- Task completion must not destroy agent context. Direct steering must notify the driving agent and reconcile dependent plans without requiring an orchestrator hop.
- Escalate consequential uncertainty rather than confidently guess; preserve unaffected parallel work.
- Jev is selected through direct TypeSafe API access, including codemode. Operator-reported price: $0.042/M input tokens, zero output price. Secure key provisioning is still needed; no key in chat/Git/argv.
- Astra is used via an OpenAI subscription in another Pi session initially. Use separate review artifacts; flag the review milestone explicitly, then use Ensemble itself when safely possible.
- MIT accepted. Legacy package baseline is latest, with inspected SHAs recorded.

## Next actions

1. Give the operator `project/research/work-setup-analysis-brief.md`; accept a sanitized report and optional project/status/spin-up skills/templates. Raw sessions stay at work.
2. Continue source comparison: pinned official Pi/Cloudflare examples and SDK, latest legacy packages, and community applications. Verify dated notes against current APIs rather than copying stale limitations.
3. Complete the concrete host/client, ownership, steering-update, ambiguity, and observability design with evidence and high-risk spikes.
4. Provision Jev through supported private credential storage and exercise it; implement portable decision/outcome metrics, not just token counters. Review Pi setup/security docs before changing global configuration.
5. Highlight the Astra review handoff when the design is ready; independent review and operator acceptance precede committing to consequential runtime architecture.

## Inputs still useful

Creator-posted Slack example links or sanitized excerpts; exact Astra model ID if needed for launch; broader provider requirements. Do not block source research waiting for these.
