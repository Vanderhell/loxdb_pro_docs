# `loxdb_monitor` (health policy + hooks)

- Public header: `modules/loxdb_monitor/include/loxdb_monitor.h`
- CMake target: `loxdb::monitor`
- Category: Observability

## Purpose

Evaluates a health snapshot against a policy and produces an action:

- `NONE`, `WARN`, `RESTART`, `REBOOT`

Extended API supports reason masks and optional hooks:

- watchdog kick
- health emit
- self-heal callback

## API

- `int loxdb_monitor_evaluate(const loxdb_monitor_snapshot_t *snapshot, const loxdb_monitor_policy_t *policy, loxdb_monitor_action_t *out_action);`
- `int loxdb_monitor_evaluate_ex(const loxdb_monitor_snapshot_ex_t *snapshot, const loxdb_monitor_policy_ex_t *policy, const loxdb_monitor_hooks_t *hooks, loxdb_monitor_decision_t *out_decision);`
- `const char *loxdb_monitor_action_name(loxdb_monitor_action_t action);`

