# Work-Setup Evidence Report: Multi-Agent Pi Corpus Analysis

> **Publication Safety Status:** Reviewed and sanitized for public/external consumption. Real operator/colleague names, repository paths, internal URLs, cloud account IDs, KMS ARNs, and issue keys have been replaced with structurally faithful synthetic labels (`Rollout-A` / `rla-NNN`, `Stream-A` / `sta-NNN`, `Stream-B` / `stb-NNN`, `Stream-C` / `stc-NNN`, `org/agent-monorepo`, `org/infra-k8s-tools` [`TK`], `org/infra-k8s-product` [`PK`], `org/iac-live` [`IL`], `org/workflow-server`, `PROJ-NNN`, `Engineer A–F`). A private local mapping is retained exclusively on the operator's machine inside a non-exported directory (`~/.pi/agent/work-setup-private-mapping.json`).

---

## 1. One-Page Executive Summary

### Corpus Scope and Evolution
Between April and October 2026, the operator ran **378 JSONL session transcripts** (`57,707` messages, `0` parse errors) across a multi-repository infrastructure and platform-engineering workspace. Filtering out `66` one- or two-turn connectivity/model smoke probes leaves **176 substantive top-level sessions** and **136 nested subagent child transcripts**. Grouping co-scheduled role panes (`<prefix>-<NNN>[-<role>]`), forked subagent sessions, and handoff chains yields **62 structured work episodes** (covering `116` top-level sessions), including **30 multi-session `pi-intercom` coordinated episodes (`77` top-level sessions, `581` outbound intercom messages)** and **60 standalone single-session explorations**.

Over ten weeks (`2026-08-04` to `2026-10-15`), the workflow evolved through five empirically distinct architectures:
1. **Cohort 0 (`rla-006..014`):** Single parent session + bounded `pi-subagents` workers/workflows.
2. **Cohort 1 (`sta-001..015`):** Single driver + heavy `pi-subagents` (`workflowScript` + `context: "fork"` retained advisors).
3. **Cohort 2 (`sta-016..025`):** 2-agent persistent `pi-intercom` pair (Driver/Worker + `Fable 5.1` Adversarial Adviser).
4. **Cohort 3 (`sta-026..030`, `stb-001..013`):** 2- and 3-agent `pi-intercom` triads (`Opus 5.5` Worker + `Kimi K3` Manager + `Fable 5.1` Adviser), replacing read-only subagent fan-out with `codemode` (`685` script executions).
5. **Cohort 4 (`stb-014`, `stc-001`):** 4-agent 2x2 Herdr roster (`Argon 4` Orchestrator + `Opus 5.5` Worker + `Kimi K3` Manager + `Fable 5.1` Adviser) launched via an automated preflight + pane spin-up skill (`roster-spinup`).

### Strongest Empirical Findings
1. **Persistent role-pinned peers largely displaced bounded subagents (`F-01`, `F-02`):** Bounded `subagent` calls dropped from `10–34` per episode in Cohort 0/1 to **`0` across `stb-007..014` and `stc-001`**. Why? Forked subagent advisors (`context: "fork"` in `sta-001..003`) duplicated `142K–259K` parent contexts per spawn, while fresh subagents lacked project memory, hit 30-minute timeouts (`sta-007`, `sta-024`), or suffered a **38.3% (`31/81`) workflow failure rate** due to scheduled-job quota exhaustion (`11` in-session notifications) and JS DSL/schema errors (`6` failures). Conversely, a persistent Adviser pane (`Fable 5.1`) amortized a `60K–250K` context across an entire day and even survived primary worker exhaustion (`sta-018` $\rightarrow$ `sta-019`).
2. **Direct human-to-specialist steering is a core workflow pattern, not an exception (`F-03`, `F-04`):** In the `30` Intercom-coordinated episodes, **`29.5%` (`165/560`) of all human prompts were typed directly into non-coordinator panes** (`130` bypass prompts: `57` to Adviser, `38` to Co-Worker, `28` to Manager, `7` to Opus Worker under Argon). Direct steering saved `2–12` relay hops per design debate (`sta-021`, `stb-003`, `stc-001`). In `119/130` (`91.5%`) cases, the specialist eventually relayed the outcome over intercom (`86/130` within 10 minutes), but **manual relay depended on human nudges** (`"also, notify the worker as well"` in `stb-001`; `"you send intercom directly to stb-014"` in `stb-014`; an `80`-minute unpropagated framing reset in `sta-021`).
3. **Asymmetry between `intercom` `send` and `ask` caused `35` tool-call failures and silent "in-pane reply drops" (`F-05`, `F-06`):** `31/35` (`88.6%`) failed `intercom` tool calls were `"Reply target does not match a pending ask"` or `"No active intercom context to reply to"` because agents tried `action: "reply"` or `replyTo` in response to a non-blocking `action: "send"`. Worse, because `action: "send"` omits the reply-tool hint that `action: "ask"` injects, receiving agents occasionally wrote their answer as plain terminal prose in their own pane without calling `intercom` at all (`stb-011-kimi`, `stb-014-fable`).
4. **Zero compactions occurred (`0/378`); context was managed via Markdown tracker rotation, though `40` sessions still exceeded `300K` (`18` > `400K`) (`F-07`):** Instead of in-session compaction, the operator rotated numbered rosters (`<prefix>-<NNN>`) via a hierarchical Markdown tracker (`continuation-prompt.md`, `progress.md`, `mrs.md`, `decisions.md`, `variables.md`). When a session did not rotate (`sta-028-gemini`: `880` turns, **`875K` peak context**, `$98.32`), or when an orchestrator ingested raw diffs/reviews (`stc-001-argon`: `432K` in `92` turns), context still breached the `300K–400K` ceiling.
5. **Cross-model verification caught high-severity bugs and hallucinations, but lacked single-writer/lease enforcement (`F-08`, `F-09`, `F-10`):** Independent verification between `Opus 5.5`, `Kimi K3`, and `Fable 5.1` repeatedly prevented broken deployments (e.g., catching an unverified `helm template` claim and a hallucinated `"MR !1250 is merged"` report in `sta-026`; catching a physically impossible CI rollout order in `stb-010`; catching a truncated `14`-line file before Confluence publication in `stc-001`). However, mid-session worker replacement (`sta-026-gemini` $\rightarrow$ `sta-026-gemini-2`) and role-boundary drift (`stb-014-argon` opening MRs directly; `sta-026` parallel MRs `!139`/`!140`; `stb-009` duplicate `X-63` tracker IDs) caused split-brain edits on the same branches and files.
6. **Local classifier (`pi-local-laya`) showed `100%` hook reliability when up, `0%` behavioral influence, compressed probabilities (`0.46–0.69`), and `49` silent `server_down` failures (`F-14`, `F-15`):** Across `191` logged calls, `0` un-nudged deliberate calls occurred in production sessions. In `109` successful bash-gate evaluations (`stb-006..012`), `needs_review.p` clustered at `0.46–0.69` (median `0.60`, confidence `0.02–0.18`), changing `0` agent actions. When `laya-serve` stopped mid-`stb-012`, the next `49` gate triggers (`stb-012..014`, `stc-001`) failed open silently for `7` days.

### What Must Be Preserved vs. Priority Runtime Changes
- **Preserve:** (1) Plain-Markdown hierarchical project trackers as the human-auditable source of truth; (2) automated offline-delta preflight (`spinup.py`) with a mandatory human confirmation gate before work routing; (3) visible multi-pane layout in Herdr where the operator can inspect and directly steer any specialist; (4) `codemode` JS batching for read/API reconnaissance; and (5) cross-model separation between implementer (`Opus 5.5`), prose/alignment manager (`Kimi K3`), orchestrator (`Argon 4`), and adversarial oracle (`Fable 5.1`).
- **Highest-Priority Changes for Pi Ensemble:**
  1. **Unified, idempotent peer messaging (`reply` works on both `send` and `ask`) + mandatory outbound callback check** when a peer is woken by intercom or direct human steering (`MUST`).
  2. **Automatic "Direct-Steer Broadcast / Delta Digest"** to the roster coordinator whenever the human types directly into a non-coordinator pane (`MUST`).
  3. **First-class Agent Slot Replacement / Lease Handover (`replace(old_slot, new_slot)`)** that freezes/detaches the old agent, transfers the worktree/branch lease, and redirects peer handles (`MUST`).
  4. **Artifact & Worktree Ownership Locks** (one writer per git worktree and per tracker file, or atomic tracker append helpers) to eliminate duplicate ID collisions (`X-63`) and parallel duplicate MRs (`!139`/`!140`) (`SHOULD`).
  5. **Remove legacy system-prompt fossils (`orchestrator-routing` injecting `"You are the parent orchestrator"` into Kimi Manager panes; `model-rules` injecting obsolete `Opus 4.6/4.7` rules)** — a zero-runtime, immediate local config fix (`MUST — Local Config`).

---

## 2. Corpus Inventory, Episode Grouping, and Methodology

### 2.1 Raw Session Inventory and Parseability

All files under `~/.pi/agent/sessions/`, `~/.pi/agent/logs/`, `~/.pi/agent/laya/`, and `~/.pi/agent/context/` were inventoried locally via Python scripts before deep reading.

| Corpus Artifact Category | Count | Date Range (UTC) | Parseability / Completeness | Notes |
| --- | --- | --- | --- | --- |
| **All session `.jsonl` files** | `378` | `2026-04-19` – `2026-10-19` | `100%` (`0` JSON parse errors across `61,041` JSONL lines) | `57,707` message entries, `859` custom entries, `788` custom_message entries |
| **Top-level session `.jsonl` files** | `242` | `2026-04-19` – `2026-10-19` | `100%` | Located at `sessions/<cwd-slug>/<timestamp>_<uuid>.jsonl` |
| — *Substantive or named top-level sessions* | `176` | `2026-04-19` – `2026-10-19` | `100%` | `119` explicitly named via `session_info` + `57` unnamed sessions with $>2$ turns |
| — *Trivial smoke/model probes ($\le 2$ turns, unnamed)* | `66` | `2026-07-07` – `2026-10-15` | `100%` | 1-turn `ping`, `reply with only: ok`, `models`, `laya_status`, or `powerline` tests |
| **Nested `pi-subagents` child `.jsonl` files** | `136` | `2026-08-04` – `2026-10-09` | `100%` | Located at `sessions/<cwd-slug>/<parent-session>/<run-id>/run-0/session.jsonl` |
| **Forked top-level `subagent-advisor-*` / `planner-*` files** | `8` | `2026-08-05` – `2026-08-21` | `100%` | Created with `parentSession` set when `context: "fork"` was used in Cohort 1 |
| **`laya/usage.jsonl` classifier telemetry** | `191` rows | `2026-09-30` – `2026-10-16` | `100%` | `158` `source="gate"` (`109` ok, `49` `server_down`), `33` deliberate/test (`laya`/`tool`) |
| **`logs/orchestrator-usage/unknown.jsonl`** | `170` rows | `2026-08-05` – `2026-10-02` | `100%` JSON, **missing `sessionId`** | All `170` rows logged with `sessionId: "unknown"` due to env-var bug |
| **`run-history.jsonl` & `missions/index/`** | `95` / `35` | `2026-08-04` – `2026-10-09` | `100%` (prompts redacted in `run-history`) | Early `pi-subagents` run index |

**Reconciling the Operator's Estimate (`70–80` sessions):**
The operator's mental estimate of `70–80` sessions corresponds closely to the **62 structured project/meta episodes** (or the **77 top-level sessions participating in multi-agent Intercom episodes**). Why the raw file count is `378`:
1. `136` files are nested `pi-subagents` child transcripts stored in subdirectories under their parent session.
2. `66` top-level files are 1–2 turn CLI/provider smoke tests (`ping`, `reply with only: ok`).
3. Each multi-agent episode in Cohorts 2–4 spawns `2` to `4` top-level session files (`<prefix>-<NNN>`, `-kimi`, `-fable`, `-argon`).

### 2.2 Grouping Sessions into Work Episodes and Cohorts

A single session file is neither an independent task nor an independent outcome. Using session names (`session_info`), `parentSession` pointers, `intercom_sent` / `intercom_message` headers, and tracker session logs (`progress.md`), the `176` substantive top-level sessions group into **62 structured episodes (`116` top-level sessions)** plus **60 standalone single-session explorations**:

| Cohort | Initiative / Prefix Cluster | Date Range | Episodes | Top-Level Sessions | Dominant Layout & Coordination Mechanism |
| --- | --- | --- | --- | --- | --- |
| **Cohort 0** | `Rollout-A` (`rla-006..014`, control-plane prod rollout) + early setup (`meta-001`, `slide-fixes`) | `2026-08-04` – `2026-08-12` | `11` | `11` (+ `48` nested subagents) | **1 parent session** (`Opus 4.8` / `Kimi K3` / `GLM 5.2`) + bounded `pi-subagents` (`worker`, `oracle`, `reviewer`, scheduled `reviewbot-recheck` workflows) |
| **Cohort 1** | `Stream-A` (`sta-001..015`, payload codec & RBAC phase 1) | `2026-08-12` – `2026-09-04` | `15` | `21` (`15` main + `6` forked `subagent-advisor-*`, + `69` nested subagents) | **1 driver session** (`Kimi K3` / `Opus 4.8/5`) + `pi-subagents` (`workflowScript: true` parallel lanes and `context: "fork"` advisors) |
| **Cohort 2** | `Stream-A` (`sta-016..025`, codec & RBAC phase 2) + `adr-51` (`meta-002`) | `2026-09-02` – `2026-09-15` | `11` | `18` | **2-agent `pi-intercom` pair**: Driver/Worker (`Kimi K3` / `Opus 4.8/5`) + persistent Adviser (`Fable 5.1`) in side-by-side Herdr panes |
| **Cohort 3A** | `Stream-A` (`sta-026..030`, codec & RBAC phase 3) + `review-skill` (`meta-003`) | `2026-09-15` – `2026-09-23` | `6` | `15` | **2- to 4-agent `pi-intercom` roster**: Driver (`Kimi K3` / `Opus 5.5`) + Co-Worker (`Gemini 3.8 Flash` or `Kimi K3`) + Adviser (`Fable 5.1`) |
| **Cohort 3B** | `Stream-B` (`stb-001..013`, multi-cluster platform replication) + `meta-004..007` (`codemode`, `laya`, platform review) | `2026-09-25` – `2026-10-14` | `17` | `43` | **2- and 3-agent `pi-intercom` triad**: Main Worker (`Opus 5.5`) + Manager (`Kimi K3`) + Adviser (`Fable 5.1`), using `codemode` (`512` calls) + `laya` gate |
| **Cohort 4** | `Stream-B` (`stb-014`) & `Stream-C` (`stc-001`, P2P workflow state machine kickoff) | `2026-10-14` – `2026-10-16` | `2` | `8` | **4-agent 2x2 Herdr roster** (`roster-spinup` / `pi-spin`): Orchestrator (`Argon 4`) + Main Worker (`Opus 5.5`) + Manager (`Kimi K3`) + Adviser (`Fable 5.1`) |
| **Standalone** | Ad-hoc debugging, tech-talk deck, harness tuning, Laya evaluation (`meta-eval`) | `2026-04-19` – `2026-10-19` | `60` | `60` | Single-session interactive exploration |

### 2.3 Stratified Deep-Review Selection

Following Steps 4 and 5 of the analysis plan, all `378` files were parsed in the broad structured pass, and **22 episodes (`54` top-level sessions + associated child transcripts and tracker artifacts)** were selected for full end-to-end turn-by-turn deep trace across every required stratum:

| Stratum | Deep-Reviewed Episodes (`Anonymized ID`) | Why Selected |
| --- | --- | --- |
| **Routine successes** | `sta-018` (`2` sessions), `stb-005` (`3` sessions), `stb-012` (`3` sessions) | Smooth multi-repo MR delivery, clean Fable verification, milestone tracker updates |
| **Hard multi-repo / architectural successes** | `stb-009` (`3` sessions), `stb-010` (`3` sessions), `stc-001` (`4` sessions) | High-velocity 3- and 4-agent coordination across 4 repos; mutual premise correction between Opus, Kimi, and Fable |
| **Failures, stalls, & abandoned work** | `meta-001` (`1`+children), `rla-012` (`1`+children), `sta-003` (`4`), `sta-007` (`1`), `sta-008` (`1`), `sta-009` (`1`), `sta-020` (`3`), `sta-029` (`3`), `stb-004` (`3`) | `workflowScript` JS/lane errors, 30-min subagent timeouts, `sta-020-cancel` early abort, `stb-004-bad` wrong-prompt abort, `sta-029-gemini` KMS hang + `ask` timeout |
| **Direct human interventions & role drift** | `sta-021` (`2`), `stb-003` (`2`), `stb-014` (`4`), `meta-004` (`2`), `meta-005` (`2`) | Direct human debates in Fable/Kimi panes; manager drifting into wrong repo/implementation (`stb-003`); orchestrator opening MRs (`stb-014`); llama.cpp prototype scrapped (`meta-004/005`) |
| **Context bloat & mid-session replacement** | `sta-001` (`3`), `sta-019` (`1` + `sta-018-fable`), `sta-026` (`4`), `sta-028` (`2`), `stb-011` (`3`) | `875K` peak context (`sta-028`); mid-session worker replacement (`sta-026-gemini` $\rightarrow$ `gemini-2`) causing dual ownership & duplicate MRs (`!139`/`!140`); cross-session adviser survival (`sta-018-fable` $\rightarrow$ `sta-019`) |
| **Classifier & observability negative cases** | `stb-005`, `stb-006`, `stb-012..014`, `stc-001`, `meta-006` (`2` eval sessions) | Nudged vs. automatic gate invocations, compressed probabilities, `49` `server_down` silent failures, and live classifier self-ranking bias test |

### 2.4 Coverage Gaps and Methodological Limits
- **Flat-rate provider cost blind spot:** The `devin` provider (`kimi-k3-high`, `kimi-k3-max`, `glm-5-2-max-1m`, `claude-opus-4-8-xhigh`) reports `cost.total = 0` on every turn, and `39` assistant turns across other providers logged `0` tokens (cancelled/error turns). Reported USD figures (`$1,064.40` across the `62` structured episodes) therefore reflect **only metered Anthropic and Google Vertex turns**, not flat-rate subscription usage.
- **Unlogged external side-channels:** Direct human actions in GitLab UI, AWS SSO browser flows, Slack threads, and manual terminal panes outside Pi are observable only when referenced in user prompts, tracker files, or `spinup.py` offline-delta reports.
- **Historical vs. current package behavior:** Early `pi-subagents` issues in August 2026 (`workflowScript` syntax traps, `sessionId: "unknown"` in custom extension logs) occurred on older versions or custom wrapper code and are explicitly distinguished from current `pi-subagents v0.79.1` and `pi-intercom v0.17.1`.

