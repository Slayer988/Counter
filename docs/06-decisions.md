# 06 · Architecture Decision Records and EC Decision Register

## Part A — Architecture decision records

| ADR | Decision | Status | Why / alternatives |
|---|---|---|---|
| ADR-001 | TypeScript monorepo (pnpm + Turborepo): `apps/api`, `apps/web`, `apps/field`, `apps/portal`, `packages/rules`, `packages/schemas`, `packages/ui` | Accepted | Shared validation and rule engine on client and server. Alternatives: Python/Java backends would split type definitions |
| ADR-002 | Modular monolith (NestJS), not microservices, for phase 1–3 | Accepted | Smaller ops surface for an EC team. Module boundaries allow later extraction (NFR-010) |
| ADR-003 | PostgreSQL + PostGIS as the single system of record; append-only enforced by triggers + grants | Accepted | Strong transactions, RLS and spatial support. Event-store databases were rejected as an extra operational burden |
| ADR-004 | Separate processing state from legal status (two state machines) | Accepted | REL-004, TAL-008 |
| ADR-005 | Declarative JSON rule packs evaluated by a sandboxed integer engine; no user-authored code | Accepted | Auditable, testable and versioned (VAL-003, TAL-004) |
| ADR-006 | Public portal and API served only from signed static release artefacts in an isolated zone | Accepted | Leak-proof by construction, cheap to scale (PUB-001, UAT-017) |
| ADR-007 | Offline field client as a PWA (optionally wrapped with Capacitor for MDM) | Proposed | Needs device inventory confirmation (D-10). A native Android app is the alternative if the EC mandates MDM-only devices |
| ADR-008 | Hash-chained audit log with daily anchors to WORM storage and SIEM | Accepted | SEC-006 tamper evidence without a blockchain |
| ADR-009 | RFC 8785 canonical JSON + SHA-256 for all content hashes; Ed25519 for device signatures | Accepted | Deterministic across client and server |
| ADR-010 | Keycloak as default IdP, federating to an EC IdP if one exists | Proposed | Depends on D-01 |
| ADR-011 | MapLibre + PMTiles generated per geography version | Accepted | No licensed basemap. Tiles version-pinned to contest geography (VIS-001) |
| ADR-013 | Model physical chain of custody (envelopes + custody events) separately from result versions; capture from an envelope only at its opening level | Accepted | SRS v1.1 CUS-001–008; reflects the EC rule that envelopes open only at district tally centres |
| ADR-014 | Treat ERTDS and BVVK as external systems integrated by signed, reconciled import/export, never by direct writes | Proposed | Depends on D-16/D-17 |
| ADR-015 | Bulk generation of contests and events from versioned templates | Accepted | 45,505 elective positions and 71,214 villages in 2025/2026 (§2.5) |
| ADR-012 | Percentages computed at display time from integer counts; missing values `null`, never 0 | Accepted | VAL-008, §8.6 |

New ADRs go in `docs/adr/NNN-title.md` using the template: Context → Decision → Consequences → Requirement IDs.

## Part B — EC decision register (from SRS §18.1 and §3.1)

Design proceeds on the **working assumption** shown. Each item must be confirmed by the named owner before the dependent phase starts.

