# 70 — Epics, Features, and User Stories

| Field | Value |
|---|---|
| Purpose | Epic/feature/story breakdown with acceptance-criteria references, priorities, and discipline tags |
| Audience | Product, engineering, QA |
| Status | Draft v1.0 |
| Version | 1.0.0 |
| Owner | Head of Product |
| Dependencies | 15 (FRs), 71 (backlog order), 72 (ACs), 79 (traceability) |
| Source references | Master prompt §17; all FR-* |
| Assumptions | ASM-02 (team) |
| Open questions | — |
| Approval required | Product owner |
| Last updated | 2026-07-16 |

Priorities: **P0** operate MVP safely · **P1** credible pilot · **P2** post-pilot value · **P3** future. Discipline tags: PRD/DES/FE/BE/AI/DATA/SEC/DEVOPS/OPS/LEGAL/COMPL. Every story's detailed acceptance criteria live in 72 under its ID (samples inline). Stories trace to FRs (79).

## EP-01 Platform foundation (P0; BE/SEC/DEVOPS)
- US-101 As an engineer I can deploy via CI/CD to isolated envs with IaC. (46) P0
- US-102 As security lead, the security baseline SB-01…18 is verifiably in place before real data. (44) P0
- US-103 As any service, all state changes emit hash-chained audit events. (36) P0 — AC sample: chain verification job passes daily; tamper test detected in staging drill.
- US-104 As R-IA I manage orgs/users/roles per the permission matrix. (12) P0
- US-105 As a user I authenticate with MFA and sessions per 43. P0

## EP-02 Intake & screening (P0; FE/BE/OPS)
- US-201 As R-PA I complete and submit a preliminary application with save-resume. (FR-APP-01) P0
- US-202 As R-AR I screen applications through the 51 checklist and decide with reason codes. (FR-APP-02) P0
- US-203 As R-PA I track my application status and answer RFIs. (SCR-P8) P0
- US-204 Acceptance auto-provisions org, workspace, checklist shell. (FR-APP-03) P0
- US-205 As R-AR I manage a waitlist with auto-expiry. (FR-APP-04) P1

## EP-03 Capital workspace (P0; FE/BE)
- US-301 Profile capture with overlay-driven fields + completeness. (FR-WS-01) P0
- US-302 Ownership/UBO structured capture with graph validations. (FR-WS-02) P0
- US-303 Capital request with UoP sum checks + repayment-source requirement. (FR-WS-03) P0 — AC sample: submit blocked until UoP = amount ±1%; CAP-02 warning shown when repayment source empty.
- US-304 Deterministic checklist generation from profile×module×overlay. (FR-WS-04) P0
- US-305 Tasks & deadlines with object-bound completion. (FR-WS-05) P1
- US-306 Report-a-change flow triggers material-change evaluation. (FR-SCORE-08) P1

## EP-04 Document room (P0; FE/BE/SEC)
- US-401 Secure upload with scan, hash, immutable original. (FR-DOC-01) P0
- US-402 Versioning + supersede chain. (FR-DOC-02) P0
- US-403 Reviewer verify/reject with defect taxonomy + guidance. (FR-DOC-04) P0
- US-404 Document room organization, search, own-room export. (FR-DOC-05) P1
- US-405 Freshness sweeps + expiry reopening checklist items. (FR-DOC-06) P1

## EP-05 AI document intelligence (P1; AI/BE)
- US-501 Per-document pipeline: classify→extract→map→defects→confidence→route. (FR-DOC-03/FR-AI-01/02) P1
- US-502 Borrower per-document feedback within SLO. (NFR-04) P1
- US-503 Cross-document reconciliation comparators + exceptions. (FR-AI-03) P1
- US-504 Contradiction/staleness detection to exception queue. (FR-AI-04) P1
- US-505 Clarification drafting with human approval gate. (FR-AI-05) P1
- US-506 Narrative drafting (reports/plans) with citation enforcement. (FR-AI-06/07) P1
- US-507 Model registry, bundles, shadow eval, regression gate. (31 §7–8) P1
- US-508 Cost budget + circuit breaker degradation. (NFR-20) P1