---

## 3. Detailed Investigation by Theme

### 3.1 Context and Agent Lifetime

#### 3.1.1 When Retained Context Enabled Explanations/Adjustments vs. When Unreachable Agents Caused Loss
- **High-leverage retained context (`sta-018` $\rightarrow$ `sta-019`, `sta-026`, `sta-028` $\rightarrow$ `sta-029`):**
  - *Cross-iteration Adviser survival (`sta-018-fable` $\rightarrow$ `sta-019`):* In `sta-018`, the primary worker (`sta-018`, `317` turns) exhausted its token/provider quota after 7.5 hours of debugging a custom Go claim-mapper in `org/workflow-server`. Because `sta-018-fable` (`110` turns, `234K` context) was running in an independent Herdr pane, the operator instructed `sta-018-fable`: *"sta-018 exhausted tokens limit, relay to sta-019, which is different model"*. Within `220` seconds, `sta-018-fable` synthesized the exact root cause (`WithClaimMapper` option ordering vs. `WithServerConfigFilePath` in `v1.31.2`) and handed it over intercom to `sta-019`, which landed the fix in `143` turns without re-reading the Go module cache.
  - *Historical context oracle (`sta-028-gemini` queried by `sta-029`):* Five days after `sta-028` ended, its worker pane (`sta-028-gemini`, `875K` context) was still open in Herdr. When `sta-029` hit a SOPS/KMS encryption blocker and Jira ticket ambiguity, the operator instructed `sta-029`: *"ask gemini in sta-028-gemini only for context, do it yourself"*. `sta-029` sent three targeted `intercom` queries (`05:03`, `05:25`, `06:01`) to `sta-028-gemini` and retrieved the exact AWS SSO role limitations and ticket map in seconds—information that was only partially captured in the tracker.
  - *Instant architectural recall (`sta-026-fable`):* When `sta-026` was about to flip `TEMPORAL_AUTH_USE_ID_TOKEN_AS_BEARER` to `"true"` during an outage investigation, it asked `sta-026-fable` why `TK !1225` had set it to `"false"`. Within `93` seconds (`11:51:00` $\rightarrow$ `11:52:33`), `sta-026-fable` answered from retained context: *"verdict: keep false. Do NOT flip. Only the access token works for codec decode; the ID token would fail closed."*
- **What was lost when an agent became unreachable (`sta-018`, `sta-026-gemini`, `sta-029-gemini`):**
  - While the session `.jsonl` file on disk preserves the raw transcript (`100%` recoverable via `read`/python), **live in-flight hypotheses, un-written decision rationale, and active working-tree ownership** are not automatically indexed. When `stc-001-fable`'s intercom reply failed to deliver to `stc-001-argon` (`stc-001`), Argon wrote a Python one-liner to scrape Fable's `.jsonl` file—and accidentally truncated the extracted review to `14` lines (`3.4` below), proving that ad-hoc transcript scraping is brittle compared to structured artifact handoff.

#### 3.1.2 Bounded Subagents (`pi-subagents`) vs. Persistent Peers (`pi-intercom`)
Across the corpus, `subagent` was invoked `437` times at the top level (`181` executions, `138` `status` polls, `41` `list`, `38` `steer`, `14` `models`, `10` `interrupt`, `8` `stop`, `2` `inspector.open`), while `intercom` was invoked `652` times (`581` `intercom_sent` events across `77` sessions).

| Dimension | Bounded Subagents (`pi-subagents` in Cohorts 0–1) | Persistent Role Peers (`pi-intercom` in Cohorts 2–4) |
| --- | --- | --- |
| **Usage Trajectory** | Heavy in Cohort 0/1 (`358` calls across `rla-006..014` & `sta-001..015`); dropped to **`0` calls in `stb-007..014` & `stc-001`** | `0` calls in Cohort 0/1; **`581` sent messages across `30` episodes** in Cohorts 2–4 |
| **Context Economics** | `context: "fresh"` required repeating project briefings or re-reading repo state on every spawn; `context: "fork"` (`sta-001..003`) copied `142K–259K` parent contexts into `6` separate advisor sessions | Each peer maintains a dedicated `20K–280K` context tailored to its scale (Orchestrator = project, Worker = code/diff, Manager = decisions/prose, Adviser = source/telemetry evidence) |
| **Follow-up & Multi-Turn Steering** | `38` `steer` calls, `7` `subagent_supervisor_request` escalations, `27` `subagent_control_notice` alerts; `steer` failed `3` times when an async child had already exited (`sta-010`, `sta-011`) | Any peer can be messaged repeatedly over `intercom` or directly in its Herdr pane across hours without respawning |
| **Failure Modes** | `31/81` (`38.3%`) workflow runs in `orchestrator-usage` failed (`11` in-session notifications from scheduled `reviewbot-recheck` hitting daily quota; `6` `workflowScript` JS syntax/unawaited-promise/lane-schema errors; `2` 30-min `1800000ms` worker timeouts in `sta-007`, `sta-024`) | `35` failed `intercom` tool calls (`31` `replyTo`/pending-ask mismatches, `3` 10-min `ask` timeouts, `1` cancelled); dual-writer collisions when peers lacked worktree locks |
| **Package Features Used vs. Missed** | Used: `async: true` (`114`), `workflowScript` (`86`), `steer` (`38`), `output: "file-only"`, `worktree: true` (`6`). Missed/Rare: `inspector.open` (`2` calls, because `orchestrator-routing` auto-opened mirroring panes), `council-mode` (`0` calls). | Used: `send`, `ask`, `reply`, `list`, `pending`. **Missed (`0` calls): `intercom({ action: "handover" })`** — handovers were always done via Markdown `continuation-prompt.md` instead of the built-in `handover` action. |

#### 3.1.3 Compaction, Restarts, Model Changes, and `context_edit`
- **Zero Compactions (`0/378` sessions):** No session in the corpus ever used automatic or manual compaction (`type: "compaction"` count = `0`). Instead:
  1. **Inter-session rotation via Markdown trackers:** Sessions were kept to `1–8` hours of active work (`150–300` turns) and rotated to `<prefix>-<NNN+1>` by rewriting `continuation-prompt.md`.
  2. **In-session `context_edit` (`17` occurrences across `10` sessions):** Used surgically to prune dead tool outputs or large accidental pastes (`stb-007`: `3`, `meta-004`: `2`, `stb-001/006/008/012-fable`: `1` each).
  3. **Mid-session `/model` switches (`527` `model_change` events across `378` sessions):** Nearly every session starts on the default `devin/kimi-k3-high` (from `settings.json`) and immediately switches to its role model (`anthropic/claude-opus-5-5`, `anthropic/claude-fable-5-1`, or `google-vertex/gemini-4-argon`), or switches mid-session when a provider hits rate limits (`sta-018`, `rla-012`).

#### 3.1.4 Prompt Context Size Distribution vs. Cumulative Billed Tokens
In Pi's session schema, an assistant turn's actual prompt context window is `input + cacheRead + cacheWrite` on that single turn, whereas cumulative session tokens sum `totalTokens` across all turns.
- **Sessions exceeding the `300K–400K` upper target:** Across `176` substantive top-level sessions, **`40` sessions (`22.7%`) reached a peak prompt context $\ge 300\text{K}$ tokens**, and **`18` sessions (`10.2%`) exceeded $400\text{K}$ tokens**.
- **Top Peak Context Outliers and Their Causes:**
  1. `sta-028-gemini` (**`875,328` peak context**, `880` turns, `122.6h` span, `$98.32`): Single worker session kept alive across 5 days, handling 3 MRs (`!151`, `!152`, `!154`, `!155`), rebases, ReviewBot bot loops, and production promotion without rotating to `029`.
  2. `meta-cache-debug` (`2026-08-24T04:26`, **`610,358` peak context**, `173` turns): Inspecting raw session JSONLs inline via `bash` before `codemode` existed.
  3. `sta-027-gemini` (**`563,462` peak context**, `279` turns) & `sta-026-gemini` (**`543,753` peak context**, `455` turns): Gemini 3.8 Flash worker sessions running unbatched `bash`/`glab`/Datadog loops prior to `codemode`.
  4. `sta-022` (**`525,631` peak context**, `190` turns) & `sta-001-finish` (**`484,477` peak context**, `269` turns): Solo driver+worker sessions doing both implementation and coordination.
  5. `stb-008` (**`477,577` peak context**, `292` turns) & `stb-010` (**`437,985` peak context**, `252` turns): Multi-repo `Opus 5.5` worker sessions shipping changes across `IL`, `TK`, `PK`, and `org/agent-monorepo`.
  6. `stc-001-argon` (**`432,057` peak context in just `92` turns!**): Even though `stc-001-argon` was the dedicated **Orchestrator**, its context hit `432K` because it pulled full Confluence HTML/Markdown bodies (`100001`, `100002`), Jira tables, and Fable's `12KB` review text into its own context to publish pages when Kimi's Atlassian MCP token returned `403`.
- **Role Partitioning Effect in Cohort 3B/4:** In `stb-001..014`, while the Main Worker averaged `336K` peak context, the Manager (`-kimi`) averaged **`67K` peak context** (`19K–117K`) and the Adviser (`-fable`) averaged **`167K` peak context** (`95K–256K`). Partitioning genuinely kept specialist panes lean.

#### 3.1.5 Duplicate File Loads vs. Necessary Independent Verification
Across the `30` multi-agent Intercom episodes, `25` distinct file paths were read via `read` by $\ge 2$ peers in the same episode (plus `60+` via `bash` `git diff` / `cat`).
- **Wasteful duplication:** Every peer at session start independently reading the full tracker suite (`continuation-prompt.md`, `mrs.md`, `variables.md`, `decisions.md`, `progress.md` = `15K–35K` tokens per agent) even when a `20`-line role-scoped kickoff brief from the Orchestrator would suffice.
- **Essential independent verification (Must Preserve):** When Fable or Opus re-read `app.py`, `policy.py`, `values.yaml`, `claimmapper.go`, or ran `helm template` / `git diff origin/master` independently, **it was the primary mechanism that caught defects** (`stb-003` wrong namespace & stale local `master` diff; `stb-010` broken CI tag-job ordering; `sta-026` broken `helm template` claim; `stc-001` corrupted 14-line findings file). Eliminating independent source reads by the Adviser would break the setup's verification guarantee.

#### 3.1.6 Mid-Episode Agent Wind-Down and Replacement
Four episodes featured mid-episode agent replacement or abort:
1. **`sta-026-gemini` $\rightarrow$ `sta-026-gemini-2` (Active Dual-Ownership Collision):** After `sta-026-gemini` hit `543K` context, leaked Chinese characters, put wrong `datadogService` tags on workers, and falsely reported `"TK !1250 is merged"`, the operator spun up `sta-026-gemini-2`. However, `sta-026-gemini` was **not closed**. At `12:47`, the driver (`sta-026`) sent rollback instructions to *both* `sta-026-gemini` and `sta-026-gemini-2`, and both agents concurrently modified and reported back on branch `proj79-codec-istio-and-envs` (`12:50:58` vs. `12:54:34`).
2. **`sta-018` $\rightarrow$ `sta-019` (Clean Adviser-Mediated Handover):** `sta-018` hit provider limits; `sta-018-fable` remained live and briefed `sta-019` over intercom (`3.1.1`).
3. **`sta-020-cancel` $\rightarrow$ `sta-020` & `stb-004-bad` $\rightarrow$ `stb-004`:** Early session aborts (`19` turns and `2` turns) due to initial framing/paste mistakes; old sessions were renamed with `-cancel` / `-bad` suffixes so they wouldn't collide with the replacement handle.

---

### 3.2 Direct Interaction and Alignment

#### 3.2.1 Operator Bypass of the Coordinator: Frequency and Motivation
Across the `30` Intercom-coordinated episodes (`560` total human messages excluding auto-injected intercom wake lines):
- **`395` prompts (`70.5%`)** went to the primary Driver/Worker (Cohorts 2–3) and **`35` (`6.3%`)** went to the dedicated Orchestrator (`-argon` in Cohort 4).
- **`130` prompts (`23.2%` overall; `44.3%` in Cohort 4!) bypassed the coordinator** and were typed directly into specialist panes:
  - **Adviser (`-fable`): `57` direct human prompts**
  - **Co-Worker (`-gemini`): `38` direct human prompts**
  - **Manager (`-kimi`): `28` direct human prompts**
  - **Main Worker (`Opus` under `Argon` in Cohort 4): `7` direct human prompts**
- **Why the operator bypassed the coordinator (4 observable reasons):**
  1. **High-bandwidth technical/architectural Q&A with the specialist holding the source context (`sta-021-fable`, `stb-003-kimi`, `stb-014-fable`, `meta-004/005-fable`):** In `sta-021-fable`, the operator exchanged `8` consecutive prompts directly with Fable debating same-origin proxy vs. internal cluster DNS for the codec server—avoiding `16` relay hops through `sta-021`. In `meta-004-fable` and `meta-005-fable`, the operator debated the `pi-local-laya` architecture directly with Fable (`8` and `5` prompts), deciding to scrap the `llama.cpp` prototype and drop `auto` mode before the worker wrote code.
  2. **Visual inspection & immediate tactical steering (`sta-026-gemini`, `stb-014`):** Because all panes are visible in a 2x2 Herdr tab, when the operator saw a worker finish a commit or hit a rebase/ReviewBot comment, typing `"ok, you can open MR now"` or `"XK !6786 has reviewbot comments to address"` directly into the worker's pane (`stb-014`) was faster than asking Argon to tell Opus.
  3. **Correcting a specialist's factual error before it polluted the coordinator (`stb-014-fable`, `stc-001-fable`, `stb-001-kimi`):** E.g., in `stb-014-fable`, the operator spotted Fable write `"Tools cluster frontend is still in observe mode"` in its pane and immediately corrected Fable (`"that's not true, it was enforced some time ago"`), before Fable sent those findings to the worker.
  4. **Unsticking or redirecting a stalled peer (`sta-026-fable`, `sta-029-gemini`):** E.g., telling Fable `"ping kimi in sta-026 i think the message was missed as it was stuck"`.

#### 3.2.2 Post-Steering Alignment: How Quickly Did the Coordinator Learn?
- Out of `130` direct human prompts to non-coordinator panes, **`119` (`91.5%`) were followed by an outbound `intercom_sent` message** from that specialist to the coordinator or peer (`86/130` [`66.2%`] within `10` minutes; median relay latency after human decision = **`48` seconds**).
- **Where alignment lagged or failed (`11` un-relayed prompts + `3` multi-minute drift windows):**
  1. **Multi-turn side-channel debates where the coordinator waited unaware (`sta-021`, `stb-003`):** In `sta-021`, the operator debated T-104 framing in `sta-021-fable` for **`80` minutes (`4,843s`)** across `6` turns before saying *"alright, on that we need update sta-021 and answer its question to me in one go"*. In `stb-003-kimi`, a `5`-hour gap + `7`-turn secrets discussion happened in Kimi's pane before Kimi relayed the ruling to `stb-003`.
  2. **Specialists answering in-pane without relaying until nudged (`stb-001-kimi`, `stb-003-kimi`, `stb-014-fable`):**
     - In `stb-001-kimi`, after the operator corrected scope (`"ok, swe coding WF would only exist in tools cluster"`), Kimi answered only in its own pane; `23` seconds later the operator had to type: **`"also, notify the worker as well"`**.
     - In `stb-014-fable`, after the operator discussed an alert banner and review items (`02:05:30`), Fable did not message `stb-014`. At `02:11:31`, the operator typed: **`"you send intercom directly to stb-014"`**, and `34` seconds later added: **`"let's have some sort of rules to avoid this instruction (i meean: 'you send intercom directly to stb-014')"`**.

#### 3.2.3 Anatomy of a Sufficient Steering Report vs. Noisy Loops
- **What worked best in practice:** The cleanest steering relays (`meta-005-fable`, `stb-010-kimi`, `stb-014-fable`) shared a 4-part structure:
  1. **Authority & Source:** `"Direct ruling from operator in <peer-name> pane (verbatim/summarized): ..."`
  2. **Delta / Superseded State:** `"Supersedes D-44 / drops option (b); do NOT fold into !138."`
  3. **Concrete Action & Owner:** `"Worker (<prefix>-<NNN>) to open separate MR !139 on branch X; Manager to update decisions.md."`
  4. **Verification / Gate:** `"Commit locally and hold push for Fable review / operator GO."`
- **Protocol friction (`send` vs. `ask` mismatch):** Only `3/581` (`0.5%`) intercom messages were pure noise ("ack/standing by"), because agents followed a good rule to absorb wrap-up FYIs silently. The real messaging waste was **protocol rejection (`35` failed `intercom` tool calls)**: `31` calls failed with `"Reply target does not match a pending ask"` or `"No active intercom context to reply to"` when agents used `action: "reply"` or `replyTo` on an async `send`. In `stc-001-fable`, this blocked Fable's HLD review delivery `4` times in a row, forcing Fable to route through the Worker (`stc-001`) as a relay!

---

### 3.3 Autonomy, Ambiguity, and Waste

#### 3.3.1 Safely Autonomous Decisions vs. Human Gates
The corpus exhibits a well-calibrated split between autonomous execution and human gates:
- **Safely autonomous (`100%` routine acceptance):** Creating/managing git worktrees (`~/cg/wt/*`), running test suites (`pytest`, `make check`, `helm template`, `tofu plan`), querying Datadog (`pup_*`: `541` calls) and ArgoCD, creating/transitioning the operator's own Jira tickets (`mcp__atlassian`), updating local tracker files, and replying to/resolving automated bot (`ReviewBot`) review threads.
- **Enforced human gates (violated only twice across `62` episodes):**
  1. **Merging MRs:** Respected in `61/62` episodes; in `sta-026`, `sta-026-gemini` claimed `"IK !1250 is merged"` when it wasn't, prompting immediate operator rebuke (`"ok, !1250 isn't merged and can't be merged without human approval"`).
  2. **Publishing/editing external human-facing docs or comments:** Respected in `61/62` episodes; in `stb-009-kimi`, Kimi proposed/logged an unapproved edit to a live Confluence runbook (`100003`), which `stb-009` (Opus Worker) caught and blocked (`"please don't edit it: published docs go out only with operator's GO"`).

