# loxdb_pro documentation

[![Docs](https://img.shields.io/badge/docs-public-blue)](#documentation)
[![Scope](https://img.shields.io/badge/scope-api--and--integration-informational)](#scope)

This repository contains the public, integration-facing documentation for the commercial `loxdb_pro` module set (around the `loxdb` core).

## Documentation

Start here:

- [docs/README.md](docs/README.md) (documentation index)
- [docs/COMMERCIAL_OVERVIEW.md](docs/COMMERCIAL_OVERVIEW.md) (what it is + integration flow)
- [docs/MODULE_CATALOG.md](docs/MODULE_CATALOG.md) (modules: what exists and why)
- [docs/api/_INDEX.md](docs/api/_INDEX.md) (per-module API reference index)
- [docs/API_SURFACE.md](docs/API_SURFACE.md) (what headers are public API)
- [docs/BUILD_AND_INTEGRATION.md](docs/BUILD_AND_INTEGRATION.md) (CMake integration + build profiles)
- [docs/LICENSING.md](docs/LICENSING.md) (packaging/licensing notes)

## Modules (what you get)

`loxdb_pro` is delivered as a set of independent C modules (CMake targets `loxdb::...`) that you can pick and choose from:

- **Stable facade:** `loxdb_api` gives one call surface that can switch between plain and secure mode at runtime.
- **Security + safety:** `loxdb_secure`, `loxdb_safety` for encryption/integrity + runtime safety validation.
- **Observability:** `loxdb_metrics`, `loxdb_logging`, `loxdb_monitor`, `loxdb_alerting` for metrics, audit/trace hooks, health evaluation, and alert state machines.
- **Policies + lifecycle:** `loxdb_policy`, `loxdb_admission`, `loxdb_retention`, `loxdb_backup`, `loxdb_schema_migrate` for gating, quotas, retention, backups, and migration flows.
- **Connectivity + ops:** `loxdb_replication`, `loxdb_net`, `loxdb_ota`, `loxdb_cli` for replication core + framing, OTA planning, and CLI tooling.
- **Optional storage backends:** `loxdb_adapter_sdcard`, `loxdb_ftl_basic` for SD card (FatFS) and a basic NAND FTL shim.

For the full table (targets + headers + one-line descriptions), see [docs/MODULE_CATALOG.md](docs/MODULE_CATALOG.md).

## Scope

This repository intentionally publishes only:
- public API-level documentation
- integration-facing behavior contracts and guidance
- packaging/licensing notes

This repository intentionally does **not** publish:
- implementation internals or private validation procedures
- source code or proprietary engineering know-how
