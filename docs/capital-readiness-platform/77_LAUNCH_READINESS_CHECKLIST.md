# 77 — Launch Readiness Checklist

| Field | Value |
|---|---|
| Purpose | Gate checklist before (a) first real borrower data, (b) pilot launch, (c) provider publication — nothing launches with open items in its gate |
| Audience | All leads |
| Status | Active checklist |
| Version | 1.0.0 |
| Owner | Head of Product |
| Dependencies | 44, 64, 73–76 |
| Source references | Master prompt §20 quality bar |
| Approval required | Founder sign-off per gate |
| Last updated | 2026-07-16 |

## Gate A — before any real borrower data (Phase 1 start)

- [ ] Security baseline SB-01…SB-18 verified with evidence (44 §2); pen test scheduled
- [ ] Audit spine live: events + chain verification + viewer (36)
- [ ] Permission matrix tests green incl. tenancy + segregation (73)
- [ ] Consent flows live with counsel-validated texts ES (CR-L3/OQ-14) — **hard block if texts unvalidated**
- [ ] Privacy notice + fee disclosure pages published (FR-ADM-03)
- [ ] Retention config + legal-hold mechanism operative (35)
- [ ] Incident response plan + on-call + breach comms drafted (45 §5); DR restore drill passed
- [ ] Insurance placed (OQ-12) or founder-accepted risk memo on file
- [ ] Scoring model v1.0.0 frozen, golden vectors green, dual-control publish exercised (20 §8)
- [ ] SOPs 51–53 rehearsed by named staff; SLAs configured (50 §5)
- [ ] Copy lint (prohibited terms) enforced in CI (73 §6)

## Gate B — pilot launch (cohort B intake)

- [ ] Phase-1 exit criteria met (75 §3) incl. 10-applicant shakedown
- [ ] Limitation statement present on every score surface/artifact (AC-G4 evidence)
- [ ] Borrower report + action plan reviewed by framework owner on 3 samples
- [ ] Complaint/appeal process staffed (57); status page/support channel live
- [ ] Pilot agreements + expectation-setting materials counsel-reviewed (76 §2)
- [ ] Pilot metrics dashboard live with Phase-0 baselines (37)
- [ ] Pen test completed; highs remediated (SB-13)
- [ ] AI (if Phase 2 features on): adversarial suite green (74 §2); thresholds at 31 §5 defaults; cost breaker tested

## Gate C — provider publication (Phase 3)

- [ ] **CR-L1 regulatory-perimeter memo accepted by risk committee (64) — absolute block**
- [ ] Provider agreement template final (54 §2) incl. independent-underwriting attestation
- [ ] ≥3 providers onboarded with KYB + screening complete
- [ ] Disclosure lint verified against seeded restricted-flag fixtures (Sev-1 class test)
- [ ] Watermarking + download tracking verified on real files (SB-15)
- [ ] Consent → grant → revocation E2E drill within SLAs (NFR-09)
- [ ] Access-expiry sweeps verified (ASM-12)
- [ ] Package samples reviewed by counsel for claims/language (02 §3)
- [ ] Outcome-recording flow tested with a design partner
- [ ] Re-screen at publication (identity/sanctions) wired (ASM-07)

## Quality-bar verification (master prompt §20 — sign-off table)

| Bar | Verified by | How |
|---|---|---|
| Engineers build without inventing business rules | Eng lead | build retro: zero undocumented rule decisions (else docs updated first) |
| Designers wireframe from specs | Design lead | 18 coverage audit |
| AI does only what 31 permits | AI lead | 74 §2 + arch review (41 §2) |
| Deterministic scoring implemented exactly | Framework owner | golden vectors + 20 §11 |
| Ops can run manually | Ops lead | Phase-0/1 SOP rehearsals |
| Reviewers know approve/reject/escalate/override | R-SA | SOP sign-offs 51–56 |
| Security understands sensitive-data model | Security lead | 44 review + DLP canaries |
| Counsel has precise jurisdictional questions | Compliance | 64 status board current |
| QA builds from ACs | QA lead | 79 coverage ≥100% of P0/P1 |
| CRF rules traceable | Framework owner | 79 matrix audit |
| Readiness ≠ underwriting everywhere | Compliance | copy lint + artifact audit |
| Confidence ≠ readiness everywhere | Framework owner | UI/report audit |
| Human approval visible & enforceable | R-SA | workflow tests (D-07 set) |
| Architecture expands beyond CR without rebuild | Eng lead | 63 §3 CI checks + PA/SV pre-drafts compile against schema |
