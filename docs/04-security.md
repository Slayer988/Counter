# 04 · Security, Access Control and Threat Model

**Traces:** §4, §4.1, §11, IAM-001–008, SEC-001–012, NOT-003, PUB-006

## 1. Access model

Access is decided per request as **role permission ∧ scope match ∧ SoD rules ∧ time window ∧ (step-up if high risk)**.

- **Role grant** = `(user, role, scope[], valid_from, valid_to, granted_by, approved_by)`.
- **Scope** = any combination of `cycle`, `contest`, `geography_node` (including descendants), `tally_centre` and `voting_event`.
- The policy is evaluated in the API (CASL-style ability builder) and again in PostgreSQL row-level security as a backstop.

## 2. Role × permission matrix

Legend: ● allowed within scope · ○ read-only · – none · ⚑ dual control (second approver required)

| Permission | EC admin | Nat. supervisor | Returning officer | Presiding officer | Tally operator | Verifier | Approver | Legal | Auditor | Agent / observer | Security admin | Records mgr | Helpdesk |
|---|---|---|---|---|---|---|---|---|---|---|---|---|---|
| Configure cycle / contest / rule pack | ⚑ | ○ | ○ | – | – | – | – | ○ | ○ | – | – | – | – |
| Import geography / candidates | ⚑ | ○ | ○ | – | – | – | – | – | ○ | – | – | – | – |
| Capture / submit result | – | – | – | ● | ● | – | – | – | – | – | – | – | – |
| Verify result | – | – | ● | – | – | ● | – | – | – | – | – | – | – |
| Approve result / close exception | – | ● | ● | – | – | – | ● | – | – | – | – | – | – |
| Approve correction | – | ⚑ | ⚑ | – | – | – | ⚑ | ○ | ○ | – | – | – | – |
| Approve / publish / withdraw release | ⚑ | ⚑ | – | – | – | – | – | ○ | ○ | – | – | – | – |
| Record official declaration | ⚑ (designated officer) | – | ⚑ (own contests) | – | – | – | – | ○ | ○ | – | – | – | – |
| View internal dashboards | ● | ● | ● | ○ own event | ○ | ○ | ○ | ○ | ○ | – | – | – | – |
| View evidence | ○ | ○ | ○ | ○ own | ○ | ○ | ○ | ○ case | ○ | per EC policy | – | – | – |
| View audit log | – | – | – | – | – | – | – | ○ case | ● | – | ○ security events | – | – |
| Legal hold / retention | – | – | – | – | – | – | – | ● request | – | – | – | ● | – |
| Manage users / devices | ⚑ grants | – | – | – | – | – | – | – | ○ | – | ● technical | – | ○ |
| Change result values outside workflow | – | – | – | – | – | – | – | – | – | – | – | – | – |

**[v1.1] Additional roles:** *Sub-county results centre officer* can record custody events (receive, hand over) for envelopes in scope but cannot open envelopes or capture results from them (CUS-003). *Tally-centre display* is a non-interactive device account that renders only the data that centre may show (CUS-006).

The last row is deliberately empty for every role. No role, including database administrators acting through the application, can change result values outside the workflow (§4.1).

## 3. Segregation-of-duties rules (enforced in code and tested)

| ID | Rule | Enforcement |
|---|---|---|
| SoD-1 | Capturer ≠ verifier ≠ approver for the same `result_version` | Checked at transition; `403 sod-violation` (VER-001, UAT-006) |
| SoD-2 | Correction initiator ≠ correction approver | Correction API |
| SoD-3 | Release approvals need N distinct approvers (default 2), none of whom created the candidate | Release gate |
| SoD-4 | A user cannot hold both a *capture* and a *publish* role in the same cycle | Grant API blocks; an exception needs a dual-approved waiver with expiry (IAM-005) |
| SoD-5 | Security admin cannot hold any results role | Grant API |
| SoD-6 | Nobody can approve their own role grant | Grant API |
| SoD-7 | Break-glass account use requires two people (split credential) and auto-expires after 4 h | Keycloak + alert (IAM-004, SEC-005) |
| SoD-8 | Helpdesk impersonation is not available; support sessions are "view as" read-only, time-bound and case-linked | §4.1 |

An emergency waiver of SoD-1 (for example, a single officer at a remote tally centre) is a configured, time-boxed, dual-approved exception that marks every affected record with `sod_waiver_id` so the waiver is visible on dashboards and audit.

## 4. Authentication and session

- Named accounts only (IAM-001). MFA is mandatory for every internal role: WebAuthn preferred, TOTP as fallback, and SMS OTP only for field roles if the EC approves it (IAM-003).
- Idle timeout is 15 min for internal roles, with an absolute maximum of 8 h. The field PWA uses a 12 h offline credential bound to the device key.
- Step-up re-authentication is required before high-risk actions (IAM-008).
- Revoking an account or device kills its sessions within 60 s via a token revocation list and short-lived (5 min) access tokens (IAM-006, UAT-016).

