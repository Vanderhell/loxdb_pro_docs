# Changelog

All notable changes to this project are documented in this file.

## [Unreleased]

### Added

- Startup policy envelope over core preflight facts:
  - `loxdb_admission_eval_startup(...)`
  - startup decision types (`ALLOW/DEGRADE/BLOCK`) and reason taxonomy.
  - startup decision string helpers in admission module.
- Policy module startup integration:
  - `loxdb_policy_eval_startup(...)` producing operator-facing action text.
  - stable mapping from startup reason to actionable operator guidance.
- Extended tests:
  - admission tests for startup mapping + determinism.
  - policy tests for startup envelope integration.
- Documentation sync baseline:
  - `docs/DOCS_MAP.md`
  - `docs/DOCS_SYNC_PLAN.md`
  - `docs/CHANGE_CYCLE_CHECKLIST.md`
  - expanded `docs/PRO_POLICY_DECISION_CONTRACT.md` with API example.
