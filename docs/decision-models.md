# Decision models: Jev integration

## Selected direction

Use **Jev through direct TypeSafe AI access** for suitable typed decisions, including codemode. Adoption is settled; practical task selection, secure provisioning, and instrumentation are implementation work. Laya is a possible alternative, not a local hardware requirement or a blocker.

Direct TypeSafe authentication and three synthetic codemode calls succeeded on 2026-10-19; see [smoke-test evidence](../project/findings/2026-10-19-jev-smoke.md). Credentials remain private to Pi, not configured in this repository. Runtime integration and reusable measurement are not implemented.

## Access and pricing

- Provider route: `typesafe`; documented model alias: `jev-latest`. Resolve actual catalog availability rather than assuming an alias is a fixed model revision.
- Operator-reported rate: **$0.042 per million input tokens; $0 per million output tokens**. Track rate source, effective date, and model/version; this has not been independently checked against an invoice/provider price page.
- At that rate, $5 corresponds to about **119 million input tokens**, before any other charges or price changes. This is cost arithmetic, not permission to consume an account balance.
- Native Pi cost totals may report zero when catalog pricing is absent. Apply the explicit rate as a clearly labeled estimate; reconcile actual billing separately.
- Use Jev actively and economically; do not turn every useful call into another procurement discussion. Bound retries/concurrency and track cumulative use rather than assume cheap means unbounded.

## Native Pi path

Installed Pi 1.3.0 docs describe `models.getAvailableOfType("classifier")` and `models.classify(model, { state, questions })` within codemode. Questions can be boolean, choice, or ordered score; results include probabilities/confidence and reported usage. Check `stopReason` and `errorMessage` as well as thrown failures.

Classifier models do not appear in the ordinary chat-model picker. Codemode must be enabled independently; installing a web-search package does not enable it. Durable also documents classifier access within its codemode integration, but the future host's authentication and version compatibility still need validation.

Start with native provider support. A custom provider plugin is not assumed necessary. **A reusable measurement component may still be necessary**, because native access/usage accounting is not the same as decision-quality observability.

## Secure provisioning

1. Use Pi's supported private credential flow (`/login`, selecting TypeSafe if offered) or a narrowly scoped secret-manager/environment credential source supported by the provider. Confirm the actual flow before changing configuration.
2. Never paste the key into this chat, tracked settings, a project `.env`, command arguments, or a command saved in shell history. Do not inspect/print credential files to diagnose availability.
3. Prefer a secret-manager-backed launch or the harness's private credential store; restrict filesystem permissions. An environment variable is a delivery mechanism, not isolation from same-user processes or child-process inheritance.
4. Keep credentials host-owned. Do not forward them in the attach/client protocol, model prompts, telemetry, or the complete environment of bash tools/child agents. Restrict subprocess environments explicitly in the runtime design.
5. Log only provider/model identity, success/failure category, and safe metadata. Sanitize errors. No authorization headers, key fingerprints, raw prompts, or private input payloads by default.
6. Preserve revocation/rotation and visible failure; do not silently fall back to a different paid provider.

Public/synthetic state is sufficient for initial use. Sending workplace state later requires the appropriate data-handling authority; a classifier endpoint is still an external service.

## Suitable uses

- Batch CI/review triage against an explicit label rubric.
- Detect potential contradictions with a versioned decision set.
- Recommend adviser attention or classify an unresolved question's decision owner.
- Score completion evidence against a bounded rubric, with verification remaining independent.

Use deterministic checks for deterministic facts. Jev may advise whether to escalate; it must not suppress a required human gate or authorize a privileged action. A classifier can be confidently wrong.

## Question integrity: avoid laundering the caller's bias

Jev is not an independent check if its question/state already encode the LLM's expected conclusion. A confident result can reflect leading wording, selective evidence, loaded labels, or a rubric designed to endorse the caller—not a useful second signal.

- Prefer **neutral, versioned question templates and criteria** for recurring decisions, reviewed independently of the particular outcome the caller wants.
- Separate observable facts, quoted requirements, and artifact evidence from agent interpretations. Do not present "the fix is obviously correct" or "the risky change" as input facts. Include relevant counterevidence and disclose missing evidence.
- Keep the agent's proposed answer/expectation out of the classifier request when using Jev as an independent signal. Record that baseline separately for later comparison, not inside the evidence Jev receives.
- Use balanced, specific alternatives and comparable criteria. Do not make one choice richly justified and the others straw men. A loaded question cannot be repaired merely by neutral label names.
- Treat insufficient evidence explicitly. Where the task permits, use a choice including unknown/needs-decision; a boolean answer alone has no native third state. An adapter may abstain before classification or on a documented policy, rather than force absence of evidence into false certainty.
- Version question/criteria changes. Do not rephrase repeatedly until the desired label appears or silently select the favorable result.
- Before relying on a recurring consequential rubric, test **meaning-preserving paraphrases, label/order variations, and reversed polarity with correctly mapped outcomes**, plus independently labeled positive/negative/insufficient-evidence cases. Separate legitimate semantic changes from framing sensitivity. Small wording variants need not have identical probabilities, but large unjustified flips need investigation.
- Independent rubric review and outcome labels matter more than the caller or Jev certifying its own neutrality. Lints can detect some leading terms/answer leakage; they cannot prove absence of bias.

Use this discipline without turning every cheap call into a committee review: maintain a small set of tested templates, and flag ad hoc or materially changed questions as unvalidated. A probabilistic advisor still never grants authority.

## Measuring its contribution

Follow [observability](observability.md). Record the decision's purpose and criteria revision, predicted answer/probabilities, what the agent would otherwise have done, action actually taken, independent eventual outcome, latency, usage, and cost provenance. Unknown outcomes remain unknown. Record template/criteria revision and provenance, ad hoc status, evidence selection/missingness, and framing-test results where available. Agreement with the caller after a leading prompt must not be counted as independent validation.

Compare ordinary and difficult cases; review misses and disagreements. Agreement with the agent is not proof of added value, and disagreement is not proof Jev is right. Additive benefit, false reassurance, escalation quality, and avoided rework are more useful than call counts alone.

## Availability policy

Jev is selected but must not become an invisible single point of failure. Distinguish unavailable, invalid response, uncertain prediction, and actual negative classification. Use an explicit rule/human escalation path when appropriate; do not invent an answer. Exact abstention thresholds and role-specific policies belong to the concrete design.

## References

[Source register](../project/research/sources.md) · [Pi classifier docs](https://github.com/earendil-works/pi/blob/main/packages/coding-agent/docs/models.md#use-classifier-models) · [Pi codemode docs](https://github.com/earendil-works/pi/blob/main/packages/coding-agent/docs/codemode.md#models)
