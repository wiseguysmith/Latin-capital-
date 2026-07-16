# 37 — Analytics, Metrics, and Reporting

| Field | Value |
|---|---|
| Purpose | Metric definitions, dashboards, outcome taxonomy, and calibration/fairness reporting — without turning CRF into an undisclosed credit model |
| Audience | Product, ops, framework owner, leadership |
| Status | Draft v1.0 |
| Version | 1.0.0 |
| Owner | Head of Product + Framework owner |
| Dependencies | 36 (events), 34 (fields), 76 (pilot targets) |
| Source references | CRF §22.1 (pilot metrics), §18 (outcome calibration), §21 (weak outcome data) |
| Assumptions | Analytics on P1/P2 aggregates only (34 §4) |
| Open questions | — |
| Approval required | Product owner |
| Last updated | 2026-07-16 |

## 1. Metric families

### Funnel & operations
| Metric | Definition | Source events |
|---|---|---|
| Application conversion | submitted → accepted rate; reason-code mix for reject/waitlist | applicant.* |
| Time to readiness package | median + P90 days: workspace_activated → assessment.approved | assessment.* (CRF §22.1) |
| First-pass completeness | share of required checklist items accepted without rework (no reject/re-upload cycle) | document.* |
| Reconciliation exception rate | material exceptions per assessment; median resolution time | exception.* |
| Queue SLAs | per-queue age distributions vs 50 §5 | queue telemetry |
| Clarification burden | clarifications per assessment; response time | assessment.clarification_requested |

### Readiness & methodology health
| Metric | Definition | Guardrail use |
|---|---|---|
| Band distribution | assessments by band over time, by business type/module | over-concentration signals calibration issues |
| Dimension score profiles | mean/spread per dimension per segment | anchor tuning (OQ-15) |
| Cap trigger frequency | per CAP-xx | CAP-01/02 dominance = intake or coaching gap |
| Override rate/direction | overrides per assessment, up vs down, by reviewer, expiry outcomes | high rate = framework weakness (CRF §22.1); monthly review 56 |
| Remediation conversion | share moving up ≥1 band after plan; median days | product efficacy (28 §5) |
| Confidence distribution | grades A–D per segment; time-to-B | evidence pipeline health |
| Assessment staleness | expiries, reassessment cycle time | freshness policy tuning |

### AI quality (with 31 §8)
Human-correction rate per field type; auto-accept precision (sampled); pipeline SLO attainment (NFR-04); cost per assessment vs budget (NFR-20); hallucination/citation-failure incidents.

### Provider engagement & outcomes
| Metric | Definition |
|---|---|
| Time to screening decision | access grant → first RFI/EOI/pass (CRF §22.1) |
| Diligence request reduction | RFIs per opportunity trend (goal: fewer repetitive/missing-info requests — not suppression of valid diligence) |
| Outcome taxonomy (ENT-29) | intro_made → term_sheet → funded_external / declined_external (+ decline reason codes: readiness-related, credit, pricing, mandate-fit, other) / withdrawn / expired |
| Package usage | views, downloads, dwell per section (watermark-tracked) |

### Fairness & access (CRF §22.1, §18)
Completion, score, publication, and outcome patterns by lawful segments (geography, sector, size band, business type). Quarterly report to leadership; disparities trigger methodology review, never quiet threshold changes. Protected-class attributes are not collected for analytics (02 §2.10).

## 2. Calibration boundary (CRF §18, §21)

Outcome data (approvals, defaults reported by providers, losses, fraud) may inform **framework revision proposals** via the governed change process (20 §10) — it must never be wired into automatic weight adjustment or converted into a default-probability product. Publish validation limits honestly (CRF §21 "weak outcome data": conservative claims, transparent recalibration).

## 3. Dashboards & cadence

- **Ops daily:** queues, SLAs, pipeline health, cost.
- **Methodology monthly:** band/cap/override/confidence panels + override register review (56).
- **Pilot review (per 76):** CRF §22.1 metric pack vs targets.
- **Leadership quarterly:** funnel, outcomes, fairness, risk-register deltas (78).

Implementation: event stream → warehouse (P4-free), dbt-style versioned metric definitions matching this document; metric changes are PR-reviewed (definitions are contracts for 76 exit criteria).
