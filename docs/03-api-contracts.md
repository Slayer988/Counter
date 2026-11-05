# 03 · API Contracts (outline)

**Traces:** §10, TRN-001–008, PUB-001–007, NFR-009, REL-*, CAP-*, VER-*

Three separately deployed APIs, each with its own OpenAPI 3.1 document under `api/` (to be generated from NestJS decorators):

| API | Base path | Consumers | Auth |
|---|---|---|---|
| Internal | `/api/v1` | Internal web | OIDC access token + session; step-up MFA for high-risk actions |
| Field sync | `/sync/v1` | Field PWA | OIDC token **plus** device certificate (mTLS or signed request) |
| Public | `/public/v1` | Anyone | None (rate-limited); optional API keys for higher quotas |

## 1. Common conventions

- JSON over HTTPS (TLS 1.2+ with 1.3 preferred), UTF-8, field names in `camelCase`, IDs as ULIDs.
- `X-Correlation-Id` is accepted or generated and echoed on every response.
- Errors use RFC 9457 `application/problem+json`, e.g. `type: /errors/sod-violation`.
- Mutating calls require an `Idempotency-Key` header (UUIDv7). The server stores `(key, actor, request hash) → response` for 30 days. A repeat with the same hash returns the original response; a repeat with a different hash returns `409 idempotency-mismatch` (TRN-002, NFR-013, UAT-007).
- Optimistic concurrency uses `If-Match: <version>` for configuration edits. Result values are never updated in place.
- High-risk endpoints (approve, publish, withdraw, correction approval, rule activation, role grant) require a fresh MFA step-up whose `acr` claim is ≤5 minutes old (IAM-008).

## 2. Internal API — main resources

| Area | Endpoints (abridged) | Notes |
|---|---|---|
| Config | `POST /cycles`, `POST /elections`, `POST /contests`, `POST /contests/{id}:submit-review`, `:approve`, `:activate`, `GET /contests/{id}/activation-checklist` | Activate fails with a list of missing items (ADM-004) and returns a config snapshot hash (ADM-006) |
| Rule packs / forms | `POST /rule-packs`, `POST /rule-packs/{id}:test`, `:approve` | `:approve` requires 2 distinct approvers |
| Geography | `POST /geography-versions:import` (async job), `GET /geography-versions/{v}/nodes?parent=`, `GET /readiness/gaps` | Import produces a validation report (GEO-008, §9.1) |
| Events | `POST /voting-events`, `POST /voting-events/{id}:relocate|split|merge|close` | Always creates links (GEO-006) |
| Participants | `POST /participants`, `PUT /contests/{id}/participants` (pre-freeze only), `POST /contests/{id}/participants:freeze` | |
| Results | `POST /results/{contestEventId}/versions`, `GET /results/{id}?include=versions,evidence,validations,verifications,history` | Tally-centre capture uses the same model as sync |
| Evidence | `POST /evidence:upload-url` → presigned PUT, then `POST /evidence/{id}:finalize` (server re-hashes and scans) | SEC-008 |
| Verification | `GET /verification/queue`, `POST /result-versions/{id}/verifications` (field findings) | 403 `sod-violation` if verifier = capturer (VER-001) |
| Exceptions | `GET /exceptions?status=&type=&scope=`, `POST /exceptions/{id}:assign|escalate|close` | Close requires disposition, reason, evidence and approval (EXC-008) |
| Corrections | `POST /results/{id}/corrections` → creates a pending CorrectionEvent; `:approve` by a different user | EXC-004, UAT-009 |
| Tally | `GET /contests/{id}/tally?level=&node=&basis=approved|verified|received`, `GET /snapshots/{id}/lineage`, `POST /snapshots/{id}:recompute-check` | Response always includes `basis`, `completeness {expected, received, verified, approved}` and `computedAt` |
| Release | `POST /releases` (candidate), `GET /releases/{id}/gates`, `POST /releases/{id}/approvals`, `:publish`, `:withdraw`, `POST /releases/{id}/legal-status` | Gate list mirrors Appendix C |
| Audit | `GET /audit/events?object=`, `GET /audit/verify?from=&to=` | Auditor role only |
| IAM | `POST /users`, `POST /users/{id}/grants`, `POST /devices`, `:revoke` | Grant API runs SoD checks (IAM-005) |
| Reports | `POST /exports` (async) → `GET /exports/{id}` (expiring signed URL) | Logged with purpose (§14) |

## 3. Field sync API

| Endpoint | Purpose |
|---|---|
| `POST /sync/v1/devices:enroll` | Enrol a device using a one-time code issued by an admin. Returns the device certificate (SEC-009) |
| `GET /sync/v1/package` | Signed, encrypted offline package for the user's scope, with expiry (OFF-001/002) |
| `POST /sync/v1/batches` | Upload ordered submissions + evidence manifests (see 05) |
| `GET /sync/v1/batches/{id}` | Per-item receipts and states |
| `GET /sync/v1/status` | Config version currency, revocation flag, server time (for drift) |

### Submission payload (abridged)

```json
{
  "clientTxnId": "01JC…", "deviceSeq": 42,
  "contestEventId": "…", "formTemplate": {"code": "PRES_DR_FORM", "version": 2},
  "rulePackVersion": 3, "configSnapshotHash": "sha256:…",
  "formSerial": "A123456",
  "lines": {"participant:01H…": 312, "participant:01J…": 1044, "VALID_VOTES": 1356, "INVALID_VOTES": 21},
  "evidence": [{"sha256": "…", "mediaType": "image/jpeg", "page": 1}],
  "capturedAt": "2026-01-15T16:42:10+03:00",
  "deviceSignature": "base64(ed25519(JCS(payload)))"
}
```

### Signed receipt (TRN-003)

```json
{
  "receiptId": "…", "clientTxnId": "01JC…", "resultVersionId": "…",
  "state": "RECEIVED", "payloadHash": "sha256:…",
  "serverReceivedAt": "2026-01-15T13:42:13Z",
  "validation": {"blocking": 0, "warnings": 1},
  "serverSignature": "base64(…)", "keyId": "prod-receipt-2026-01"
}
```

The client verifies `payloadHash` and the signature before marking an item *acknowledged*.

## 4. Public API (read-only, served from the public release store)

| Endpoint | Returns |
|---|---|
| `GET /public/v1/cycles` | Released cycles |
| `GET /public/v1/contests?cycle=` | Contests that have at least one release |
| `GET /public/v1/contests/{id}/results?node=` | Latest release for that scope: totals, `legalStatus`, `releaseId`, `publishedAt`, completeness and methodology link |
| `GET /public/v1/contests/{id}/results/children?node=` | Child-area results for drill-down (station level only if policy allows, VIS-008) |
| `GET /public/v1/releases/{id}` | Manifest + signature (anyone can verify) |
| `GET /public/v1/releases/{id}/history` | Superseded and withdrawn chain with notices (REL-007, PUB-007) |
| `GET /public/v1/geography/{version}/tiles/{z}/{x}/{y}` | Vector tiles |
| `GET /public/v1/downloads/{releaseId}.csv` | Bulk data. Header rows include release ID, status, timestamp, scope and methodology (§8.6) |

The response schema is a **field allowlist** generated from the publication policy (REL-005, PUB-006). Contract tests fail the build if any non-allowlisted field appears. Responses set `Cache-Control` with ETag and are served from CDN.

## 5. Rate limits (PUB-005, TRN-008)

| Surface | Default |
|---|---|
| Public, anonymous | 60 req/min/IP at the edge, burst 120; CDN absorbs repeat reads |
| Field sync | 30 batches/min/device, exponential backoff hints via `Retry-After` |
| Internal | 600 req/min/user; exports limited to 5 concurrent jobs/user |
