# Change Cycle Checklist (Product Layer: loxdb_pro)

Use this checklist for every change batch.

## 1) Scope
- [ ] Define change goal in 1-3 sentences.
- [ ] Classify scope: `policy` / `security` / `ops` / `api orchestration` / `docs` / `tests`.
- [ ] Confirm ownership boundary vs core (`docs/CORE_PRO_BOUNDARY.md`).

## 2) Design Guardrails
- [ ] Confirm no semantic fork of core behavior.
- [ ] Define mapping: `core fact/error -> policy decision -> operator action`.
- [ ] Ensure deterministic decision outputs for identical inputs.

## 3) Implementation
- [ ] Apply minimal patch set.
- [ ] Keep policy outcomes explainable (`ALLOW/DEGRADE/BLOCK` + reason).
- [ ] Preserve structured logging fields for diagnostics/evidence.

## 4) Tests
- [ ] Run targeted module tests for touched modules.
- [ ] Run at least one end-to-end flow including core interaction.
- [ ] Run negative-path for policy denial/recovery if touched.

## 5) Documentation Sync
- [ ] Update canonical PRO docs (`PRO_POLICY_DECISION_CONTRACT`, `ERROR_MAPPING_MODEL`, etc.).
- [ ] Update `docs/DOCS_MAP.md` if new docs were added.
- [ ] Verify cross-repo links per `docs/DOCS_SYNC_PLAN.md`.

## 6) Release Hygiene
- [ ] Summarize operator impact and migration notes.
- [ ] Update commercial/evidence notes if relevant.
- [ ] Confirm tree is ready for commit.

## 7) Done Criteria
- [ ] No core/pro boundary drift.
- [ ] Deterministic + auditable outcomes.
- [ ] Tests pass for touched scope.
- [ ] Docs synchronized across repos.
