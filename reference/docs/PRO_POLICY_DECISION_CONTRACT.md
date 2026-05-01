# PRO Policy Decision Contract

## Purpose
Deterministic policy layer contract over core facts.

## Inputs
- core preflight result,
- runtime pressure stats,
- policy profile,
- operation type/context.

Minimum required preflight fields consumed from core report:
- `status`
- `ram_kb`, `kv_pct`, `ts_pct`, `rel_pct`
- `kv_arena_bytes`, `ts_arena_bytes`, `rel_arena_bytes`
- `storage_required_bytes`, `storage_capacity_bytes`
- `wal_size`, `bank_size`

## Outputs
- `ALLOW`
- `DEGRADE`
- `BLOCK`

Each output must include:
- reason code,
- evidence fields,
- recommended operator action.

## Determinism Rules
- same inputs => same decision
- no implicit fallback without explicit event/log

## Decision Examples
- low memory during init:
- storage pressure saturation:
- policy-denied operation:

Suggested startup mapping:
- preflight `LOX_OK` -> `ALLOW` startup
- preflight `LOX_ERR_NO_MEM` -> `DEGRADE` (switch to lower resource profile) or `BLOCK`
- preflight `LOX_ERR_STORAGE` -> `DEGRADE` (smaller storage profile/recreate policy) or `BLOCK`
- preflight `LOX_ERR_INVALID` -> `BLOCK` (configuration/contract fix required)

## API Example

```c
loxdb_startup_preflight_t pf = {0};
loxdb_policy_startup_decision_t out = {0};

/* pf fields are populated from core preflight report */
pf.preflight_status = rep.status;
pf.storage_required_bytes = rep.storage_required_bytes;
pf.storage_capacity_bytes = rep.storage_capacity_bytes;
pf.storage_erase_size = rep.storage_erase_size;
pf.storage_write_size = rep.storage_write_size;

if (loxdb_policy_eval_startup(&pf, &out) == LOXDB_POLICY_OK) {
    /* out.decision: ALLOW / DEGRADE / BLOCK */
    /* out.operator_action: user-facing next step */
}
```
