# loxdb_cli

## Purpose

`loxdb_cli` is a terminal tool for inspecting and operating on plain and secure `loxdb` files.

## Main capabilities

- non-interactive commands for KV/TS/REL/info/export/import/hex/watch
- interactive TUI mode
- secure open with key/keyfile
- JSON/CSV export and JSON import

## Key binaries

- `loxdb-cli`
- `mdbc_fixture_gen`

## Notes

- Built and tested inside this repository CMake workflow.
- Uses real backend integration (`loxdb`, `loxdb_secure`, `loxdb_codec`).
