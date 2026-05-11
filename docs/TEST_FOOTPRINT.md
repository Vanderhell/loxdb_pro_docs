# Test footprint

This page documents the verified test coverage across all `loxdb_pro` modules. Implementation source and test source are not part of this public documentation repository — only the resulting test surface is summarized here for evaluation purposes.

## Summary

- **Total modules**: 21
- **Total test files**: 36
- **Total test cases**: 323
- **Test framework**: `microtest` (`mtest.h`) for most module test binaries; `main()` return-code style for `loxdb_cli`
- **Verified on hardware**: ESP32-S3 (Arduino bench reports under `docs/test_reports/`, dated 2026-05-06)
- **Sanitizers**: Not detected in the repository configuration (no dedicated ASan/UBSan/TSan lanes found)
- **Static analysis**: Not detected in the repository configuration (no `clang-tidy` / `cppcheck` integration found)

## Per-module test footprint

| Module | Category | Test files | Test cases |
|--------|----------|------------|------------|
| loxdb_api | Facade | 1 | 5 |
| loxdb_secure | Security | 4 | 81 |
| loxdb_safety | Safety | 1 | 3 |
| loxdb_codec | Codec | 6 | 140 |
| loxdb_metrics | Observability | 1 | 6 |
| loxdb_metrics_microtimer | Observability | 0 | 0 |
| loxdb_logging | Observability | 1 | 2 |
| loxdb_monitor | Observability | 1 | 4 |
| loxdb_alerting | Observability | 1 | 3 |
| loxdb_admission | Policy | 1 | 3 |
| loxdb_policy | Policy | 1 | 3 |
| loxdb_retention | Policy | 1 | 4 |
| loxdb_scheduler | Policy | 1 | 2 |
| loxdb_backup | Policy | 1 | 7 |
| loxdb_schema_migrate | Policy | 1 | 3 |
| loxdb_replication | Connectivity | 1 | 4 |
| loxdb_net | Connectivity | 1 | 8 |
| loxdb_ota | Connectivity | 1 | 4 |
| loxdb_adapter_sdcard | Storage | 3 | 9 |
| loxdb_ftl_basic | Storage | 1 | 6 |
| loxdb_cli | Tooling | 7 | 26 |
| **Total** | | **36** | **323** |

## Cross-module / integration testing

In addition to module-level test binaries, the following cross-module validation surfaces are present:

- **Full-stack host integration**: a single “full stack” host test binary exists under `tests/pro/` in the private repo, combining most modules in one execution path (this is intentionally not mirrored here; only its existence is summarized).
- **Storage adapter integration**: SD-card adapter tests include both a soft simulation profile and a “real vendor FatFS headers” profile (see `docs/test_reports/TEST_MATRIX.md` and the SD stress bench reports dated 2026-05-06).
- **CLI/TUI integration**: `loxdb_cli` includes multiple host-driven integration/blackbox test binaries which validate end-user flows via program exit codes.
- **Hardware bench evidence**: ESP32-S3 serial-driven bench runs are captured under `docs/test_reports/` (KV/TS/REL benches + integrity/selfcheck checks, dated 2026-05-06).

Module-level test coverage remains the primary validation surface; end-to-end integration tests are present but comparatively smaller in number.

## Continuous integration

- **CI system**: GitHub Actions is used in the private source repository (a workflow exists under `.github/workflows/`).
- **Execution model**: Host builds use CMake + CTest (`ctest`) to build and run the module test binaries; a practical validation matrix for host + SD-card profiles + ESP32 bench evidence is documented in `docs/test_reports/TEST_MATRIX.md`.

