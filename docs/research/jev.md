# Jev: active decision-model research

Status: planned integration and experiments. No Jev calls have been made, credentials configured, pricing verified, or performance measured in this project.

## Intent

The operator has Jev access and wants to develop practical intuition through active use, including codemode. This is an explicit research track, not an optional idea to forget after scaffolding. Use bounded, meaningful experiments once access and a spend envelope are confirmed.

## Existing Pi support

Installed Pi 1.3.0 documentation (`docs/models.md` and `docs/codemode.md`, read in full) describes native classifier access:

- `models.getAvailableOfType("classifier")` discovers authenticated classifier models.
- `models.classify(model, { state, questions })` answers typed boolean, choice, and ordered-score questions about JSON state.
- Calls return probabilities/confidence and usage where supported; provider errors are reported in `stopReason`/`errorMessage`, not necessarily thrown.
- Documented routes include direct TypeSafe (`typesafe` / `jev-latest`, `TYPESAFE_API_KEY`) and other providers. Confirm the operator's actual route rather than assuming one.
- Classifier calls are not chat agents and do not appear in the ordinary `/model` picker.
- Codemode availability is separate from installing a web extension. The active research session does not expose codemode; enabling it is a separate, explicit setup task.
- The Durable README also documents `models.classify()` within its codemode integration, but credential reuse and exact version compatibility with our eventual host remain unverified.

Public upstream entry points (moving references, not pinned evidence):

- https://github.com/earendil-works/pi/blob/main/packages/coding-agent/docs/models.md#use-classifier-models
- https://github.com/earendil-works/pi/blob/main/packages/coding-agent/docs/codemode.md#models
- [Pinned Durable README](sources.md)

No custom provider extension should be assumed necessary until the native route is tested.

## Cost and data boundaries

- Establish provider, model/version, billing units, price, quota, and a hard experiment budget first. Do not infer price from speed or model size.
- Pi's docs state that absent catalog pricing can yield zero reported cost, including direct TypeSafe Jev. **Zero reported cost is not proof of zero billed cost.** Record unknown cost explicitly and reconcile with provider billing.
- Use public/synthetic inputs initially. Decide what private code, tracker data, or workplace context may leave the machine before sending it.
- Batch questions about the same state when useful; cache only with explicit state, question, and model-version keys. Cap calls/concurrency and do not silently choose another paid route.
- Deterministic policy checks stay deterministic. Classifier output cannot grant a capability, approve publication, merge, or authorize production mutation.

## Initial experiments

| ID | Task | Baseline and evaluation |
| --- | --- | --- |
| J1 | Triage synthetic CI failures and review findings into next-action categories | Compare deterministic filters and agent judgments against manually labeled outcomes; inspect confusion and abstention. |
| J2 | Flag disagreements or contradictions between a proposed change and explicit decisions | Seed known contradictions and clean controls; measure false negatives and false positives, not just agreement. |
| J3 | Select whether a work item needs adviser attention | Shadow mode first; compare escalations with human review and actual discovered issues. No automatic dismissal of review. |
| J4 | Score completion evidence against an explicit bounded rubric | Use tests/artifacts and seeded omissions; compare with human judgments. A score cannot replace required verification. |

For each batch retain public-safe examples, question wording, criteria, model/provider/version, predictions/probabilities, independently established labels/outcomes, disagreements, latency, usage, and billed/estimated/unknown cost. Store private raw results outside Git.

Start with deliberately varied and difficult examples as well as routine ones. The point is to learn where Jev changes a useful decision, where it fails, and where simpler rules suffice—not to maximize calls.

## Later application boundary

Keep decision tasks/provider selection configurable and optional, with explicit unavailable/uncertain outcomes and per-project budgets. Consider task routing only after calibration. Laya may be a separate alternative; neither hardware suitability nor protocol compatibility is assumed on this laptop.

## Next actions

1. Confirm the operator's Jev route/access method and budget without exposing secrets in chat or Git.
2. Review configuration/security docs before enabling codemode or changing credentials/settings.
3. Discover the available classifier through the supported runtime API.
4. Run one small, budgeted public/synthetic experiment; record failures and actual cost visibility.
5. Use findings to shape the eventual Ensemble integration, rather than designing around untested assumptions.
