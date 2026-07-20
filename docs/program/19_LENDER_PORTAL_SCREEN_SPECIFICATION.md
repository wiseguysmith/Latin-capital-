# 19 — Lender Portal Screen Specification (SCR-D Series)

| Field | Value |
|---|---|
| Purpose | Screen-by-screen specification of the partner-lender decisioning portal — the MVP surface through which the lender's staff perform and record their regulated actions (`06`) |
| Audience | Product, design, frontend/backend engineers, partner-lender operations |
| Status | Draft v1.0 |
| Version | 1.0.0 |
| Owner | Head of Product |
| Dependencies | `06_PARTNER_LENDER_PORTAL_AND_INTEGRATION_SPEC.md` (state machine §4, roles §3, settlement §8), `05` (boundary), `07` (RAP + KYC schemas), `11` (disclosures); CRP `18_SCREEN_BY_SCREEN_SPECIFICATION.md` (conventions, SCR registry), `12` (permissions), `36` (audit events) |
| Registry note | Extends the CRP `SCR-x##` registry with the **`SCR-D`** series (lender portal). SCR-A/B/C and the internal admin console remain specified in CRP `18`. |
| Last updated | 2026-07-19 |

---

## 1. Global rules (apply to every SCR-D screen)

1. **Role gating is the boundary.** Only `lender_underwriter` / `lender_admin` see or can activate the controls that emit legally-meaningful states (`06` §4). capitalYA staff roles can view workflow status but the decision controls **do not render** for them — there is no hidden-but-disabled decision UI for non-lender roles.
2. **Every action is an audit event** (actor, role, timestamp, reason where applicable — CRP `36`). The portal shows "recorded by [name], [role], [time]" on every state change.
3. **The limitation statement** (`05` §5) renders wherever the CRP readiness score appears — non-dismissable, above the fold of the score panel.
4. **No prohibited terms** anywhere in portal copy (`05` §3.5). The portal says "record decision," never "approve application" in capitalYA-attributed UI; approval language appears only inside lender-attributed action labels ("Lender decision: Final approved").
5. **Attribution banner.** Every decisioning screen carries a persistent banner: *"Credit decisions on this screen are made and recorded by [LENDER NAME] as the sole lender of record."*
6. **Languages:** ES primary, EN secondary. Operative legal strings (disclosures, limitation statement) use counsel-approved text only (`11`).
7. **Session:** OAuth per `06` §3; inactivity timeout; IP-allowlist enforcement where configured.
8. **States pattern** (per CRP `18` conventions): every screen defines loading / empty / error / permission-denied states; wide tables scroll within their container.

## 2. Screen inventory

| ID | Screen | Primary role(s) | State-machine coverage (`06` §4) |
|---|---|---|---|
| SCR-D1 | Review queue (workspace home) | all lender roles | entry to `ready_for_lender_review` + |
| SCR-D2 | Application detail & RAP viewer | all lender roles | context for all states |
| SCR-D3 | Document viewer | all lender roles | evidence access (`06` §7) |
| SCR-D4 | KYC review | `lender_kyc_officer` | `lender_kyc_review` |
| SCR-D5 | Underwriting workspace | `lender_underwriter` | `lender_underwriting` ↔ `additional_information_required` |
| SCR-D6 | Decision recording | `lender_underwriter`/`lender_admin` | `conditionally_approved` / `final_approved` / `declined` |
| SCR-D7 | Offer & terms entry | `lender_underwriter`/`lender_admin` | terms attached to decision; `contract_issued` |
| SCR-D8 | Conditions & acceptance tracking | `lender_underwriter`, `lender_servicer` | `borrower_accepted`, `conditions_satisfied` |
| SCR-D9 | Disbursement confirmation | `lender_admin`/`lender_servicer` | `disbursed` → `active` |
| SCR-D10 | Servicing & reconciliation | `lender_servicer` | `active`/`delinquent`/`restructured`/`paid`/`defaulted`/`written_off`; recon import (`06` §8.1) |
| SCR-D11 | Lender admin (users, roles, checklist) | `lender_admin` | configuration, not states |

