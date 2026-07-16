# 22 — Evidence Confidence Model

| Field | Value |
|---|---|
| Purpose | Computable model for Evidence Confidence at document, control, dimension, and assessment levels — always separate from readiness |
| Audience | Backend engineers, reviewers, framework owner |
| Status | Draft v1.0 (thresholds provisional — ASM-08) |
| Version | 1.0.0 |
| Owner | Framework owner |
| Dependencies | 20 (engine), 21 (controls), 26 (evidence taxonomy), 32 (extraction) |
| Source references | CRF §4.2 (grades), Appendix B (tiers E0–E3, evidence rules); D-11 |
| Assumptions | ASM-07 (freshness), ASM-08 (numeric model is a proposed computable stand-in for CRF's qualitative grades — REF-06) |
| Open questions | OQ-15 (calibration) |
| Approval required | Framework owner |
| Last updated | 2026-07-16 |

## 1. Invariants

1. Confidence **never** changes the 0–100 readiness score; it changes what may be **published/certified** (CRF §4.2 use column; 20 §4 step 10).
2. Confidence is displayed wherever the score is displayed (D-06), as a letter grade with explanation.
3. Confidence accounts for: source, authenticity, completeness, recency, reconciliation, independent verification, reliability, cross-document consistency (master prompt §5.4).

## 2. Grade semantics (CRF §4.2 — verbatim policy)

| Grade | Definition | Use |
|---|---|---|
| A High | Material fields source- or independently verified; financials reconcile; current and complete | Full publication eligible (subject to gates, human approval) |
| B Good | Most material fields verified; limited non-critical reliance on borrower-provided evidence | Publication with disclosed conditions |
| C Limited | Material gaps, stale info, weak reconciliation, heavy self-declaration | Preliminary/conditional result only |
| D Insufficient | Cannot support reliable assessment or unresolved contradictions | No publication; remediation required |

## 3. Evidence tiers (CRF Appendix B)

E0 unsubstantiated claim; E1 borrower-supplied document; E2 source-system/counterparty (bank data, registry extract, signed contract); E3 independent professional (audit/review, legal opinion, licensed appraisal, notary, regulator).

## 4. Computation (proposed numeric model, ASM-08)

### 4.1 Document-level confidence score (DCS, 0–100)

```
base(tier): E0=10, E1=40, E2=75, E3=95
adjust:
 −15 stale (past max age per evidence type, 26)         −8 approaching expiry (≥80% of max age)
 −10 incomplete (missing pages/sections detected)       −20 failed/absent authenticity check where required
 −10 expected reconciliation not performed              −25 unresolved material contradiction touching this doc
 +5  corroborated by ≥1 independent source (different tier-E2+ origin)
clamp 0–100
```
Authenticity checks per type (26): signatures present, issuer verification, registry cross-check, hash-source (API-ingested = auto-pass).

### 4.2 Control-level confidence (CCS)

Weighted average of the DCS of evidence items cited for the control, weighted by each item's relevance weight (config per control-evidence pair; default 1.0). Then rules:
- Any **unresolved material contradiction** among the control's evidence → CCS capped at 25 (forces D-region; contradictions are never averaged, CRF App B).
- Control at maturity 0 with "absent" note → CCS = n/a (excluded; absence is a readiness issue, not an evidence issue).
- Human verification of material fields (52) is recorded on the evidence and required for CCS > 60 on controls marked `material` (21).

### 4.3 Dimension-level = Σ(CCS × control weight)/Σ(weights) over applicable controls with CCS ≠ n/a.

### 4.4 Assessment-level grade

```
ACS = Σ(dimension confidence × dimension weight)/100
Grade A: ACS ≥ 80 AND every dimension ≥ 70 AND zero unresolved material contradictions AND identity/sanctions evidence current (ASM-07)
Grade B: ACS ≥ 65 AND every dimension ≥ 50 AND zero unresolved material contradictions
Grade C: ACS ≥ 45
Grade D: otherwise, OR any unresolved material contradiction, OR GATE-06 dataset unmet
```
Thresholds are scoring-model config (v1.0.0 provisional; calibrate in Phase 0 — OQ-15).

## 5. Freshness policy (ASM-07 defaults; per-type overrides in 26)

Identity & sanctions: at assessment approval and re-screened at publication. Bank/financial data ≤90 days at approval. Registry/lien searches ≤30 days at approval. Appraisals: RE ≤12m, equipment/inventory ≤6m. Corporate standing ≤30 days. Staleness after approval degrades the assessment (assessment.expired / reassessment_required rules, 14 §3; CRF §21 "stale score" safeguard).

## 6. Display rules

- Letter grade + one-line reason ("B — bank statements verified; tax filings pending independent verification").
- Dimension-level grades in internal + provider views; borrower sees grade + top confidence-improving actions (28 integrates them).
- Reports embed the grade adjacent to the score with the separation sentence: "Evidence Confidence describes how well the information is supported. It is separate from the readiness score."

## 7. Interaction with workflow

- Grade D → assessment cannot leave `final_review` as approved; only `not_ready` or return-to-remediation (20 §4 step 10).
- Grade C → approval allowed only as `approved_with_conditions` with confidence-remediation conditions listed.
- Publication re-check: grade recomputed at packaging; degradation below B requires R-SA re-approval (55).
