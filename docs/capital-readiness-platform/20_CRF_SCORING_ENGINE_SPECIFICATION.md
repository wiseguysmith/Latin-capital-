# 20 — CRF Scoring Engine Specification

| Field | Value |
|---|---|
| Purpose | Implementation-exact specification of the deterministic scoring engine: inputs, formulas, N/A handling, caps, bands, versioning, overrides, recalculation |
| Audience | Backend engineers, QA, framework owner |
| Status | Draft v1.0 |
| Version | 1.0.0 (scoring model v1.0.0) |
| Owner | Framework owner (methodology) + Backend lead (implementation) |
| Dependencies | 21 (control library), 22 (confidence), 23 (gates/caps/overrides), 24 (applicability) |
| Source references | CRF §4, §4.1–4.3, Appendix A, Appendix B; D-05, D-20 |
| Assumptions | ASM-07 (expiry), ASM-09 (override limits), ASM-16 (visibility) |
| Open questions | OQ-15 (Phase-0 calibration) |
| Approval required | Framework owner + R-SA for model publish |
| Last updated | 2026-07-16 |

## 1. Engine principles

1. **Pure and deterministic:** `score(assessment_inputs, model_version) → result` with no randomness, no clock reads (evaluation date is an input), no AI calls, no external I/O in the computation path.
2. **Reproducible forever:** inputs, model version, and result are persisted; recomputation must be bit-identical (NFR-15).
3. **Narrative AI may explain results; it may not alter formulas, thresholds, evidence status, caps, or overrides** (CRF §4.1).
4. **Decimal arithmetic** (D-20): computations in exact decimal (e.g., scaled integers or decimal type); rounding half-up to 1 decimal at dimension display, to integer at final score display only.

## 2. Inputs

```
AssessmentInput {
  assessment_id, evaluation_date,
  scoring_model_version,           // pins 21/23/24 config + 22 thresholds
  jurisdiction_overlay_version,
  applicability_profile_id,        // from 24: business type × financing module × structure
  controls: [{control_id, maturity: 0..4 | "NA",
              na_reason_code?,      // from controlled taxonomy 24 §4
              evidence_refs: [evidence_id], assessor_id, assessed_at}],
  gates: [{gate_id, status: pass|fail|enhanced_review|pending, disposition_ref}],
  flags: [{flag_id, severity, effect_binding, status}],
  evidence_confidence_inputs,      // per 22
  external_risk_context: {level: 1..5, factors[], mitigants[]},   // reported, never scored
  overrides: [{override_id, target, system_value, override_value, approver, expiry, reason_code}]
}
```

Validation before computation: every applicable control present (else `incomplete`); maturities integer 0–4; N/A only where profile permits; gate set complete.

## 3. Dimension and weight registry (scoring model v1.0.0, from CRF Appendix A)

| Dim | Name | Weight | Layer |
|---|---|---|---|
| A | Financial Information Quality | 15 | Core |
| B | Financial Capacity & Resilience | 20 | Core |
| C | Management & Governance | 12 | Core |
| D | Operations, Controls & Technology | 10 | Core |
| E | Legal & Corporate Readiness | 13 | Core |
| F | Compliance, Integrity & ESG/E&S | 10 | Core |
| G | Capital Request, Structure & Repayment | 14 | Module |
| H | Collateral / Alternative Support & Monitoring | 6 | Module |

Core subtotal 80; module subtotal 20; total 100. Control-level weights in 21 (sum check enforced at model publish: Σ control weights per dimension = dimension weight; Σ dimensions = 100).

## 4. Computation sequence

```
1. resolve applicability profile → active control set + weight reallocation map (24)
2. for each active control c:
     contribution(c) = weight'(c) × maturity(c) / 4          // weight' = post-reallocation weight
3. dimension_score(d) = Σ contributions of active controls in d
4. core_score = Σ dims A..F ; module_score = Σ dims G..H
5. raw_total = core_score + module_score                      // 0..100 decimal
6. apply caps (23 §5) in ascending cap-value order:
     capped_total = min(raw_total, min(applicable cap values))
     record each triggered cap {cap_id, pre_value, post_value}
7. apply approved, unexpired overrides (23 §7):
     overrides adjust specific control maturities or attach conditions;
     they NEVER edit weights or formulas; recompute steps 2–6 with
     override-adjusted inputs → published_total
     (system_total retained alongside; ASM-09 band-move limit validated here)
8. band = band(published_total): 90–100 Institutional Ready; 75–89 Capital Ready;
     60–74 Conditionally Ready; 40–59 Developing; 0–39 Not Ready
     // band edges inclusive lower bound; integer comparison after final rounding
9. evidence_confidence = grade per 22 (independent pipeline; never modifies score)
10. publication_eligibility:
     - any gate ≠ pass → NOT_PUBLISHABLE (status per gate rules 23 §3)
     - GATE-06 unmet → result labeled PRELIMINARY (REF-05)
     - confidence D → NOT_CERTIFIABLE: internal + borrower remediation view only (CRF §4.2)
     - confidence C → publishable as CONDITIONAL only
```

