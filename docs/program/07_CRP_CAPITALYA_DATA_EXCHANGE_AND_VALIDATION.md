# 07 — CRP ↔ capitalYA Data Exchange & Validation Architecture

| Field | Value |
|---|---|
| Purpose | Define the conventional, signed, event-driven integration between CRP and capitalYA, and own the **canonical JSON schemas** for the whole program. No blockchain in the credit-decision path for the MVP. |
| Audience | Backend/platform engineers (CRP + capitalYA), integration engineers, data, compliance |
| Status | Draft v1.0 |
| Version | 1.0.0 |
| Owner | Platform lead |
| Dependencies | `05_MVP_REGULATORY_BOUNDARY_STATEMENT.md` (binding), `02_INTEGRATED_ARCHITECTURE.md`; CRP `27_READINESS_REPORT_SPECIFICATION.md`, `33_DATA_MODEL_AND_ENTITY_RELATIONSHIPS.md`, `22_EVIDENCE_CONFIDENCE_MODEL.md`, `36_AUDIT_LOG_AND_EVENT_MODEL.md` |
| Canonical-schema rule | **All shared JSON schemas live here.** `06` and `08` reference these; they never redefine them. |
| Last updated | 2026-07-19 |

---

## 1. Design stance

- **Conventional first.** The MVP integration is a **signed API + event** exchange. No blockchain, oracle, or token infrastructure sits in the credit-decision path — that would add complexity without solving the near-term regulatory problem (`03_...` §; `01` two-horizon rule).
- **Three artifacts, never merged.** CRP produces a **readiness assessment**; capitalYA produces a **validation/eligibility result**; the lender produces the **credit decision**. Each is a separate, separately-owned object. capitalYA never overwrites the CRP score, and neither CRP nor capitalYA ever emits a credit decision.
- **capitalYA is a validation gate in the MVP, not a scorer.** With a single pilot lender there is no matching or routing and no capitalYA credit logic: capitalYA validates signature, schema, and completeness against the lender's intake checklist, then hands off. Scoring/routing is a Horizon-1-later addition when a second lender joins.
- **Schemas are versioned configuration**, mirroring how CRP ships its control library (CRP `00` §6.3) and how fees are configured (`08`).

## 2. End-to-end flow (MVP)

```
Borrower
   ↓
capitalYA intake
   ↓
Consent ledger record created  (scope + purpose + timestamp)
   ↓
Documents securely shared with CRP  (time-limited, access-controlled)
   ↓
CRP automated analysis  (extraction/verification — assistive AI, deterministic scoring)
   ↓
CRP human review + approval  (D-07)
   ↓
Assessment approved and SIGNED  → Readiness Assessment Package (RAP)
   ↓
CRP emits assessment.approved event  → capitalYA
   ↓
capitalYA verifies signature + schema + consent scope
   ↓
capitalYA validation/eligibility gate  (completeness vs. lender checklist; NO credit judgment)
   ↓
Application package → partner lender  (portal handoff, per 06)
   ↓
Lender conducts INDEPENDENT underwriting and decision
```

Nothing in this path writes to a blockchain. Payment/servicing data never flows back into CRP scoring (`05` §3.10).

## 3. Signing & verification

- CRP signs each RAP (detached signature over a canonical serialization). capitalYA verifies before acting; an invalid signature or schema version halts the handoff and raises an incident.
- The RAP is **immutable and versioned**; corrections produce a new versioned RAP referencing the prior `assessment_id`, never an in-place edit (CRP `35` retention).
- Every emit/verify is an audit event (CRP `36`).

## 4. Canonical schema registry

These are the authoritative shapes for the program. Field names are normative; illustrative values only.

### 4.1 CRP Readiness Assessment Package (RAP) — CRP → capitalYA

> **Boundary note:** the RAP recommends **remediations, never exposure.** There is deliberately **no** `maximum_recommended_exposure` or any amount/price/PD field — those would push CRP into underwriting. The `limitation_statement` is a **non-removable** field.

