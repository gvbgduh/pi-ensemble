# Pi Ensemble

Researching a durable, human-steered multi-agent workspace built around Pi.

**Status: research and design scaffold. No runtime implementation yet.**
This is an independent project, not an official Pi component.

## Why

The starting workflow uses long-lived worker, orchestrator, manager, and adviser sessions, peer messaging, bounded subagents, and shared Markdown project trackers. The question is whether `@earendil-works/pi-durable` can replace the coordination infrastructure without losing role separation, direct human steering, or inspectable project state.

Replacing packages is not the objective by itself. Preserving useful behavior while improving recovery, control, and observability is.

The initial target is a local setup, portable to another laptop, with a host/client boundary that can accommodate remote access later. Agents must remain directly addressable with their context after completing work. The UI is interchangeable; activity, logs, summaries, and direct conversation are essential. Role layouts are operator-selected per project rather than hard-coded to four agents.

## Start here

- [Documentation index](docs/index.md)
- [Legacy workflow reference](docs/reference/pi-setup-legacy.txt) — sanitized source export
- [Requirements](docs/requirements.md) — behavior to preserve and improve
- [Research plan](docs/research/plan.md) — questions, evidence, and experiments
- [Initial findings](docs/research/initial-findings.md) — preliminary, source-linked observations
- [Sources](docs/research/sources.md) — pinned upstream references and evidence status
- [Jev research track](docs/research/jev.md) — active, measurable decision-model experiments
- [Architecture options](docs/architecture-options.md) — alternatives, not a commitment
- [Decisions](docs/decisions/README.md)
- [Astra review brief](docs/reviews/astra-brief.md)
- [Progress and next session](docs/progress.md)

## Is this a Pi extension?

Undecided. Pi CLI extensions, Pi packages, the coding-agent SDK, and Pi Durable are different integration surfaces. A durable host with an optional Pi CLI adapter is one hypothesis; extending the existing multi-session workflow is another. We will choose after source research, failure-injection experiments, and independent design review.

## Research sequence

1. Extract requirements from the existing workflow.
2. Inspect pinned upstream documentation, implementation, and examples.
3. Compare replacement, coexistence, and incremental migration options.
4. Write a concrete design and review it with Astra and the human operator.
5. Build the smallest recovery-tested vertical slice, then expand.

No package manifest or extension entry point is included yet: doing so would prematurely choose an integration surface.

## Public repository boundary

Only generic workflow design and public upstream research belong here. The original legacy export is kept locally at `.local/imports/pi-setup-legacy.txt`, excluded from Git because it includes third-party comments and workplace details. The tracked reference removes that material and export boilerplate; it is not an endorsement of historical model-performance claims.

Credentials, transcripts, databases, real work trackers, and machine-local configuration must remain outside tracked files. Global agent memory is not stored in this repository.

A redistribution license has not been selected yet.
