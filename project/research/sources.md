# Source register

Access date: 2026-10-19. Documentation/source reading is not a successful integration test. Follow latest upstream as requested, while recording the exact inspected SHA for reproducibility; `main` snapshots are not selected runtime dependency versions.

## Bounded completion and execution limits

Official normative/source/test inspection is recorded in [Durable findings](durable-source-findings.md). Latest legacy-package, Cloudflare and selected community comparison is recorded in [ecosystem findings](ecosystem-source-findings.md), with exact paths and SHAs. These complete the bounded design-research assignment, not exhaustive ecosystem/security review. No upstream tests, builds, demos, recovery or sandbox experiments ran. Selected released dependency compatibility is an M0 implementation gate.

## Official Pi

Repository: https://github.com/earendil-works/pi

Snapshot: `1402973a9dbe8c627341e04c9744c54953e285ca`.

| ID | Pinned source | Inspection status |
| --- | --- | --- |
| S1 | [Durable README](https://github.com/earendil-works/pi/blob/1402973a9dbe8c627341e04c9744c54953e285ca/packages/durable/README.md) | Read in full; not experimentally verified. |
| S2 | [Normative spec](https://github.com/earendil-works/pi/blob/1402973a9dbe8c627341e04c9744c54953e285ca/packages/durable/docs/spec.md) | Luna reports complete normative review and selected source/test inspection; [findings](durable-source-findings.md). Tests not run. |
| S3 | [Background agents](https://github.com/earendil-works/pi/blob/1402973a9dbe8c627341e04c9744c54953e285ca/packages/durable/test/examples/23-subagent-background.ts) | Source read in full; not run. |
| S4 | [Foreground child](https://github.com/earendil-works/pi/blob/1402973a9dbe8c627341e04c9744c54953e285ca/packages/durable/test/examples/22-subagent-foreground.ts) | Identified; review pending. |
| S5 | [Reviewer](https://github.com/earendil-works/pi/blob/1402973a9dbe8c627341e04c9744c54953e285ca/packages/durable/test/examples/28-reviewer.ts) | Identified; review pending. |

## Installed Pi docs

Pi 1.3.0 local README and `sdk.md`, `extensions.md`, `packages.md`, `codemode.md`, `models.md` read in full. Do not assume exact equivalence to the snapshot above.

Moving public entry points: [SDK](https://github.com/earendil-works/pi/blob/main/packages/coding-agent/docs/sdk.md), [extensions](https://github.com/earendil-works/pi/blob/main/packages/coding-agent/docs/extensions.md), [packages](https://github.com/earendil-works/pi/blob/main/packages/coding-agent/docs/packages.md), [classifiers](https://github.com/earendil-works/pi/blob/main/packages/coding-agent/docs/models.md#use-classifier-models).

## Latest legacy-package baseline

Operator confirmed these package identities and requested latest, not an audit blocked on installed work versions.

| ID | Source | Snapshot and inspection |
| --- | --- | --- |
| S6 | [pi-subagents README](https://github.com/nicobailon/pi-subagents/blob/95683a4427e1e4af8de03f1a9ca03ec4c2799b73/README.md) | `95683a4427e1e4af8de03f1a9ca03ec4c2799b73`; README read in full. |
| S7 | [Subagents tool reference](https://github.com/nicobailon/pi-subagents/blob/95683a4427e1e4af8de03f1a9ca03ec4c2799b73/docs/tool-reference.md) | Same revision; read in full. Documents retained-child resume and steering/receipt/eligibility boundaries; not run or source-audited. |
| S8 | [pi-intercom README](https://github.com/nicobailon/pi-intercom/blob/ddf288186d4c40b8a54fbe8c0d136cbe4893a032/README.md) | `ddf288186d4c40b8a54fbe8c0d136cbe4893a032`; docs and selected broker/receipt/mailbox source/tests inspected; tests unrun. |

The operator reports follow-up friction after delegated work completes. Current docs describe revival from a persisted session file, distinct from keeping the same live child. Treat the historical experience as evidence of a problem to understand, not proof that latest packages erase all context.

## Cloudflare implementations

Two distinct sources, not one interchangeable SDK/demo:

| ID | Source | Snapshot and inspection |
| --- | --- | --- |
| S9 | [Official standalone Cloudflare example](https://github.com/earendil-works/pi-durable-cloudflare/blob/dcb56ce2d174babb3f00112ac5b8cd597e58ab45/README.md) | `dcb56ce2d174babb3f00112ac5b8cd597e58ab45`; README read in full. Application/tests not yet reviewed. |
| S10 | [Agents SDK Pi harness guide](https://github.com/cloudflare/agents/blob/7dd51c518c517b0f9f92ec7e2c3ad1004222955e/docs/agents/harnesses/pi.md) | `7dd51c518c517b0f9f92ec7e2c3ad1004222955e`; read in full; SDK implementation/tests pending. |
| S11 | [SDK Pi example README](https://github.com/cloudflare/agents/blob/7dd51c518c517b0f9f92ec7e2c3ad1004222955e/examples/next/harnesses/pi/README.md) | Same revision; read in full. |
| S12 | [SDK example design notes](https://github.com/cloudflare/agents/blob/7dd51c518c517b0f9f92ec7e2c3ad1004222955e/examples/next/harnesses/pi/NOTES.md) | Same revision; read in full; historical/partly stale claims must be reconciled with current Pi. |

Reusable documented direction: Pi owns admission/execution/recovery; SDK owns storage/wake/lifecycle; transport and UI are application choices. Do not add a second replay authority unnecessarily. The standalone example explicitly lacks authentication/rate limiting: it is not a safe remote production template unchanged.

## Creator and community examples

- Creator-posted Slack links/excerpts: operator can supply; not accessible or reviewed here yet.
- https://github.com/disler/durable-pi — docs/app contracts inspected at `37165c619573dbaf425463038bbffecfe576beec`; not full source audit.
- https://github.com/TannerMidd/pi-pocket — selected architecture/attach/linked-session/auth source/tests inspected at `26542deded0123d6b217828ed2f71f566d9a7a19`; unrun, not a security audit.
- https://github.com/thruwire/foreman — deferred; bounded comparison already covers attach/control boundary. Not required breadth before first design review.

Pick implementations that answer a concrete design question; do not install/reuse them merely because they mention Durable.

## Empirical workflow input

[Work-setup report](../reports/work-setup-insights.md) received after the operator corrected an accidentally supplied brief. Orchestrator and Astra read all 716 lines. [Intake assessment](../findings/2026-10-19-work-report-intake.md) records chronology, provenance and quantitative/causal caveats. Report includes sanitized skill behavior summaries, not an executable bundle. Only two final episodes used the four-role roster. Imported reported corpus results are not independently reproduced on this machine.

## Access and provenance

Authenticated GitHub CLI access works. In the fresh session codemode works and web-access tools were enabled; direct TypeSafe classifier calls succeeded (see smoke findings). Primary GitHub source research can also proceed through Git/GitHub CLI. Do not send private imports/session text as search queries. Price and service facts supplied by the operator are labeled as such; actual Jev billing is not verified by native zero-cost metadata.
