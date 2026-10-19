# Open variables

| ID | Question / watch item | Status / next evidence |
| --- | --- | --- |
| V-001 | Exact Astra model ID and subscription authentication available to an embedded Durable host | Astra works via OpenAI subscription in Pi; verify host reuse without copying secrets or assuming API-key equivalence. |
| V-002 | Runtime/host/client protocol, agent lifetime, ownership, and upgrade boundaries | Preferred direction C, optional D; concrete design and recovery experiments pending. |
| V-003 | Secure Jev credential provisioning | Direct TypeSafe selected; use supported private credential storage, never Git/chat/argv. Key has not been supplied here. |
| V-004 | Cross-runtime decision/outcome instrumentation | Need shared schema/core and thin adapters; native usage totals are insufficient. See current observability doc. |
| V-005 | Empirical workflow requirements | Await public-safe analysis of work sessions and optional sanitized skills/templates. |
| V-006 | Paused/blocked work, ambiguity ownership, and steering notifications | Design explicit revision/correlation, affected-scope pause, decision requests, reconciliation, and stale-plan prevention. |
| V-007 | Source version drift | Use latest legacy packages as baseline, pin exact inspected revisions, and distinguish historical session behavior. Cloudflare notes may describe older Pi contracts. |
| V-008 | Full production chat-provider set, concurrency and autonomy envelope | Capture before provider/runtime integration; avoid fixed model identities and blanket spend assumptions. |

Resolved: project name, MIT license, local-first scope, UI flexibility, configurable topology, Jev adoption/provider. Their implementation details remain work, not reasons to reopen the decisions.
