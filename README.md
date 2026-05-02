# loxdb_pro_docs

[![Docs Status](https://img.shields.io/badge/docs-public-blue)](#)
[![Source](https://img.shields.io/badge/source-internal-orange)](#)
[![Scope](https://img.shields.io/badge/scope-api_only-informational)](#)

Public documentation for the internal `loxdb_pro` project.

## Scope

This repository intentionally publishes only:
- public API-level documentation
- integration-facing behavior contracts
- usage limits and error-handling guidance
- release notes and public roadmap

This repository intentionally does **not** publish:
- internal module internals
- implementation internals
- private validation procedures
- source code or proprietary engineering know-how

## Structure

- `docs/api/` — API reference documentation
- `docs/assets/` — documentation assets
- `docs/spec/` — public contracts and interface-level specifications
- `docs/roadmap/` — public milestones
- `docs/releases/` — public release notes
- `docs/PRODUCT_README.md` — product overview

## Internal/External Split

- Internal engineering repository: `loxdb_pro` (private)
- Public docs repository: `loxdb_pro_docs` (this repository)

## Publishing Discipline

Before each publish, validate:
- no absolute local paths
- no personal/private contact leakage
- no internal implementation disclosure

See [DOCS_PUBLISH_CHECKLIST.md](DOCS_PUBLISH_CHECKLIST.md).