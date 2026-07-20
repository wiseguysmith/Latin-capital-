# 06 — Partner Lender Portal & Integration Specification

| Field | Value |
|---|---|
| Purpose | Define how a licensed partner lender receives borrowers, performs its own credit decisions, and settles loans through the capitalYA program — via a **portal (MVP)** with a **phased API**. Keeps every legally-meaningful action owned by the lender. |
| Audience | Platform/integration engineers, product, compliance, the partner lender's operations & technical staff |
| Status | Draft v1.0 — subject to the partner lender agreement (`04`) and counsel validation (`03_...` §17) |
| Version | 1.0.0 |
| Owner | Platform lead + Head of Product |
| Dependencies | `05_MVP_REGULATORY_BOUNDARY_STATEMENT.md` (binding), `07_...` (canonical schemas), `08_...` (fees), `02_INTEGRATED_ARCHITECTURE.md`, `04_ENTITY_AND_PARTNER_STRUCTURE.md`; CRP `14_WORKFLOW_STATES_AND_TRANSITIONS.md`, `42_API_AND_INTEGRATION_SPECIFICATION.md`, `36_AUDIT_LOG_AND_EVENT_MODEL.md` |
| MVP scope decisions | Single partner lender; **portal-first** surface; capitalYA holds no funds and makes no credit decision |
| Last updated | 2026-07-19 |

---

## 1. Scope and the integration-surface decision

This spec supports **one licensed partner lender** for the Q3 2026 closed pilot. Two decisions shape everything below:

- **Direction:** capitalYA is the **canonical provider** of the interface. capitalYA exposes the portal (and, later, the API); the lender consumes them. A "lender-provides" adapter is possible but is an appendix, not the default (§12).
- **Surface:** **portal-first.** The pilot runs on a capitalYA-provided **web portal** through which the lender's staff perform and record their own decisions. The full API (§11) is specified but is **phase-2** — not a launch dependency, so a supervised lender with no engineering team can still be the partner.

**Non-negotiable (from `05`):** capitalYA never holds funds and never emits a credit decision. The portal is a workflow and record surface for the lender; it is not a lending system operated by capitalYA.

## 2. Surface roadmap

| Capability | Portal (MVP, Q3 2026) | API (Phase 2) |
|---|---|---|
| Lender reviews borrower package | ✅ portal screens | `GET /applications/{id}` |
| Lender records decision/terms | ✅ portal actions | `POST /applications/{id}/decision`, `/offers` |
| Document retrieval | ✅ time-limited links | signed URLs via API |
| KYC evidence review | ✅ portal | included in application payload |
| Status back to capitalYA | ✅ portal writes state | webhooks + polling |
| Settlement reconciliation | ✅ daily file upload/download | `POST /loans/{id}/payments` + recon API |
| Servicing events | ✅ portal + file | webhooks |

Everything required for the closed pilot is in the Portal column. The API column is the forward path and is specified so the lender can plan, not so the pilot waits.

## 3. Identity and access

Applies to the portal now and the API in phase 2.

- **Authentication:** OAuth 2.0 (authorization-code + PKCE for portal users; client-credentials for phase-2 API). Optional SSO/SAML federation to the lender's IdP.
- **Transport:** TLS 1.2+ for the portal; **mutual TLS** required for production API traffic.
- **Authorization:** role-based. Minimum roles — `lender_underwriter`, `lender_kyc_officer`, `lender_admin`, `lender_servicer`, `lender_read_only`. Only `lender_underwriter`/`lender_admin` may emit legally-meaningful decision states (§4).
- **Environments:** separate **sandbox** and **production** credentials and data; no production PII in sandbox.
- **Network:** IP allowlisting for API and admin portal access where the lender can provide fixed ranges.
- **Webhook signing (phase 2):** HMAC-SHA256 signature header + timestamp; reject stale/invalid signatures.
- **Key management:** documented rotation schedule and revocation; no long-lived shared secrets in code.
- **Audit:** every access and every state transition is written to the immutable audit log (CRP `36`), with actor, role, timestamp, and reason.

## 4. Application state machine (ownership is part of the contract)

The state list is not a happy path — each state has an **owner**, and only the lender may emit the legally-meaningful ones. Engineers must enforce owner-at-transition; capitalYA services have no code path to write a lender-owned state.

