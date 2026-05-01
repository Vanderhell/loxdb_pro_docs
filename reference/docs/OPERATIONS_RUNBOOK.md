# Operations Runbook

## Purpose
Operator-facing lifecycle for healthy production behavior.

## Standard Flows
- startup health gate,
- backup/restore,
- retention execution,
- alert response,
- controlled shutdown.

## Incident Flow
1. detect
2. classify
3. decide (allow/degrade/block)
4. act
5. verify
6. record evidence

## Mandatory Telemetry
- pressure metrics,
- policy decisions,
- recovery counters,
- storage health indicators.