## EP-06 Scoring & assessment (P0; BE/PRD)
- US-601 Deterministic scoring engine with golden vectors. (FR-SCORE-01) P0 — AC sample: TC-SCORE-01 vectors byte-identical across runs/deploys; no network in engine.
- US-602 Control assessment UI with anchors, citations, N/A rules. (FR-SCORE-02, SCR-A7) P0
- US-603 Gates board + enforcement of publication blocks. (FR-SCORE-04) P0
- US-604 Flags register with severity/effect/visibility incl. restricted. (FR-REV-03) P0
- US-605 Caps engine + borrower-visible release paths. (23 §5) P0
- US-606 Overrides workflow with maker-checker + register. (FR-SCORE-07) P0
- US-607 Evidence-confidence computation + display separation. (FR-SCORE-03) P1 (manual grade P0)
- US-608 External Risk Context capture, isolated from score. (FR-SCORE-05) P0
- US-609 Versioned scoring-model config with dual-control publish + simulation. (FR-SCORE-06, SCR-A13) P0
- US-610 Assessment lifecycle + final approval flow with segregation. (FR-REV-04) P0
- US-611 Borrower results screens + readiness report artifact. (FR-SCORE-09, 27 §3) P0
- US-612 Gap-remediation plan generation + tracking. (28) P1
- US-613 Assessment history + delta view. (SCR-B14) P1
- US-614 Expiry/reassessment automation. (14 §3) P1

## EP-07 Review operations (P0; FE/BE/OPS)
- US-701 Typed queues with SLA timers + assignment. (FR-REV-01) P0
- US-702 Document review workspace with extraction overlays. (SCR-A5) P1 (manual variant P0)
- US-703 AI exception queue with side-by-side resolution. (SCR-A6) P1
- US-704 Clarification lifecycle end-to-end. (FR-REV-02) P0
- US-705 Internal reviewer report artifact. (FR-REV-05) P1

## EP-08 Capital-provider workspace (P1; FE/BE/OPS/LEGAL)
- US-801 Provider onboarding records + agreement gating. (FR-CP-02, 54) P1
- US-802 Opportunity packaging with disclosure lint. (FR-OPP-01) P1
- US-803 Consent flow (R-BA) per named provider. (55 §3) P1
- US-804 Publication approval + grants with expiry. (FR-OPP-02) P1
- US-805 Provider dashboard/list/profile with watermarked rooms. (FR-CP-01) P1
- US-806 RFI flow with routing. (FR-OPP-03) P1
- US-807 EOI flow with non-binding acknowledgment + borrower response. (FR-OPP-04) P1
- US-808 Outcome recording + provider status tracking. (SCR-C9, ENT-29) P1

## EP-09 Notifications & comms (P0 core; FE/BE)
- US-901 Event-driven notification engine + matrix templates. (FR-NOT-01) P0
- US-902 Digest preferences + non-suppressible classes. (19 §1) P1

## EP-10 Compliance & audit (P0; BE/COMPL/SEC)
- US-1001 Consent ledger + revocation propagation. (ENT-24, NFR-09) P0
- US-1002 Audit viewer + object timelines + dual-approved export. (FR-AUD-01) P0
- US-1003 Access/download tracking + watermark on provider downloads. (FR-AUD-02) P1
- US-1004 Retention jobs + destruction certificates + legal holds. (35) P1
- US-1005 Manual screening protocol tooling (checklists, evidence capture). (51 §2.6) P0

## EP-11 Jurisdiction & config (P0; BE/COMPL)
- US-1101 Overlay schema + CR overlay v1 + pinning. (63, FR-ADM-02) P0
- US-1102 Counsel-signoff gating on overlay activation. (63 §3) P0

## EP-12 Analytics (P1; DATA)
- US-1201 P4-free event export + warehouse metric definitions per 37. P1
- US-1202 Methodology-health dashboard (bands/caps/overrides/confidence). P1
- US-1203 Pilot metric pack (CRF §22.1). P1

## EP-13 Legal & compliance validation (P0; LEGAL/COMPL — non-software)
- US-1301 CR perimeter memo (CR-L1) delivered and accepted. P0
- US-1302 Privacy texts + consents validated (CR-L3/OQ-14). P0
- US-1303 Provider agreement template finalized. (54 §2) P1
- US-1304 Insurance placed (OQ-12). P0

## Post-MVP parking lot (P2/P3)
Matching engine (D-18); portfolio monitoring/early warning; open-finance & accounting integrations; KYC vendor automation; bureau access (OQ-05); PA/SV activation (61/62); provider API/webhooks; certification mark program (REF-02); tokenization compatibility work (81).
