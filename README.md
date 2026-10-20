# Pi Ensemble

A durable, human-steered multi-agent runtime built around Pi.

**Status: early design and research; no functioning runtime yet.** Research is a means to delivering the runtime, not the end product. This is an independent project, not an official Pi component.

## What we are building

An ensemble of persistent agents with separate contexts, complementary roles, direct peer communication, and shared project knowledge. Agents work autonomously within agreed boundaries; the human can attach to any agent, inspect its activity and summary, ask why it made a decision, or request an adjustment without routing through a coordinator.

Think **tmux for agent work**, not just a command that spawns disposable children: detaching the UI or finishing a task must not discard an agent's identity and useful context. Direct human steering must also update the driving agent so the rest of the ensemble stays aligned.

Layouts are operator-selected per project, with no fixed number of agents or mandatory role topology. The initial target is a local runtime portable between laptops. The design should accommodate future remote clients without requiring remote deployment in the first version.

When an ambiguity or consequential decision cannot be resolved from evidence and existing authority, the runtime should surface it rather than confidently build on an unsupported assumption. Preserving sound progress matters more than keeping an agent busy or a prompt cache warm.

## Start here

- [Pi Durable explained](docs/pi-durable.md) — concepts, diagrams, and the attachable-runtime analogy
- [Requirements](docs/requirements.md) — the current behavioral target
- [Architecture direction and alternatives](docs/architecture-options.md)
- [Concrete design proposal](docs/design.md) · [Implementation plan](project/implementation-plan.md) — v1.1 amended after independent review; operator acceptance pending, not implemented guarantees
- [Project structure and reading protocol](docs/project-structure.md)
- [Context lifecycle](docs/context-lifecycle.md) — budgets, wind-down, handoff, and stable-address replacement (proposed)
- [Bounded task contract](docs/task-contract.md) — project substrate, continuation tasks, horizontal workers, and routing (proposed)
- [Legacy workflow](docs/reference/pi-setup-legacy.md) — reformatted, sanitized reference
- [Decision models](docs/decision-models.md) — Jev integration and secure access
- [Observability](docs/observability.md) — portable measurement, beyond native token totals
- [Documentation index](docs/index.md)

## Current work

[Project tracker](project/README.md) · [Progress](project/progress.md) · [Open questions](project/variables.md) · [Next-session handoff](project/continuation-prompt.md)

The preferred direction is a **Pi Durable host with attachable clients**. A thin Pi CLI extension could be an additional client later, not the owner of agent lifetime. We are studying official and community implementations and the existing workflow before finalizing the host design.

Work-session report intake and bounded upstream/ecosystem source inspection are complete, with evidence caveats and unrun tests explicit. Astra reviewed concrete design v1; v1.1 and its phased plan address four mechanical contract findings. Astra’s focused amendment check found no remaining high-risk contract blocker. Operator acceptance and runtime proof gates remain pending. Start with a recovery-tested vertical slice and grow it into a functioning runtime. Manual review coordination is a bootstrap mechanism; the runtime should eventually support the same review exchange itself.

## License

[MIT](LICENSE).