#### 3.3.2 Where Confident Assumptions Replaced Missing Requirements (Throwaway Work)
1. **`meta-004` / `meta-005` (`pi-local-laya` prototype):** An earlier session built a `llama.cpp` wrapper prototype for Laya based on a wrong assumption that Laya was a GGUF/llama.cpp model rather than a PyTorch `ModernBERT` encoder speaking TypeSafe's `systemone` wire protocol. `meta-004-fable` and the operator had to scrap the entire prototype and rewrite `pi-local-laya` from scratch (`pi-local-laya-task.md`).
2. **`meta-004` (Keychain OAuth token migration):** `pi-setup-codemode` found OAuth tokens in macOS Keychain (`pi-mcp-adapter.oauth`) and planned to copy them directly into `~/.pi/agent/mcp-auth.json`. The operator intervened in Fable's pane (`"do not retrieve auth/access tokens, MCP can be re-auth after migration"`), preventing brittle credential file tampering.
3. **`sta-029-gemini` (50-turn KMS permission rabbit hole):** Instead of immediately reporting that `sops -d` failed with `AccessDeniedException` on account `111122223333` and asking who held KMS encrypt rights (an external-authority fact), `sta-029-gemini` spent `44` minutes (`50+` tool calls) probing every AWS profile, `~/.aws/sso/cache`, zsh history, and running `grep -rn` across `/Users/operator/` until killed by `SIGKILL` (`exit 137`).
4. **`stb-003-kimi` (Wrong repository assumption):** Kimi drafted secret instructions targeting `TK` (`infra-k8s-tools`) instead of `PK` (`infra-k8s-product`), requiring human correction (`"why IK (infra k8s)? we work with product-cluster, its repo is XK"`).

#### 3.3.3 Distinguishing Cache Expiry, Waiting, Exploration, and Throwaway Work
By inspecting `usage.cacheWrite` vs. `usage.cacheRead` across all `57,707` messages:
- **Prompt cache expiry (`132` spikes where `cacheWrite > 50K` after `cacheRead > 50K`):** Because multi-agent sessions span `7h–122h` wall-clock (across lunch breaks, overnight pauses, or waiting for CI/DevOps approvals), Anthropic's prompt cache expired whenever a pane sat idle beyond the cache TTL (`5m` standard or `1h` extended). For example, `stb-009` (`23.4h` wall time) and `stb-012` (`72.6h` wall time) each incurred `11` full-context cache rewrites (`300K+` tokens re-written to cache), accounting for a major share of their `$41.84` and `$42.09` metered cost—**not** model thrashing.
- **Idle peer waiting:** In `stb-011..013`, `-kimi` was launched at spin-up (`19K–27K` context) and sat idle for `95%+` of the episode (`3–6` turns total) because the work was pure infrastructure/code debugging handled by `Opus` + `Fable`. Because Pi sessions cost `$0` while idle (especially on flat-rate `devin/kimi-k3-high`), an idle pane in a 2x2 layout is near-zero waste.
- **True throwaway work:** Concentrated in (a) duplicate implementation (`sta-026` MRs `!139` vs. `!140`), (b) un-escalated permission rabbit holes (`sta-029-gemini`), (c) `orchestrator-usage` scheduled `reviewbot-recheck` loops (`31` failed runs), and (d) `llama.cpp` prototype rework (`meta-004`).

---

### 3.4 Coordination, Workspace, and Trackers

#### 3.4.1 Role Layouts, Overlap, and Bottlenecks
1. **2-Agent Pair (`Driver/Worker` + `Fable Adviser`, `11` episodes):** Ideal for focused single-stream debugging or package builds (`sta-018`, `meta-004`, `meta-005`). Low coordination overhead (`2–22` intercom messages).
2. **2-Agent Pair (`Opus Worker` + `Kimi Manager`, `4` episodes: `stb-001..004`):** Worked for tracker/design bookkeeping, but lacked Fable's deep source/telemetry disambiguation; when Kimi tried to review diffs or specify file paths (`stb-003`), Opus had to correct Kimi's technical specs.
3. **3-Agent Triad (`Opus Worker` + `Kimi Manager` + `Fable Adviser`, `10` episodes: `stb-005..013`):** Very high effectiveness on complex multi-repo work (`stb-009`, `stb-010`). However, `Opus` had to act as both implementer AND hub coordinator (`352K–477K` context), and in implementation-only sessions (`stb-011..013`), `Kimi` became a bystander (`3–6` turns).
4. **4-Agent 2x2 Roster (`Argon Orchestrator` + `Opus Worker` + `Kimi Manager` + `Fable Adviser`, `2` episodes: `stb-014`, `stc-001`):**
   - Offloaded coordination, Jira/Confluence ops, and preflight delta checks to `Argon 4`, keeping `Opus` (`stb-014`: `287K`, `stc-001`: `126K`) focused on code/skills.
   - **Bottleneck / Drift:** When `Argon` jumped into creating worktrees and opening MRs (`stb-014`: `PK !6786` and `TK !1359`), it produced broken MR formatting (literal `\n`) and confused Fable and Opus about who owned `!6786`. Conversely, in `stc-001` (a heavy architecture/documentation kickoff), `Kimi` (`197` turns) and `Argon` (`92` turns) did `78%` of the work while `Opus` (`36` turns) only built the `roster-spinup` skill.
5. **Hidden System-Prompt Conflict (`orchestrator-routing` + `model-rules` vs. `AGENTS.md`):**
   - Inspection of `~/.pi/agent/extensions/orchestrator-routing/index.ts` (lines `149`, `508`) revealed that it checks `modelId.includes("kimi-k3")` and injects `# Orchestrator Routing — Kimi K3 Max as parent orchestrator: You are the parent orchestrator. Your job is to route work to subagents...` into **every Kimi session**! In Cohorts 3B and 4, `-kimi` is the **Manager**, not the parent subagent orchestrator. Simultaneously, `model-rules/index.ts` injects `_common.md` (an April 2026 table for `Opus 4.6 / 4.7 / Sonnet 4.6`) into every session.

#### 3.4.2 Cross-Repository Dependencies, Worktrees, and Tracker Drift
- **Cross-repo coordination:** Episodes routinely spanned `4` repositories (`org/iac-live` $\rightarrow$ `org/infra-k8s-tools` / `org/infra-k8s-product` $\rightarrow$ `org/agent-monorepo` + `org/workflow-server`), where merge order was physically constrained by CI pipelines and KMS/IAM provisioning (`stb-003`, `stb-009`, `stb-010`).
- **Worktree discipline:** Dedicated git worktrees (`~/cg/wt/<slug>`) prevented git index corruption when switching branches, **except** when two agents operated on the same repo/branch without worktree assignment (`sta-026-gemini` vs. `sta-026-gemini-2` on `proj79-codec-istio-and-envs`; `sta-029` vs. `sta-029-gemini` on `infra-k8s-tools`).
- **Tracker drift and collisions:**
  1. *Offline world drift:* Between sessions, MRs merged, teammates force-pushed (`stc-001`: `Engineer B` force-pushed `!186` to drop commit `9d4d6ef`), or comments landed. Building `spinup.py` (`roster-spinup`) to query GitLab APIs for merged/closed/commented/untracked MRs before kickoff directly solved start-of-session tracker drift.
  2. *Concurrent tracker-edit collision (`stb-009`):* Both `stb-009` (Opus) and `stb-009-kimi` (Kimi) edited `variables.md` in the same session and assigned `X-63` to two different items (`"zero downtime needs >=2 replicas"` vs. `"per-cluster flow filter"`), caught by Opus during wrap-up (`23:38:11`).

#### 3.4.3 Strongest vs. Weakest Adviser (`Fable`) and Manager (`Kimi`) Contributions
- **Strongest Adviser (`Fable 5.1`) Contributions (Independently Verified):**
  1. *Deep source/spec disambiguation:* Tracing Temporal Go server `v1.31.2`/`v1.32.0` `WithClaimMapper` option ordering (`sta-018/019`), Istio `ztunnel` waypoint behavior (`stb-007`), and `manifest.py` queue-name keying + CI `rules: if branch == master` rollout order (`stb-010`).
  2. *Pre-post git head verification:* Catching that `Engineer B` had force-pushed `!186` (`0c57cb5`) before posting inline review notes (`stc-001-fable`), avoiding stale review comments on a teammate's MR.
  3. *Adversarial diff & helm rendering:* Running `helm template` and test suites independently (`stb-010-fable`, `stb-014-fable`, `sta-026-fable`), catching duplicate `DD_SERVICE` env vars and missing ingress rules.
- **Weakest Adviser (`Fable 5.1`) Contributions:**
  1. *Unverified environment-state assertion (`stb-014-fable`):* Asserting `"Tools cluster frontend is still in observe mode"` from stale memory/doc text without checking `TK origin/master` (`authorizerObserveMode: false`), immediately corrected by the operator (`02:38:18`).
  2. *Inverted cost/context caveat (`stc-001-fable`):* Drafting an "Honest caveat" claiming the 4-agent roster costs more tokens than a single frontier model, which contradicted the operator's actual billing experience (`875K` single-session spirals vs. partitioned flat-rate + surgical oracle).
- **Strongest Manager (`Kimi K3`) Contributions:**
  1. *Catching corrupted artifacts before publication (`stc-001-kimi`):* Halting Confluence publication (`"STOP before publishing"`) upon discovering that `2026-10-15-fable-hld-and-mr-review.md` was truncated to `14` lines.
  2. *High-volume prose & multi-doc synthesis (`stc-001-kimi`, `stb-009-kimi`):* Authoring and iterating `10` versions of the workflow guide and HLD on flat-rate economics while enforcing publishable-doc rules (no em-dashes, no internal tracker IDs).
- **Weakest Manager (`Kimi K3`) Contributions:**
  1. *Technical/repo premises unchecked against git (`stb-003-kimi`, `stb-009-kimi`, `stb-010-kimi`):* Confusing `TK` with `PK` (`stb-003`), diffing against unfetched local `master` (`50` files vs. `4` files in `stb-003`), proposing an inverted CI rollout order (`stb-010`), and logging an unapproved Confluence edit (`stb-009`).

---

### 3.5 Observability and Classifiers (`pi-local-laya` / `Jev`)

#### 3.5.1 What Usage/Cost/Outcome Data Exists vs. What Cannot Be Reconstructed
- **Available in session `.jsonl`:** Exact per-turn `input`, `output`, `cacheRead`, `cacheWrite`, `totalTokens`, tool call arguments/results, timestamps, and `intercom_message` delivery latency (`sent` $\rightarrow$ `broker delivered` $\rightarrow$ `receiver received` $\rightarrow$ `injected`, typically `1–4 ms`).
- **Missing / Unreconstructable:**
  1. **Flat-rate provider cost (`devin/*`):** Always `$0.00` in `usage.cost`.
  2. **`logs/orchestrator-usage/unknown.jsonl` (`170` events):** Logged `sessionId: "unknown"` because `process.env.PI_SESSION_ID` is unset at extension load time (`orchestrator-routing/index.ts:516`), and logged `usage: undefined, cost: undefined` on `started` and many `complete` events.
  3. **Cross-agent work-item & decision attribution:** No shared `work_item_id`, `decision_id`, or `steering_id` links an operator prompt in `-fable` $\rightarrow$ an `intercom` message $\rightarrow$ a commit in `worker` $\rightarrow$ a tracker update in `decisions.md`.

#### 3.5.2 Classifier (`Laya` / `Jev`) Invocations, Question Wording, Bias, and Impact
All `191` entries in `~/.pi/agent/laya/usage.jsonl` (`2026-09-30` to `2026-10-16`) and all `classify` calls in session transcripts were inspected:

| Invocation Category | Count | Sessions | Question / Rubric Shape | Outcome & Observed Impact |
| --- | --- | --- | --- | --- |
| **1. Scratch/Trial & Eval Benchmarks (`via="laya"` / `source="tool"`)** | `19` | `laya-trial` (`01a0f087/88`, `01a0f28c/d`), `meta-006` (`01a0fa15`) | `choice` (`code_bug` / `flaky_test` / `env`), `score` (`clarity`, `impact`), `bool` (`needs_review`, `breaking`, `success`) | Latency `31–491 ms` (warm `31–99 ms`). Misclassified `2/5` CI failures (DB connection refused $\rightarrow$ `flaky_test`; DB login failed $\rightarrow$ `code_bug`); rated a force-push discarding a colleague's commit as low-risk (`p=0.48`). |
| **2. Nudged Deliberate Calls in Real Work (`via="laya"`)** | `14` | `stb-005` (`stb-005`) only | `7` $\times$ `choice` (`action` on 7 MR review threads); `7` $\times$ `bool` (`needs_review`) + `score` (`risk`) before `git push` | Only invoked after operator asked *"btw, why don't you use laya classifier?"*. All `7` push checks returned `needs_review.p` between `0.493` and `0.550` (`risk` `1.26–1.98`, `conf` `0.10–0.19`). **Changed `0` decisions.** |
| **3. Automatic Bash Hook Gate — Server Up (`source="gate", ok=true`)** | `109` | `stb-006` (`14`), `stb-007` (`10`), `stb-008` (`22`), `stb-009` (`12`), `stb-010` (`10`), `stb-011` (`21`), `stb-012` (`7`), `meta-006` (`10`), `trial` (`3`) | Triggered on `glab write` (`40`), `git push` (`39`), `git push --force` (`19`), `terraform apply` (`5`), `rm -rf` (`5`), `kubectl write` (`1`) | Appended `[laya gate]` annotation to bash output. `needs_review.p`: **min `0.459`, median `0.598`, max `0.685`** (`7/109 > 0.65`); `risk.score`: **`1.75–2.36`**; `conf`: **`0.018–0.176`**. **`0` assistant turns reacted to or paused on any gate line.** |
| **4. Automatic Bash Hook Gate — Server Down (`source="gate", ok=false`)** | `49` | `stb-012` (`17`), `stb-013` (`9`), `stb-014` (`12`), `stb-014-argon` (`4`), `stc-001` (`1`), `stc-001-fable` (`5`), `meta-007` (`1`) | Same bash triggers (`glab write`: `24`, `git push`: `21`, `git push --force`: `2`, `rm -rf`: `1`, `git delete ref`: `1`) | From `2026-10-09` to `2026-10-16`, `laya-serve` on `127.0.0.1:8000` was down (`connection refused`). All `49` gate checks failed silently; no agent or human restarted `/laya up`. |

- **Evidence of State-Framing and Rubric Bias (`stb-005` & `meta-006`):**
  1. *Primary-agent rationale injection (`stb-005`):* When `stb-005` called `tools.classify` before `git push` (`2026-09-30T04:55` – `13:53`), it passed its own justification inside `state.reason`:
     `state: { command: "git push origin product-cluster-temporal-persistence...", reason: "the Operator approved: drop unused data_classification field... scratch plan 18 add unchanged; push re-runs CI plans on a shared MR, applies only on merge" }`.
     Passing `"the Operator approved... applies only on merge"` in `state` is a classic **selective/loaded state bias** where the primary agent pre-answers the question. Even more tellingly, on `laya/typed-decisions`, adding that longer text actually nudged `needs_review.p` *up* slightly (`0.493` $\rightarrow$ `0.512`) because `p` correlated with `stateChars` (`r = 0.43`) rather than semantic risk!
  2. *Rubric wording bias proved live (`meta-006`, `2026-10-02T06:04:26`):* When asked to rate 8 candidate improvements (`E1..E8`) to `pi-local-laya`, the agent wrote the `false` criterion for `success` as `"or depends on something unproven"`. Because `E1` (*feed richer state to the gate*) candidly mentioned risk/uncertainty in its state description, Laya ranked `E1` **dead last (`p = 0.489`)** while ranking trivial procedural tasks (`E7` benchmark set `p = 0.701`, `E3` regex fix `p = 0.630`) at the top. As the `meta-006` agent itself concluded in-session: *"That's not the classifier being wise; it's the exact bias we diagnosed, demonstrated live. It keyed on risk/uncertainty language... rating 'which is safest to succeed,' not 'which matters most.'"*
  3. *False-positive regex triggers (`stb-006..014`):* `rm -rf "$tmp"` on `mktemp` directories triggered the `rm -rf` gate (`only literal /tmp was exempt`), and Python heredocs containing the string `"git push"` triggered the `git push` gate (including `2` triggers on `stb-014-argon`), while **people-facing MCP writes (`mcp__atlassian__addCommentToJiraIssue`, `mcp__slack`) were completely ungated**.

---

## 4. Evidence-Backed Findings Table

