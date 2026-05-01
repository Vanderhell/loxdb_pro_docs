# loxdb_adapter_sdcard

## Purpose

`loxdb_adapter_sdcard` exposes a FatFS file as a `lox_storage_t` backend for `loxdb`.

## Main API

- `loxdb_adapter_sdcard_create(...)`
- `loxdb_adapter_sdcard_destroy(...)`

## Behavior

- `read`: `f_lseek` + `f_read`
- `write`: direct write or cache-coalesced write (when cache is enabled)
- `erase`: no-op (`LOX_OK`)
- `sync`: flush cache + `f_sync`
- `erase_size = 1`, `write_size = 1`

## Notes

- Caller owns FatFS mount/unmount lifecycle.
- File is created lazily on first write.
- Intended as logical file storage, not raw flash emulation.

## Test profiles

- `test_sdcard`: default unit profile using local stub `tests/ff.h`
- `test_sdcard_softsim`: soft simulation using host file-backed FatFS shim
- `test_sdcard_real_ff`: optional profile compiling against external real `ff.h`
- `sdcard_bench_latency`: optional latency micro-benchmark (fake FatFS harness)

Enable real-header profile:

```bash
cmake -S . -B build/sdcard-real ^
  -DLOXDB_PRO_ENABLE_TESTS=ON ^
  -DLOXDB_PRO_BUILD_SDCARD_ADAPTER=ON ^
  -DLOXDB_PRO_ENABLE_SDCARD_REAL_FATFS_TESTS=ON ^
  -DLOXDB_FATFS_INCLUDE_DIR=<path-to-fatfs-headers>
cmake --build build/sdcard-real
ctest --test-dir build/sdcard-real -R "sdcard|sdcard_real_ff" --output-on-failure
```

Windows one-command runner:

```powershell
.\scripts\run_sdcard_real_vendor.ps1 -FatFsIncludeDir "C:\vendor\fatfs\include"
```

Linux/macOS runner:

```bash
./scripts/run_sdcard_real_vendor.sh /path/to/vendor/fatfs/include
```

## Power-loss scenario (simulated)

- unit test `sdcard_power_loss_drops_unflushed_cache` verifies that non-flushed cache writes are lost after hard reset
- persisted bytes that were `sync()`ed before reset stay readable after reopen

## Benchmark (latency)

Build and run:

```bash
cmake --build build/sdcard-real --config Debug --target sdcard_bench_latency
./build/sdcard-real/Debug/sdcard_bench_latency.exe
```

Output contains average write latency in microseconds for:

- cache disabled (`cache=0`)
- cache enabled (`cache=512`)
