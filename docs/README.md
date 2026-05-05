# loxdb_pro documentation

This folder contains the **official product documentation** for `loxdb_pro` (the commercial integration layer around the `loxdb` core).

Start here:

- [COMMERCIAL_OVERVIEW.md](COMMERCIAL_OVERVIEW.md) - product overview (positioning + integration flow)
- [MODULE_CATALOG.md](MODULE_CATALOG.md) - module catalog (what exists and what it provides)
- [api/_INDEX.md](api/_INDEX.md) - per-module API reference index
- [API_SURFACE.md](API_SURFACE.md) - which headers are intended as public API and how to consume them
- [BUILD_AND_INTEGRATION.md](BUILD_AND_INTEGRATION.md) - CMake integration, presets, and recommended build profiles
- [LICENSING.md](LICENSING.md) - packaging/licensing notes

## Modules (at a glance)

The commercial value of `loxdb_pro` is that you can assemble a production-ready feature set by enabling only the modules you need:

- **Facade / minimal integration churn:** `loxdb_api`
- **Security + safety:** `loxdb_secure`, `loxdb_safety`
- **Observability:** `loxdb_metrics`, `loxdb_logging`, `loxdb_monitor`, `loxdb_alerting`
- **Policies + resource control:** `loxdb_policy`, `loxdb_admission`
- **Retention + data lifecycle:** `loxdb_retention`, `loxdb_backup`, `loxdb_schema_migrate`
- **Connectivity + fleet ops:** `loxdb_replication`, `loxdb_net`, `loxdb_ota`, `loxdb_cli`
- **Optional storage backends:** `loxdb_adapter_sdcard`, `loxdb_ftl_basic`

See [MODULE_CATALOG.md](MODULE_CATALOG.md) for the full table including CMake targets and public headers.

## Recommended entrypoint (most users)

If you want one stable API that can switch between **plain** and **secure** modes without changing your app code, use:

- module `loxdb_api` (`modules/loxdb_api/include/loxdb_api.h`)

## Scope

This documentation describes the **`loxdb_pro` modules under `modules/`** and their public headers in `modules/*/include/`.
The underlying `loxdb` core API is intentionally treated as a dependency and documented separately.
