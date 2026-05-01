# loxdb_pro_docs

Official public documentation repository for `loxdb_pro`.

## Purpose

This repository contains public, technical documentation only:
- architecture and module specifications
- integration and deployment guides
- API and behavior contracts
- release notes and migration notes
- roadmap and non-sensitive project status

No internal source code, private test assets, local machine paths, secrets, or personal contact details are published here.

## Repository Scope

Included:
- `docs/`
- `spec/`
- `roadmap/`
- `releases/`
- `assets/` (documentation assets only)

Excluded:
- runtime source code
- private CI/CD internals
- local benchmark logs with host-specific details
- internal-only operational procedures

## Source of Truth

Development happens in internal repository: `loxdb_pro`.
This repository is a sanitized public documentation mirror.

## Publishing Rule

Before publishing updates, run a documentation sanitization check to ensure there are no:
- absolute local paths (e.g., `C:\Users\...`)
- personal emails and identities
- internal-only infrastructure references

## Initial Structure

- `docs/` technical manuals and guides
- `spec/` formal specs and contracts
- `roadmap/` public roadmap
- `releases/` release notes/changelog snapshots
- `assets/` images/diagrams used by docs