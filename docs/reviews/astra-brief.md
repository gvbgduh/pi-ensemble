# Astra review brief

Status: draft brief only. No review has been requested or performed. The initial reviewer will be another Pi session using Astra; exact provider/model and supported thinking levels remain to be confirmed.

## Entry criteria

Before requesting a design review, finish the capability matrix, inspect relevant normative specification/source, run the highest-risk recovery experiments, and prepare a concrete proposed design with alternatives. This scaffold is not yet that design.

## Review workflow and spend

- Use shared, versioned Markdown as the review substrate; no intercom or custom runtime is needed to review this project.
- The author prepares the proposal, capability matrix, evidence links, and explicit unresolved questions. Record the Git revision submitted for review.
- Astra reads the artifacts independently and writes a separate findings document, rather than editing away the author's proposal. The author records dispositions and evidence; the operator resolves consequential trade-offs.
- Avoid concurrent editing of the same file. Separate proposal, review, and disposition files preserve independent perspectives and provenance.
- Proposed initial thinking level: **high**, if supported, for one focused architecture review. This is a starting heuristic, not an evidence-backed optimum. Do not assume maximum thinking is necessary.
- Bound review scope and output, inspect actual usage/cost, and return only unresolved high-risk findings for a second pass. Routine editorial changes need not consume another deep Astra review.
- Keep review artifacts public-safe. Access to an ignored import or private session log is not needed.

## Read

1. `README.md`
2. `docs/reference/pi-setup-legacy.txt`
3. `docs/requirements.md`
4. `docs/research/initial-findings.md` and `docs/research/sources.md`
5. `docs/architecture-options.md`
6. The eventual proposed design and experiment results

## Questions

- Does the design preserve persistent peer roles, direct human intervention after work completion, and decentralized callbacks, or accidentally reduce everything to disposable children?
- Can operators choose different role layouts without changing runtime code? Is the local MVP portable without prematurely building remote deployment?
- Is Jev integrated as a measurable decision aid, including codemode, without becoming an unvalidated authority or an uncontrolled cost multiplier?
- Are host ownership, scheduler ownership, conversation lifetime, and worktree ownership kept distinct?
- Can duplicate delivery, a failed run, or a late callback create a message loop or restart stopped work?
- Can any tool, script, delegated agent, or peer bypass a role capability or approval gate?
- Where can an external action complete without its local receipt, and how is that reconciled rather than blindly replayed?
- What remains authoritative when the database, Markdown tracker, Git, and a remote system disagree?
- Are pause/stop semantics and budgets mechanically enforceable across background work?
- Is a new runtime justified over incremental improvements to the existing packages?
- Are migration and rollback possible without losing project knowledge?
- Which claims lack evidence, and what is the smallest experiment that could falsify the recommendation?

## Requested output

Prioritized findings with requirement IDs, concrete failure scenarios, evidence, proposed fixes, and blocking versus non-blocking classification. Separate architectural objections from personal preferences. Record the reviewed revision and unresolved assumptions. Review feedback is not operator approval.