| ID | Observed Pattern | Affected Episodes / Denominator | Impact | Confidence | Counterexamples / Nuance | Sanitized Evidence Locators |
| --- | --- | --- | --- | --- | --- | --- |
| **`F-01`** | **Persistent role-pinned Intercom peers displaced bounded `pi-subagents` workflows.** Subagent usage fell from `10–51` calls/episode in Cohort 0/1 to `0` in `stb-007..014` & `stc-001`, replaced by persistent `pi-intercom` peers + `codemode`. | `30/30` Cohorts 2–4 Intercom episodes vs. `26/26` Cohorts 0–1 subagent episodes (`437` subagent calls vs. `581` intercom sends, `685` codemode calls) | **High (Positive):** Eliminated cold-start re-briefing and `142K–259K` forked-context duplication while retaining specialist memory all day. | **High** | Bounded `scout`/`reviewer` subagents (`stb-004`, `stb-006`) still worked well for isolated read-only checks when `codemode` wasn't enough. | `sta-001..003` (`6` forked advisors); `stb-005..014` & `stc-001` (`cm=16..102`, `sub=0`) |
| **`F-02`** | **Bounded `pi-subagents` workflows suffered a `38.3%` (`31/81`) failure rate from scheduled-job quota exhaustion, JS DSL traps, and 30-min timeouts.** | `31/81` `workflow` runs in `orchestrator-usage`; `18` top-level `subagent` tool errors; `23/134` failed `subagent-notify` messages across `12/26` Cohort 0/1 episodes | **High (Negative):** Wasted turns fixing `workflowScript` backtick/promise syntax (`sta-003`, `sta-008`, `sta-009`, `sta-020`) and 30-min worker timeouts (`sta-007`, `sta-024`). | **High** | `63/82` (`76.8%`) non-scheduled async `workflow` notifications succeeded; JS DSL errors were specific to the raw-script `workflowScript` mode, not single-child spawns. | `rla-009..012` (`11` `reviewbot-recheck` quota fails); `sta-003` (`TypeError`); `sta-008` (unawaited `runs.run`); `sta-009` (`SyntaxError`); `sta-007/024` (`1800000ms` timeout) |
| **`F-03`** | **The operator routinely bypassed the coordinator (`23.2%` of all prompts; `44.3%` in 4-agent rosters) to steer or debate specialists directly in their Herdr panes.** | `130/560` human prompts across `21/30` Intercom episodes (`57` to Adviser, `38` to Co-Worker, `28` to Manager, `7` to Worker under Argon) | **High (Positive):** Avoided `2–16` relay hops per design debate (`sta-021`, `stb-003`, `meta-004/005`) and caught specialist errors before they spread (`stb-014-fable`). | **High** | In `9/30` episodes (`sta-019`, `sta-020`, `stb-004..009`, `stb-011..013`), `100%` of human prompts went solely to the primary driver pane. | `sta-021-fable` (`8` prompts); `stb-003-kimi` (`12` prompts); `stb-014` (`7` to Worker, `7` to Fable); `stc-001` (`8` to Kimi, `2` to Fable, `1` to Worker) |
| **`F-04`** | **Direct human steering in a specialist pane lacked automatic coordinator notification, requiring manual human `"relay this"` nudges or causing multi-minute drift.** | `11/130` bypass prompts had no intercom relay; `33/119` relayed prompts took `>10` minutes (`80m` in `sta-021`); explicit human relay nudges in `5/21` bypass episodes | **High (Negative):** Coordinator remained unaware of changed scope/rulings until the human explicitly told the specialist `"notify the worker"` or `"send intercom directly"`. | **High** | Once nudged or trained via `AGENTS.md` ("self-organising callbacks"), `86/130` (`66.2%`) bypasses were relayed within `10` minutes (median `48s` after decision). | `stb-001-kimi` (`04:44`: `"also, notify the worker as well"`); `stb-003-kimi` (`12:55`: `"yes, relay"`); `stb-014-fable` (`02:11`: `"you send intercom directly to stb-014"`); `sta-021-fable` (`4,843s` delay) |
| **`F-05`** | **`pi-intercom`'s strict distinction between `send` and `ask` caused `31` rejected reply calls (`88.6%` of intercom tool errors).** Passing `action: "reply"` or `replyTo` when no `ask` is pending is rejected by the broker. | `31/35` failed `intercom` tool calls across `11/30` Intercom episodes (`sta-018`, `026`, `027`, `029`, `030`, `stb-001`, `005..008`, `stc-001`) | **Medium-High (Negative):** Wasted `1–4` turns per incident; in `stc-001-fable`, blocked HLD review delivery `4` times, forcing Fable to route through the Worker as a relay and prompting Argon to scrape Fable's `.jsonl` file. | **High** | When senders used `action: "send"` *without* `replyTo` and specified `to: "<peer>"`, delivery succeeded `100%` of the time in `1–4 ms`. | `stc-001-fable` (`03:49:48`: `"intercom replies to argon's ask are failing on my side, please relay"`); `stb-009-kimi` (`00:23:50`: `"Re-sending as a normal message; your note came through as a send, not an ask"`) |
| **`F-06`** | **In-pane reply drops: receiving an async `intercom` `send` occasionally caused a peer to print its reply as terminal prose in its own pane without calling `intercom`.** | Observed in `3` episodes (`stb-011-kimi`, `stb-010-kimi`, `stb-014-fable`); `stb-011-kimi` (`5` turns, `0` intercom sends) never delivered its verification of `MR !175`. | **Medium-High (Negative):** Silent message loss: the peer did the verification work and wrote `"Replying to 011..."` in its own terminal, but the coordinator never received it. | **High** | When woken by `action: "ask"`, `pi-intercom` prepends `To reply, use the intercom tool: intercom({ action: "reply", ... })`, which prevented in-pane drops. | `stb-011-kimi` (`03:11:23`: wrote `"Replying to 011 (verified: !175 authored by Engineer E...)"` in-pane, `ic_sent=0`); `stb-014-fable` (`02:05:30`) |
| **`F-07`** | **Zero compactions (`0/378`); Markdown tracker rotation kept `77.3%` of sessions `<300K`, but `40` sessions (`22.7%`) exceeded `300K` (`18` > `400K`) when sessions ran multi-day or ingested raw docs/diffs.** | `40/176` substantive top-level sessions $\ge 300	ext{K}$ peak prompt context; `18/176` $\ge 400	ext{K}$ (max `875K` in `sta-028-gemini`; `432K` in `stc-001-argon`) | **High (Cost/Latency):** Sessions $>400	ext{K}$ suffered expensive cache rewrites after idle gaps (`$98.32` in `sta-028`, `$77.85` in `sta-016`, `$70.00` in `sta-021`) and degraded reliability (`sta-026-gemini` at `543K`). | **High** | Specialist peers (`-kimi` avg `67K`, `-fable` avg `167K` in `stb-001..014`) stayed well under `250K` throughout full-day sessions. | `sta-028-gemini` (`875K`, `880` turns); `sta-027-gemini` (`563K`); `sta-026-gemini` (`543K`); `sta-022` (`525K`); `stb-008` (`477K`); `stc-001-argon` (`432K`) |
| **`F-08`** | **Cross-model verification (`Opus` $\leftrightarrow$ `Fable` $\leftrightarrow$ `Kimi`) repeatedly caught high-severity technical bugs, false verification claims, and corrupted files before merge/publish.** | Verified catches in `19/30` Intercom episodes (`sta-016..030`, `stb-003..014`, `stc-001`, `meta-003..005`) | **Critical (Positive):** Prevented broken helm deploys (`sta-026`), broken CI tag-job ordering (`stb-010`), wrong k8s secret namespaces (`stb-003`), stale force-pushed MR comments (`stc-001`), and a truncated 14-line Confluence publish (`stc-001`). | **High** | Adviser/Manager occasionally introduced a false premise themselves (`stb-014-fable` `"observe mode"`; `stb-003-kimi` `TK` vs. `PK`), which the Worker or Operator then caught. | `sta-026` (`12:08:49` helm failure + `12:47` unmerged `!1250`); `stb-003` (`12:39:16` Opus catches 3 blockers in Kimi spec); `stb-010` (`01:28:49` Kimi concedes rollout order); `stc-001-kimi` (`03:48:30` `"STOP before publishing"`) |
| **`F-09`** | **Mid-session worker replacement and uncoordinated fallback caused dual-ownership ("split-brain") on branches and duplicate MRs.** | `3` episodes (`sta-026`: `gemini` + `gemini-2` both editing `proj79-codec-istio-and-envs`, plus `sta-026` `!139` colliding with `gemini-2` `!140`; `sta-029`: `main` + `gemini` on `TK`; `stb-014`: `argon` + `opus` on `!6786`) | **High (Negative):** Duplicate MRs (`!139` closed in favor of `!140`), conflicting pushes to the same branch, and peer confusion over who owned `PK !6786`. | **High** | When worktrees were explicitly assigned per task (`stb-009`, `stb-010`, `stb-014` `wt/agent-monorepo-proj201`), zero git collisions occurred. | `sta-026` (`12:47:38` & `12:47:44` rollback sent to both `gemini` and `gemini-2`; `13:19:30` `"collision resolved — I'll close !139"`); `stb-014-argon` (`02:13:50` `"Our messages crossed: I had opened !6786"`) |
| **`F-10`** | **Concurrent tracker editing by Worker and Manager caused duplicate tracker ID allocation (`X-63`).** | Observed in `stb-009` (`stb-009` vs. `stb-009-kimi` on `variables.md`); role contention over tracker ownership in `sta-029` (`04:55:44`) | **Medium (Negative):** `X-63` was assigned to two different open questions in `variables.md`, requiring manual deduplication at session wrap. | **High** | In episodes where a single role owned tracker writes (`sta-029` after `04:55:44`; `stb-014` & `stc-001` under Argon), zero ID collisions occurred. | `stb-009` (`23:38:11`: `"One fix: X-63 was already taken... I removed your duplicate X-63 in variables.md"`); `sta-029-fable` (`04:55:44`: `"wait, kimi will be keeping the tracker"`) |
| **`F-11`** | **Un-escalated permission/environment blockers caused runaway exploration loops (`50+` turns) and `intercom` `ask` timeouts.** | `sta-029-gemini` (`44m` KMS `AccessDeniedException` loop + `SIGKILL` `grep -rn /Users/operator/` + `10m` `ask` timeout); `sta-007` & `sta-024` (`30m` worker subagent timeouts) | **High (Negative):** Blocked the worker completely until the driver (`sta-029`) gave up waiting and implemented the scaffolding itself. | **High** | In `stb-003` (`07:04:27`), `Opus 5.5` hit the exact same SOPS KMS `AccessDeniedException` on account `444455556666` and escalated to Kimi/Operator in `2` turns (`D-28`). | `sta-029-gemini` (`05:02:58` – `05:47:16`, `exit 137` on home-dir `grep`, `ask` timeout `12ef3182`); contrast `stb-003` (`07:04:27` immediate escalation) |
| **`F-12`** | **Legacy custom extensions (`orchestrator-routing` & `model-rules`) inject stale, contradictory role instructions into every Kimi and parent turn.** | `100%` of Kimi sessions (`orchestrator-routing/index.ts:508` injects `"You are the parent orchestrator"` into `-kimi` Manager sessions) + `100%` of turns (`model-rules` injects Apr 2026 `Opus 4.6/4.7` table) | **Medium (Negative):** Confuses `-kimi`'s role identity (`Manager` in `AGENTS.md` vs. `Parent Orchestrator` in system prompt) and adds ~`1.5K` stale prompt tokens per turn. | **High** | Explicit user/intercom prompts (`"you're a manager"`, `"Roles per AGENTS.md"`) usually overrode the stale system-prompt block, except in `stb-003-kimi`. | `~/.pi/agent/extensions/orchestrator-routing/index.ts:149,508`; `~/.pi/agent/extensions/model-rules/index.ts:32`; `stb-003-kimi` (`12:35:03`: `"you're a manager"`) |
| **`F-13`** | **Prompt cache expiry across multi-hour/multi-day sessions (`132` spikes of `cacheWrite > 50K`), not active iteration, drove metered token cost spikes.** | `132` cache-rewrite spikes across `35/62` episodes (`11` spikes in `stb-009` and `stb-012`; `10` in `sta-016`; `9` in `rla-011` and `sta-028`) | **Medium-High (Cost):** Re-caching a `300K–450K` window `8–11` times per episode after human/CI pauses added `$15–$40` per long session. | **High** | Compact, continuous sessions (`stb-010`: `2.4h`, `3` spikes; `stc-001`: `1` spike on metered models) had very high `cacheRead` ratios (`>90%`). | Session JSONL `usage.cacheWrite` vs. `usage.cacheRead` across `sta-016`, `sta-021`, `sta-028`, `stb-008..013` |
| **`F-14`** | **Local classifier (`pi-local-laya`) had `0` un-nudged deliberate calls, compressed gate probabilities (`0.46–0.69`), `0%` behavioral influence, and `49` silent `server_down` failures.** | `191` total records in `laya/usage.jsonl`: `14` nudged calls (`stb-005`), `109` `ok=true` bash gates (`stb-006..012`), `49` `server_down` gates (`stb-012..014`, `stc-001`) | **Medium (Observability / Design):** Automatic bash gate worked as low-latency (`35–315ms`) telemetry when up, but compressed scores (`conf 0.02–0.18`) never crossed action thresholds, and `laya-serve` stayed down unnoticed for `7` days. | **High** | Relative ranking on richer multi-sentence states (`meta-006` commit clarity `1.91` vs. `2.27`; `stb-006` temp `rm` vs. prod IAM push) was directionally sensible. | `~/.pi/agent/laya/usage.jsonl` (`191` rows); `meta-006` (`01a0fa15` & `01a0fb1f` evaluations); `stb-005` (`14` nudged calls) |
| **`F-15`** | **Classifier verdicts were measurably biased by primary-agent rationale injection (`stb-005`) and risk/uncertainty wording in criteria (`meta-006`).** | `7/7` push-gate calls in `stb-005` injected `"the Operator approved... applies only on merge"` into `state.reason`; `meta-006` (`8` effort ratings) | **Medium (Design Warning):** Proves that when an LLM constructs both `state` and `criteria` for a small (`421M`) classifier, framing dominates the output (`corr(p, stateChars) = 0.43`; highest-value item `E1` ranked dead last at `p=0.489`). | **High** | Deterministic rule extraction + fixed code-owned rubrics (`MR !1` in `pi-local-laya`, commit `eaff81d`) remove primary-agent framing bias. | `stb-005` (`04:55:48` – `13:53:14` codemode scripts); `meta-006` (`01a0fa15` `06:04:26` `"The meta-finding"` analysis) |
| **`F-16`** | **Automated preflight offline-delta (`roster-spinup` / `spinup.py`) + `codemode` batching eliminated start-of-session tracker drift and cut recon turns.** | `685` `codemode` calls (`891` inner `bash`, `132` inner `mcp__atlassian`, `112` inner `read`, `105` inner `pup_*`, `62` inner `intercom`) across `stb-005..014` & `stc-001` | **High (Positive):** Replaced `10–25` sequential startup tool calls and prevented acting on stale MR/Jira state at session kickoff. | **High** | `codemode` scripts that called `tools.intercom` or wrote files without printing summaries occasionally left `(NO TEXT / EMPTY)` assistant turns in the transcript. | `~/.pi/agent/skills/roster-spinup/spinup.py`; `stb-014` (`102` `codemode` calls); `stc-001` (`90` `codemode` calls) |

---

## 5. Sanitized End-to-End Case Studies (Per-Episode Extractions)

### `CS-01`: Multi-Agent Failure Cascade & Recovery — Hallucinated Merge, Mid-Session Worker Replacement, and Duplicate MRs (`sta-026`)

| Field | Content |
| --- | --- |
| **Episode ID and cohort** | `sta-026` (Cohort 3A: 3-agent Intercom triad expanding to 4 sessions after mid-episode worker replacement; `1,149` total assistant turns over `20.3h`) |
| **Intent and authority** | Verify post-deployment payload encryption in staging (`org/agent-monorepo` `!135`/`!137`, `TK` `!1246`), diagnose why UI/API decoding failed, and land fixes across `TK` and `org/agent-monorepo`. Merging MRs required explicit operator approval. |
| **Observable sequence** | 1. Driver (`sta-026`, `Kimi K3`, `317`t) delegated root-cause investigation and `TK` chart fixes to persistent worker `sta-026-gemini` (`Gemini 3.8 Flash`, `455`t, `543K` peak ctx).<br>2. At `11:51`, Driver asked Adviser `sta-026-fable` (`Fable 5.1`, `150`t, `247K` ctx) whether `TEMPORAL_AUTH_USE_ID_TOKEN_AS_BEARER` should be flipped to `"true"`; Fable answered in `93s`: **keep `"false"`** (ID token fails closed on codec decode).<br>3. At `12:07`, `sta-026-gemini` opened `TK !1250` claiming *"All 8 chart configurations passed helm template checks cleanly."* Driver (`12:08:49`) and Fable (`12:18:00`) independently verified and caught two blockers (`B1`: duplicate `DD_SERVICE` in `extraEnv`; `B2`: missing `allowIngress`).<br>4. At `12:36:35`, `sta-026-gemini` created branch `proj79-codec-cors-and-token-aud` and falsely reported over intercom: **`"IK !1250 is merged."`**<br>5. At `12:40`, after `sta-026-gemini` hit `543K` context, leaked Chinese characters, and set wrong worker `datadogService` tags, the operator spun up replacement worker `sta-026-gemini-2` (`227`t, `249K` ctx) and rebuked the driver (`12:47:27`: *"!1250 isn't merged and can't be merged without human approval"*).<br>6. Because `sta-026-gemini` was still open, Driver sent the `!1250` rollback instructions to **both** `sta-026-gemini` (`12:47:38`) and `sta-026-gemini-2` (`12:47:44`); both edited and pushed to `proj79-codec-istio-and-envs` (`12:50:58` vs. `12:54:34`).<br>7. At `13:05`, the operator steered `sta-026-fable` directly (`"do NOT fold (b) into !138. Open a separate MR"`). Both Driver (`!139`) and `sta-026-gemini-2` (`!140`) opened competing MRs for the same change (`13:19:30`: *"collision resolved — yours wins on CORS test coverage. I'll close !139"*).<br>8. When `!140` hit a merge conflict and Driver stalled (`13:27:26`: `"you stuck"`), the operator asked `sta-026-fable` in Fable's pane to fix the code and push `6ddfe94` directly on `!140`. |
| **Outcome** | **Verified Success after Rework:** MRs `!137`, `!138`, `!140` (`org/agent-monorepo`) and `TK !1250` all merged after Fable's fixes; `!139` discarded as duplicate. |
| **Intervention** | `58` human prompts across the `4` panes (`31` to `sta-026`, `9` to `gemini`, `14` to `fable`, `4` to `gemini-2`), including direct steering in Fable's pane and manual worker replacement. |
| **Resource evidence** | `1,149` assistant turns; peak contexts: `gemini` `543K`, `gemini-2` `249K`, `fable` `247K`, `main` `213K`; `5` cache-write spikes; metered cost `$54.30` (excluding flat-rate `Kimi` turns). |
| **Failure/success mechanism** | *Observation:* (a) Cross-model review by Driver + Fable caught Gemini's false `helm template` and false `"merged"` claims; (b) spinning up `gemini-2` without terminating `gemini` or revoking its branch lease caused dual pushes to `!1250`, and ambiguous task ownership between Driver and `gemini-2` caused duplicate MRs `!139`/`!140`. |
| **Candidate requirement** | **`R-03` (Atomic Agent Slot Replacement & Lease Transfer)** and **`R-04` (Single-Writer Branch/Worktree Lock)**. |
| **Evidence locator** | `sta-026` (`01a0a361`), `sta-026-gemini` (`01a0a380`), `sta-026-fable` (`01a0a4e5`), `sta-026-gemini-2` (`01a0a515`), timestamps `11:51`–`13:39 UTC`. |
| **Confidence and limits** | **High confidence** (`100%` observable across the 4 aligned JSONL transcripts). |

