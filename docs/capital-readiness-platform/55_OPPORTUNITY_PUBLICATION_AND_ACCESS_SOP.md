# 55 — Opportunity Publication and Access SOP

| Field | Value |
|---|---|
| Purpose | Procedure for packaging an approved assessment, obtaining consent and approval, and managing provider access |
| Audience | R-IA, R-SA, R-BA (consent), compliance |
| Status | Draft v1.0 |
| Version | 1.0.0 |
| Owner | Ops lead |
| Dependencies | 14 §5, 23 §6 (disclosure), 27 §5 (package), 12 §3–4 (consent/access) |
| Source references | CRF §14 stages 9–11, §15 stages 3–5, §19.2 (borrower-controlled introduction); D-08, D-18 |
| Assumptions | ASM-12 (expiry/watermark) |
| Open questions | OQ-08 |
| Approval required | R-SA per publication |
| Last updated | 2026-07-16 |

## 1. Preconditions

Assessment `approved`/`approved_with_conditions`, unexpired; borrower attestation signed (CRF §14 stage 9); confidence ≥ C (C → conditional labeling; 22 §7); GATE re-checks fresh (identity/sanctions re-screen at publication — ASM-07); OQ-08 counsel memo in force for the provider-distribution activity.

## 2. Packaging (R-IA, SCR-A10)

1. Compose package per 27 §5; select approved-room documents (verified + shareable classes only; P4 redaction verified — 12 §3).
2. **Disclosure set:** apply 23 §6 rules — all result-affecting S2+ flags and all conditions must be disclosed; run disclosure lint (blocks restricted/P4/internal-note leakage).
3. **Provider curation (manual, D-18):** select candidate providers from mandate fit (ENT-28); record curation rationale (auditable — prevents favoritism claims; D-15 fee firewall applies).
4. Preview exact provider view.

## 3. Consent (R-BA)

Borrower reviews: package summary, document list, named providers, access duration; grants consent per provider (ENT-24; SCR-B16). Partial consent (subset of providers) fully supported; no consent = no grant, no exceptions (CRF §19.2). Consent copy is counsel-validated (OQ-14).

## 4. Approval & publication (R-SA)

Checklist: package lint clean; consents recorded and matching grants; assessment fresh; re-screen timestamps current; conditions accurately represented; curation rationale present. Approve → system publishes; grants created with expiry (default 90d, ASM-12); providers notified (PROV-01).

## 5. In-flight management

- **RFI routing** (ops ≤1 business day): answer from approved data or route to borrower as task; new document sharing requires R-BA action.
- **EOI handling:** logged; borrower accepts/declines introduction; acceptance → contact exchange + status `in_external_underwriting`; platform steps back (boundary 02) except status tracking.
- **Access hygiene:** expiry warnings T-7d; extensions by R-IA with reason; revocation ≤1h on consent withdrawal, borrower request, provider breach, or assessment expiry (auto-pause, 14 §6).
- **Monitoring:** download/view reports per opportunity (SB-15); anomalies → suspend grant + review.

## 6. Closure

Outcome recorded (ENT-29 taxonomy 37) from borrower/provider reports; opportunity closed/archived; provider access ends; borrower sees final state; metrics update. Declined-external with readiness-related reason codes feeds remediation loop (28).