```json
{
  "assessment_id": "crp_asmt_123",
  "application_id": "cap_app_456",
  "subject_id": "org_789",
  "schema_version": "1.0",
  "methodology_version": "CRP-2026.1",
  "assessment_status": "approved",
  "overall_score": 78,
  "readiness_band": "conditionally_ready",
  "evidence_confidence_overall": 0.83,
  "dimensions": [
    { "dimension_code": "FINANCIAL_REPORTING", "score": 82, "confidence": 0.91, "material_flags": [] },
    { "dimension_code": "DEBT_CAPACITY", "score": 68, "confidence": 0.76, "material_flags": ["CUSTOMER_CONCENTRATION"] }
  ],
  "gating_results": {
    "legal_entity_verified": true,
    "beneficial_ownership_complete": true,
    "minimum_document_set_complete": true,
    "material_fraud_flag": false
  },
  "recommendation": {
    "status": "refer_to_lender",
    "required_remediations": [
      "Provide updated accounts receivable aging",
      "Resolve beneficial owner address discrepancy"
    ]
  },
  "evidence_manifest_hash": "sha256:…",
  "consent_record_id": "consent_987",
  "reviewed_by": "human_reviewer_reference",
  "approved_at": "2026-07-19T18:00:00Z",
  "expires_at": "2027-01-19T18:00:00Z",
  "limitation_statement": "The Capital Readiness Score measures how prepared this business is for capital review. It is not a credit score, a probability of default, a loan approval, an investment recommendation, or a guarantee of funding. Capital providers must perform their own independent underwriting, diligence, and approval process.",
  "signature": "…"
}
```

Notes: `overall_score` and `evidence_confidence_overall` are **separate axes** — confidence is never blended into the score (CRP `22`; `02` invariant). `recommendation.status` uses neutral routing values (`refer_to_lender`, `remediation_required`), never `approved`/`recommended`.

### 4.2 capitalYA validation/eligibility result — capitalYA (internal, shared with lender)

> **Boundary note:** `final_credit_decision` is **always `null`** here. In the MVP this object records only signature/schema/consent validity and completeness against the lender's checklist. No score, no credit rules.

```json
{
  "risk_evaluation_id": "risk_321",
  "source_assessment_id": "crp_asmt_123",
  "schema_version": "1.0",
  "lender_policy_version": "LENDER-A-2026.3",
  "signature_valid": true,
  "schema_valid": true,
  "consent_scope_valid": true,
  "completeness_status": "complete",
  "missing_items": [],
  "eligibility_status": "eligible_for_lender_review",
  "manual_review_required": true,
  "final_credit_decision": null
}
```

`eligibility_status` ∈ {`eligible_for_lender_review`, `returned_for_completion`}. `lender_policy_version` references the **lender's own** intake checklist — capitalYA does not author credit criteria (`05` §4).

### 4.3 KYC status object — granular, preliminary, non-relied

> **Boundary note:** never `kyc_passed: true`. capitalYA's results are `basis: "preliminary_non_relied"`; the lender re-performs KYC of record. A sanctions **match** is never auto-cleared (CRP `02` §2.7) — it escalates.

```json
{
  "kyc": {
    "basis": "preliminary_non_relied",
    "screening_status": "pre_screened",
    "provider": "provider_name",
    "completed_at": "2026-07-19T18:00:00Z",
    "identity_match": "confirmed",
    "beneficial_ownership_status": "complete",
    "pep_status": "no_match",
    "sanctions_status": "no_match",
    "sanctions_potential_match_escalated": false,
    "adverse_media_status": "review_required",
    "lender_reverification_required": true
  }
}
```

If `sanctions_status` would be anything other than `no_match`, the record sets `sanctions_potential_match_escalated: true` and routes to the lender MLRO; capitalYA makes no clearance determination.

### 4.4 Consent record — capitalYA

```json
{
  "consent_record_id": "consent_987",
  "subject_id": "org_789",
  "scope": ["share_with_lender:LENDER-A", "us_cloud_processing", "crp_assessment"],
  "purpose": "loan_application",
  "granted_at": "2026-07-19T17:55:00Z",
  "expires_at": "2027-01-19T17:55:00Z",
  "withdrawable": true,
  "language": "es-CR"
}
```

