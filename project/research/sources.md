# Source register

Access date: 2026-10-19. Documentation/source reading is not a successful integration test. Follow latest upstream as requested, while recording the exact inspected SHA for reproducibility; `main` snapshots are not selected runtime dependency versions.

## Official Pi

Repository: https://github.com/earendil-works/pi

Snapshot: `1402973a9dbe8c627341e04c9744c54953e285ca`.

| ID | Pinned source | Inspection status |
| --- | --- | --- |
| S1 | [Durable README](https://github.com/earendil-works/pi/blob/1402973a9dbe8c627341e04c9744c54953e285ca/packages/durable/README.md) | Read in full; not experimentally verified. |
| S2 | [Normative spec](https://github.com/earendil-works/pi/blob/1402973a9dbe8c627341e04c9744c54953e285ca/packages/durable/docs/spec.md) | Located; complete review pending. |
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
| S8 | [pi-intercom README](https://github.com/nicobailon/pi-intercom/blob/ddf288186d4c40b8a54fbe8c0d136cbe4893a032/README.md) | `ddf288186d4c40b8a54fbe8c0d136cbe4893a032`; read in full after continuation. Implementation/tests pending. |

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
- https://github.com/disler/durable-pi — discovered, not reviewed.
- https://github.com/TannerMidd/pi-pocket — discovered, not reviewed.
- https://github.com/thruwire/foreman — discovered, not reviewed.

Pick implementations that answer a concrete design question; do not install/reuse them merely because they mention Durable.

## Empirical workflow input

[Work-setup analysis brief](work-setup-analysis-brief.md) prepared. No corpus report or sanitized skill bundle has been received. Operator statements are requirements/experience, not corpus-wide measured frequencies.

## Access and provenance

Authenticated GitHub CLI access works. Web-access is installed but tools are not exposed to this active session; primary GitHub research can proceed without it. Do not send private imports/session text as search queries. Price and service facts supplied by the operator are labeled as such; actual Jev billing is not verified by native zero-cost metadata.
