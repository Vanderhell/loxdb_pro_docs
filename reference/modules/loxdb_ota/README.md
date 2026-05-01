# loxdb_ota

## Purpose

`loxdb_ota` assists OTA compatibility checks and migration/rollback runtime flow.

## Main API

- planning: `loxdb_ota_plan_update(...)`
- runtime flow: `loxdb_ota_runtime_*`
- compatibility helper: `loxdb_ota_check_compat(...)`

## Features

- schema compatibility gating
- migration-required signaling
- failed-boot based rollback trigger
