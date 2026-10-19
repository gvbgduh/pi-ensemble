# Source register

Initial access: 2026-10-19. This is a first-pass register, not a completed literature review.

## Official Pi source snapshot

Repository: https://github.com/earendil-works/pi

Inspected commit: `1402973a9dbe8c627341e04c9744c54953e285ca`.
This was the observed `main` tip during initial research; it is **not** a chosen release/dependency pin.

| ID | Source | Evidence status |
| --- | --- | --- |
| S1 | [Durable README](https://github.com/earendil-works/pi/blob/1402973a9dbe8c627341e04c9744c54953e285ca/packages/durable/README.md) | Read in full; claims not yet experimentally verified. |
| S2 | [Durable normative spec](https://github.com/earendil-works/pi/blob/1402973a9dbe8c627341e04c9744c54953e285ca/packages/durable/docs/spec.md) | Located and cached locally; full review pending. |
| S3 | [Persistent background subagents example](https://github.com/earendil-works/pi/blob/1402973a9dbe8c627341e04c9744c54953e285ca/packages/durable/test/examples/23-subagent-background.ts) | Source read in full; not run. |
| S4 | [Foreground subagent example](https://github.com/earendil-works/pi/blob/1402973a9dbe8c627341e04c9744c54953e285ca/packages/durable/test/examples/22-subagent-foreground.ts) | Identified; detailed inspection pending. |
| S5 | [Reviewer example](https://github.com/earendil-works/pi/blob/1402973a9dbe8c627341e04c9744c54953e285ca/packages/durable/test/examples/28-reviewer.ts) | Identified; detailed inspection pending. |

## Local installed Pi documentation

The installed Pi reports documentation for version `1.3.0`. Its README and `docs/sdk.md`, `docs/extensions.md`, `docs/packages.md`, `docs/codemode.md`, and `docs/models.md` were read in full. These are local installed docs, not assumed identical to the upstream snapshot above.

Public entry points for readers (moving links, not evidence pins):

- [Coding-agent SDK](https://github.com/earendil-works/pi/blob/main/packages/coding-agent/docs/sdk.md)
- [Coding-agent extensions](https://github.com/earendil-works/pi/blob/main/packages/coding-agent/docs/extensions.md)
- [Pi packages](https://github.com/earendil-works/pi/blob/main/packages/coding-agent/docs/packages.md)

## Existing workflow packages: operator-confirmed identities

- https://github.com/nicobailon/pi-intercom — operator confirmed; installed version and source review pending.
- https://github.com/nicobailon/pi-subagents — operator confirmed; installed version and source review pending.

Do not make compatibility or replacement claims until inspected. The operator reports that finished delegated agents are no longer available with context for follow-up adjustments; verify which of the current package's retention/resume features affect that experience.

## Examples and alternative implementations to investigate

Discovered through GitHub repository search; not reviewed or endorsed:

- https://github.com/earendil-works/pi-durable-cloudflare — official example host.
- https://github.com/disler/durable-pi — terminal/web and persistent-agent application.
- https://github.com/TannerMidd/pi-pocket — web client/application.
- https://github.com/thruwire/foreman — supervisor/evaluation-oriented application.

## Research access

Authenticated GitHub CLI access works. The operator has installed `pi-web-access`; its tools were not exposed to the active research turn. GitHub source research does not require it. Broader ecosystem/announcement research may use it after reload or restart. Never send private source imports as search queries.
