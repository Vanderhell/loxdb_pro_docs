# loxdb_monitor

## Purpose

`loxdb_monitor` maps runtime health snapshots to policy decisions.

## Main API

- `loxdb_monitor_evaluate(...)`
- `loxdb_monitor_evaluate_ex(...)`

## Features

- pressure threshold evaluation
- warning/restart/reboot decision mapping
- optional watchdog and health-report hooks
- optional self-heal callback path