| State | Owner | Legally meaningful? | Notes |
|---|---|---|---|
| `draft` | capitalYA/Borrower | no | Borrower assembling application |
| `submitted_to_capitalya` | capitalYA | no | Intake complete |
| `crp_assessment_in_progress` | CRP | no | Assessment underway |
| `ready_for_lender_review` | capitalYA | no | Validated + complete package handed off |
| `lender_kyc_review` | **Lender** | no | Lender KYC of record underway |
| `lender_underwriting` | **Lender** | no | Independent underwriting |
| `additional_information_required` | **Lender** | no | Back-edge → returns to borrower/capitalYA |
| `conditionally_approved` | **Lender** | **yes** | Lender only |
| `declined` | **Lender** | **yes (terminal)** | Lender only |
| `final_approved` | **Lender** | **yes** | Lender only |
| `contract_issued` | **Lender** | **yes** | Lender generates/serves contract |
| `borrower_accepted` | Borrower (via portal) | records acceptance | Borrower signs lender's contract |
| `conditions_satisfied` | **Lender** | **yes** | Lender confirms conditions |
| `disbursed` | **Lender** | **yes** | Lender moves funds; capitalYA records only |
| `active` | **Lender**/servicer | yes | Loan servicing |
| `delinquent` | **Lender**/servicer | **yes** | Missed payment(s) |
| `restructured` | **Lender** | **yes** | Lender restructure decision |
| `paid` | **Lender**/servicer | **yes (terminal)** | Loan satisfied |
| `defaulted` | **Lender** | **yes** | Lender determination |
| `written_off` | **Lender** | **yes (terminal)** | Lender accounting |

### 4.1 Allowed transitions (including back-edges)

```
draft → submitted_to_capitalya → crp_assessment_in_progress → ready_for_lender_review
ready_for_lender_review → lender_kyc_review → lender_underwriting
lender_underwriting ↔ additional_information_required        (loops until resolved)
lender_underwriting → conditionally_approved | declined | final_approved
conditionally_approved → final_approved | declined
final_approved → contract_issued → borrower_accepted → conditions_satisfied → disbursed → active
active → delinquent → active | restructured | defaulted
restructured → active
active | delinquent → paid
defaulted → written_off
(any lender-review state) → declined
```

- `additional_information_required` is an explicit **back-edge**: it returns the file to the borrower/capitalYA for completion, then re-enters `lender_underwriting`. capitalYA may facilitate the round-trip but must not resolve the underwriting question.
- Terminal states: `declined`, `paid`, `written_off`.
- capitalYA-owned transitions are limited to the pre-review lane (`draft` → `ready_for_lender_review`). Everything from `lender_kyc_review` onward is lender- or borrower-driven. This mapping must match CRP `14` conventions; where they differ, log an OQ.

## 5. Borrower handoff package (what the lender receives)

At `ready_for_lender_review`, capitalYA presents a consented package. **All field-level schemas are canonical in `07`**; this section lists contents only.

- Application ID, borrower identity reference, business identity + beneficial owners.
- Requested amount and purpose.
- Financial-document manifest (retrieved via time-limited links, §7 — never email).
- CRP assessment reference (the signed Readiness Assessment Package; schema in `07`).
- Consent records (scope + timestamp).
- KYC status object (§6; schema in `07`).
- AML screening status (preliminary; §6).
- Fraud and document-integrity flags.
- Required supporting evidence / open remediations.

The lender receives this as **decision-support input to its own underwriting** — not an instruction, recommendation, or approval (`05` §1).

## 6. KYC / AML handoff — preliminary and non-relied

The handoff distinguishes four layers and never lets capitalYA's pre-screen read as a regulated determination:

1. **capitalYA pre-screening** — preliminary, non-relied.
2. **CRP verification** — document/evidence verification.
3. **Lender regulatory KYC** — of record; the lender decides whether the evidence satisfies its legal and internal requirements.
4. **Lender ongoing monitoring** — of record.

The KYC object (canonical schema in `07`) is **granular, never `kyc_passed: true`**, and carries `lender_reverification_required: true` plus an explicit basis flag marking capitalYA's results as **preliminary / non-relied**. Hard rule (CRP `02` §2.7): capitalYA **never auto-clears a sanctions match** — any potential match is escalated to the lender's MLRO for determination; it is never resolved by automation. AML reporting of record is the lender's (`02` §7 responsibility matrix).

## 7. Document exchange

- Sensitive documents are retrieved through **time-limited, access-controlled links** (or phase-2 signed URLs), scoped to the consent record. **Never** transmitted by email.
- Documents are stored encrypted at rest and in transit (US cloud permitted with cross-border consent per CRP D-23 / `03_...` §9.1); access is logged.
- Link lifetime, download limits, and revocation-on-consent-withdrawal are configurable and audited.

## 8. Settlement & reconciliation model

Straight from `05` and `03_...`: **capitalYA never touches principal or repayments.**

- The **lender disburses directly** to the borrower from its own or its regulated processor's accounts.
- The **borrower repays into a lender/processor-controlled account.**
- capitalYA **fees are billed and collected separately** (per `08`) and are **never deducted from loan proceeds** (`08` forces `deduct_from_proceeds: false` for capitalYA fees).
- capitalYA **displays** payment status but is **not the legal payment ledger** — the lender's system of record is authoritative.
- The lender sends **daily reconciliation files** (disbursements, payments received, balances, delinquency) to capitalYA for status display and servicing technology.

### 8.1 Reconciliation import — idempotency (required)

Because reconciliation and any programmatic payment events can be re-sent:

