# 52 — Document Review SOP

| Field | Value |
|---|---|
| Purpose | Procedure for verifying documents and AI extractions, handling defects, exceptions, and evidence upgrades |
| Audience | R-FR, R-LR, R-CR, R-IA |
| Status | Draft v1.0 |
| Version | 1.0.0 |
| Owner | Ops lead + Framework owner |
| Dependencies | 14 §4, 26 (taxonomy/defects), 30 (pipeline), 22 (tiers), 31 §5 (thresholds) |
| Source references | CRF §14 stages 5–6, §16 layer 10, Appendix B |
| Assumptions | Phase-1 fully manual variant included |
| Open questions | — |
| Approval required | Ops lead |
| Last updated | 2026-07-16 |

## 1. Routing

Document class → reviewer: financial (EV-FIN/BNK/TAX/DBT/AR/AP/REV/UOP) → R-FR; corporate/legal/contracts/property (EV-COR/OWN/CON/LEG/LIC/PRO/IP) → R-LR; KYC/screening/source-of-funds (EV-CMP, EV-OWN-03) → R-CR; collateral valuations may add partner review (58). SLA ≤3 business days (50 §5).

## 2. Verification procedure (Phase 2 — AI-assisted)

1. Open queue item (SCR-A5): preview with extraction overlays.
2. **Classification check:** confirm/correct document type (correction = feedback data, 31 §8).
3. **Field verification:** verify all material fields (21) and any field below threshold (31 §5) against the source region; correct values write with `human_verified` provenance. Never accept a value not visible in the document (hallucination check — discrepancy = AI incident if systemic, 45 §5).
4. **Defect pass:** completeness (pages/sections), signatures/notarization per 26 registry, period correctness, entity match (DEF-06), legibility. Reject with defect code + guidance, or raise clarification for explainable issues.
5. **Authenticity pass (targeted):** issuer verification for registry/tax/bank docs where feasible (stamp/QR/verification portals — CR specifics in 60); suspected alteration → **stop**, neutral borrower messaging, S1 flag, GATE-04 protocol (56).
6. **Reconciliation panel:** review comparator results (FR-AI-03); material mismatch → exception (ENT-15): document both values, never average (CRF App B); draft/approve clarification or resolve with authoritative source rationale.
7. **Tier assignment:** confirm evidence tier (E1 default for uploads; E2/E3 upon verification per 22 §3 paths) — tier drives confidence (22).
8. **Verify** → document `verified`; linked checklist item and tasks update automatically.

Phase 1 (manual variant): steps identical minus overlays; reviewer enters key fields into structured forms (32 schemas rendered as forms) — this keeps Phase-1 data forward-compatible.

## 3. Escalation rules

- Any GATE-relevant discovery (identity mismatch, undisclosed lien/debt, sanctions-adjacent name) → flag + route per 23 §2 resolver.
- Legal-interpretation doubt (R-LR): mark "counsel review needed" → 58 partner or external counsel; never guess enforceability (D-07).
- Repeated defects from same borrower (≥3 same-code rejections) → borrower-success outreach task (coaching, not penalty).

## 4. Quality control

Weekly sample: 10% of verified documents re-checked by a second reviewer (different person); disagreement rate reported (37); material misses → targeted retraining + threshold review (31 §5).
