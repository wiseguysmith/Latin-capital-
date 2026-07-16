# 51 — Application Review SOP

| Field | Value |
|---|---|
| Purpose | Step-by-step procedure for screening preliminary applications and deciding accept/reject/waitlist/RFI |
| Audience | R-AR, R-IA, R-SA |
| Status | Draft v1.0 |
| Version | 1.0.0 |
| Owner | Ops lead |
| Dependencies | 14 §2 (states), 15 FR-APP, 12 (segregation), 60 (CR overlay) |
| Source references | D-02; CRF §3.1 (gates preview), §14 stage 1 |
| Assumptions | Manual screening protocol pre-vendor (ASM-06) |
| Open questions | OQ-10 (eligibility floor + exclusion list v1) |
| Approval required | Ops lead + compliance (screening protocol) |
| Last updated | 2026-07-16 |

## 1. Intent

Screening decides **platform entry only** — it is not underwriting, not a funding signal (D-02). It filters for: legitimacy, MVP fit (country/amount/type), workability, and integrity basics. SLA: ≤5 business days (50 §5).

## 2. Procedure

1. **Claim** from queue (SCR-A1); conflicts declared before opening detail (12 §7).
2. **Completeness check:** all preliminary fields present and coherent; else RFI-preliminary with the exact missing items (single consolidated RFI — avoid drip requests).
3. **Duplicate check:** system soft matches (name, registry id, contacts); confirm whether reapplication (link history) or duplicate (merge/reject).
4. **MVP-fit check:** country = CR; business-purpose (ASM-13 — sole proprietors out); amount vs $500k–$2M soft range; borrower type within D-16 set; capital purpose within module set (25). Out-of-range but credible → escalate to R-SA rather than reject.
5. **Prohibited-activity screen** (exclusion list v1 — OQ-10 pending confirmation; conservative interim list): weapons, narcotics, sanctioned trade, gambling without license, adult content, crypto-asset issuance/trading businesses, shell companies without operations, activities illegal in CR. Any hit → reject (category stated) or risk-committee referral if ambiguous.
6. **Manual integrity screen (protocol, pre-vendor):**
   - Registry lookup: entity exists and active (CR: Registro Nacional query; record extract + date).
   - Sanctions: manual check of borrower name + named contact against OFAC/UN/EU consolidated lists (record search evidence). **Potential match → do not resolve solo:** open S1 flag, route to R-CR (GATE-03 discipline applies even pre-acceptance).
   - Adverse media: standardized search protocol (name + fraud/laundering/corruption terms, ES+EN); record queries + notable results (OQ-16).
7. **Plausibility:** revenue band vs amount requested; purpose coherence; contact domain/identity sanity.
8. **Decision** (SCR-A2): accept / reject(reason code §3) / waitlist(reason: capacity, near-miss eligibility, timing) / RFI. Escalations to R-SA: out-of-range, excluded-sector doubt, integrity ambiguity.
9. **Record:** screening checklist saved (ENT-02) with evidence attachments; audit events auto-emitted; acceptance triggers provisioning (FR-APP-03).

## 3. Rejection reason codes

RJ-01 outside jurisdiction · RJ-02 consumer/sole-proprietor (ASM-13) · RJ-03 amount/type outside MVP · RJ-04 prohibited activity · RJ-05 integrity concern (external wording: "unable to proceed at this time") · RJ-06 duplicate · RJ-07 insufficient information after RFI · RJ-08 capacity (invite to waitlist). Borrower-facing text per code is templated (19; counsel-reviewed).

## 4. Quality control

R-IA samples 10% of decisions weekly (both accepts and rejects) for checklist completeness and reason validity; sampling results to monthly methodology board; reversal path per 14 §2 (revoke-acceptance / reopen ≤30 days by R-SA).
