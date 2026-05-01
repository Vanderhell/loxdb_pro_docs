# Development Notes

## Local Validation

```bash
cmake -S . -B build
cmake --build build --config Debug
ctest --test-dir build -C Debug --output-on-failure
```

For release validation:

```bash
cmake --build build --config Release
ctest --test-dir build -C Release --output-on-failure
```

## Repository Layout

- `src/`: CLI and TUI implementation
- `include/`: headers
- `tests/`: focused, integration, TUI, and black-box tests
- `tools/`: fixture and helper utilities
- `docs/`: maintained project documentation

## Test Split

- `test_export`, `test_import`, `test_hex`: focused module tests
- `test_cli_integration`: in-process backend and command flow tests
- `test_cli_tui`: TUI rendering and interactive flow tests
- `test_cli_blackbox`: plain/config/default-RAM external-process tests
- `test_cli_blackbox_secure`: secure and corrupted-input external-process tests

## Build Notes

- default local build tree: `build/`
- dependency updates are disconnected by default during configure
- enable explicit refresh only with `-DMDBC_FETCH_UPDATES=ON`

## Current External Blocker

- `loxdb_cli` now consumes the upstream `loxdb`, `loxdb_secure`, `loxdb_codec`, and `microtest` CMake targets directly
- `loxdb_codec` is temporarily pinned to upstream commit `3ccbbca` until the target-name conflict fix lands in a release tag
- `loxdb_secure` now consumes the upstream `microcrypt` CMake target directly
