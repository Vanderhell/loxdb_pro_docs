# CLI And TUI Behavior

## Scope

This document is the maintained project reference for user-facing behavior.

## Global Defaults

- RAM budget default: `64 KB`
- watch default when enabled without an explicit interval: `500 ms`
- write operations require explicit `--write`
- destructive actions require confirmation unless `-y/--yes` is used

## Command Surface

- `kv list|get|set|del|clear`
- `ts list|query|insert|clear`
- `rel list|schema|dump|insert|update|delete|clear`
- `export`
- `import`
- `hex`
- `info`
- `watch`
- `help`

## TUI

- panels: `KV`, `TS`, `REL`, `HEX`, `INFO`
- `Tab` or `1-5` switches panels
- `Left/Right` scrolls `REL` horizontally
- `Up/Down` moves selection, including selected REL row in expanded view
- `H` focuses hex on the selected record or region
- `W` toggles live watch
- `?` opens help
- destructive actions use explicit `y/N` confirmation

## Encrypted Databases

- `-k/--key` accepts the master key in hex
- `-K/--keyfile` reads the master key from a file
- if the database is encrypted and no key was provided, the CLI prompts interactively