Output: `AssessmentResult {system_total, published_total, band, dimension_scores[], control_contributions[], caps_triggered[], overrides_applied[], confidence (22), publication_eligibility, model_version, computed_at, input_hash}`.

## 5. Worked example (test vector TC-SCORE-01 seed)

Established SME, working-capital module, unsecured (profile P-SME-WC-UNSEC, 24):
- H1–H4 N/A (unsecured), reallocation per 24 §3: H5 weight' = 6.0.
- Sample: A controls maturities (A1:3, A2:2, A3:3, A4:1, A5:1) → A = 4×.75 + 3×.5 + 4×.75 + 2×.25 + 2×.25 = 3 + 1.5 + 3 + 0.5 + 0.5 = **8.5/15**.
- Suppose other dims compute to B 12.0, C 8.0, D 6.5, E 9.75, F 6.0, G 9.5, H5 (maturity 2) = 6.0×0.5 = 3.0.
- raw_total = 8.5+12+8+6.5+9.75+6+9.5+3 = **63.25** → displayed 63, band Conditionally Ready.
- If financial statements unreconciled and material → CAP-01: capped_total = min(63.25, 59) = 59 → band Developing, cap recorded and displayed with reason.

## 6. Recalculation triggers

Recompute (new result version, same assessment) on: control assessment change; evidence verification/rejection/expiry affecting cited evidence; gate/flag status change; override approval/expiry; N/A ruling change. Recompute (new assessment) on: scoring-model or overlay version change (never silently rescore history — old results stand under old versions); reassessment lifecycle (14 §3). Every recomputation stores a result version with diff.

## 7. Score caps interaction rules

- Caps bind the **total** (or module subtotal where cap is module-specific, CAP-04) — they do not rewrite dimension scores; UI shows "score capped at X because Y" with pre-cap value visible to internal roles only.
- Multiple caps: lowest wins; all triggered caps recorded.
- Caps release automatically when their trigger condition resolves (evidence verified, disclosure remediated) and recomputation runs.

## 8. Scoring model versioning

Model bundle = {control registry + weights (21), applicability profiles + reallocation maps (24), caps/gates definitions (23), band edges, confidence thresholds (22), materiality parameters (21 §4)}. Semver: MAJOR = weight/band/gate changes; MINOR = new applicability profile, new N/A codes; PATCH = text anchors. Publish workflow: R-IA drafts → validation suite (sum checks, band continuity, simulation on historical assessments SCR-A13) → R-SA approves → active for **new** computations only. Historical versions immutable and loadable (NFR-15). Change governance per CRF §18 (impact analysis, documentation, communication).

## 9. Material-change rules (trigger reassessment_required, FR-SCORE-08)

Ownership change >10% or any UBO change; new debt/lien/guarantee; litigation initiated; revenue event beyond ±20% vs assessed forecast (borrower-reported); loss of material license/contract/customer >10% revenue; capital request amount change >10% or purpose change; any new S1/S2 flag; evidence class expiry breaching freshness minima (ASM-07). Parameters are model-version config.

## 10. Approval chain for methodology changes

| Change | Approvers |
|---|---|
| Weights, bands, gates, caps, formula | Framework owner + R-SA + documented impact analysis (MAJOR) |
| Applicability profiles, N/A taxonomy | Framework owner + R-SA |
| Confidence thresholds, materiality params | Framework owner (OQ-15 during Phase 0) |
| Text anchors, translations | Framework owner |

## 11. Engine test obligations (full plan 73)

TC-SCORE-01 golden vectors (≥1 per applicability profile); TC-SCORE-02 reproducibility (replay historical inputs across deploys); TC-SCORE-03 cap ordering & release; TC-SCORE-04 external-context isolation (context change ⇒ score unchanged); TC-SCORE-05 override limits & expiry; TC-SCORE-06 N/A misuse rejection; TC-SCORE-07 weight-sum validation on model publish; TC-SCORE-08 decimal/rounding edges (e.g., 89.5 → 90 Institutional? No: rounding half-up 89.5→90, band inclusive → verify intended: band applied AFTER rounding, so 89.5 rounds to 90 = Institutional Ready; framework owner must confirm this edge in Phase 0 — flagged OQ-15).
