# 75 — Implementation Phases and Roadmap

| Field | Value |
|---|---|
| Purpose | Phased delivery plan with goals, inclusions/exclusions, dependencies, risks, team, acceptance, metrics, and exit criteria per phase |
| Audience | Leadership, all leads |
| Status | Draft v1.0 |
| Version | 1.0.0 |
| Owner | Head of Product |
| Dependencies | 70/71 (work), 76 (pilot), 77 (launch checklist) |
| Source references | Master prompt §16 (phases 0–4, controlling); CRF §22 (phases 0–7, mapped below) |
| Assumptions | ASM-02 (team); durations indicative, not commitments |
| Open questions | OQ-01/02 resolution inside Phase 0 |
| Approval required | Founder |
| Last updated | 2026-07-16 |

## 1. Phase map (prompt controls; CRF §22 mapping)

| Prompt phase | CRF §22 equivalent | Indicative duration |
|---|---|---|
| 0 Validation & operating design | Phase 0 + Phase 1 (manual pilot) | 6–10 weeks |
| 1 Internal operations MVP | Phase 2 (digital workspace) | 10–14 weeks |
| 2 AI-assisted MVP | Phase 3 (document intelligence) | 8–12 weeks (overlaps 1) |
| 3 Capital-provider workspace | Phase 4 (lender workflow, minus matching) | 6–10 weeks |
| 4 Regional overlays & integrations | Phase 6 + parts of 5 | per module |
| (post-MVP) monitoring/validation; tokenization rails | Phases 5, 7 | 80/81 |

## 2. Phase 0 — Validation and operating design

**Goals:** validate CRF controls, country assumptions, document matrix, scoring rules, reviewer workflow — manually, before code hardens them.
**Included:** 3–5 full manual assessments (real or realistic borrowers) using score workbook implementing 20/21/23 exactly; checklist dry-runs per profile (24 §6); anchor calibration + materiality/threshold proposals (OQ-15); SOP dry-runs (51–53); counsel engagement started (CR-L1…L3 minimum); commercial-model decision (OQ-01); vendor shortlists (47 §4); golden fixtures built from dry-run docs (73 §3).
**Excluded:** production code beyond spikes; any real provider distribution.
**Dependencies:** framework owner time; counsel availability; 2–3 friendly businesses.
**Risks:** anchor ambiguity discovered late (mitigate: workbook first); counsel latency (start day 1).
**Team:** PM, framework owner, 1 R-FR, fractional legal/compliance; 1–2 engineers on spikes/fixtures.
**Acceptance/exit:** ≥3 manual assessments senior-approved; workbook results reproducible by second reviewer (inter-rater check); calibration memo accepted by methodology board; CR-L1 engagement letter signed; scoring model v1.0.0 frozen for build.
**Pilot metrics baseline captured:** manual time-to-package, exception counts (37 baselines).

## 3. Phase 1 — Internal operations MVP

**Goals:** run the whole readiness process in-product, manually reviewed.
**Included:** applicant intake + manual approval; org accounts; capital workspace; document uploads + manual review; manual scoring support (engine + control UI, no AI); audit log; borrower readiness report; notifications core; CR overlay v1; consent ledger.
**Excluded:** AI pipelines (except spikes), provider workspace, integrations, billing automation.
**Dependencies:** Phase 0 frozen model; security baseline (SB gate) before real data.
**Risks:** small team + segregation rules (12 §6) — mitigate with explicit role calendar; scope creep into AI early.
**Team:** 3–4 engineers, PM, designer, ops (2), R-SA.
**Acceptance/exit (gate):** 10 real applicants processed end-to-end in-product; all Phase-1 P0 stories pass ACs; security baseline verified (44 §2); audit chain verified; SOPs operating with SLAs measured; borrower NPS/interviews collected.

## 4. Phase 2 — AI-assisted MVP

**Goals:** cut review effort and time-to-package without ceding control.
**Included:** classification, structured extraction, evidence mapping, clarification generation, contradiction detection, provisional scoring support (AI-suggested maturities), verification queues, evidence-confidence computation, remediation-plan generation, internal reviewer report.
**Excluded:** auto-acceptance of material fields; borrower-visible provisional scores (ASM-16); matching.
**Dependencies:** golden datasets (Phase 0/1 docs); AI vendor contracts (ASM-15); model registry.
**Risks:** extraction quality on local documents (mitigate: dual-vendor OCR fallback, manual entry path always available); cost overruns (NFR-20 breaker).
**Team:** +1 AI engineer; same core.
**Acceptance/exit (gate):** AI-assisted assessment verifiably faster than Phase-1 manual baseline (target ≥30% reviewer-time reduction) with equal-or-better accuracy on sampled QC (52 §4); 31 §8 metrics at target; adversarial suite (74 §2) passing; human-correction rate trending down across 4 weeks.

## 5. Phase 3 — Capital-provider workspace

**Goals:** close the loop: curated opportunities reviewed by real providers.
**Included:** provider onboarding (manual diligence + agreements), packaging + disclosure lint, consent + publication, approved document sharing (watermarked/tracked), RFI, EOI, opportunity tracking, outcome recording.
**Excluded:** automated matching (D-18), provider API, fee automation.
**Dependencies:** **CR-L1 perimeter memo + provider agreement template (US-1301/1303) — hard gate**; ≥3 onboarded providers; ≥5 approved assessments ready.
**Risks:** provider expectations of "ratings" (mitigate: onboarding training 54 §3, package language 27 §5); consent friction (clear borrower UX 55 §3).
**Acceptance/exit (gate):** ≥3 opportunities published with full consent flow; ≥1 provider completes review through RFI; EOI flow exercised; zero disclosure-lint incidents in production; provider feedback collected; pilot metric pack live (37).

## 6. Phase 4 — Regional overlays and integrations

**Goals:** second/third country + verification automation.
**Included (sequenced by value):** Panama overlay activation (61 §4), El Salvador (62 §4); registry-check integrations (CR first), KYC/AML vendor, accounting integrations, credit-information where legally available (OQ-05); open-finance where coverage is real (CRF §17.1: never gate on it).
**Dependencies:** per-country counsel sign-off (63 §3 blocking); vendor coverage validation (47 §2).
**Risks:** perimeter differences invalidating flows (mitigate: overlay-only changes; GATE-07 discipline); integration data conflicting with documents (exceptions per 42 §5.1).
**Acceptance/exit:** each country passes activation checklist incl. guarded pilot cohort; integration evidence upgrades measurably raise confidence tiers (22) without new incident classes.

## 7. Cross-phase workstreams

Legal/compliance (EP-13) runs continuously from day 1; security reviews at each gate (77); methodology board monthly from Phase 0; documentation kept current (00 §7) — a phase is not exited with stale docs.
