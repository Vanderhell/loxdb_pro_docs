# Error Mapping Model

## Purpose
Canonical mapping from core failures to PRO outcomes and operator guidance.

## Mapping Shape
- `core_error`
- `policy_decision`
- `operator_action`
- `severity`
- `recoverability`

## Example Table
| core_error | policy_decision | operator_action | recoverable |
|---|---|---|---|
| LOX_ERR_NO_MEM | BLOCK | reduce profile / increase ram_kb | yes |
| LOX_ERR_FULL | DEGRADE | compact/fallback profile | yes |
| LOX_ERR_CORRUPT | BLOCK | recreate/recover image | depends |

## Logging Contract
- structured event fields,
- correlation id,
- timestamp and profile snapshot.
