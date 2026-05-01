# Architecture

The public API is declared in `include/loxdb_secure.h`, while the implementation lives in `src/loxdb_secure.c`.

## Key Derivation

The master key is expanded into three domain-separated keys:

- encryption key
- MAC key
- IV derivation key

## Record Protection

Each stored payload is encoded as:

```text
[IV | tag | ciphertext]
```

The MAC covers:

- database ID
- optional engine-specific metadata
- IV
- ciphertext

## Scope

Protected payloads:

- KV values
- TS samples
- REL row blobs

Not protected:

- key names
- table names
- schema metadata outside encrypted row payloads

## Persistence Notes

When `loxdb` storage is backed by the POSIX port, the secure wrapper rides on top of the upstream WAL and recovery flow. The integration suite covers both:

- clean flush and reopen
- simulated power loss with replay on next open
