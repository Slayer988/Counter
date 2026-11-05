# Counter — Uganda Election Tallying and Visualisation Application (UETVA)

A secure, auditable platform for capturing, verifying, aggregating, releasing and visualising Ugandan election results across all contest types (presidential, parliamentary, SIG, local government, referenda), based on the *UETVA Software Requirements Specification v1.0 (4 Nov 2026)*.

> **Authority statement.** This software supports the lawful process. It does not declare or certify winners. Declaration and certification remain with the Electoral Commission and other competent authorities. No production use until the EC and legal counsel validate rules, forms and procedures.

## Status

**Design phase.** No application code yet.

## Design documents

| # | Document | Covers |
|---|---|---|
| 01 | [Architecture overview](docs/01-architecture.md) | Drivers, stack, C4 context/containers, modules, environments, hosting, DR |
| 02 | [Domain and data model](docs/02-domain-data-model.md) | ERD, state machines, immutability, rule packs, hashing |
| 03 | [API contracts](docs/03-api-contracts.md) | Internal, field-sync and public APIs; idempotency; signed receipts |
| 04 | [Security](docs/04-security.md) | Role matrix, segregation of duties, crypto, threat model |
| 05 | [Offline sync](docs/05-offline-sync.md) | Offline packages, outbox states, sync and conflict protocol |
| 06 | [Decisions](docs/06-decisions.md) | ADRs, EC decision register, phase plan |

## Planned stack

TypeScript monorepo · NestJS · PostgreSQL 16 + PostGIS · Next.js (internal web, public portal) · offline PWA field client · Keycloak · MinIO (WORM) · Redis · MapLibre.
