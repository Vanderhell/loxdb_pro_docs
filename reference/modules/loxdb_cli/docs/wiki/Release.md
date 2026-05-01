# Release Workflow

## Tag-Based Release

GitHub Actions publishes Windows release artifacts when a tag matching `v*` is pushed.

Artifacts:

- `loxdb-cli.exe`
- `mdbc_fixture_gen.exe`
- `README.md`
- `LICENSE`
- `.sha256` checksum file

## Local Validation Before Tagging

```bash
cmake -S . -B build
cmake --build build --config Debug
ctest --test-dir build -C Debug --output-on-failure
cmake --build build --config Release
ctest --test-dir build -C Release --output-on-failure
```