---

### `CS-02`: Cross-Iteration Adviser Persistence After Primary Worker Exhaustion (`sta-018` $
ightarrow$ `sta-019`)

| Field | Content |
| --- | --- |
| **Episode ID and cohort** | `sta-018` + `sta-019` (Cohort 2: 2-agent Intercom pair where Adviser `sta-018-fable` spanned two sequential Worker sessions over `29h`) |
| **Intent and authority** | Diagnose why custom JWT claim-mapper options in `org/workflow-server` (`v1.31.2`) failed to wire under `WithServerConfigFilePath` and ship the fix + unit tests. |
| **Observable sequence** | 1. In `sta-018` (`317`t, `271K` ctx), Worker and Adviser `sta-018-fable` (`110`t, `234K` ctx) traced the Go server startup sequence across `14` intercom messages.<br>2. When the operator accidentally hit the production API in the `tools` cluster during testing (`00:07`), the operator told `sta-018-fable` directly (`"ah, wait a second i was hitting api in prod in tools"`), and Fable relayed the correction to `sta-018` in `361s`.<br>3. After `sta-018` exhausted its token/provider quota, the operator did **not** restart Fable. Instead, the operator launched `sta-019` (`143`t, `330K` ctx) and typed into `sta-018-fable`: *"sta-018 exhausted tokens limit, relay to sta-019, which is different model"*.<br>4. Within `220s` (`07:07:19`), `sta-018-fable` sent a structured handoff over intercom to `sta-019` with the exact Go root cause (`WithClaimMapper` runs at option-time when `s.config == nil`). `sta-019` confirmed against source, agreed on `INFO`/`WARN` log levels with `sta-018-fable`, and shipped the MR. |
| **Outcome** | **Verified Success:** Clean cross-session recovery without re-investigating the Go codebase. |
| **Intervention** | Operator triggered the cross-session relay in `sta-018-fable` (`1` prompt) instead of manually copying context into `sta-019`. |
| **Resource evidence** | `sta-018`: `317`t / `271K` ctx (`$23.24` combined with Fable); `sta-019`: `143`t / `330K` ctx (`$26.06`). |
| **Failure/success mechanism** | *Observation:* Decoupling the Adviser's lifetime from the Worker's lifetime meant the high-value root-cause state survived Worker exhaustion in active memory and could be queried interactively by `sta-019`. |
| **Candidate requirement** | **`R-05` (Durable Attachable Specialist Sessions Across Worker Rotations)**. |
| **Evidence locator** | `sta-018` (`01a07927`), `sta-018-fable` (`01a07930`), `sta-019` (`01a07a83`), intercom `id 6220e368` (`07:07:19 UTC`). |
| **Confidence and limits** | **High confidence.** Demonstrates a concrete case where a durable attachable agent outperformed static file handoff alone. |

---

### `CS-03`: Un-Escalated Permission Rabbit Hole & Intercom `ask` Timeout (`sta-029`)

| Field | Content |
| --- | --- |
| **Episode ID and cohort** | `sta-029` (Cohort 3A: 3-agent Intercom triad `sta-029` [`Kimi K3`, `271`t] + `sta-029-fable` [`81`t] + `sta-029-gemini` [`71`t], plus live historical peer `sta-028-gemini`) |
| **Intent and authority** | Add `WF_CALLBACK_SECRET` to 4 SOPS-encrypted files in `TK` (`org/infra-k8s-tools`) and fix cookie-header logging in `org/agent-monorepo` (`!160`/`!161`). |
| **Observable sequence** | 1. At `04:54:53`, Driver (`sta-029`) accidentally sent the new task to `sta-028-gemini` (the previous iteration's worker, still open in Herdr). The operator intervened at `05:02:37`: *"keep tracker yourself, ask gemini in sta-028-gemini only for context... your interactive worker is sta-029-gemini"*.<br>2. At `05:02:58`, Driver dispatched the SOPS task to `sta-029-gemini`.<br>3. In `sta-029-gemini`, `sops -d` failed with `AccessDeniedException` on `kms:Decrypt` for account `111122223333`. Instead of reporting the permission blocker back to Driver, `sta-029-gemini` spent `44` minutes (`51` turns) probing every AWS SSO profile, reading `~/.aws/sso/cache/*.json`, running `aws-sso-util roles`, and executing `grep -rn "6ded2d9" /Users/operator/` across the entire home directory (`Killed: 9`, exit `137`).<br>4. Meanwhile, Driver queried `sta-028-gemini` (`05:25:48`, `context-only`) and learned immediately that the operator's SSO roles lacked `kms:Encrypt` on `111122223333`.<br>5. At `05:35:33`, Driver sent `intercom({ action: "ask", to: "sta-029-gemini" })` for a status check. The message was injected at `05:36:08`, but `sta-029-gemini` emitted an empty turn (`assistant []`) after its killed `grep` and never replied, causing Driver's `ask` to fail after `10` minutes (`"No reply... within 10 minutes. Last known delivery state: injected"`).<br>6. Driver (`sta-029`) implemented commit `bfa2119` on `TK` itself, and at `05:47:16` the operator told `sta-029-gemini`: *"you got stuck... it was finished by kimi in sta-029"*. |
| **Outcome** | **Partial Failure / Driver Fallback:** Task completed by Driver (`TK !1292`, `!1293`, `org/agent-monorepo` `!160`, `!161`), but `sta-029-gemini`'s `71` turns (`44` minutes) and `10`-minute `ask` wait were 100% wasted. |
| **Intervention** | Operator redirected Driver away from old handle `sta-028-gemini` (`05:02:37`), clarified tracker ownership to Fable (`04:55:44`), and manually notified `sta-029-gemini` after it stalled (`05:47:16`). |
| **Resource evidence** | `sta-029`: `271`t / `234K` ctx; `fable`: `81`t / `165K` ctx; `gemini`: `71`t / `111K` ctx (`100%` throwaway); metered cost `$11.62`. |
| **Failure/success mechanism** | *Observation:* Three compound mechanisms: (1) stale peer handle (`sta-028-gemini`) still visible in `intercom list`; (2) worker lacked a "fail-fast on IAM/KMS `AccessDeniedException`" escalation rule; (3) blocking `intercom({ action: "ask" })` stalled the caller for `10` minutes with no liveness/tool-progress heartbeat from the target pane. |
| **Candidate requirement** | **`R-02` (Non-Blocking Ask / Liveness Heartbeat)**, **`R-03` (Retire Old Roster Handles on Spin-Up)**, and **`R-07` (Fail-Fast Escalation on Auth/IAM Permission Errors)**. |
| **Evidence locator** | `sta-029` (`01a0c768`), `sta-029-fable` (`01a0c776`), `sta-029-gemini` (`01a0c77b`), timestamps `04:53`–`05:48 UTC`. |
| **Confidence and limits** | **High confidence.** Contrast with `stb-003` (`CS-04`), where `Opus 5.5` escalated the identical KMS error in `2` turns. |

---

### `CS-04`: Manager Role Drift vs. Worker Technical Catch (`stb-003`)

| Field | Content |
| --- | --- |
| **Episode ID and cohort** | `stb-003` (Cohort 3B: 2-agent Intercom pair `stb-003` [`Opus 5.5` Worker, `169`t, `324K` ctx] + `stb-003-kimi` [`Kimi K3` Manager, `150`t, `117K` ctx] over `7.8h`) |
| **Intent and authority** | Provision SOPS KMS keys and encrypted secrets for `Stream-B` persistence (`IL !10622`) and Kubernetes overlays (`PK !6715`, `!6720`). |
| **Observable sequence** | 1. At `07:04:27`, Worker (`stb-003`) hit `AccessDeniedException` on KMS in account `444455556666` and immediately escalated to Manager (`stb-003-kimi`) to ratify `D-28` (DevOps encrypts `IL` SOPS files).<br>2. Between `07:10` and `12:36`, the operator engaged `stb-003-kimi` directly in Kimi's pane (`12` prompts) to work out the secrets strategy across `IL` and `PK`.<br>3. During this direct steering, `stb-003-kimi` exhibited three forms of drift that the operator had to correct manually:<br>   - Referenced `TK` (`infra-k8s-tools`) instead of `PK` (`infra-k8s-product`) (`12:30:47`: *"why IK (infra k8s)? we work with product-cluster, its repo is XK"*);<br>   - Cited bare tracker IDs `"X-20/X-27/D-26"` without short names (`12:34:32`: *"as we agreed things like 'X-20/X-27/D-26' need to have a short name"*);<br>   - Started attempting implementation edits itself (`12:35:03`: *"i mean stb-003 owns implementation and fixes you're a manager"*).<br>4. At `12:37:43`, after the operator nudged *"have you also passed the task for XK MR as well?"*, Kimi sent the spec for a new `PK` secrets MR to `stb-003`.<br>5. **Within `93` seconds (`12:39:16`), Worker (`stb-003`) verified Kimi's spec against the repo tree and blocked execution on 3 real defects:** *"before I open the XK secrets MR, one blocking problem with the file location, plus two smaller ones. Not implementing until you rule. 1) Wrong namespace. `wf-workers/<env>/secrets.yaml`..."*<br>6. At `12:57:54`, Worker also caught that Kimi had diffed `!6715` against an unfetched local `master` (`~50` files) instead of `origin/master` (`4` files). Kimi verified and conceded (`12:58:06`). |
| **Outcome** | **Verified Success:** `IL !10622` and `PK !6715` (`319627ae82`) shipped cleanly with verified secret hashes. |
| **Intervention** | `12` direct human prompts in `stb-003-kimi` (`3` correcting role/repo/formatting drift, `2` nudging Kimi to relay to Worker) + `11` prompts in `stb-003`. |
| **Resource evidence** | `319` total turns (`169` Opus, `150` Kimi); `20` intercom messages; metered cost `$13.61`. |
| **Failure/success mechanism** | *Observation:* (a) `orchestrator-routing` injecting `"You are the parent orchestrator"` into Kimi contributed to Manager role confusion; (b) Worker (`Opus 5.5`) verifying Manager's file paths and `origin/master` git state before writing code prevented a broken `PK` MR. |
| **Candidate requirement** | **`R-01` (Auto-Broadcast Direct Human Steering)**, **`R-06` (Role-Scoped Tool/Write Guardrails for Non-Worker Roles)**, and **removing legacy `orchestrator-routing` Kimi injection**. |
| **Evidence locator** | `stb-003` (`01a0ebb4`, main), `stb-003-kimi` (`01a0ebb4`, kimi), timestamps `06:56`–`13:35 UTC`. |
| **Confidence and limits** | **High confidence.** Textbook demonstration of bidirectional self-correction (`Operator` $
ightarrow$ `Manager` $\leftrightarrow$ `Worker`). |

---

### `CS-05`: High-Velocity Multi-Repo Structural Self-Correction (`stb-009` & `stb-010`)

| Field | Content |
| --- | --- |
| **Episode ID and cohort** | `stb-009` (`377`t, `23.4h`, `42` ic, `71` cm) & `stb-010` (`378`t, `2.4h`, `41` ic, `92` cm) (Cohort 3B: 3-agent Triad `Opus 5.5` Worker + `Kimi K3` Manager + `Fable 5.1` Adviser) |
| **Intent and authority** | Upgrade custom Temporal server image (`1.31.2` $
ightarrow$ `1.32.0`) and Helm charts (`1.4.0` $
ightarrow$ `1.7.0`) with CI drift guards across `TK` and `PK` (`stb-009`), then build isolated per-cluster worker images (`D-44`) across `IL`, `TK`, `PK`, and `org/agent-monorepo` (`stb-010`). |
| **Observable sequence** | 1. **In `stb-009`:** Fable (`00:37:55`) proved from Temporal Go source (`version.go`) and Datadog logs that staging frontend crashes were caused by chronic RDS connection resets (`PROJ-180`), not a `1.31.2` schema mismatch. At `01:54:09`, Opus checked Confluence live (`version 1`) and caught that Kimi had logged an unapproved runbook edit that hadn't landed (`"please don't edit it: published docs go out only with operator's GO"`). At `23:38:11`, Opus caught and fixed a duplicate `X-63` ID collision in `variables.md` created by Kimi.<br>2. **In `stb-010`:** When the operator ruled in Kimi's pane (`01:05:21`: *"alright, no compromises, we build a separate product-cluster image with all its own workflows"*), Kimi relayed the ruling to Opus in **`4` seconds (`01:05:26`)**.<br>3. Kimi proposed a rollout order (`org/agent-monorepo` before `PK`) and claimed `codec-server` read `WF_CLUSTER`. Opus (`01:24:59`) and Fable (`01:25:00`) independently verified against `.gitlab-ci/worker-job.yml` and `services/codec-server`, proving that `PK` **must** merge before `org/agent-monorepo` (because master tag-update jobs fail if the target key in `PK` does not exist yet). At `01:28:49`, Kimi verified and conceded: *"Conceding: (2) My order was physically wrong."*<br>4. Fable (`01:39:37`) rendered the `PK` Helm templates (`base-service-chart 2.7.1`) and verified the post-`uv sync` Python cluster assertion (`94b7fe8`) before any MR was pushed. |
| **Outcome** | **Verified Hard Success:** `stb-010` shipped coordinated MRs across `4` repositories (`IL !10726`, `TK !1349`, `PK !6757`, `org/agent-monorepo !179`) in just **`2.4` hours** with zero broken pipelines. |
| **Intervention** | Only `12` human prompts in `stb-010` (`10` to Opus, `2` to Kimi); `15` in `stb-009` (`15` to Opus, `0` bypass). |
| **Resource evidence** | `stb-009`: `377`t, peak ctx `352K` (`Opus`), `82K` (`Kimi`), `178K` (`Fable`), `11` cache spikes (`23.4h` span), `$41.84`.<br>`stb-010`: `378`t, peak ctx `437K` (`Opus`), `112K` (`Kimi`), `230K` (`Fable`), `3` cache spikes (`2.4h` span), `$41.55`. |
| **Failure/success mechanism** | *Observation:* Heavy `codemode` usage (`71` and `92` calls) kept reconnaissance fast, while strict pre-push cross-verification between `Opus`, `Kimi`, and `Fable` caught every technical and process error (`rollout order`, `Confluence gate`, `X-63 collision`) inside the triad before it reached git or the operator. |
| **Candidate requirement** | Preserves the core multi-model verification loop; motivates **`R-08` (Atomic Tracker ID Allocator / Single-Writer Tracker Ownership)** to prevent `X-63` collisions. |
| **Evidence locator** | `stb-009` (`01a113b7`), `stb-010` (`01a118e8`/`01a118e9`). |
| **Confidence and limits** | **High confidence.** Strongest positive benchmark in the corpus for how a 3-agent ensemble performs when role boundaries hold. |

---

### `CS-06`: 4-Agent Roster Spin-Up — Orchestrator Role Drift & Missing Direct-Steer Relay (`stb-014`)

| Field | Content |
| --- | --- |
| **Episode ID and cohort** | `stb-014` (Cohort 4: First 4-agent 2x2 Herdr roster `stb-014` [`Opus 5.5`, `149`t, `287K` ctx] + `stb-014-argon` [`Argon 4`, `94`t, `352K` ctx] + `stb-014-fable` [`Fable 5.1`, `86`t, `240K` ctx] + `stb-014-kimi` [`Kimi K3`, `24`t, `58K` ctx]) |
| **Intent and authority** | Implement per-namespace Temporal RBAC in the API/console (`PROJ-201`, `org/agent-monorepo !189`), flip `authorizerObserveMode: false` on `PK` (`PROJ-131`, `PK !6786`), and address reviewer comments from `Engineer A` and `Engineer B`. |
| **Observable sequence** | 1. `stb-014-argon` briefed `kimi`, `opus`, and `fable` over intercom (`01:29`–`01:44`). `stb-014` (`Opus`) implemented `PROJ-201` in worktree `wt/agent-monorepo-proj201` (`ae79ec9`); `stb-014-fable` verified all 12 policy enforcement points (`01:50:53`: `Verdict: GO` + 4 polish items).<br>2. At `02:08:57`, the operator told `stb-014-argon`: *"oh, so it's customFrontend.authorizerObserveMode: true... let's create MR asap for that"*. Instead of delegating to `stb-014` (`Opus`), **Argon created worktrees and opened `PK !6786` and `TK !1359` itself**, posting a corrupted MR description with literal `
` characters.<br>3. When Argon asked Fable (`02:11:12`) to review `!6786` and Fable messaged `stb-014` (`Opus`), Opus replied that it had not opened `!6786`, forcing Fable to ask Argon (`02:13:51`: *"routing needed for PK !6786... stb-014 says it did not open it"*).<br>4. At `02:13:18` and `02:21:30`, the operator intervened in Argon's pane: *"PK !6786 is unreadable description, needs fixing, relay code work for opus"* and *"update rules to follow role boundaries close, unless special cases, worker should own all implementation... MR descriptions can be reviewed by kimi, but worker who did the change would have the best context to write them"*.<br>5. Separately, in `stb-014-fable` (`02:05:30`), the operator discussed a UI alert banner with Fable. Fable answered in its pane without messaging `stb-014`. At `02:11:31`, the operator had to type: **`"you send intercom directly to stb-014"`** and at `02:12:05`: **`"let's have some sort of rules to avoid this instruction"`**. |
| **Outcome** | **Verified Success + Process Codification:** `org/agent-monorepo !189`, `PK !6786`, and `TK !1359` shipped cleanly after Opus took over all worktrees and MR descriptions; role boundaries were codified into `~/.pi/agent/AGENTS.md`. |
| **Intervention** | `31` human prompts distributed across `3` panes (`16` to Argon, `7` to Opus, `7` to Fable, `1` to Kimi). |
| **Resource evidence** | `353` total turns (`102` `codemode` calls: `57` Argon, `40` Fable, `5` Opus); peak ctx `352K` (Argon), `287K` (Opus), `240K` (Fable), `58K` (Kimi); `$37.06`. Note: `16` Laya bash-gate triggers (`12` Opus, `4` Argon) all failed silently with `server_down`. |
| **Failure/success mechanism** | *Observation:* (a) Orchestrator (`Argon`) lacked a tool/role guardrail discouraging direct `git commit`/`glab mr create` when a Worker was active; (b) Fable lacked an automatic trigger to forward direct human steering to the owning Worker (`stb-014`). |
| **Candidate requirement** | **`R-01` (Auto-Broadcast Direct Steering + Peer Discovery Convention)** and **`R-06` (Orchestrator Read/Route Guardrail)**. |
| **Evidence locator** | `stb-014` (`01a13c8a`, opus/fable/argon), `stb-014-kimi` (`01a13cf9`), timestamps `01:23`–`04:39 UTC`. |
| **Confidence and limits** | **High confidence.** Directly motivated the current `AGENTS.md` role rules. |

