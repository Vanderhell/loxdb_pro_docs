# CORE vs PRO Boundary

## Purpose
Authoritative ownership split to prevent duplicate logic and semantic drift.

## Core Owns
- engine semantics,
- fail-code definitions,
- storage/recovery invariants,
- feasibility facts.
  - including deterministic preflight sizing/feasibility (`lox_preflight` in core).

## PRO Owns
- policy decisions,
- governance and operational workflows,
- security orchestration,
- compliance packaging.

## Forbidden Duplication
- re-implementing core layout math in PRO,
- redefining core error meanings,
- hidden semantic rewrites in wrappers.
 - duplicating core preflight feasibility calculations in policy layer.

## Dependency Rules
- allowed module dependencies:
- forbidden dependency edges:
