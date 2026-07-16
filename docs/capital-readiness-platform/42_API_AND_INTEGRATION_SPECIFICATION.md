# 42 — API and Integration Specification

| Field | Value |
|---|---|
| Purpose | API design standards, resource map, representative endpoint contracts, and external-integration principles |
| Audience | Backend/frontend engineers, future integration partners |
| Status | Draft v1.0 |
| Version | 1.0.0 |
| Owner | Backend lead |
| Dependencies | 41 (modules), 12 (authz), 14 (states), 34 (enums) |
| Source references | CRF §17 (integrations), §17.1 (principles) |
| Assumptions | REST+JSON internal API; OpenAPI-first; external partner API is post-MVP |
| Open questions | — |
| Approval required | Backend lead |
| Last updated | 2026-07-16 |

## 1. Standards

- OpenAPI 3.1 spec is the contract; generated typed clients; breaking changes = new version path (`/v1/`).
- AuthN: OIDC bearer tokens (43); every request resolved to `{user, org, role, scopes}`; object-level authz in modules.
- Idempotency: all POSTs accept `Idempotency-Key` (NFR-19); mutations return the updated resource + emitted event names.
- Errors: RFC 7807 problem+json with stable `code` (maps to UI states 18 §1); validation errors field-keyed.
- Pagination: cursor-based; filtering via documented query params; all list endpoints scoped by tenancy automatically.
- Every response containing score/confidence data embeds `limitation_statement` object (copy key + version) so clients can never render numbers without it (02 §3).

## 2. Resource map (v1, internal SPA-facing)

```
/applications                     POST (public+captcha) · GET/PATCH own · admin queue GET
/applications/{id}/decision      POST (R-AR; body: accept|reject|waitlist|rfi + reason)
/orgs /orgs/{id}/members /invitations /grants
/workspaces/{id}/profile|ownership|capital-request|checklist|tasks
/workspaces/{id}/documents       POST multipart → {doc_id, scan_status}
/documents/{id}                  GET meta · /versions · /content (→ signed URL) · /feedback (AI result)
/documents/{id}/verify|reject    POST (reviewer roles)
/clarifications                  CRUD per role (15 FR-REV-02)
/assessments/{id}                GET state+summary (role-filtered view model)
/assessments/{id}/controls       GET grid · PATCH maturity {control, maturity|na_reason, evidence_refs}
/assessments/{id}/gates|flags|overrides|exceptions    per 23
/assessments/{id}/transitions    POST {transition} (workflow-validated)
/assessments/{id}/result         GET (role-filtered: borrower sees approved only — ASM-16)
/assessments/{id}/report         GET artifact refs (27)
/opportunities                   POST (from approved assessment) · GET (role-scoped)
/opportunities/{id}/package|publish|grants|rfis|eois|outcome
/providers/{id}/profile|mandate
/consents                        POST/DELETE (R-BA) · GET ledger
/config/scoring-models           GET/POST draft · POST {v}/publish (dual-control)
/config/overlays                 same pattern
/audit/events                    GET (scoped) · POST /export-requests (dual-approval)
/notifications /preferences
```

## 3. Representative contracts

**PATCH /assessments/{id}/controls** — body `{control_id, maturity: 0..4 | null, na_reason_code?, evidence_refs: [], note?}`. 403 unless caller's role owns the dimension (12 §2); 409 if control inactive in pinned profile; 422 if maturity ≥3 without E2+ evidence where control marked material (21 §2). Emits `control.assessed`; triggers provisional recompute (internal-only result version).

**POST /assessments/{id}/transitions** — body `{transition: "approve", conditions?: [...], note}`. Workflow validates actor/preconditions/blockers (14 §3); segregation check (12 §6); on success returns new state + result release info; emits lifecycle event; 409 with machine-readable blocker list otherwise (UI renders unblocking path).

**POST /opportunities/{id}/publish** — validates: R-SA approval present, consent records for each grantee, disclosure lint clean (23 §6), assessment unexpired, fresh sanctions re-screen timestamp (ASM-07). Creates grants with expiry (ASM-12).

**GET /assessments/{id}/result** — role-filtered projection: borrower view omits restricted flags/internal notes/pre-cap values; provider view only via opportunity endpoints. Includes `{system_total?, published_total, band, dimensions[], confidence, caps[], conditions[], external_risk_context, model_version, valid_until, limitation_statement}`.

## 4. Webhooks / events (internal)

Outbox → bus topics mirror audit names (36 §2); consumers: notifier, projections, analytics exporter. No external webhooks in MVP; provider-facing API/webhooks are post-MVP (80) behind partner agreements.

## 5. External integrations (Phase-4 posture; CRF §17)

Integration domains and example vendors per CRF §17 (registries/OpenCorporates/GLEIF; KYC: Trulioo/Sumsub/Persona/Veriff; sanctions: official lists + ComplyAdvantage/World-Check-class; open finance: Belvo/Prometeo; accounting: QuickBooks/Xero/…; doc AI: Google/AWS/Azure; e-sign: DocuSign/Adobe/local qualified; bureaus per-country with consent; valuation/appraiser networks; macro/climate sources). Binding principles (CRF §17.1):
1. API data never automatically outranks signed/audited evidence — conflicts create exceptions (ENT-15).
2. Every material field keeps source, timestamp, consent basis, transformation history, reviewer status (30 §6).
3. Least-privilege tokenized credentials, rotation, vendor-risk review (47), retention controls.
4. **Manual fallback is a design requirement** — many LatAm registries/institutions lack reliable APIs; document-based workflow is the MVP baseline, integrations are accelerators (CRF §17.1: don't gate launch on open banking).
5. "Supported" claims only after country coverage + lawful-use validation (47 checklist).
