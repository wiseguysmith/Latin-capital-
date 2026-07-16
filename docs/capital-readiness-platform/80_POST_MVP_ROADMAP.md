# 80 — Post-MVP Roadmap

| Field | Value |
|---|---|
| Purpose | Direction after Phase 4: capabilities deliberately deferred, their triggers, and sequencing logic |
| Audience | Leadership, product, architects |
| Status | Draft v1.0 — directional, re-planned after pilot |
| Version | 1.0.0 |
| Owner | Head of Product |
| Dependencies | 75 (phases), 81, 82 |
| Source references | CRF §22 phases 5–7, §14 stages 11–14, §16 layer 9/11 |
| Assumptions | Pilot validates core loop before any of this starts |
| Open questions | OQ-01 (monetization shapes several items) |
| Approval required | Founder |
| Last updated | 2026-07-16 |

## 1. Sequencing logic

Post-MVP work follows validated demand, not architecture ambition: (1) deepen the standard (certification), (2) reduce friction (integrations/monitoring), (3) widen the network (matching, provider tooling), (4) widen the map (82), (5) new rails (81). Each item lists its **trigger** — the evidence that justifies starting.

## 2. Roadmap items

### R1. Monitoring & early warning (CRF §14 stage 14, §16 layer 11)
Periodic evidence refresh, covenant/metric-change alerts, missing-report detection, sanctions re-screening automation, collateral/news alerts; readiness status becomes maintainable, not just point-in-time. **Trigger:** ≥10 funded-external outcomes whose providers request ongoing packages. Builds on: expiry engine (14 §3), staleness sweeps, ENT-29.

### R2. Certification program (REF-02 realization)
"CRF Certificate" brand with published methodology, validation results, governance charter, advisory council (CRF §22 Phase 0 deliverable matured), possibly independent audit of the certification process (SOC-2-style ambition, 01 §2). Includes certificate registry with verification endpoint (hash-anchored, CRF App B.1). **Trigger:** pilot outcome data supports credible public claims (76 §7 honesty bar); trademark + liability posture cleared (64 P4/P3).

### R3. Matching engine (CRF §16 layer 9; deferred by D-18)
Hard mandate filters first, explainable ranking second, consent-based introductions; never suitability/approval claims. **Trigger:** ≥10 active providers and manual curation demonstrably bottlenecks (55 §2 rationale logs show queue delay). Perimeter re-check per fee model (64 A1) before launch.

### R4. Integration expansion (CRF §17)
Open finance (Belvo/Prometeo-class) for bank-data evidence tier upgrades; accounting/ERP pulls; registry APIs; credit-information channels (OQ-05); e-invoice data where available. Each behind 47 §2 country validation and the §17.1 conflict-exception rule. **Trigger:** per-integration ROI: evidence classes where manual verification is the measured bottleneck (37 queue data).

### R5. Provider workspace v2
Portfolio views, saved mandates with notification, package comparison tools, provider API/webhooks (agreement-gated), configurable benchmark views (CRF §6 "configurable lender benchmarks" — provider-side only, never platform thresholds). **Trigger:** provider retention data + explicit demand from ≥3 design partners.

### R6. Borrower self-serve growth
Lighter "readiness snapshot" pre-product (lead gen, clearly non-assessment); template marketplace; cohort education. Guardrail: snapshot must not look like a score (02 §3 risk). **Trigger:** CAC economics demand top-of-funnel automation.

### R7. Billing automation
Per OQ-01 resolution; Stripe-class integration; fee firewall preserved architecturally (FR-SCORE-10 stays). **Trigger:** manual invoicing >4h/week ops load.

### R8. Monitoring of methodology at scale
Statistical calibration (band vs outcome correlations with honest limits), inter-rater reliability tooling, anchor A/B evaluation under governance (20 §10; never silent recalibration). **Trigger:** ≥100 assessments.

### R9. Regional scaling per 82; tokenization compatibility per 81.

## 3. Explicit non-goals (unchanged from 02 unless re-decided)

Balance-sheet lending, funds handling, secondary markets, consumer products, credit scoring/PD products, selling borrower data. Any change = founder + counsel + new decision log entries.
