# Security Policy

## Scope

Security reports are accepted for:

- LoxDB PRO security-at-rest behavior,
- integrity and tamper-detection paths,
- secure reopen and key/database identity handling,
- backup/import/export security implications,
- replication or wire-framing misuse that can affect data integrity,
- CLI paths that can expose or corrupt protected data.

The public repository does not contain the PRO source code. Reports against private PRO code require commercial access or coordinated disclosure with the maintainer.

---

## Reporting

Do not publish exploit details publicly before coordination.

Preferred reporting path:

1. Use GitHub private security advisory if available.
2. Otherwise contact the repository owner through the GitHub profile or the related public LoxDB project channel.

Include:

- affected module,
- affected version or commit/package identifier,
- reproduction steps,
- expected behavior,
- observed behavior,
- impact assessment,
- whether the issue affects confidentiality, integrity, availability, or safety evidence.

---

## Non-goals

LoxDB PRO does not replace:

- secure boot,
- hardware root of trust,
- operating system sandboxing,
- transport-layer security,
- deployment-specific key management,
- full product threat modeling.

Security guarantees depend on correct integration, key handling, storage backend behavior, and platform assumptions.