## 3. Screen specifications

### SCR-D1 — Review queue

**Purpose:** the lender's working inventory of applications, by state.
**Elements:** state-grouped queue (`ready_for_lender_review`, `lender_kyc_review`, `lender_underwriting`, `additional_information_required`, awaiting-conditions, servicing); per-row: borrower name, product, requested amount, readiness band (not raw score at queue level), days-in-state, SLA indicator (`06` §10 targets); filters (state/product/amount/age); sort.
**Rules:** SLA indicators are informational (proposed targets, not regulatory). Queue position is never influenced by any capitalYA fee status (`05` §3.10).
**Acceptance:** a `lender_read_only` user sees the queue but no action affordances; SLA colors match `06` §10 defaults.

### SCR-D2 — Application detail & RAP viewer

**Purpose:** the single file view — everything the lender needs before acting.
**Elements:** header (borrower, product, amount, current state + history timeline with actors); **RAP panel** rendering `07` §4.1: overall score + band, **limitation statement (non-dismissable, first)**, 8 dimension scores with confidence shown as a *separate* axis, flags with severities, gating results, remediations, methodology version, assessment date/expiry, human-approver reference; consent-scope summary; capitalYA validation result (`07` §4.2 — shown as "completeness validation," `final_credit_decision: null` never rendered as a recommendation); links to SCR-D3/D4/D5.
**Rules:** score and confidence are never blended into one visual; expired RAP shows a prominent "assessment expired — request refresh" state; nothing on this screen suggests a recommended amount or rate.
**Acceptance:** limitation statement renders before the score is visible in the panel; dimension confidence is visually distinct from the score.

### SCR-D3 — Document viewer

**Purpose:** review evidence via time-limited links (`06` §7).
**Elements:** evidence manifest (type, tier, freshness, hash), inline viewer, download (logged), link-expiry indicator.
**Rules:** access logged per document; links revoke on consent withdrawal; no email-forward affordance.
**Acceptance:** an expired link shows a re-request state, not an error dump.

### SCR-D4 — KYC review

**Purpose:** the lender's KYC-of-record determination.
**Elements:** the granular KYC object (`07` §4.3) rendered field-by-field with the **"preliminary / non-relied"** basis banner; UBO list; screening results (PEP/sanctions/adverse-media) each labeled "capitalYA pre-screen — re-verify"; `lender_reverification_required` always visible; actions: *Mark KYC satisfied* / *Request more KYC information* / *Escalate to MLRO*.
**Rules:** a `sanctions_potential_match_escalated` file renders a blocking escalation banner; the portal offers **no** "clear match" control for anyone — clearance happens in the lender's own AML process, and the portal records only the outcome (`14` §4.1).
**Acceptance:** with an escalated sanctions flag, *Mark KYC satisfied* is not available until the escalation outcome is recorded.

### SCR-D5 — Underwriting workspace

**Purpose:** where the lender's underwriter works the file.
**Elements:** RAP summary (from D2), lender's own checklist (from SCR-D11 configuration), notes (lender-private — not shared to capitalYA/borrower), action: *Request additional information* (structured request list → borrower via capitalYA; moves state to `additional_information_required`), action: *Proceed to decision* (→ SCR-D6).
**Rules:** the back-edge loop (`06` §4.1) is explicit: returned files re-enter `lender_underwriting` with a diff of what changed since last review. Lender notes are excluded from borrower-visible data and from CRP scoring inputs.
**Acceptance:** a file returning from `additional_information_required` shows "what's new" since the request.

### SCR-D6 — Decision recording

**Purpose:** the legally-meaningful action — lender only.
**Elements:** attribution banner (§1.5); decision selector: *Conditionally approved* (requires conditions list) / *Final approved* / *Declined* (requires reason category + free text for the borrower-facing adverse notice, per `11` §3); confirmation modal restating: "This decision is made by [LENDER], not capitalYA"; two-step confirm.
**Rules:** renders **only** for `lender_underwriter`/`lender_admin`; the emitted event is `decision.recorded` with lender actor identity (`07` §4.5); a declined file triggers the borrower notice flow with score-rights content (`11` §3).
**Acceptance:** capitalYA-role sessions never render this screen's action panel (not even disabled); every decision stores actor + reason + timestamp.