| ID | Question | Working assumption | Owner | Blocks phase | Status |
|---|---|---|---|---|---|
| D-01 | Which institution owns and operates the platform? Who approves each contest configuration? | EC ICT operates; EC sponsor approves configuration | EC sponsor | 1 | Open |
| D-02 | Which source record is authoritative: signed paper form, electronic record or both? | Signed paper form (DR form) is authoritative; electronic capture and image are copies with provenance | EC legal | 2 | Open |
| D-03 | What is the legally approved status vocabulary and release authority per contest? | Appendix B vocabulary; release authority = EC Chairperson or delegate | EC legal | 3 | Open |
| D-04 | Exhaustive list of offices, SIG categories, delegate groups and methods | Config catalogue seeded from the 2026 roadmap; all treated as data | EC operations | 1 | Open |
| D-05 | Official forms, signatures, seals, fields and arithmetic for every contest | Placeholder form templates; arithmetic rules marked *unconfirmed* | EC operations + legal | 2 | Open |
| D-06 | Authoritative geographic codes, boundaries, station lists and college/conference registers | EC master data import; UBOS boundaries for geometry if licensed | EC + UBOS | 1 | Open |
| D-07 | Which fields may be published at station / constituency / district / national level, and when? | Station-level public detail **off** by default; enabled per contest by policy (VIS-008) | EC sponsor | 4 | Open |
| D-08 | Retention, legal hold, data-subject and archival requirements | Retain all result/audit/evidence data until EC records policy; no disposal job enabled | EC records + legal | 3 | Open |
| D-09 | Hosting, data residency, connectivity, devices and DR | In-country hosting (EC DC or NITA-U), two sites | EC ICT | 1 | Open |
| D-10 | Actual counts of events, users, submissions, evidence sizes and peak traffic | 2025/2026 baseline: 50,739 polling stations, 71,214 villages, 353 constituencies, 146 districts/cities, 10,717 parishes/wards, 45,505 elective positions, 83,597 candidates, 21.7 M voters; 10k internal users, 50k concurrent public sessions, ~3 MB evidence/form | EC operations | 5 | Open |
| D-11 | Legal standing of electronic copies, images and signed system records | Electronic artefacts are supporting records only | EC legal | 3 | Open |
| D-12 | Is independent double entry required, and for which contests or samples? | Required for presidential; sampled (10%) elsewhere | EC operations | 2 | Open |
| D-13 | May SMS OTP be used for field MFA? | Allowed only for field roles, WebAuthn/TOTP elsewhere | EC ICT security | 1 | Open |
| D-14 | Is provisional publication permitted before declaration, and with what label? | Allowed with a prominent *Provisional — not an official declaration* banner | EC sponsor + legal | 4 | Open |
| D-15 | Which local elections use line-up voting vs secret ballot; are EC10/EC11 captured centrally? | LC I chairperson and village women's committees by line-up; captured at sub-county from paper | EC operations + legal | 2 | Open |
| D-16 | Replace, feed or consume ERTDS? Which figures are authoritative at the national tally centre? | UETVA feeds or consumes ERTDS via signed files; ERTDS stays authoritative for presidential transmission until the EC decides | EC ICT + sponsor | 3 | Open |
| D-17 | May BVVK scans of DR forms be primary evidence? | Supporting evidence only; tally-centre scan of the opened form is primary | EC legal | 2 | Open |
| D-18 | Envelope opening, re-sealing and custody rules per level; what counts as a seal anomaly | Open only at district tally centre; any seal-number mismatch is an anomaly | EC operations | 2 | Open |
| D-19 | Statutory declaration deadlines per contest type | Presidential 48 h from poll close; others configurable | EC legal | 3 | Open |
| D-20 | What may the tally-centre display show before declaration? | Opened and verified station results for that centre's contests | EC sponsor | 4 | Open |

## Part C — Implementation phase plan (Appendix E, refined)

| Phase | Build scope | Exit evidence |
|---|---|---|
| 1 Foundation | Monorepo, CI/CD, IAM + SoD engine, audit chain, config (cycle → contest), geography import, events, participants | Access-control and config acceptance tests; UAT-001/002/003 config parts |
| 2 Capture / verification | Form renderer, evidence service, rules engine, field PWA + sync, verification, exceptions, corrections | Field simulation; UAT-004–009, 016 |
| 3 Tally / release | Aggregation snapshots, lineage, completeness, release gates, manifests, publisher | UAT-010, 013, 018 |
| 4 Visualisation | Internal dashboards, public portal, map drill-down, public API, exports | UAT-011, 012, 017; WCAG audit |
| 5 Hardening / pilot | Pen test, DR drills, load tests, training environment, dress rehearsal | G4–G6 |
| 6 Rollout | Operations, support, archive mode | G7, post-election report |
