# Public API Surface

This document defines the public documentation boundary for `loxdb_pro`.

## Exposed in this repository

- Stable public API names and expected behavior.
- Error-code classes and caller handling expectations.
- Integration lifecycle (init/open/use/close) at interface level.
- Resource limits required by callers.

## Not exposed in this repository

- Internal module composition.
- Scheduling, persistence, or allocator internals.
- Internal security implementation details.
- Proprietary implementation strategy.

## Compatibility policy

- Public API changes are documented in release notes.
- Breaking changes are explicitly marked before adoption.
- Deprecated API usage includes migration notes when available.