## 5. Cryptography and keys (SEC-001–003)

| Key | Holder | Use | Rotation |
|---|---|---|---|
| TLS certificates | Edge | Transport | 90 days, automated |
| Device key (Ed25519) | Device secure storage (WebCrypto non-extractable) | Signs submissions | Per enrolment, revocable |
| Receipt signing key | Vault Transit | Signs receipts | Per cycle |
| Release signing key | HSM / Vault, dual-control | Signs manifests | Per cycle; public key published |
| Offline package key | Vault | Signs + encrypts packages | Per package |
| Data-at-rest keys | DB / object store KMS | Encryption at rest | Annual + on incident |

## 6. Application security controls (SEC-004, SEC-007, SEC-008)

- **Input:** validation via shared zod schemas. Parameterised SQL only (ORM/query builder). Output encoding and strict CSP with no inline scripts.
- **CSRF:** SameSite=strict cookies plus a CSRF token on the internal web. The sync API is token-only.
- **SSRF:** no user-supplied URLs are fetched. Egress is allowlisted.
- **Uploads:** type sniffing, size cap (default 15 MB/page), ClamAV scan before `finalize`, storage in a non-executable bucket, and content served with `Content-Disposition: attachment` from a separate origin.
- **Supply chain:** lockfiles, Renovate, SCA scanning in CI, SBOM per release, signed container images (cosign), and reproducible builds where feasible.
- **Pipeline:** SAST (CodeQL/Semgrep), DAST (ZAP) against UAT, and independent penetration testing before each cycle (SEC-011).

## 7. Privacy (§11.3)

- The system has no voter-level data and no voter-to-ballot linkage. There is no table for either.
- Delegate rosters live in the `restricted` schema. Public and reporting roles have no grants on it, and public artefacts carry aggregate counts only (CON-008).
- Staff phone numbers, signatures and ID documents are column-level encrypted and visible to scoped roles only.
- Notification templates go through a privacy lint that rejects any template variable typed as a vote count, signature or personal data (NOT-003).
- A privacy impact assessment is a G1 gate deliverable.

## 8. Threat model (STRIDE, mapped to SRS §11.4)

| # | Threat | STRIDE | Target | Primary mitigations | Test |
|---|---|---|---|---|---|
| T1 | Insider alters a stored count | T, R | `result_line` | Append-only triggers, INSERT-only DB role, hash chain, versioned corrections with dual approval | UAT-009, tamper test |
| T2 | DBA modifies data directly | T | Database | Separate migration role, pgAudit to SIEM, daily audit anchor in WORM, snapshot recompute check | Chain verification job |
| T3 | Forged submission from network or stolen device | S, T | Sync API | mTLS/device signature, revocation, package expiry, form-serial duplicate check | UAT-016 |
| T4 | Replay / duplicate submission | T | Capture | Idempotency key + `clientTxnId` + unique `(contest_event, form_serial)` | UAT-007 |
| T5 | Fake or reused evidence image | S | Evidence | sha256 duplicate detection across cycle, EXIF/time checks, verifier comparison | UAT-005 |
| T6 | Malicious configuration (wrong candidates or formula) | T | Rule packs | Dual approval, test fixtures, config snapshot hash pinned in every submission | ADM-006 |
| T7 | Premature or unauthorised publication | E, I | Release | Gates, 2+ approvers, step-up MFA, publisher is the only writer to the public store | UAT-010/012 |
| T8 | Data leakage via public API | I | Public | Allowlist schemas, separate zone, privacy scan in CI | PUB-006 |
| T9 | DDoS on portal | D | Public | CDN static artefacts, WAF, rate limits, isolated from tally | UAT-017 |
| T10 | Ransomware | D, T | Infra | Segmentation, immutable/offline backups, restore drills | UAT-015 |
| T11 | Account takeover | S, E | IAM | MFA/WebAuthn, anomaly alerts, short tokens, geo/time-bound grants | IAM-007 |
| T13 | Envelope substitution or opening before the authorised level | T | Custody | Seal numbers recorded at sealing, checked at each handover; opening rule enforced in software; anomaly exceptions | CUS-002–004 |
| T14 | Divergence between UETVA and ERTDS figures | T, R | Integrations | Signed exchange files, reconciliation report, no silent overwrite | §10.1 |
| T12 | Misleading presentation (partial read as final) | — (integrity of information) | Portal | Mandatory status banner, denominator, timestamp, no "winner" wording | §8.6, R-07 |
