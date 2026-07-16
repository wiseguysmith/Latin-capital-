# 78 — Risk Register

| Field | Value |
|---|---|
| Purpose | Living register of product, legal, operational, AI, security, and commercial risks with owners and mitigations |
| Audience | Leadership, risk & compliance committee |
| Status | Active — reviewed quarterly (50 §3) |
| Version | 1.0.0 |
| Owner | Founder (register), owners per row |
| Dependencies | 45 (incidents), 64 (legal), 37 (signals) |
| Source references | CRF §21 (risks & limitations — primary source), §18 |
| Approval required | Risk & compliance committee |
| Last updated | 2026-07-16 |

Scales: Likelihood L1–L5, Impact I1–I5. Top exposure first.

| ID | Risk | L×I | Mitigations (doc refs) | Early signals (37) | Owner |
|---|---|---|---|---|---|
| RISK-01 | Readiness mistaken for creditworthiness by borrowers, providers, or press (CRF §21) | 4×5 | limitation statement everywhere (02 §3); provider attestation (54); copy lint (73 §6); training (54 §3); no predictive claims (76 §7) | provider language in RFIs/EOIs; press mentions; borrower complaints | Compliance |
| RISK-02 | Regulatory perimeter breach / recharacterization (platform seen as broker/intermediary) (CRF §21) | 3×5 | GATE-07; counsel memos per country+fee model (64 A1–A4); boundaries (02 §2); fee decisions gated (OQ-01) | counsel watch items; regulator inquiries | Founder+Counsel |
| RISK-03 | Cross-border privacy/security violation (UBO/PII) (CRF §21) | 3×5 | 44 program; OQ-04 resolution pre-data; DPAs; consent ledger; DLP canaries | DLP hits; DSR complaints | Security |
| RISK-04 | Score gaming / document theater (CRF §21) | 4×4 | reconciliation + cross-checks (FR-AI-03); attestation; anomaly watch (28 §6); red team (74 §3); sampling (52 §4) | exception patterns; rapid resubmits | Framework owner |
| RISK-05 | Poor/fraudulent data passing review | 3×5 | evidence tiers + confidence (22); GATE-04 protocol (56 §3); independent verification paths | verification miss rate; post-approval discoveries | Compliance |
| RISK-06 | AI hallucination/drift contaminating assessments (CRF §21) | 3×4 | deterministic core (31 §2); citation rules; verification queues; regression gates; incident rollback (45 §5) | correction rates; citation failures | AI lead |
| RISK-07 | Certification/reliance liability (provider claims damages) (CRF §21) | 2×5 | reliance limitations in terms (64 P1/P2); insurance (OQ-12); "Report not Certificate" in MVP (REF-02); honest validation limits | legal notices | Founder |
| RISK-08 | Conflicts from fees distorting scores (CRF §21) | 2×5 | D-15 firewall; FR-SCORE-10; analyst comp rules; override monitoring; disclosure (FR-ADM-03) | override direction bias; complaint patterns | Founder |
| RISK-09 | Bias/exclusion in outcomes (CRF §21) | 3×4 | context separated from score (P2/FR-SCORE-05); fairness reporting (37); appeal path (57); no protected-class features (31 §8) | segment gaps | Framework owner |
| RISK-10 | Stale scores relied upon (CRF §21) | 3×3 | expiry + material-change rules (20 §9); auto-pause of opportunities (14 §6); freshness sweeps | stale-view attempts | Ops |
| RISK-11 | Lender heterogeneity — packages don't fit provider policies (CRF §21) | 3×3 | core standard + disclosed conditions; design-partner feedback loops (76); provider overlays post-MVP (80) | decline reasons "mandate-fit" | Product |
| RISK-12 | Over-standardization penalizing valid business models (CRF §21) | 3×3 | applicability profiles + N/A taxonomy (24); expert review; periodic consultation (methodology board) | appeal patterns by type | Framework owner |
| RISK-13 | Weak outcome data → unvalidated claims (CRF §21) | 4×3 | conservative claims (76 §7); design-partner outcome reporting (54); publish limits | sparse ENT-29 data | Product |
| RISK-14 | Pilot supply failure (too few credible borrowers/providers) | 3×4 | partner-network recruitment (76 §2); remediation loop converts near-misses (28); waitlist | funnel metrics | Founder |
| RISK-15 | Key-person dependency (framework owner, R-SA) (mirror of C3) | 4×3 | documentation-first (this suite); cross-training; workbook redundancy | — | Founder |
| RISK-16 | Vendor failure/lock-in (LLM/OCR/KYC) | 3×3 | dual-vendor OCR; manual fallbacks (42 §5.4); exit terms (47); breaker (30 §7) | vendor SLO breaches | Eng lead |
| RISK-17 | Cost overrun on AI at scale | 3×2 | budgets + breaker (NFR-20); model tiering (30 §7) | cost dashboard | AI lead |
| RISK-18 | SME digital/document capacity lower than assumed → funnel stalls | 4×3 | coaching UX (28); partner referrals (58); realistic checklist tiers by amount band (24 §7) | first-pass completeness | Product |
| RISK-19 | Small-team segregation failures (12 §6 vs ASM-02) | 3×4 | system-enforced self-approval blocks; conflict declarations; hire trigger (50 §6) | segregation 403s; audit sampling | Ops |
| RISK-20 | Restricted-flag or P4 leakage to external users | 2×5 | disclosure lint (Sev-1 tests 73 §4); audience filter tests (27 §6); DLP | lint blocks; canary hits | Security |

## Process

New risks via any lead → triage at monthly ops review → owner + mitigation assigned; quarterly full review re-scores L×I; risks materializing → incident process (45 §5) + register update; closed risks archived, never deleted.
