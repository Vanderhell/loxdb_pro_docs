# Documentation Sync Plan (loxdb_pro <-> loxdb)

Date: 2026-05-01
Status: active baseline

## Goal
Keep product-layer docs aligned with core semantics, without re-defining core behavior.

## Canonical Ownership
- Core semantics/fail-codes/limits/startup flow: `loxdb` docs are canonical.
- Policy/ops/compliance/product behavior: `loxdb_pro` docs are canonical.

## Do Not Duplicate
- Do not copy core fail-code meanings into PRO as independent definitions.
- Do not fork startup invariants or storage contract text in PRO.

## Sync Workflow
1. Change canonical source doc first.
2. Update cross-references in counterpart repo.
3. Validate links + headings + ownership labels.
4. Record sync in release notes/changelog context.

## Reference Set
- Core source docs: `LIMITS_AND_FAILURES`, `STARTUP_DECISION_FLOW`, `FAIL_CODE_CONTRACT`.
- PRO source docs: `PRO_POLICY_DECISION_CONTRACT`, `ERROR_MAPPING_MODEL`, `OPERATIONS_RUNBOOK`, `COMPLIANCE_EVIDENCE_PIPELINE`.

## Release Gate Item
- "Core/PRO docs synchronized" must be explicitly checked before release tagging.
