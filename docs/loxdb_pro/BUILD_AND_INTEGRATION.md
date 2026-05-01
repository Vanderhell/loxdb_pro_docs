# Build & integration

## CMake presets (repo build)

Typical local build:

```bash
cmake --preset debug
cmake --build --preset debug
```

Release build:

```bash
cmake --preset release
cmake --build --preset release
```

Safety-focused release build:

```bash
cmake --preset safety-release
cmake --build --preset safety-release
ctest --preset safety-release --output-on-failure
```

## Consuming as a library (recommended)

`loxdb_pro` is typically consumed as CMake targets. Targets are exported as `loxdb::...` (for example `loxdb::api`, `loxdb::secure`, `loxdb::metrics`).

Keep the integration minimal:

- link only the modules you use
- include only their public headers

## Optional storage modules

These are designed for embedded deployments but are optional in host builds:

- `LOXDB_PRO_BUILD_SDCARD_ADAPTER=ON|OFF` (default `OFF`)
- `LOXDB_PRO_BUILD_FTL_BASIC=ON|OFF` (default `ON`)

For SD card adapter with real FatFS headers:

- `LOXDB_FATFS_INCLUDE_DIR=<path>` (must provide `ff.h`)
