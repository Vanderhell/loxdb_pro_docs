# Build and Test

## Dependencies

The workspace expects these helper clones:

- `loxdb` into `.deps/loxdb`
- `microcrypt` into `.deps/microcrypt`
- `microtest` into `.deps/microtest`

## Linux

```sh
cmake -S . -B build -DCMAKE_BUILD_TYPE=Debug
cmake --build build
ctest --test-dir build --output-on-failure
```

## Windows

```powershell
cmake -S . -B build
cmake --build build --config Debug
ctest --test-dir build -C Debug --output-on-failure
```

## macOS

```sh
cmake -S . -B build -DCMAKE_BUILD_TYPE=Debug
cmake --build build --parallel
ctest --test-dir build --output-on-failure
```

## Current Local Status

The repository currently contains four CTest suites:

- `secure_kv` with 25 tests
- `secure_ts` with 20 tests
- `secure_rel` with 20 tests
- `secure_integration` with 16 tests

That gives a current local total of 81 tests.

## GitHub Actions

- `ci.yml` runs build, hygiene checks, and tests on Linux, Windows, and macOS.
- `release.yml` runs on tags matching `v*`.
- A tagged push such as `git tag v1.0.0 && git push origin v1.0.0` produces zipped release assets for Linux, Windows, and macOS.
