# LoxDB PRO Specification: Boundary with Core, Non-Duplication, and Productization Plan

Date: 2026-05-01
Repository: loxdb_pro

## 1) Executive Summary

`loxdb_pro` should be the product layer over `loxdb` core.
It should not fork or re-implement core engine semantics.
It should compose core primitives into governance, security, operations, and certification-ready workflows.

This document defines strict PRO ownership, anti-duplication rules, and concrete improvement tracks.

## 2) Role of This Repository (PRO Contract)

PRO is responsible for:
- governance/admission policy orchestration,
- security-at-rest wrappers and tamper workflows,
- operational runtime modules (backup/retention/scheduler/alerting/monitor),
- product CLI and operator flows,
- evidence and validation packaging for regulated adoption.

PRO is not responsible for:
- redefining core data model semantics,
- diverging `lox_err_t` meanings,
- maintaining duplicate engine algorithms that already exist in core.

## 3) Non-Duplication Boundary (PRO vs Core)

### 3.1 Must remain in core (`loxdb`)
- storage layout math and durable bootstrap logic,
- fundamental feasibility calculations,
- canonical fail-code definitions and core error mapping,
- primitive integrity/recovery semantics.

### 3.2 Must remain in PRO (`loxdb_pro`)
- policy bundles and profile governance,
- secure deployment workflows,
- fleet/operator diagnostics and maintenance UX,
- compliance evidence aggregation/reporting.

### 3.3 Composition rule
- PRO consumes core facts (`preflight`, pressure, stats, errors),
- PRO decides product policy and user action,
- no duplicated engine-side capacity calculators if core already exposes them.

## 4) Duplication Risk Assessment in PRO

### Risk A: Re-implemented feasibility estimators
- Fix: depend on core `preflight` APIs once available.

### Risk B: Security logic mixed into core-facing compatibility adapters
- Fix: keep security wrappers as explicit PRO modules; avoid implicit security behavior in generic adapters.

### Risk C: Operator CLI becomes second engine API
- Fix: CLI must call module APIs, never bypass invariants or create side semantics.

### Risk D: Separate error vocabulary for same failure cause
- Fix: two-layer model:
  - layer 1: canonical core error,
  - layer 2: PRO policy decision/outcome code.

## 5) Concrete Improvement Plan for `loxdb_pro`

## Phase 1: Boundary and Dependency Governance
1. Add explicit module dependency policy:
   - which modules can depend on `loxdb::core` only,
   - which can depend on security/policy layers.
2. Add CI rule preventing reverse leakage:
   - no pro-only headers in core adapter-facing interfaces.

## Phase 2: Admission/Governance Refinement
1. Build policy engine on top of core feasibility facts:
   - input: core preflight + runtime pressure,
   - output: allow/degrade/block with deterministic rationale.
2. Add profile ladder management:
   - safe/default/aggressive policy packs,
   - explicit fallback decisions with auditable trace.

## Phase 3: Operator-Grade Explainability
1. Unified diagnostic envelope:
   - core status + policy outcome + action recommendation.
2. Stable machine-readable incident record:
   - for CLI, logs, and compliance evidence.

## Phase 4: Regulated Readiness Packaging
1. Evidence pipeline:
   - requirement->test->result linkage export.
2. Failure mode matrix packaging:
   - storage fault, low-memory, corruption, policy denial, recovery outcomes.
3. Versioned validation bundles for customer evaluations.

## 6) Proposed `loxdb_pro` Artifacts to Add

1. `docs/CORE_PRO_BOUNDARY.md`
- mirrored boundary ownership from PRO perspective.

2. `docs/PRO_POLICY_DECISION_CONTRACT.md`
- defines allowed/degrade/block outcomes and rationale schema.

3. `docs/ERROR_MAPPING_MODEL.md`
- canonical mapping:
  - core error,
  - policy decision,
  - operator recommendation.

4. `docs/COMPLIANCE_EVIDENCE_PIPELINE.md`
- artifact format and generation flow.

5. `tests/test_policy_decision_determinism.c`
- same inputs -> same policy output.

6. `tests/test_core_contract_no_fork.c`
- ensures PRO wrappers do not alter core invariants.

## 7) API and Module Guidance

- `loxdb_api` should remain orchestration-only; no hidden semantic rewrite.
- `loxdb_admission` should consume core facts and output policy decisions.
- `loxdb_logging`/`loxdb_metrics` should expose structured events that include core and policy context.
- `loxdb_cli` should provide explainability first, not only command execution.

## 8) What to Avoid in PRO Going Forward

- duplicating low-level WAL/storage layout calculators,
- introducing “PRO-only reinterpretation” of core error meanings,
- creating parallel schemas for diagnostics where a canonical one can be shared.

## 9) Near-Term Prioritized Backlog (PRO)

1. Boundary doc + dependency governance gates.
2. Decision contract for admission/policy outcomes.
3. Canonical error mapping model (core->policy->operator action).
4. Deterministic policy ladder implementation.
5. Compliance evidence export pipeline (minimal first version).

## 10) Success Criteria

- Clear ownership split with no duplicated engine logic.
- Deterministic and auditable policy decisions over core facts.
- Operator-facing diagnostics are actionable and certification-friendly.
- PRO remains a strong product layer without semantic drift from core.
