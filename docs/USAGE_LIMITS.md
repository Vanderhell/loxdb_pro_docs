# Usage Limits and Integration Rules

## Purpose

Define caller-visible limits required for predictable integration.

## Rules

- Callers must initialize configuration explicitly.
- Storage and memory limits must be configured before runtime usage.
- Return codes must be handled deterministically.
- Recovery paths must be defined for startup and write failures.

## Operational expectation

The public API is contract-driven: callers should rely on documented behavior only,
not on inferred internal implementation details.