# LoxDB PRO Guarantees and Boundaries

This document describes the intended public positioning of LoxDB PRO.

It is deliberately strict: it separates what the PRO layer is designed to provide from what must still be handled by the final product, platform, or deployment.

---

## Intended guarantees

LoxDB PRO is designed to provide:

1. secure-at-rest storage paths,
2. integrity verification around protected data,
3. tamper-detection scenarios,
4. runtime safety validation,
5. evidence-friendly safety output,
6. policy and admission hooks around operation flow,
7. monitoring and alert decision support,
8. backup/import/export operational flows,
9. retention and scheduling logic,
10. transport-agnostic replication state logic,
11. lightweight wire framing,
12. OTA and schema migration orchestration boundaries,
13. optional storage backend abstractions.

---

## Explicit non-goals

LoxDB PRO does not replace:

- secure boot,
- hardware root of trust,
- trusted execution environment,
- operating system sandboxing,
- full platform access control,
- TLS or transport-layer security,
- physical attack resistance,
- deployment-specific key management,
- product-level threat modeling,
- safety certification by itself,
- a general-purpose SQL database,
- a cloud database server,
- a distributed consensus system.

---

## Security boundary

The security layer can protect data paths only inside the assumptions given to it.

Correct behavior still depends on:

- correct key provisioning,
- correct key storage,
- correct database identity handling,
- storage backend behavior,
- system clock and monotonic counter assumptions where applicable,
- correct integration of secure and plain API paths,
- platform-specific failure behavior.

---

## Replication boundary

`loxdb_replication` provides replication state logic.

It is not a full network stack and does not claim to provide:

- authenticated transport,
- encrypted transport,
- congestion control,
- peer discovery,
- consensus,
- cluster membership,
- Byzantine fault tolerance.

Transport security and peer authentication must be handled by the integrating product.

---

## OTA boundary

`loxdb_ota` provides planning and runtime flow boundaries for chunk/finalize-style update logic.

It does not replace:

- bootloader verification,
- firmware signing infrastructure,
- anti-rollback hardware counters,
- secure update distribution,
- complete device recovery strategy.

---

## Safety boundary

`loxdb_safety` provides runtime validation and evidence-friendly reporting.

It does not certify the final product by itself. Certification depends on the full product design, requirements, hazard analysis, traceability, verification evidence, and deployment context.
