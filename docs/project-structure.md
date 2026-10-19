# Project structure and reading protocol

## Two kinds of Markdown

**`docs/` is the current best account of the product.** It explains what the system is, its intended/implemented behavior, interfaces, operation, and current design. It can contain explicit open/proposed status; it is not immutable or a claim that the design is final.

**`project/` is the project's working memory.** It records current work and the evidence/history behind it: decisions, open variables, findings, reviews, issue/PR state, and handoff. Its current-state tables change; its dated evidence and session logs are preserved.

Git history is valuable but does not replace a readable decision record or current handoff. Conversely, a long chronological log is not a good onboarding guide. Promote conclusions into `docs/` and link the evidence rather than copying the whole investigation.

## Present repository layout

```text
pi-ensemble/
├── README.md                 # product entry point and honest current status
├── AGENTS.md                 # working rules and selective reading protocol
├── LICENSE
├── docs/                     # current product knowledge
│   ├── index.md
│   ├── requirements.md
│   ├── architecture-options.md
│   ├── pi-durable.md         # substrate explained, with diagrams
│   ├── project-structure.md  # this convention
│   ├── decision-models.md
│   ├── observability.md
│   └── reference/
│       ├── index.md
│       └── pi-setup-legacy.md
└── project/                  # development tracker for Pi Ensemble itself
    ├── README.md             # goal, scope, strategy, related context
    ├── index.md              # navigation, not a second README
    ├── progress.md           # current workstream table + dated session log
    ├── decisions.md          # accepted/superseded decisions with IDs
    ├── variables.md          # open questions, trade-offs, watch items
    ├── tickets.md            # issue map, including explicitly untracked work
    ├── prs.md                # GitHub pull-request/change map
    ├── continuation-prompt.md# compact current handoff
    ├── research/             # plans, source register, analysis briefs
    ├── findings/             # dated evidence and experiment outcomes
    ├── reports/              # status snapshots and imported safe reports
    └── reviews/              # review briefs, findings, dispositions
```

Source packages and tests are intentionally absent today. Add their layout when the runtime boundaries are agreed; do not imply a monorepo/package split is already decided.

## Tracker backbone and hierarchy

Use the legacy workflow's predictable tracker backbone: `README.md`, `index.md`, `progress.md`, `decisions.md`, `variables.md`, `tickets.md`, `prs.md`, and `continuation-prompt.md`. Empty external maps explicitly say none exist; never fabricate issue or PR IDs.

Create subtrackers only for independently substantial workstreams. A subtracker lives under `project/<workstream>/`, links its parent/siblings, and carries its own backbone as needed. Small tasks stay in a table, not a new directory forest. Each directory has an `index.md` navigation map. Context links should identify purpose and authority rather than copy parent text.

This is the development repository's tracking convention. The runtime's future support for user projects may adopt it as a template, but the runtime must not require every project to use this exact tree.

## Document outlines and layering

Use predictable sections, not a growing stream of unrelated notes. A document has one primary purpose and links its neighbors.

| File/type | Recommended outline |
| --- | --- |
| Root README | Product purpose → honest current status → primary entry points → current direction/next milestone → license |
| AGENTS | Delivery objective → reading protocol → update/work rules → safety → current constraints/review workflow; link detailed contracts rather than copy them |
| Current design doc | Purpose/status and scope → concepts/contract → diagrams or examples → behavior and failure boundaries → acceptance criteria → evidence links/open decisions |
| Tracker README | Goal → strategy/current phase → scope → Related Context table → artifact links |
| Index | Every immediate file/subdirectory with a one-line purpose; no duplicate design narrative |
| Progress | Current phase/workstream table with state/next action → append-only dated milestone log → current handoff link |
| Decisions | ID/name/status index → accepted entries with context/evidence/trade-off → explicit supersession links; substantial ADRs can be linked separately |
| Variables | Stable ID → question/options/impact → owner or next evidence → current state; resolved items link a decision rather than disappear unexplained |
| Tickets / PRs | External ID/link → owner/scope → current verified state → dependencies/reviewed revision → last known evidence; explicitly say none if empty |
| Continuation | Goal/current state → just-needed decisions/constraints → blockers and next actions → relevant artifact pointers; refresh, do not append indefinitely |
| Findings / reports | Date/revision and question → method/coverage → observations/evidence → interpretation/confidence/counterexamples → implications/next test |
| Review | Proposal revision/scope/questions → independent prioritized findings → evidence/failure scenario → author dispositions in a separate artifact → acceptance/residual risk |

State whether behavior is **implemented, proposed, documented upstream, observed in a test, or operator-reported**. Avoid front-loading every file with an exhaustive changelog; chronology belongs in the tracker and Git. A current doc may cite a dated finding without inheriting its outdated conclusions.

Layer from project goal to workstream to bounded task. Keep top-level summaries small and reference details by path, section, ID, and revision. Do not reproduce the same prose at each layer. For a substantial subproject, use Related Context to inherit authority explicitly; do not silently assume parent decisions apply to a different scope.

## Reading by task

| Reader/task | Read first | Load selectively |
| --- | --- | --- |
| New contributor | README, current docs | Evidence linked from a relevant design claim |
| Resuming agent | AGENTS, continuation, current progress table | Relevant variables/decisions and recent deltas |
| Coordinator | Goals, workstream/PR topology, decisions, handoff | Findings summaries and blockers; not every implementation transcript |
| Implementer | Approved task/design, workspace contract, relevant acceptance criteria | Exact source/tests and dependency state |
| Adviser/reviewer | Proposal revision, requirements, evidence, explicit review questions | Independent sources and disputed findings |
| Status report | Current tracker tables, continuation, cheap live Git/remote checks | History only to explain deltas |

Do not read all chronology before every task. Verify live state before acting on stale tracking. A source artifact can establish an observation without becoming current policy.

## Update and promotion rules

1. At a milestone, update current tracker tables and compact handoff in the same change as the work.
2. Append dated results; preserve earlier evidence and decision IDs. Record supersession rather than erase a wrong assumption.
3. Update docs, README, and AGENTS immediately when the current contract, behavior, structure, or working convention changes.
4. Record open decisions in variables rather than presenting them as settled architecture.
5. Keep detailed drafts/review cycles in `project/`; publish the current design and concise rationale in `docs/`.
6. Commit/push coherent reviewed changes frequently. Separate writers' artifacts and do not let a reviewer silently overwrite the proposal it reviews.
