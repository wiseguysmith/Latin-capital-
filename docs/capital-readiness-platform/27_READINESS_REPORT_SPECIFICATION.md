# 27 — Readiness Report Specification

| Field | Value |
|---|---|
| Purpose | Exact content, structure, and rules for the three assessment artifacts: borrower report, internal reviewer report, capital-provider package |
| Audience | Product, engineers, reviewers, designers |
| Status | Draft v1.0 |
| Version | 1.0.0 |
| Owner | Framework owner + Head of Product |
| Dependencies | 20 (results), 22 (confidence), 23 (flags/conditions), 28 (plan), 55 (publication) |
| Source references | CRF §1 (five artifacts), §14 stage 7/10, Appendix B.1 (certificate fields); D-06 |
| Assumptions | REF-02 ("Report/Package" naming in MVP) |
| Open questions | OQ-14 (validated Spanish legal text) |
| Approval required | Framework owner; counsel for limitation/disclaimer text |
| Last updated | 2026-07-16 |

## 1. Common rules (all artifacts)

1. Numbers come **only** from the scoring engine result object; narrative sections are AI-drafted with citations and human-approved before release (FR-AI-06).
2. Every artifact carries: framework version, scoring-model version, overlay version, assessment date + expiry, reviewer identity (internal artifacts) or reviewing-institution statement (external), and the mandatory limitation statement (02 §3).
3. Artifacts are generated as immutable versioned renders (HTML + PDF) with SHA-256 hash recorded in audit (CRF App B.1 hash requirement); regenerating = new version.
4. Watermarks: borrower report "Prepared for <borrower>"; provider package "Prepared for <provider> — confidential" + per-download watermark (ASM-12).
5. Language: borrower ES (EN option); provider ES/EN per provider preference; internal EN.

## 2. Data spine (CRF Appendix B.1 fields — used by all artifacts)

Borrower legal name, jurisdiction, entity identifier, group/UBO status summary; assessment + expiration dates; framework/module/model versions; reviewer(s); total score + band; dimension scores; Evidence Confidence grade; External Risk Context level + disclosed factors; gate status; red flags/conditions/unresolved exceptions (per-audience filtered); evidence coverage/freshness/verification summary; capital request (purpose, amount, currency, tenor, repayment source, collateral/support summary); explicit limitation; document/package hash + audit reference.

## 3. Borrower Readiness Report (SCR-B15; released on approval — D-06)

| # | Section | Content rules |
|---|---|---|
| 1 | Cover & summary | Score (large), band + band meaning, confidence grade + one-line reason, validity dates, limitation statement |
| 2 | How to read this report | Plain-language explanation of score vs confidence vs external context (fixed, counsel-reviewed copy) |
| 3 | Dimension results | 8 dimensions: earned/possible, 2–3 sentence explanation each, strengths called out |
| 4 | What's holding the score back | Caps triggered (reason + release path), maturity-0/1 controls in plain language, missing evidence list |
| 5 | Flags & conditions | Borrower-visible flags (23 §6), conditions with owners/dues |
| 6 | Evidence confidence detail | Grade per dimension + top confidence-improving actions |
| 7 | External Risk Context | Level, factors, mitigants — labeled "not part of your score" |
| 8 | Action plan | Embedded gap-remediation plan (28) |
| 9 | Changes since last assessment | Score/band/dimension deltas, resolved/new flags (omit section on first assessment) |
| 10 | Methodology annex | Band table, maturity scale, tier definitions, model version |

Never in borrower report: internal notes, reviewer names (institutional signature only), restricted flags, provider identities.

## 4. Internal Reviewer Report (FR-REV-05; committee-style record)

Sections: assessment summary (spine); screening & gate dispositions with evidence links; per-dimension reviewer analysis (control grid: maturity + citations + notes); exceptions log & resolutions; contradiction register; flags incl. restricted; caps with pre-cap values; overrides (system vs published, approver, expiry); confidence computation breakdown; External Risk Context rationale; reviewer sign-offs + segregation attestation; recommendation to R-SA. Retention: audit class (35).

## 5. Capital-Provider Package (FR-OPP-01; published per 55)

| # | Section | Content rules |
|---|---|---|
| 1 | Standardized opportunity summary | 1 page: business snapshot, request (amount/purpose/tenor/structure), score+band+confidence, top strengths, key risks & conditions (disclosed set), external context level. Fixed layout for comparability |
| 2 | Business profile | Disclosed profile subset; ownership **summary** (structure + verified-status statement; UBO identities summarized, not documents — 12 §3) |
| 3 | Capital request detail | UoP lines, sources & uses, repayment structure & source, collateral/support summary |
| 4 | Readiness detail | Dimension scores + explanations; evidence-confidence per dimension; gates all-pass statement; disclosed flags/conditions verbatim (23 §6 lint) |
| 5 | External Risk Context | Level, factors, mitigants; explicit "separate lender input" note (CRF §3.2) |
| 6 | Evidence inventory | Approved-room index: type, period, verified date, tier |
| 7 | Boundaries & reliance | Limitation statement + reliance limitations + provider independent-underwriting obligation (54 agreement reference) |

Never in provider package: restricted flags, internal notes, other providers' existence/activity, borrower documents outside the approved room, raw P4 items.

## 6. Acceptance criteria (72 `AC-RPT-*` summary)

Render determinism (same result object → identical artifact); hash recorded; per-audience filter tests (restricted leak = Sev-1); limitation statement present; ES/EN parity; PDF accessibility (tagged).
