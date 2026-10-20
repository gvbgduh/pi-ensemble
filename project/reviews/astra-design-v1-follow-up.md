# Astra v1.1 amendment check

Date: 2026-10-20. Reviewer: live `pi-ensemble-001-adviser` (Astra). Evidence: intercom message `a2c04e25-8d9d-4ba4-bac3-00a8d8998aa6`. This is the orchestrator's attributed record of the communicated independent result; Astra made no file edits.

## Reviewed scope

Exactly `ab0d906..8f14c9d` for `docs/design.md`, `project/implementation-plan.md`, and `project/reviews/astra-design-v1-disposition.md`, against AD1–AD4. No broad rereview or tests. Original [independent findings](astra-design-v1-findings.md) and [author disposition](astra-design-v1-disposition.md) remain unchanged.

## Result

**No residual high-risk contract blocker identified.**

- AD1 now serializes claim/pin, inventories unknown admission, and preserves immutable retry draft/recoverable wake.
- AD2 separates app admission from recovered native execution, accounts for implicit submit/wait resumes, and includes minimum safe startup/stop in M1. The supported dispatch/parking mechanism appropriately remains an M0 stop/revisit feasibility gate, not an invented existing API.
- AD3 requires crash-safe discoverable executor ownership and blocks writable takeover until old mutation capability is gone. M3/M4 proof is not a prerequisite to restricted M1.
- AD4 provides revisioned per-obligation reconciliation; old steering/driver reports cannot clear newer holds. Driverless human release is actionable.
- AG1–AG3 remain accurate phase-specific implementation gates.

## Recommendation and authority boundary

V1.1 is ready for explicit operator acceptance of direction/scope and separately bounded M0-01 authorization. This closes proposal-level findings only. No implementation, compatibility/auth, dispatch-gating, sandbox or recovery guarantee has been validated. Failed M0 feasibility must stop/revisit, not bypass controls or introduce a second scheduler.

**Operator acceptance and implementation authorization are still pending.**