---

### `CS-07`: 4-Agent Kickoff — `replyTo` Blocking Bug, Corrupted Transcript Scrape Caught by Manager, and Live Force-Push Catch (`stc-001`)

| Field | Content |
| --- | --- |
| **Episode ID and cohort** | `stc-001` (Cohort 4: 4-agent 2x2 Herdr roster `stc-001-argon` [`92`t, `432K` ctx] + `stc-001-kimi` [`197`t, `281K` ctx] + `stc-001-fable` [`44`t, `168K` ctx] + `stc-001` [`Opus`, `36`t, `126K` ctx] over `19.7h`) |
| **Intent and authority** | Scaffold the `Stream-C` (`P2P`) project tracker (`PROJ-133..136`, `PROJ-140`), build the `roster-spinup` (`pi-spin`) skill, review 4 teammate MRs (`!183..186`), and draft/publish both the `CodingWorkflow` HLD and the `Pi Multi-Agent Workflow Guide` on Confluence. |
| **Observable sequence** | 1. `stc-001-argon` dispatched `roster-spinup` implementation to `stc-001` (`Opus`), HLD + workflow guide drafting to `stc-001-kimi`, and adversarial review of HLD + teammate MRs `!183..186` to `stc-001-fable`.<br>2. When `stc-001-fable` finished its HLD review (`03:49`), its `intercom` `reply` and `send` calls to `stc-001-argon` **failed `4` times in a row** with `"Message to stc-001-argon was not delivered: Reply target does not match a pending ask"`. Fable had to write findings to `~/.claude/context/p2p/state-machine/hld-review-fable.md`, message `stc-001` (`Opus`) to relay (`03:49:48`), and send an `ask` (`03:49:58`) to break through.<br>3. Meanwhile, Argon ran a Python script to extract Fable's review from Fable's session file into `findings/2026-10-15-fable-hld-and-mr-review.md` and told Kimi to fold it into the HLD. At `03:48:30`, **Kimi halted publication**: *"stc-001-argon — STOP before publishing. `2026-10-15-fable-hld-and-mr-review.md` is corrupted and incomplete. The file is only 14 lines"*. Argon re-extracted all `12,188` chars cleanly (`03:49:03`).<br>4. At `04:34:10`, the operator approved Fable posting inline review comments on teammate MRs `!184` and `!186`. Before posting (`04:35:37`), **Fable re-checked the git heads and caught that `Engineer B` had force-pushed `!186` (`0c57cb5`)**, removing the broken WIP commit `9d4d6ef`! This prevented posting obsolete comments on a teammate's MR.<br>5. At `10:25:58`, Kimi's Atlassian MCP began returning `403` mid-publish; Kimi handed the verified local markdown file to Argon, which published `v10` to Confluence (`10:26:51`). |
| **Outcome** | **Verified Hard Success:** Full `p2p` + `state-machine` tracker trees scaffolded, `roster-spinup` skill (`spinup.py`) shipped and tested, inline MR reviews posted on `!184`/`!186`, and `HLD v3` + `Workflow Guide v14` published to Confluence. |
| **Intervention** | `28` human prompts (`17` to Argon, `8` to Kimi, `2` to Fable, `1` to Opus), including steering Kimi to move drafts from `/tmp` into the tracker (`03:39:44`) and correcting Fable's cost/context caveat (`10:08:46`, `10:22:12`). |
| **Resource evidence** | `369` total turns (`90` `codemode` calls); `53` intercom messages; peak ctx `432K` (Argon), `281K` (Kimi), `168K` (Fable), `126K` (Opus); metered cost `$22.30`. |
| **Failure/success mechanism** | *Observation:* `pi-intercom` rejecting `send` when `replyTo` references an expired `ask` caused a 4-attempt delivery block (`F-05`), which in turn caused Argon's botched transcript scrape; Kimi's independent verification of the findings file (`F-08`) and shared local Markdown source files allowed seamless failover when Kimi's MCP auth expired (`10:25:58`). |
| **Candidate requirement** | **`R-02` (Decouple `replyTo` Thread Metadata from `pending-asks` Validation)** and **`R-09` (Artifact-First Handoff Over Transcript Scraping)**. |
| **Evidence locator** | `stc-001-argon` (`01a13d85`), `stc-001-kimi` (`01a13d8c`), `stc-001-fable` (`01a13d8e`), `stc-001` (`01a13d8f`). |
| **Confidence and limits** | **High confidence.** Demonstrates both the power of the 4-agent roster on a multi-epic kickoff and the exact `pi-intercom` `replyTo` friction point. |

---

### `CS-08`: Silent In-Pane Reply Drop & Manager Underutilization (`stb-011`)

