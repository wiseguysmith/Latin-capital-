# 54 — Capital-Provider Onboarding SOP

| Field | Value |
|---|---|
| Purpose | Procedure for vetting, contracting, and provisioning capital providers |
| Audience | Ops, compliance, R-SA |
| Status | Draft v1.0 |
| Version | 1.0.0 |
| Owner | Ops lead + Compliance |
| Dependencies | 12 (provider roles/scoping), 55 (publication), 15 FR-CP-02 |
| Source references | CRF §15 stages 1–2; D-08 |
| Assumptions | Manual onboarding in MVP (10 §3) |
| Open questions | OQ-08 (perimeter memo before Phase 3), OQ-13 (provider-side reliance conditions) |
| Approval required | Compliance + R-SA |
| Last updated | 2026-07-16 |

## 1. Intake & diligence

1. Application (SCR-P3): institution identity, type, regulatory status/licenses (as claimed), jurisdictions, mandate summary (sizes, sectors, structures, tenors, exclusions), team contacts.
2. **Provider KYB:** verify legal existence + authority of signatory; sanctions/adverse-media screen on institution + key persons (same discipline as borrower-side, GATE-03 rules); regulatory-status plausibility check (e.g., claimed bank → verify against SUGEF/SBP/SSF public registers, 60–62).
3. Risk assessment: any indication of consumer-lending intent, prohibited funding sources, or reputation issues → risk committee (50 §4).

## 2. Agreement (with counsel — OQ-08 gating Phase 3 activation)

Mandatory terms: confidentiality + permitted-use of packages/data rooms; **independent-underwriting attestation** (provider acknowledges CRF outputs are readiness only — no approval, rating, or PD; provider performs own diligence — 02 §4); no-redistribution + watermark acknowledgment; access-expiry terms (ASM-12); outcome-reporting cooperation (37, best-efforts); fee terms per OQ-01 (disclosed); data-protection terms (borrower P3 handling); termination + data-destruction obligations.

## 3. Provisioning

R-IA creates provider org (ENT-01) + ProviderProfile mandate (ENT-28) + R-CPA invitation (MFA enforced); R-CPA invites R-CPN analysts (seat count per agreement); access = zero opportunities until first grant (D-08). Training packet: platform walkthrough + methodology explainer (what score/confidence/context mean and don't mean) + RFI/EOI etiquette.

## 4. Ongoing management

Annual re-screen (sanctions/adverse media) + agreement review; activity monitoring (download anomalies — SB-15); dormant providers (>6 months no activity) reviewed; offboarding: revoke grants (≤1h, NFR-09), suspend org, confirm data-destruction attestation, retain audit trail (35).

## 5. Boundaries

Providers never see: other providers' identity/activity, restricted flags, internal notes, raw P4, unpublished opportunities. The platform never ranks providers for borrowers or vice versa in MVP (D-18); curation rationale is documented per publication (55 §2).