Cross-border/US-cloud consent is explicit (`03_...` §9.1; CRP D-23). Withdrawal revokes document links (`06` §7).

### 4.5 Event envelope — CRP/capitalYA/lender

```json
{
  "event_id": "evt_…",
  "event_type": "assessment.approved",
  "occurred_at": "2026-07-19T18:00:00Z",
  "producer": "crp",
  "idempotency_key": "crp_asmt_123:approved",
  "schema_version": "1.0",
  "payload_ref": "crp_asmt_123",
  "signature": "…"
}
```

Event types (MVP): `assessment.approved`, `assessment.revised`, `application.ready_for_lender_review`, `decision.recorded`, `terms.recorded`, `disbursement.recorded`, `assignment.notice_dispatched` (factoring perfection — carries notice hash + delivery evidence, `06` §8.2), `servicing.event`. All state-changing consumers are **idempotent on `idempotency_key`** (`06` §8.1).

### 4.6 Fee configuration — canonical shape (used by `08`)

Kept here per the single-source rule; `08` owns the **policy and catalogue** and references this shape.

```json
{
  "fee_code": "CAPITALYA_SUCCESS_FEE",
  "version": "1.0",
  "calculation_method": "percentage",
  "calculation_base": "funded_principal",
  "rate_basis_points": 0,
  "minimum_amount": null,
  "maximum_amount": null,
  "currency": "USD",
  "payer": "lender",
  "earned_event": "loan_disbursed",
  "deduct_from_proceeds": false,
  "counts_toward_interest_cap": false,
  "effective_from": "2026-09-01"
}
```

`rate_basis_points: 0` is deliberate — the schema is locked before the commercial rate. Per `21` D-P1 (B2B-only billing): for any **mandatory** capitalYA/CRP fee, `payer` is constrained to `"lender"` — `"borrower"` is rejected by schema validation in Phase 1/2; `deduct_from_proceeds` is **forced `false`** (`08`); `counts_toward_interest_cap` defaults `false` for lender-paid platform fees but the field is retained — the borrower-side cap test covers the lender's own charges (`08` §4), and counsel must confirm the anti-evasion treatment.

## 5. Oracle & blockchain design (deferred; no PII ever)

- **MVP:** the blockchain receives **nothing**. No scores, no PII, no documents.
- **Horizon 2 (gated):** an oracle may publish only a **commitment/attestation** — never underlying data:

```json
{
  "asset_id": "loan_pool_123",
  "assessment_commitment": "0x…",
  "eligibility_attestation": true,
  "methodology_version_hash": "0x…",
  "valid_until": 1790000000
}
```

- **Never published on-chain:** borrower names, ID numbers, financial statements, full CRP reports, raw scores, loan documents, KYC records, beneficial-owner details (`03_...` §9.1; `02` §8).
- **Future ZK proofs** may assert qualifying results without exposing data — e.g., "score ≥ approved threshold," "required documents verified," "entity not sanctioned," "debt-service ratio within range." A proof reduces disclosure; it does not legalize source data or replace consent (`03_...` §9.1).

## 6. Versioning, validation & failure handling

- Every schema carries `schema_version`; `methodology_version` pins the scoring model for reproducibility (CRP `20` §10).
- Consumers **reject unknown/incompatible versions** rather than best-guess.
- Signature/schema/consent failures halt the handoff and raise an incident (CRP `45`).
- RAP corrections are new versions; the borrower correction/appeal path is CRP `57`.
- Backward-compatible changes bump minor; breaking changes bump major and require coordinated deployment.

## 7. What lives where (anti-duplication)

| Artifact | Canonical home |
|---|---|
| RAP, validation result, KYC, consent, event, fee-config **schemas** | **This doc (07)** |
| Portal, state machine, settlement, SLAs, dispute routing | `06` |
| Fee **policy & catalogue** (numbers, governance) | `08` |
| Decision-rights, AML split, funds-plane narrative | `02` |
| Regulatory rationale for every boundary | `03`, `05` |