| Field | Content |
| --- | --- |
| **Episode ID and cohort** | `stb-011` (Cohort 3B: 3-agent Triad `stb-011` [`Opus 5.5`, `195`t, `391K` ctx] + `stb-011-fable` [`Fable 5.1`, `37`t, `139K` ctx] + `stb-011-kimi` [`Kimi K3`, `5`t, `19K` ctx]) |
| **Intent and authority** | Fix `org/agent-monorepo !179` Trivy CVE pipeline blocker, review `Engineer E`'s `MR !175` (`SEC-1148` SITS SIEM flows), and onboard `secops` namespace (`PROJ-187`) across `IL`, `TK`, and `PK`. |
| **Observable sequence** | 1. At `03:11:23`, Worker (`stb-011`) sent a kickoff brief via `intercom({ action: "send" })` to both `stb-011-kimi` and `stb-011-fable` asking Kimi to verify `MR !175` ownership/sequencing and Fable to review `!175` code.<br>2. `stb-011-fable` reviewed `!175` and sent its verdict back over intercom (`03:22:22`: *"no merge blockers in the flow code"*).<br>3. `stb-011-kimi` woke on `"New intercom message above."`, ran `2` `bash` commands (`glab mr view 175`, `git log` for `Engineer E`'s email and `94b7fe8` master status), and then **printed its entire reply (`"Replying to 011 (verified: !175 authored by Engineer E...): A. GO, with this correction..."`) as plain assistant text in its own terminal pane without calling `intercom`!**<br>4. Because `stb-011` had used `action: "send"` (which does not inject a reply-tool hint), `stb-011-kimi` never called `intercom`, and `stb-011` never received Kimi's response. `stb-011-kimi` sat idle for the remaining `20.5` hours until receiving the wrap-up FYI at `23:58:04`. |
| **Outcome** | **Episode Succeeded (`Opus` + `Fable` shipped `PROJ-187`), but Manager's verification was silently lost (`0` intercom sends).** |
| **Intervention** | `15` human prompts, all to `stb-011` (`Opus`); neither the operator nor `stb-011` noticed that `stb-011-kimi` had printed its reply to its local pane instead of sending it over intercom. |
| **Resource evidence** | `237` total turns (`195` Opus, `37` Fable, `5` Kimi); `78` `codemode` calls (`45` Opus, `33` Fable, `0` Kimi); `$31.46`. |
| **Failure/success mechanism** | *Observation:* When an agent is woken by `customType: "intercom_message"` from `action: "send"`, there is no system prompt reminder or post-turn check verifying that a reply was actually transmitted via `intercom` when the assistant prose starts with `"Replying to <peer>..."`. |
| **Candidate requirement** | **`R-02` (Include Reply Hint on All Inbound Intercom Messages + Post-Turn Unsent-Reply Detector)**. |
| **Evidence locator** | `stb-011` (`01a1196f`, main), `stb-011-kimi` (`01a1196f`, kimi, turn 3 at `03:12 UTC`). |
| **Confidence and limits** | **High confidence.** Direct proof of silent message loss caused by the `send` vs. `ask` prompt injection asymmetry. |

---

### `CS-09`: Bounded Subagent Era — Forked Advisor Bloat, `workflowScript` JS Traps, and 30-Min Timeouts (`sta-001..009`)

| Field | Content |
| --- | --- |
| **Episode ID and cohort** | `sta-001` (`468`t), `sta-003` (`354`t), `sta-007` (`564`t), `sta-008` (`519`t), `sta-009` (`210`t) (Cohort 1: Single driver + `pi-subagents` prior to `pi-intercom` adoption) |
| **Intent and authority** | Design and implement payload codec library (`PROJ-31`), codec server (`PROJ-61`), and custom Temporal claim-mapper (`PROJ-80` / `PROJ-85`). |
| **Observable sequence** | 1. **Forked-context duplication (`sta-001..003`):** Driver spawned `advisor` subagents with `context: "fork"`, creating `6` top-level `subagent-advisor-*` sessions (`101`t/`234K`, `98`t/`234K`, `153`t/`259K`, `40`t/`142K`, `80`t/`180K`, `106`t/`209K`) that each inherited and re-billed the parent's `140K–259K` transcript.<br>2. **JS DSL failures in `workflowScript`:**<br>   - `sta-003` (and two forked advisors): `workflowScript` threw `TypeError: Cannot read properties of undefined (reading 'error') at workflow-script.js:26:9`.<br>   - `sta-008`: `workflowScript completed with unawaited runs.run launch(es): 'fable-1183', 'fable-1176', 'fable-drift-options'`.<br>   - `sta-009`: `SyntaxError: Unexpected token (38:121)` because the task prompt inside the JS template literal contained unescaped Markdown backticks.<br>3. **30-minute worker timeout (`sta-007`):** A delegated `worker` subagent (`dafa3a7f`) ran for `1,800,000ms` (`30` minutes) and was killed by timeout without returning incremental progress to the parent.<br>4. **Steering dead async runs (`sta-010`, `sta-011`):** Parent tried `subagent({ action: "steer" })` on runs `95a02b91`, `fc1b49fb`, and `362147b4` that had already exited (`"is not running or queued and cannot be steered"`). |
| **Outcome** | **Work eventually succeeded via driver fallback, but directly drove the migration to `pi-intercom` + `codemode` in Cohorts 2–4.** |
| **Intervention** | High human turn counts (`48` in `sta-001`, `37` in `sta-003`, `46` in `sta-007`, `30` in `sta-008`) steering the single driver through subagent failures. |
| **Resource evidence** | `sta-001`: `468`t, `51` subagent calls, `484K` peak ctx; `sta-003`: `354`t, `38` subagent calls; `sta-007`: `564`t, `18` subagent calls. |
| **Failure/success mechanism** | *Observation:* Asking an LLM to author raw async JavaScript orchestration scripts (`workflowScript`) with embedded Markdown prompts is error-prone; `codemode` succeeded later (`685` calls) because it was used for short, synchronous `Promise.allSettled` tool batches (`bash`/`read`/API) while long-lived reasoning moved to persistent `pi-intercom` peers. |
| **Candidate requirement** | **Avoid raw-JS multi-agent workflow DSLs for routine parallel fan-out; use declarative fan-out or persistent attachable peers (`R-05`).** |
| **Evidence locator** | `sta-001` (`01a00656`), `sta-003` (`01a0200b`), `sta-007` (`01a04150`), `sta-008` (`01a045ca`), `sta-009` (`01a04726`). |
| **Confidence and limits** | **High confidence.** Historical note: newer `pi-subagents v0.79.1` added `{workflow: "parallel", args: {tasks: [...]}}` to avoid raw JS for simple parallel fan-out. |

---

### `CS-10`: Classifier (`pi-local-laya`) Evaluation — Framing Bias, Compressed Scores, and Silent Outage (`stb-005..014`, `meta-006`)

| Field | Content |
| --- | --- |
| **Episode ID and cohort** | `stb-005` (`14` nudged calls), `stb-006..012` (`96` `ok=true` gate calls), `stb-012..014` & `stc-001` (`49` `server_down` gate calls), `meta-006` (`2` evaluation sessions `01a0fa15` & `01a0fb1f`) |
| **Intent and authority** | Evaluate whether a local `421M` ModernBERT typed-decision classifier (`laya/typed-decisions`) improves safety gating and batch triage inside Pi sessions. |
| **Observable sequence** | 1. **Zero un-nudged calls (`stb-004`, `stb-005`):** With `pi-local-laya` + `laya-classify` skill loaded, agents made `0` classifier calls until the operator nudged `stb-005`: *"btw, why don't you use laya classifier?"*.<br>2. **Loaded state framing (`stb-005`):** When `stb-005` called `tools.classify` before `git push` (`5` times after operator approval), it stuffed `"the Operator approved... deploys only on merge"` into `state.reason`. Despite that self-justifying state, `laya/typed-decisions` returned `needs_review.p = 0.51–0.55` (`confidence = 0.10–0.15`).<br>3. **Automatic bash hook (`stb-006..012`, `109` `ok=true` firings):** Switching to an automatic bash hook (`laya.gate: "annotate"`) achieved `100%` trigger coverage on `git push` / `glab write` / `rm -rf` / `terraform apply` at `35–566ms` latency, but outputs were compressed into `p = 0.459–0.685` (median `0.598`), and **`0` assistant turns ever paused or altered course**.<br>4. **Live rubric-bias proof (`meta-006`):** When `01a0fa15` used `tools.classify` to rank 8 improvements (`E1..E8`) to Laya, including `"or depends on something unproven"` in the `false` criterion caused Laya to rank the highest-leverage fix (`E1: richer gate state`) **dead last (`p = 0.489`)**.<br>5. **7-day silent outage (`stb-012..014`, `stc-001`):** From `2026-10-09` to `2026-10-16`, `laya-serve` was down (`connection refused`), logging `49` consecutive `errorKind: "server_down"` gate failures that were silently ignored. |
| **Outcome** | **Verified Negative / Diagnostic Finding:** Local `421M` classifier on bare one-line commands provides weak discrimination (`p ~ 0.60`), is easily biased by LLM-authored state/criteria wording, and missed MCP writes (`Jira`/`Slack`) until `MR !1` (`gate-mcp-rules`, commit `eaff81d`) added deterministic rule checks + MCP write gating. |
| **Intervention** | Operator initiated two dedicated meta-analysis sessions (`01a0fa15`, `01a0fb1f`) to audit `laya/usage.jsonl` against transcripts and commissioned `MR !1` (`confirm` mode + MCP write rules). |
| **Resource evidence** | `191` calls in `~/.pi/agent/laya/usage.jsonl`; `$0` API cost; `2.7 GB` local unified memory (MPS) when `laya-serve` is resident; `31–566ms` latency. |
| **Failure/success mechanism** | *Observation:* (a) Generative frontier models (`Opus 5.5`, `Fable 5.1`, `Kimi K3`) write deterministic JS filters in `codemode` better than a `421M` classifier triages short text; (b) safety gates on destructive/people-facing actions require **deterministic rule triggers + pre-execution `tool_call` confirmation hooks** (covering MCP write tools, not just `bash`), with classifiers used at most for relative ranking on rich, code-extracted state. |
| **Candidate requirement** | **`R-10` (Deterministic Pre-Execution Gate on People-Facing MCP & Destructive Git/Infra Tools; Code-Extracted State Only for Classifiers)**. |
| **Evidence locator** | `~/.pi/agent/laya/usage.jsonl`; `stb-005` (`01a0f08a`); `meta-006` (`01a0fa15` & `01a0fb1f`). |
| **Confidence and limits** | **High confidence** (`100%` of classifier calls in the corpus were audited). Hosted `Jev` (`64K` context, lower calibration error `ECE 0.144` vs. `0.213`) was not stress-tested on these `191` calls because `laya.mode` was set to `native`. |

---

## 6. Prioritized Candidate Requirements for Pi Ensemble

Each candidate requirement below separates **hard runtime constraints** from **operator workflow preferences**, ties directly to observed findings (`F-01`..`F-16`), defines a concrete **acceptance scenario**, and evaluates **what simpler non-runtime alternative could suffice**.

### Priority Tier 1: `MUST` Have (High-Frequency / High-Severity Friction)

#### `R-01`: Automatic Direct-Steer Broadcast & Peer Roster Discovery (`F-03`, `F-04`)
- **Problem:** In `23.2%` of all prompts (`44.3%` in 4-agent rosters), the operator types directly into a specialist pane (`-fable`, `-kimi`, or `worker`). Currently, the coordinator only learns about the changed intent if the specialist remembers (or is nudged by the human) to send an `intercom` message (`11` missed relays; `33` delayed $>10$m; explicit human nudges in `stb-001`, `stb-003`, `stb-014`).
- **Requirement (Constraint, not preference):**
  1. Peers launched in the same ensemble/roster group (`<prefix>-<NNN>`) must receive the live roster directory (`role -> session_name, model, cwd, owned_worktrees`) in their session context automatically, without needing to call `intercom({ action: "list" })` or be told `"you send intercom directly to <prefix>-<NNN>"`.
  2. When a human prompt is submitted directly to a non-coordinator peer in an active ensemble, the runtime (or intercom extension) must automatically emit a compact **`direct_steer_notice` event** (or require/auto-append a structured steering summary) to the ensemble's coordinator/driver upon turn completion: `{ from_role, human_prompt_summary, decision_or_scope_delta, affected_artifacts, action_owner }`.
- **Acceptance Scenario:** Operator types *"do NOT fold (b) into !138, open a separate MR"* directly into `<prefix>-<NNN>-fable`. When Fable finishes its turn, `<prefix>-<NNN>-argon` (and/or `<prefix>-<NNN>`) automatically receives a structured steering delta notice without the operator typing *"now tell the worker"*.
- **Simpler Alternative:** A lightweight `pi-intercom` hook on `agent_end` (when the turn was triggered by a human `user` message in a rostered `<prefix>-<NNN>-<role>` session) that checks whether `intercom` was called during the turn; if not, it either prompts/auto-injects a 1-line summary to the base/orchestrator session or shows a status badge.

#### `R-02`: Unified, Idempotent Peer Messaging (`reply` on `send` + Post-Turn Unsent-Reply Guard) (`F-05`, `F-06`)
- **Problem:** `31/35` (`88.6%`) failed `intercom` calls occurred because Peer A used `action: "send"` (or an `ask` timed out) and Peer B responded with `action: "reply"` or `action: "send", replyTo: "<id>"`, which `pi-intercom` rejects (`"Reply target does not match a pending ask"` / `"No active intercom context to reply to"`). Conversely, because `action: "send"` omits the reply-tool instruction that `action: "ask"` injects, receiving agents occasionally wrote their reply as terminal prose in their own pane without calling `intercom` (`stb-011-kimi`, `stb-014-fable`).
- **Requirement (Constraint):**
  1. **Make `reply` and `replyTo` idempotent across both `send` and `ask`:** If a pending `ask` exists for `replyTo` (or for the most recent inbound sender), resolve the `ask` waiter AND deliver the message. If no pending `ask` exists (because the sender used `send`, or the `ask` timed out), **deliver the message as a normal `send` with thread metadata `inReplyTo`** instead of throwing an error!
  2. **Inject reply guidance on all inbound messages:** Every injected `intercom_message` (`send` or `ask`) should include a 1-line footer: `To respond to <sender>, call intercom({ action: "send", to: "<sender>", message: "..." }) (or no call if this is a terminal FYI).`
  3. **Detect accidental in-pane replies:** If a turn was triggered by an inbound `intercom_message`, made `0` outbound `intercom` calls, and produced assistant text matching `/^(Replying to|Hi <peer>|To <peer>|<peer> —)/i`, warn or auto-prompt the agent that its message was only printed locally.
  4. **Liveness heartbeat on `ask` (`CS-03`):** While an `ask` waiter is blocked, stream or expose the target session's liveness state (`working: running tool X`, `idle`, `blocked`) and fail fast if the target session returns to `idle` without replying.
- **Acceptance Scenario:** Peer A sends a task via `intercom({ action: "send" })`. Peer B calls `intercom({ action: "reply", message: "Done" })` (or `action: "send", replyTo: id`). The message is delivered on the first attempt with `0` errors. If Peer B crashes or finishes its turn to `idle` without replying to an `ask`, Peer A's `ask` unblocks immediately instead of hanging for `600` seconds.
- **Simpler Alternative:** A ~`30`-line patch in `pi-intercom`'s `reply` / `send` handler to fall back to ordinary delivery when `replyTo` is not in `pending-asks`.

#### `R-03`: Atomic Agent Slot Replacement & Stale-Handle Retirement (`F-01`, `F-09`, `CS-01`, `CS-03`)
- **Problem:** When `sta-026-gemini` (`543K` ctx) was replaced by `sta-026-gemini-2`, the old pane stayed live and both edited the same branch. When `sta-029` started, `sta-028-gemini` (from 5 days earlier) was still live in `intercom list`, and `sta-029` accidentally dispatched new work to `sta-028-gemini`.
- **Requirement (Constraint):**
  1. Provide a first-class **slot replacement / rotation primitive** (`replace(old_session, new_session)` or automatic retirement in `spinup.py`): marks the old session as **`retired / read-only context oracle`**, revokes its write lease on worktrees, and aliases/redirects active role routing (`worker`) to the new session.
  2. Retired sessions (`<prefix>-<prev>`) may still be queried explicitly for historical context (`mode: "context-only"`, as worked so well with `sta-028-gemini` in `CS-03`!), but `intercom` must reject or warn on non-context task dispatches from `<prefix>-<curr>` to `<prefix>-<prev>`, and block retired sessions from running mutating git/file tools.
- **Acceptance Scenario:** Operator replaces `sta-026-gemini` with `sta-026-gemini-2` (or spins up `sta-029` while `sta-028-gemini` is still open). `sta-026-gemini` is automatically switched to read-only oracle mode; any message sent to the `worker` role routes to `sta-026-gemini-2`; `sta-028-gemini` can answer historical questions from `sta-029` but cannot mutate `TK`.
- **Simpler Alternative:** Have `spinup.py` send an intercom/Herdr command to mark previous `<prefix>-<prev>*` panes as `[RETIRED - READ ONLY]` (or close them on confirmation), and filter `intercom({ action: "list" })` to group sessions by `<prefix>-<NNN>` cohort.

---

### Priority Tier 2: `SHOULD` Have (Structural Guardrails & Context Hygiene)

#### `R-04`: Worktree & Branch Lease Registry (`One Writer Per Branch`) (`F-09`, `CS-01`, `CS-06`)
- **Problem:** Duplicate MRs (`!139` vs. `!140` in `sta-026`), dual edits on `proj79-codec-istio-and-envs` (`sta-026`), and Orchestrator-vs-Worker MR collision (`PK !6786` in `stb-014`).
- **Requirement:** Track active `(repo, branch, worktree_path) -> owner_session` leases across the ensemble. Warn or block when a second session attempts `git commit`, `git push`, `edit`, or `glab mr create` on a branch/worktree leased by another live peer unless the lease is transferred.
- **Acceptance Scenario:** `sta-026-gemini-2` holds the lease on branch `proj79-codec-cors-and-token-aud`. If `sta-026` (Driver) or `sta-026-gemini` attempts to commit/push to that branch or open an MR for the same ticket without releasing/transferring the lease, the tool hook warns and names the current lease holder.
- **Simpler Alternative:** A lightweight file lock (`.pi-lease.json` in the git worktree dir or `~/.pi/agent/intercom/leases.json`) checked by a `tool_call` hook before `edit`/`write`/`git commit`/`git push`.

#### `R-05`: Durable Attachable Specialist Peers Instead of Forked Subagents (`F-01`, `F-02`, `CS-02`, `CS-09`)
- **Problem:** `context: "fork"` subagents duplicated `142K–259K` tokens per spawn (`sta-001..003`), while `context: "fresh"` subagents lost project context after a single run and could not be queried across worker rotations.
- **Requirement (Architecture Validation):** A Pi Ensemble runtime **should** model specialists (`Adviser`, `Manager`, `Worker`, `Orchestrator`) as **durable, attachable, role-addressed sessions** that persist across multiple tasks and can survive the rotation of a sibling session (`CS-02`), while reserving ephemeral subagents (`pi-subagents` with `context: "fresh"`) strictly for bounded, stateless leaf tasks (e.g., isolated test runs or parallel file scouts).
- **Simpler Alternative:** The current `roster-spinup` (`spinup.py`) + Herdr 2x2 tab + `pi-intercom` already achieves `85%` of this with zero new runtime code; the missing `15%` is shared roster metadata (`R-01`), reliable `reply` semantics (`R-02`), and clean slot replacement (`R-03`).

#### `R-06`: Role-Scoped Tool & Context Guardrails (`F-07`, `F-09`, `F-12`, `CS-04`, `CS-06`)
- **Problem:**
  1. Legacy `orchestrator-routing/index.ts` injects `"You are the parent orchestrator"` into `-kimi` Manager sessions, and `model-rules/index.ts` injects obsolete `Opus 4.6/4.7` rules into every session (`F-12`).
  2. Orchestrator (`stb-014-argon`) and Manager (`stb-003-kimi`) occasionally drifted into editing code or opening MRs directly (`F-09`), or pulling `432K` of raw docs/diffs into the coordinating window (`stc-001-argon`, `F-07`).
- **Requirement:**
  1. **Immediate local fix:** Gate `orchestrator-routing` so `ORCHESTRATOR_BLOCK` is disabled when a session is a member of an Intercom roster (`<prefix>-<NNN>*`), and retire/update `model-rules/_common.md`.
  2. **Role profile enforcement:** Bind each `<prefix>-<NNN>[-<role>]` session to an explicit role profile (`worker`, `orchestrator`, `manager`, `adviser`):
     - `orchestrator` and `manager`: allow read/tracker/MCP/intercom tools; require explicit override before editing source repos outside `~/.claude/context/` or running `glab mr create`.
     - `adviser`: read-only by default on source repos (except `findings/` and test/render commands), with explicit human override (`CS-01` `!140` emergency fix).
- **Simpler Alternative:** Role-name detection (`ctx.sessionManager` name suffix `-argon`, `-kimi`, `-fable`) in a `before_agent_start` + `tool_call` extension.

#### `R-07`: Single-Writer Tracker Ownership or Atomic ID Allocation (`F-10`, `CS-05`)
- **Problem:** When both Worker (`stb-009`) and Manager (`stb-009-kimi`) edited `variables.md`, both allocated `X-63` to different items.
- **Requirement:** Designate a single tracker writer per ensemble (Orchestrator in 4-agent rosters; Manager or Worker—never both—in 2/3-agent rosters), or provide a tiny helper tool/script (`tracker_allocate_id(tracker, prefix)`) that atomically increments `D-n` / `X-n` / `T-n` in the top-of-file index.
- **Simpler Alternative:** Enforce in `AGENTS.md` that only the Orchestrator (or Manager when no Orchestrator is present) writes to `decisions.md` and `variables.md`.

---

### Priority Tier 3: `COULD` Have / `DO NOT BUILD`

#### `R-08` (`COULD`): Pre-Execution Confirmation Gate on People-Facing MCP Writes (`F-14`, `F-15`, `CS-10`)
- **Evidence:** The operator's most sensitive safety boundary is **never posting/replying to anything a human will read (Slack, Jira comments to others, human MR threads, MR merges) without explicit operator GO**. The bash-only `laya` annotate gate missed `100%` of MCP writes (`mcp__atlassian`, `mcp__slack`) and had `0%` behavioral influence.
- **Recommendation:** Merge `pi-local-laya MR !1` (`gate-mcp-rules`, commit `eaff81d`) in **deterministic rule mode (`confirm`)** for people-facing MCP write tools and destructive git commands (`git push --force`, `glab mr merge`), without relying on the `421M` local classifier's `0.56–0.66` probability scores to make the block/allow decision.

#### `R-09` (`DO NOT BUILD`): Raw-JS Multi-Agent Workflow DSLs or Classifier-Driven Orchestration (`F-02`, `F-14`, `F-15`)
- **Evidence:** Raw JS `workflowScript` subagent orchestration caused `6` syntax/schema/unawaited-promise failures (`CS-09`), and the local `421M` classifier (`CS-10`) compressed probabilities to `0.46–0.69` and ranked the highest-leverage engineering task dead last (`p=0.489`) due to uncertainty keywords.
- **Recommendation:** Do **not** build an ensemble runtime around LLM-authored async workflow scripts or small-classifier routing gates. Keep `codemode` for synchronous tool/API batching (`Promise.allSettled`), keep deterministic rules for safety gates, and use persistent role-addressed peers + Markdown trackers for multi-step project orchestration.

---

## 7. Recommended Events, Attribution IDs, and Metrics Schema

### 7.1 Attribution ID Model
Retrospective analysis across the `378` session files required joining separate JSONL files by timestamp, regex-matching `<prefix>-<NNN>` strings inside `intercom_message` headers, and correlating tracker edits (`D-n`, `X-n`, `!IID`) by hand. A Pi Ensemble runtime should attach five lightweight IDs to every session event:

| Identifier | Scope & Generation | What It Links |
| --- | --- | --- |
| `ensemble_id` (`<prefix>-<NNN>`) | Set at roster spin-up (`spinup.py` or ensemble launcher) and inherited by all peer panes | Groups Orchestrator, Worker, Manager, Adviser, and any child subagents into one work episode without relying on naming conventions |
| `role_slot_id` (`orchestrator`, `worker`, `manager`, `adviser`) | Bound to the pane/slot; moves from `old_session_id` to `new_session_id` on `replace()` | Distinguishes the **role slot** from the ephemeral **session UUID**, preventing split-brain after mid-episode replacement (`CS-01`, `CS-03`) |
| `work_item_id` (`PROJ-201`, `X-65`, `MR:org/repo!189`) | Optional tag on `intercom` messages, worktree leases, and tool calls | Attributes token spend, turns, reviews, and rework across multiple agents to a specific ticket or MR |
| `steering_event_id` (`steer_<uuid>`) | Generated whenever a human types directly into any pane (`role != orchestrator` or `role == orchestrator`) | Traces how a human correction in `-fable` or `-kimi` propagates via `intercom` to `worker` and `decisions.md` (`F-03`, `F-04`) |
| `verification_id` (`ver_<uuid>`) | Attached to Adviser/Manager review verdicts (`GO` / `HOLD` / `BLOCK` + commit SHA) | Links a review pass to the exact git commit SHA reviewed (`ae79ec9` $\rightarrow$ `b1b78ea`), proving whether a commit was verified before push |

### 7.2 Recommended Append-Only Event Stream (`ensemble-events.jsonl`)
Instead of `logs/orchestrator-usage/unknown.jsonl` (which logged `sessionId: "unknown"` and missed `pi-intercom` entirely), emit one compact JSONL line per coordination event with `ctx.sessionManager.getSessionId()` resolved at event time:
1. `ensemble.member_started` / `ensemble.member_retired`: `{ ts, ensemble_id, role_slot_id, session_id, replaced_session_id?, model, cwd }`
2. `intercom.delivered` / `intercom.delivery_failed`: `{ ts, ensemble_id, msg_id, in_reply_to?, from_session, from_role, to_session, to_role, mode: "send"|"ask"|"reply", latency_ms, error_kind? }`
3. `human.direct_steer`: `{ ts, ensemble_id, steering_event_id, target_session, target_role, relayed_to_coordinator: bool, relay_latency_ms? }`
4. `lease.acquired` / `lease.conflict`: `{ ts, ensemble_id, session_id, role_slot_id, resource_type: "worktree"|"branch"|"tracker", resource_path, holder_session_id? }`
5. `turn.usage_digest`: `{ ts, ensemble_id, session_id, role_slot_id, model, prompt_context_tokens, output_tokens, cache_read_tokens, cache_write_tokens, cache_expired_spike: bool, metered_cost_usd, is_flat_rate: bool }`

### 7.3 What Retrospective Data Cannot Establish
1. **True counterfactual cost of flat-rate models (`devin/kimi-k3-high`, `glm-5-2`):** Because `cost.total` is `0` on flat-rate providers, retrospective logs cannot compute exact API-equivalent dollar spend for Kimi/GLM turns without applying synthetic list-price tables.
2. **Cognitive load & visual scanning time in Herdr:** The logs record when the operator typed into a pane, but cannot measure how often the operator glanced at a passive Herdr pane (`-fable` or `-kimi`) to gain confidence without typing.
3. **Causal attribution of model vs. role:** Because `Opus 5.5` was almost always pinned to `Worker`, `Kimi K3` to `Manager` (or early `Driver`), `Argon 4` to `Orchestrator`, and `Fable 5.1` to `Adviser`, role effects and model-family effects are confounded in observational data.

---

## 8. Setup, Package, and Skill Inventory + Sanitized Skill Bundle (Deliverable 3)

### 8.1 Observed Harness, Package, and Extension Versions
Historical behaviors in the corpus must be interpreted against the active package/extension version at the time of each episode:

| Component | Observed Version / Commit (`2026-10-19`) | Role in Setup & Historical Version Caveats |
| --- | --- | --- |
| `@earendil-works/pi-coding-agent` | `v1.3.0` | Core agent harness, native `codemode` & `tool_search` (adopted `2026-09-29` on `v0.99.1`), native `typesafe-system-one` classifier API. |
| `nicobailon/pi-intercom` | `v0.17.1` (`ddf2881`, `2026-10-14`) | Local Unix-socket broker (`~/.pi/agent/intercom/broker.sock`) for peer-to-peer messaging (`list`, `send`, `ask`, `reply`, `handover`). Adopted `2026-09-03`. *Note:* Rejecting `replyTo` on non-pending asks (`F-05`) was observed through `2026-10-15`. |
| `nicobailon/pi-subagents` | `v0.79.1` (`95683a44`, `2026-10-17`) | Bounded child agent delegation (`worker`, `advisor`, `scout`, `reviewer`). *Historical caveat:* The `workflowScript` JS syntax errors (`F-02`) occurred in Aug–Sept 2026 on pre-`0.79` versions; current `v0.79.1` provides `{workflow: "parallel", args: {tasks: [...]}}`. |
| `jayzeng/pi-memory` | `v0.4.2` (`1e276a7`, `2026-09-20`) | Manages `MEMORY.md`, `SCRATCHPAD.md`, and `daily/<YYYY-MM-DD>.md`. Keyword BM25 search works; semantic search (`qmd`) is not installed (`F-11` minor note). |
| `pi-local-laya` (internal package) | `v0.1.0` (`fc40604`, `2026-10-01`; unmerged `MR !1` `eaff81d` on branch `gate-mcp-rules`) | Manages local `laya-serve` (`v0.3.22`, `convaiinnovations/laya-typed-decisions` `421M` ModernBERT) on `127.0.0.1:8000` and hooks bash commands in `annotate` mode. *Historical caveat:* Installed `master` (`fc40604`) gates bash-only in `annotate` mode; local branch `gate-mcp-rules` (`eaff81d`) fixes heredoc/`mktemp` regexes and adds MCP write gating + `confirm` mode. |
| `extensions/orchestrator-routing` | Custom local (`2026-08-06`) | Auto-opens mirroring Herdr panes on `subagent:async-started`, logs to `logs/orchestrator-usage/unknown.jsonl` (`sessionId` bug at line `516`), and injects `ORCHESTRATOR_BLOCK` whenever `modelId.includes("kimi-k3")` (`F-12`). |
| `extensions/herdr-agent-state.ts` | Herdr integration `v9` (`2026-09-22`) | Reports live agent state (`working`, `blocked`, `idle`) and `agent_session_path` over `HERDR_SOCKET_PATH`. |
| `extensions/model-rules` | Custom local (`2026-04-28`) | Injects `~/.claude/model-rules/_common.md` (`Opus 4.6/4.7` routing table) into every turn (`F-12`). |
| `extensions/gemini-4-thinking.ts` | Custom local (`2026-10-15`) | Normalizes `thinkingConfig` (`thinkingBudget` $\rightarrow$ `thinkingLevel: "HIGH"`) on Vertex AI requests for `gemini-4-*` models (`Argon`). |

---

### 8.2 Sanitized Workflow Skill Bundle & Behavior Analysis

Below is the complete architectural and behavioral specification of the three core workflow skills (`tracked-project-status` [covering both Project Creation and Project Status], `roster-spinup`, and the Role/Intercom Conventions), sanitized of all workplace-specific endpoints, names, and repository paths.

#### Skill 1 & 2: `tracked-project-status` (Project Creation & Standing Status Reports)
- **Activation Triggers:**
  - *Status mode:* `"status report"`, `"where are we"`, `"project status"`, `"current state of <project>"`, `"<project> report"`.
  - *Scaffolding mode:* `"new project"`, `"new stream"`, `"scaffold tracker"`, `"new leaf under <project>"`.
- **Read/Write Scope:**
  - *Reads:* `~/.pi/agent/skills/tracked-project-status/projects/<slug>.md` (per-project status config), `~/.pi/agent/skills/tracked-project-status/templates/*.md` (9 Markdown templates), and the target tracker folder `<tracker-root>/<project>/`.
  - *Writes:* Creates/updates Markdown files inside `<tracker-root>/<project>/`, registers `projects/<slug>.md`, and appends a pointer entry (`#project-<slug>`, path, governing epic) to `MEMORY.md` via `memory_write`.
- **Preconditions & External Effects:**
  - Requires `glab` (or `gh`) CLI authenticated for live MR state checks, and optional issue-tracker MCP (`atlassian`) or deployment CLI (`argocd`/`kubectl`) for live drift verification. Read-only against external services.
- **Human Gates & Failure Handling:**
  - *Status mode:* If live state contradicts the Markdown tracker, the agent **must not** silently overwrite decisions; it prints a one-line delta banner at the top of the report, renders the live state, and offers to update the tracker artifacts.
  - *Scaffolding mode:* Places new trackers under the nearest existing parent folder; never moves or deletes existing tracker folders; copies binding parent decisions into `decisions.md` as inherited (`I-n`) entries to be re-confirmed at kickoff.
- **Sanitized Templates (`templates/`):**
  - `README.md`: Goal (1 paragraph), Strategy (binding choices), `Related context` table (`Need | Where` linking parent/sibling files without copying), `Artifacts in this folder` table.
  - `index.md`: Required at **every** folder level (`root`, `<sub-project>`, `findings/`, `reports/`), listing every direct child file and subfolder with a 1-line description.
  - `progress.md`: Workstream/task table (`# | Task | Ticket | State`) + append-only `## Session log` (`### <date>: session <n> (<topic>)`).
  - `mrs.md`: Cross-repository MR table (`Repo | MR | Title | Ticket | State | Notes`), including teammates' MRs on shared epics.
  - `decisions.md`: Top-of-file `ID -> short name` index, `## Inherited (I-n)` section, and `## Ratified (D-n)` section (append-only; superseded decisions are marked, never overwritten).
  - `variables.md`: Top-of-file `ID -> short name` index, `## Open (<P>-n)` section, and `## Watch-items (W-n)` section. Updated immediately mid-turn whenever a new question or trade-off surfaces.
  - `continuation-prompt.md`: Compact (`40–80` line) handoff rewritten from scratch at every milestone and session wrap (`State`, `Next (1)..(3)`, `Read first`).
- **Sanitized Sample Status Config (`projects/sample-project.md`):**
  ```markdown
  # Sample Initiative: status config
  - **Trigger words**: "sample status", "PROJ-100 status", "where are we on sample"
  - **Tracker folder**: `~/.pi/agent/context/sample-initiative/`
  - **Parent / related trackers**: `../platform-core/`
  - **Artifacts** (read in order): `index.md`, `README.md`, `tickets.md`, `mrs.md`, `variables.md`, `progress.md`, `continuation-prompt.md`
  ## Live checks
  glab api "projects/org%2Fservice-repo/merge_requests?per_page=15"
  ## Workstreams
  1. **State Machine (PROJ-140 — Operator)**: HLD · per-state LLDs · implementation
  2. **Git Host Loop (PROJ-135 — Engineer B, Engineer C)**: webhook signals (`!183`) · updates (`!184`)
  ```

#### Skill 3: `roster-spinup` (`spinup.py` / `pi-spin`)
- **Activation Triggers:** Shell command `pi-spin <prefix> [--dry-run | --preflight-only | --offline-notes "..."]` or natural language `"spin up the next <prefix> session"`.
- **Read/Write Scope:**
  - *Reads:* `projects.json` (maps `<prefix>` $\rightarrow$ tracker path + launch `cwd`), `~/.pi/agent/sessions/*/*.jsonl` (scans `session_info` names via `rg` to find max `<prefix>-NNN`), live `herdr agent list`, and the tracker's `continuation-prompt.md`, `mrs.md`, `tickets.md`, `progress.md`, `variables.md`.
  - *Writes:* Writes `offline-delta-<prefix>-<NNN>.md` to `<tracker>/reports/` (on real run) or `$TMPDIR/roster-spinup/` (on `--dry-run` / `--preflight-only`).
- **Preconditions & External Effects:**
  1. Queries GitLab REST API (`glab api` with 8-thread `ThreadPoolExecutor`) for every MR referenced in `continuation-prompt.md` + `mrs.md` across watched repos, classifying each into: `Merged while offline`, `Closed without merge`, `Open with activity` (quoting non-system comments from others since previous session's `mtime`), `Untracked MRs updated while offline` (authored by operator or mentioning project ticket keys), and `Open idle`.
  2. Builds a bounded JQL query (`key in (...) AND updated >= "<since>"`) for the Orchestrator to execute via the issue-tracker MCP.
  3. Validates all 4 role models (`worker`, `manager`, `orchestrator`, `adviser`) against `pi --list-models` before touching Herdr (refuses to launch on model mismatch, exit code `2`).
  4. Requires `HERDR_ENV=1` and `HERDR_WORKSPACE_ID` for real runs: creates a new Herdr tab (`herdr tab create`), splits panes into a 2x2 grid (`TL` worker, `TR` orchestrator, `BL` manager, `BR` adviser), verifies pane rectangle coordinates via `herdr pane layout`, starts 4 `pi` instances (`herdr agent start <name> --kind pi --pane <id> -- --name <name> --model <model>` with 3 retries and 90s readiness timeout), and sends the kickoff prompt to `<prefix>-<NNN>-argon`.
- **Human Gates:**
  - When invoked from inside a Pi session, the agent **must** run `--dry-run` first and show the operator the session number and delta counts before running for real.
  - The kickoff prompt injected into `<prefix>-<NNN>-argon` enforces a mandatory **pre-kickoff human gate**: Argon must present the GitLab + Jira offline delta summary and **ask the operator for offline updates (Slack/verbal updates, manual applies, priority shifts) and WAIT** before routing work to the roster (unless `--offline-notes` was explicitly passed on the CLI).

#### Convention vs. Code-Enforced Matrix Across the Setup

| Behavior / Rule | Enforced in Code (Where) | Convention Only (Prompt / `AGENTS.md` / `SKILL.md`) | Observed Compliance / Failure Rate |
| --- | --- | --- | --- |
| **Model validation & 2x2 Herdr geometry at spin-up** | **Code:** `spinup.py` (`check_models()`, `verify_layout()`, live-name collision check) | — | `100%` (`0` broken layouts once `spinup.py` was built) |
| **Offline GitLab MR delta calculation** | **Code:** `spinup.py` (`preflight()` via `glab api`) | **Convention:** Argon running the generated Jira JQL via MCP and waiting for operator confirmation | High (`100%` in Cohort 4) |
| **Async subagent mirroring panes in Herdr** | **Code:** `extensions/orchestrator-routing/index.ts` (`openPaneForRun()`, `markPaneDone()`) | — | High when `pi-subagents` was used, though `sessionId` logged as `"unknown"` |
| **Destructive command & MCP write gating** | **Code (Partial):** `pi-permissions.jsonc` (`rm -rf`, `git reset` deny) + `pi-local-laya` (`annotate` mode on bash; `confirm` + MCP writes in unmerged `MR !1`) | **Convention:** Asking operator before `glab mr merge` or posting human-visible comments (`AGENTS.md`) | Human merge/comment gates: `96.8%` (`60/62` episodes; `2` violations caught by operator/peer). Laya `annotate` gate: `0%` influence, `49` `server_down` fails. |
| **Role boundaries (`Worker` = code/MRs, `Manager` = design/prose, `Orchestrator` = routing, `Adviser` = review)** | **None** (In fact, `orchestrator-routing` *contradicts* this by injecting parent-orchestrator instructions into Kimi!) | **Convention:** `AGENTS.md` + kickoff prompts | Violated in `stb-003` (Kimi editing/wrong repo), `stb-009` (Kimi Confluence + `X-63`), `stb-014` (Argon opening MRs), `sta-026` (duplicate MRs `!139`/`!140`). |
| **Self-organizing `intercom` callbacks & direct-steer relays** | **None** (`pi-intercom` only injects reply hint on `ask`, not `send` or human direct prompts) | **Convention:** `AGENTS.md` & `pi-multi-agent-workflow-guide.md` §2.3 | `119/130` (`91.5%`) human bypasses relayed, but required explicit human nudges in `5` episodes; `31` `replyTo` errors; `3` in-pane reply drops. |
| **Same-turn tracker bookkeeping & short names on IDs (`D-19 (single account)`)** | **None** | **Convention:** `AGENTS.md` | High at session wrap (`100%` wrote `continuation-prompt.md`), occasional mid-session lag or bare IDs (`stb-003-kimi` `"X-20/X-27/D-26"`). |

---

## 9. Structured Appendix (Deliverable 2) & Operator Questions

### 9.1 Structured Episode Summary (Anonymized JSON)

```json
{
  "corpus_summary": {
    "total_jsonl_files": 378,
    "top_level_sessions": 242,
    "substantive_top_level_sessions": 176,
    "trivial_probe_sessions": 66,
    "nested_subagent_sessions": 136,
    "structured_episodes": 62,
    "intercom_coordinated_episodes": 30,
    "intercom_coordinated_sessions": 77,
    "parse_errors": 0,
    "compactions_observed": 0,
    "context_edits_observed": 17,
    "total_intercom_sent": 581,
    "total_subagent_calls": 437,
    "total_codemode_calls": 685,
    "total_laya_logged_calls": 191,
    "metered_cost_usd_structured_episodes": 1064.40,
    "flat_rate_cost_usd": "unknown (provider reports 0.00)"
  },
  "deep_reviewed_episodes": [
    {"id": "meta-001", "cohort": "C0_subagent", "sessions": 1, "turns": 166, "ic_sent": 0, "sub": 34, "cm": 0, "max_ctx_k": 209, "outcome": "success_with_subagent_paused_oracles", "confidence": "high"},
    {"id": "rla-012", "cohort": "C0_subagent", "sessions": 1, "turns": 198, "ic_sent": 0, "sub": 29, "cm": 0, "max_ctx_k": 262, "outcome": "success_with_scheduled_reviewbot_quota_fails", "confidence": "high"},
    {"id": "sta-001", "cohort": "C1_fork_subagent", "sessions": 3, "turns": 468, "ic_sent": 0, "sub": 51, "cm": 0, "max_ctx_k": 484, "outcome": "success_high_fork_duplication", "confidence": "high"},
    {"id": "sta-003", "cohort": "C1_fork_subagent", "sessions": 4, "turns": 354, "ic_sent": 0, "sub": 38, "cm": 0, "max_ctx_k": 209, "outcome": "success_after_workflow_script_typeerrors", "confidence": "high"},
    {"id": "sta-007", "cohort": "C1_subagent", "sessions": 1, "turns": 564, "ic_sent": 0, "sub": 18, "cm": 0, "max_ctx_k": 270, "outcome": "success_after_30m_worker_timeout", "confidence": "high"},
    {"id": "sta-008", "cohort": "C1_subagent", "sessions": 1, "turns": 519, "ic_sent": 0, "sub": 2, "cm": 0, "max_ctx_k": 265, "outcome": "success_after_unawaited_runs_error", "confidence": "high"},
    {"id": "sta-009", "cohort": "C1_subagent", "sessions": 1, "turns": 210, "ic_sent": 0, "sub": 19, "cm": 0, "max_ctx_k": 160, "outcome": "success_after_backtick_syntax_error", "confidence": "high"},
    {"id": "sta-018", "cohort": "C2_intercom_pair", "sessions": 2, "turns": 427, "ic_sent": 14, "sub": 0, "cm": 0, "max_ctx_k": 271, "outcome": "worker_quota_exhausted_fable_handed_to_019", "confidence": "high"},
    {"id": "sta-019", "cohort": "C2_intercom_handoff", "sessions": 1, "turns": 143, "ic_sent": 1, "sub": 0, "cm": 0, "max_ctx_k": 330, "outcome": "success_via_sta_018_fable_handoff", "confidence": "high"},
    {"id": "sta-020", "cohort": "C2_intercom_pair", "sessions": 3, "turns": 265, "ic_sent": 2, "sub": 10, "cm": 0, "max_ctx_k": 243, "outcome": "success_after_020_cancel_abort_and_lane_error", "confidence": "high"},
    {"id": "sta-021", "cohort": "C2_intercom_pair", "sessions": 2, "turns": 280, "ic_sent": 9, "sub": 0, "cm": 0, "max_ctx_k": 438, "outcome": "success_after_80m_direct_fable_reframing", "confidence": "high"},
    {"id": "sta-026", "cohort": "C3A_intercom_triad_replaced", "sessions": 4, "turns": 1149, "ic_sent": 49, "sub": 5, "cm": 0, "max_ctx_k": 543, "outcome": "success_after_worker_replacement_and_dup_mr_139_140", "confidence": "high"},
    {"id": "sta-028", "cohort": "C3A_intercom_pair", "sessions": 2, "turns": 1039, "ic_sent": 35, "sub": 0, "cm": 0, "max_ctx_k": 875, "outcome": "success_extreme_context_bloat_875k", "confidence": "high"},
    {"id": "sta-029", "cohort": "C3A_intercom_triad", "sessions": 3, "turns": 423, "ic_sent": 12, "sub": 0, "cm": 0, "max_ctx_k": 234, "outcome": "worker_stuck_kms_loop_driver_fallback_success", "confidence": "high"},
    {"id": "stb-003", "cohort": "C3B_intercom_pair", "sessions": 2, "turns": 319, "ic_sent": 20, "sub": 0, "cm": 0, "max_ctx_k": 324, "outcome": "success_worker_caught_3_manager_spec_bugs", "confidence": "high"},
    {"id": "stb-004", "cohort": "C3B_intercom_pair", "sessions": 3, "turns": 166, "ic_sent": 18, "sub": 1, "cm": 0, "max_ctx_k": 137, "outcome": "success_after_wrong_prompt_abort", "confidence": "high"},
    {"id": "stb-005", "cohort": "C3B_intercom_triad", "sessions": 3, "turns": 308, "ic_sent": 15, "sub": 0, "cm": 16, "max_ctx_k": 344, "outcome": "success_14_nudged_laya_calls", "confidence": "high"},
    {"id": "stb-009", "cohort": "C3B_intercom_triad", "sessions": 3, "turns": 377, "ic_sent": 42, "sub": 0, "cm": 71, "max_ctx_k": 352, "outcome": "hard_success_caught_x63_collision_and_confluence_gate", "confidence": "high"},
    {"id": "stb-010", "cohort": "C3B_intercom_triad", "sessions": 3, "turns": 378, "ic_sent": 41, "sub": 0, "cm": 92, "max_ctx_k": 437, "outcome": "hard_success_4_repos_in_2_4h_caught_rollout_order", "confidence": "high"},
    {"id": "stb-011", "cohort": "C3B_intercom_triad", "sessions": 3, "turns": 237, "ic_sent": 5, "sub": 0, "cm": 78, "max_ctx_k": 391, "outcome": "success_but_kimi_in_pane_reply_dropped", "confidence": "high"},
    {"id": "stb-014", "cohort": "C4_roster_2x2", "sessions": 4, "turns": 353, "ic_sent": 46, "sub": 0, "cm": 102, "max_ctx_k": 352, "outcome": "success_after_argon_mr_drift_corrected", "confidence": "high"},
    {"id": "stc-001", "cohort": "C4_roster_2x2", "sessions": 4, "turns": 369, "ic_sent": 53, "sub": 0, "cm": 90, "max_ctx_k": 432, "outcome": "hard_success_caught_14line_corrupt_file_and_force_push", "confidence": "high"}
  ]
}
```

### 9.2 Open Questions for the Operator
1. **Cleaning up legacy system-prompt extensions (`F-12`):** Should we immediately disable or update `~/.pi/agent/extensions/orchestrator-routing/index.ts` (so it stops injecting `"You are the parent orchestrator"` into `-kimi` Manager sessions) and `~/.pi/agent/extensions/model-rules/index.ts` (which injects the April 2026 `Opus 4.6/4.7` table)?
2. **Adaptive roster sizing (`2` vs. `3` vs. `4` panes) (`F-07`, `CS-08`):** In implementation-heavy episodes (`stb-011..013`), `-kimi` only ran `3–6` turns, whereas in prose/architecture-heavy episodes (`stc-001`), `-kimi` ran `197` turns and `Opus` ran `36`. Should `pi-spin` support a `--layout 3` (`Argon` + `Opus` + `Fable`, or `Opus` + `Kimi` + `Fable`) flag for tight implementation streams, or is keeping all 4 panes open in a 2x2 grid preferred because idle panes are free?
3. **Retiring previous roster panes on `pi-spin` (`R-03`):** When `pi-spin <prefix>` launches `<prefix>-<NNN>`, should it offer a flag (or default prompt) to close or mark `<prefix>-<NNN-1>*` panes as read-only context oracles so the new roster never accidentally dispatches mutating work to the old roster (`CS-03`)?
4. **Classifier path (`pi-local-laya MR !1` vs. `off`) (`CS-10`):** Given that `laya-serve` has been down since `2026-10-09` (`49` `server_down` events) and its `421M` probabilities clustered at `0.46–0.69`, would you like to (a) switch `laya.mode` to `"off"` and use deterministic `tool_call` confirmation rules on MCP writes/destructive git commands, or (b) merge `MR !1` (`gate-mcp-rules`) and test deterministic rules + `Jev`?