### SCR-D7 — Offer & terms entry

**Purpose:** the lender records the terms it sets.
**Elements:** amount, currency, nominal + effective rate, fee items (lender's own), schedule, collateral, conditions; **all-in cost readout with BCCR cap check** (`08` §4) — a breach blocks presentation with "exceeds applicable cap"; disclosure-preview (renders the `11` §2 content as the borrower will see it); action: *Issue contract* (→ `contract_issued`).
**Rules:** terms fields are lender-editable only; capitalYA fees display read-only from config (`17`) and are **excluded from proceeds math** (`08` §5); the cap check runs before *Issue contract* activates.
**Acceptance:** a terms set breaching the cap for its currency/semester cannot be issued.

### SCR-D8 — Conditions & acceptance tracking

**Purpose:** track contract acceptance and condition satisfaction.
**Elements:** contract status (issued/accepted — acceptance recorded from the borrower flow), per-condition checklist with evidence links, action: *All conditions satisfied* (lender) → enables disbursement stage.
**Acceptance:** *Conditions satisfied* unavailable until every condition row is closed by a lender role.

### SCR-D9 — Disbursement confirmation

**Purpose:** the lender confirms it moved funds — capitalYA records, never moves.
**Elements:** banner: *"Disbursement is executed by [LENDER] through its own accounts. capitalYA records the event for workflow purposes only."*; fields: date, amount, account reference (masked), lender transaction ID (= idempotency key, `06` §8.1); action: *Record disbursement* → `disbursed` → `active`.
**Rules:** duplicate lender-transaction IDs no-op with an explanatory notice; no payment-initiation control exists anywhere in the portal.
**Acceptance:** re-submitting the same confirmation does not create a second event.

### SCR-D10 — Servicing & reconciliation

**Purpose:** daily reconciliation import and servicing-state recording.
**Elements:** recon-file upload (schema-validated preview → commit; per-row idempotency on `lender_txn_id`; failed rows quarantined with reasons — `06` §8.1); loan list with payment status (labeled *"as reported by [LENDER] — the lender's ledger is the system of record"*); lender actions: record `delinquent` / `restructured` / `paid` / `defaulted` / `written_off` (each with reason + two-step confirm).
**Rules:** recon data never flows to CRP scoring (`05` §3.10); partial imports roll back; every import is an audit event with row counts.
**Acceptance:** uploading the same file twice changes nothing on the second run and says so.

### SCR-D11 — Lender admin

**Purpose:** the lender manages its own users, roles, and intake checklist.
**Elements:** user list + role assignment (roles per `06` §3 — role changes are audit events); **intake checklist editor** — the completeness checklist capitalYA validates against (`07` §4.2 `lender_policy_version`), versioned, with effective dates; MLRO escalation contact + channel configuration (`14` §8); SLA display (read-only from agreement).
**Rules:** checklist versions are immutable once effective (new version to change); only `lender_admin` assigns decision-capable roles.
**Acceptance:** a checklist edit produces a new `lender_policy_version` visible in subsequent validation results.

## 4. What is deliberately absent

No investor screens, no token/tokenization UI, no payment initiation, no pooled-capital views, no capitalYA-side decision or override controls, no "recommended amount/rate" anywhere (`05` §3; `01` Horizon rule). If a design iteration proposes any of these, it is a boundary change requiring Founder + counsel sign-off (`05` §8).

## 5. Build notes

- The portal is capitalYA-hosted (`06` §1) but **lender-attributed** in all decisioning UI (§1.5).
- Reuses CRP platform services (auth per CRP `43`, audit per `36`, documents per `35`); no new scoring surface.
- A clickable mockup of the core flow (D1 → D2 → D4 → D5 → D6 → D7 → D9 → D10) accompanies this spec at `mockups/lender-portal-mockup.html` — illustrative fake data only.
