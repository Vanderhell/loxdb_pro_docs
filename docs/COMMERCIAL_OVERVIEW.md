# Product overview (`loxdb_pro`)

## What is `loxdb_pro`

`loxdb_pro` is a set of production-ready C modules around the `loxdb` core, focused on embedded deployments:

- security (encryption + integrity)
- safety validation and auditable build profile
- observability (metrics, logging, monitoring, alerting)
- OTA and schema migration flows
- replication and a minimal framing protocol for MCU links
- optional storage backends (SD card + basic NAND FTL)

## Typical deliverables

Common deliverables for commercial distributions:

- a versioned source drop (or repository access) of `loxdb_pro`
- the public headers (API contract) and release notes
- integration support (CMake, embedded ports, storage backend selection)
- optional safety evidence bundle generation in CI

## Typical integration flow

Integrations typically proceed in phases:

1. integrate `loxdb` core + chosen storage backend
2. adopt `loxdb_api` facade (plain mode)
3. turn on `loxdb_secure` (secure mode) and validate key management
4. add `loxdb_metrics` + `loxdb_logging` for observability
5. add policy/retention/backup/migration/OTA as needed