- Every reconciliation record carries an **idempotency key** (e.g., `lender_txn_id`); re-processing the same key is a **no-op** (exactly-once effect).
- Imports are validated (schema + totals) before status updates; partial/failed imports roll back and alert.
- Portal-entered actions are idempotent-by-UI but still audit-logged.
- **No reconciliation or payment data ever flows back into CRP scoring** (`05` §3.10; CRP `02` P10).

### 8.2 Factoring assignment notice — automated perfection (Ley 9244)

Per `21` D-P2: the moment the lender **accepts and finances an invoice** (state `disbursed` recorded for a factoring product), the platform **programmatically generates and dispatches a digitally signed notice of assignment (notificación de cesión) to the underlying corporate debtor**:

- The notice is issued **in the lender's name** — the lender is the assignee; capitalYA is dispatch infrastructure only.
- Legal basis: Ley de Garantías Mobiliarias (Law 9244) — formal debtor notification perfects the assignment so payments cannot legally be redirected back to the SME.
- Dispatch is automatic on the `disbursement.recorded` event, emits `assignment.notice_dispatched` (`07` §4.5), stores the signed notice + delivery evidence against the loan record, and is a full audit event.
- Failure to dispatch is a **blocking incident** (perfection at risk), alerting both capitalYA ops and the lender.
- Notice form, content, signature, and delivery-evidence requirements — and any additional registry filing per receivable type — are **counsel-confirmed** before the pilot (`03_...` §17 Q8; `briefs/COUNSEL_ENGAGEMENT_BRIEF`).

## 9. Dispute & complaint routing (ownership matrix)

Prevents capitalYA from accidentally owning a regulated credit matter, and prevents the lender from dumping everything on capitalYA:

| Dispute type | Owner | capitalYA role |
|---|---|---|
| Loan decision dispute | **Lender** | Route + record only |
| Payment / balance dispute | **Lender** | Display, route |
| CRP score dispute | **CRP** | Route to CRP appeal (CRP `57`) |
| Data correction request | **Data controller** for that data | Facilitate, log |
| Platform functionality complaint | **capitalYA** | Own + resolve |
| Fraud allegation | **Joint escalation** | Co-investigate per agreement |
| Regulatory complaint | **Responsible regulated party** | Cooperate |

Routing rules are encoded, logged, and mirrored in the partner agreement (`04`) and the complaints SOP (CRP `57`).

## 10. Service levels (proposed defaults — negotiated, not regulatory)

Presented to the lender as **default proposals** for the agreement, explicitly **not** regulatory requirements:

| Process | Proposed SLA |
|---|---|
| API acknowledgement | < 5 seconds |
| Webhook retry window (phase 2) | 24 hours |
| KYC exception response | 1 business day |
| Initial lender review | 2 business days |
| Full credit decision | 3–5 business days after complete file |
| Additional-document request | within 1 business day of review |
| Disbursement | T+1 after all conditions met |
| Payment reconciliation | daily |
| Material security incident notification | within 4 hours |
| Borrower complaint acknowledgement | 1 business day |
| Critical service restoration | 4 hours |

## 11. API contracts (Phase 2 — specified, not a pilot dependency)

capitalYA exposes these; the lender consumes them when it is ready to move off portal-only operation. Legally-meaningful endpoints (`/decision`, `/offers`, `/disbursement`, `/delinquency`, `/restructure`) are **writable only by lender-role credentials**.

```
POST   /borrowers
POST   /applications
POST   /applications/{id}/documents
POST   /applications/{id}/submit
GET    /applications/{id}/status
POST   /applications/{id}/conditions
POST   /applications/{id}/decision        # lender-only
POST   /applications/{id}/offers          # lender-only
POST   /applications/{id}/acceptance
POST   /loans/{id}/disbursement           # lender-only; records lender's action
GET    /loans/{id}/schedule
POST   /loans/{id}/payments               # idempotent (§8.1)
POST   /loans/{id}/delinquency            # lender-only
POST   /loans/{id}/restructure            # lender-only
POST   /disputes
GET    /disputes/{id}
```

Conventions: idempotency keys on all state-changing POSTs; consistent error envelope; cursor pagination; request IDs for tracing; versioned base path (`/v1`). Payloads reference the canonical schemas in `07`.

## 12. Appendix — "lender-provides" adapter (non-default)

If the partner lender already operates a loan-origination system with APIs, capitalYA can consume the lender's endpoints instead of exposing its own. In that mode: the state machine (§4), ownership rules, settlement model (§8), and boundary (`05`) are unchanged; only the transport direction differs. This is documented for completeness and is **not** the MVP path.

## 13. Open items routed to the partner agreement (`04`) / counsel (`03_...` §17)

- Exact roles/permissions mapping to the lender's staff titles.
- Which party hosts the borrower-facing acceptance step legally (lender contract served in capitalYA portal vs. lender system).
- Reconciliation file format + delivery mechanism (SFTP vs. portal upload).
- Incident-notification contacts and severities.
- Backup-servicer identity and trigger (`04` §5).
- Whether/when the lender moves from portal to API (phase-2 trigger).
