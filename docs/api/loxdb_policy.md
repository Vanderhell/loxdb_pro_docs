# `loxdb_policy` (central operation gate)

- Public header: `modules/loxdb_policy/include/loxdb_policy.h`
- CMake target: `loxdb::policy`

## Purpose

Policy checks for KV/TS/REL operations:

- key/value size limits
- auth and encryption requirements per operation
- rate limiting (ops/sec) + failure streak gating
- optional pre/post hook coverage enforcement

## API

- `loxdb_policy_err_t loxdb_policy_init(loxdb_policy_t *policy, const loxdb_policy_cfg_t *cfg);`
- `loxdb_policy_err_t loxdb_policy_eval(const loxdb_policy_t *policy, const loxdb_policy_request_t *request, const loxdb_policy_usage_t *usage, loxdb_policy_decision_t *out_decision);`
- `loxdb_policy_err_t loxdb_policy_check_hook_coverage(const loxdb_policy_t *policy, const loxdb_policy_hooks_t *hooks, uint32_t *out_covered_mask, uint32_t *out_missing_mask);`
- `loxdb_policy_err_t loxdb_policy_before_op(const loxdb_policy_t *policy, const loxdb_policy_request_t *request, const loxdb_policy_usage_t *usage, const loxdb_policy_hooks_t *hooks, loxdb_policy_decision_t *out_decision);`
- `loxdb_policy_err_t loxdb_policy_after_op(const loxdb_policy_t *policy, loxdb_policy_op_t op, int32_t op_result, const loxdb_policy_hooks_t *hooks);`
- name helpers:
  - `const char *loxdb_policy_err_name(loxdb_policy_err_t err);`
  - `const char *loxdb_policy_action_name(loxdb_policy_action_t action);`
  - `const char *loxdb_policy_op_name(loxdb_policy_op_t op);`
