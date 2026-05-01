# Hardware Validation

## Goal

This checklist is for validating `loxdb-cli` on real hardware after the recent CLI, TUI, test, CI, and dependency-graph changes.

Use it as a smoke test plus operator log. The aim is to confirm:

- release binaries run on real systems
- ANSI TUI behaves correctly on physical terminals
- file I/O and watch mode behave correctly on real storage
- encrypted database flows work outside the development machine
- the user experience is still acceptable on weaker hardware

## Suggested Targets

Minimum target set:

- Windows desktop or laptop
- Linux machine or SBC
- one lower-end machine with slower CPU or storage

Recommended examples:

- Windows 10/11 desktop or notebook
- Raspberry Pi 4/5 over SSH terminal
- small x86 Linux box, NUC, or older laptop

## Test Inputs

Prepare these before starting:

- release archive from GitHub Releases
- one plain database fixture
- one secure database fixture
- one import JSON file
- one writable test directory on the target device
- one known-good master key and one wrong key

Known secure test key used in project fixtures:

```text
000102030405060708090a0b0c0d0e0f
```

## Environment Log

Record this per device:

- device name:
- OS:
- architecture:
- terminal app:
- local build or release ZIP:
- storage type:
- result:

## Baseline Checks

Pass criteria:

- binary starts
- help works
- plain DB opens
- secure DB opens with the correct key
- secure DB fails with the wrong key

Commands:

```powershell
.\loxdb-cli.exe --version
.\loxdb-cli.exe test_fixtures\plain.bin info
.\loxdb-cli.exe -k 000102030405060708090a0b0c0d0e0f test_fixtures\secure.bin info
.\loxdb-cli.exe -k ffffffffffffffffffffffffffffffff test_fixtures\secure.bin info
```

Expected:

- `--version` exits successfully
- plain `info` exits successfully
- secure `info` with the correct key exits successfully
- secure `info` with the wrong key fails clearly

## CLI Smoke

Run these on each device:

```powershell
.\loxdb-cli.exe test_fixtures\plain.bin kv list
.\loxdb-cli.exe test_fixtures\plain.bin ts query temp
.\loxdb-cli.exe test_fixtures\plain.bin rel dump users --format json
.\loxdb-cli.exe -o out.json test_fixtures\plain.bin export --format json
.\loxdb-cli.exe --write test_fixtures\plain.bin import out.json --dry-run
.\loxdb-cli.exe test_fixtures\plain.bin hex --length 64
```

Pass criteria:

- output is readable in the local terminal
- JSON export creates a valid file
- import dry-run succeeds without mutating data
- hex output is stable and not visually corrupted

## Secure Flow

Validate all three secure entry paths:

- `-k`
- `-K`
- interactive prompt

Commands:

```powershell
.\loxdb-cli.exe -k 000102030405060708090a0b0c0d0e0f test_fixtures\secure.bin kv list
.\loxdb-cli.exe -K good.key test_fixtures\secure.bin kv list
.\loxdb-cli.exe test_fixtures\secure.bin info
```

Pass criteria:

- correct key succeeds
- wrong key fails
- prompt path does not echo the key
- secure DB is never silently treated as plain DB

## TUI Validation

Start the TUI:

```powershell
.\loxdb-cli.exe test_fixtures\plain.bin
```

Verify manually:

- `Tab` and `1-5` switch panels correctly
- `?` opens and closes help
- `H` focuses HEX on the current selection
- `W` toggles watch
- `Left/Right` scrolls REL horizontally
- `Up/Down` changes selection
- `U` updates the selected REL row
- activity log updates after prompts and actions

Pass criteria:

- no broken redraws
- no stuck keyboard handling
- no obvious ANSI corruption
- no missing footer/help hints

## Watch Mode

Validate both watch paths:

```powershell
.\loxdb-cli.exe --watch 500 test_fixtures\plain.bin kv list
.\loxdb-cli.exe --watch 500 test_fixtures\plain.bin
```

Pass criteria:

- polling works
- output refreshes predictably
- TUI shows live mode clearly
- no runaway CPU behavior on weaker devices

## Write Operations

Use a disposable copy of the database.

Validate:

- KV set/delete
- TS insert
- REL insert/update/delete
- clear confirmation flow

Pass criteria:

- `--write` gating behaves correctly
- destructive actions require confirmation unless explicitly bypassed
- data changes persist and can be read back

## Performance Notes

Record rough operator observations:

- startup latency:
- TUI responsiveness:
- watch mode CPU impact:
- export speed:
- import speed:
- any visible lag:

Use simple ratings if needed:

- `good`
- `acceptable`
- `poor`

## Failure Log

For each failure, record:

- device:
- command:
- expected:
- actual:
- screenshot or terminal capture:
- reproducible:
- probable layer:

Suggested layer tags:

- `cli`
- `tui`
- `watch`
- `secure`
- `filesystem`
- `terminal`
- `build`
- `dependency`

## Exit Criteria

A target device can be marked `PASS` when:

- all baseline checks pass
- CLI smoke passes
- secure flow passes
- TUI smoke passes
- watch mode is usable
- no unexplained crash or data corruption is observed

## Recommended Next Step

After first hardware validation round:

- summarize failures by device
- separate terminal-only issues from backend issues
- create follow-up fixes before tagging a wider release
