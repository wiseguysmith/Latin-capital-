# 23 — Gates, Flags, Caps, Conditions, and Overrides

| Field | Value |
|---|---|
| Purpose | Complete specification of eligibility/integrity gates, red-flag model (severity, resolution, visibility), score caps, conditions, and override governance |
| Audience | Backend engineers, reviewers, compliance, QA |
| Status | Draft v1.0 (scoring model v1.0.0) |
| Version | 1.0.0 |
| Owner | Framework owner + Compliance lead |
| Dependencies | 20 (engine), 21 (triggers), 12 (visibility/permissions), 56 (SOP) |
| Source references | CRF §3.1 (gates), §4.3 (caps/overrides), §7 (enhanced-review triggers); master prompt §5.5; D-12 |
| Assumptions | ASM-09 (override limits) |
| Open questions | — |
| Approval required | Framework owner + compliance |
| Last updated | 2026-07-16 |

## 1. Concept map

- **Gate** — binary/tri-state precondition (pass / fail / enhanced_review / pending). Any non-pass blocks result publication (not internal computation).
- **Red flag** — recorded issue with severity S1–S4 and an **effect binding**: gate-linked, cap-linked, condition, or warning. Flags are workflow objects with owners and resolution requirements. **A red flag is never an automatic rejection** (D-12).
- **Cap** — deterministic ceiling on the published score while its condition holds.
- **Condition** — disclosed requirement attached to `approved_with_conditions`.
- **Override** — governed human deviation from a system outcome.

## 2. Eligibility & integrity gates (CRF §3.1)

| ID | Gate | Minimum requirement | Default treatment | Resolver |
|---|---|---|---|---|
| GATE-01 | Legal existence | Active legal entity; authority + signatories verified | fail/suspend until verified | R-LR (evidence) + R-CR (identity) |
| GATE-02 | Beneficial ownership | Natural-person UBOs + control chain identified & verified to applicable standard | enhanced_review or fail if unresolved | R-CR |
| GATE-03 | Sanctions / prohibited parties | Borrower, UBOs, directors, guarantors, material counterparties screened | block + escalate; **no automated clearance of potential matches** | R-CR (maker-checker for dispositions, 12 §6) |
| GATE-04 | Material misrepresentation / suspected fraud | Conflicting docs, manipulated statements, identity anomalies, fabricated contracts investigated | suspend; independent review | R-SA + R-CR |
| GATE-05 | Consent & lawful data use | Valid authorization for access, screening, sharing, retention | no processing without lawful basis | system-enforced + R-CR audit |
| GATE-06 | Minimum core dataset | Required identity, financial, debt, tax, legal, UoP, bank info present | assessment may be PRELIMINARY; no publication (REF-05) | R-FR |
| GATE-07 | Regulatory perimeter | Platform role + partner licensing path confirmed for transaction/jurisdiction | route to counsel; no orchestration beyond permitted scope | R-SA + counsel (64) |

Gate evaluation: continuous (any state change re-evaluates); status displayed on SCR-A8; borrower sees plain-language blocker for GATE-01/02/05/06 classes; GATE-03/04 investigations may be **restricted-visibility** (§6).

## 3. Gate effects on lifecycle

Non-pass gate → assessment cannot enter `final_review` approval outcome; existing published opportunity → auto-`paused`. GATE-04 suspected-fraud → assessment `suspended` + restricted flag + senior review (56). Enhanced_review status permits work to continue but blocks approval until dispositioned.

## 4. Red-flag model

```
Flag {id, source (system-rule | reviewer | AI-proposed→human-confirmed | screening),
 title, description, severity S1–S4, effect_binding, related (controls, evidence, gates),
 resolver_role, required_resolution_evidence, status (open|in_remediation|resolved|accepted-with-condition|expired),
 visibility {borrower: full|summary|hidden, provider: disclosed|hidden, internal: full, restricted: bool},
 created_by, timestamps, resolution_note}
```

| Severity | Meaning | Default effect binding | Who may resolve | Examples (CRF §4.3, §7) |
|---|---|---|---|---|
| S1 Critical | Integrity/eligibility threat | Gate link (block publication) | R-CR/R-SA only | Unverified legal existence; unresolved sanctions; fraud indicators; unverified UBO |
| S2 High | Material reliability/legal issue | Cap link or no-certification | Dimension reviewer + R-SA confirm | Unreconciled financials (CAP-01); undisclosed debt/liens (CAP-03); material litigation; material tax arrears; missing repayment source (CAP-02); material contradictions |
| S3 Medium | Material but manageable | Condition on approval | Dimension reviewer | High customer concentration unmitigated; expiring license; stale appraisal |
| S4 Low | Noteworthy | Warning (no score effect) | Any reviewer | Minor doc defects; approaching freshness limits |

Rules: severity may be raised by any reviewer with reason; lowering severity or closing S1/S2 requires maker-checker (proposer ≠ approver). AI may **propose** flags with citations; a human confirms before the flag has any effect (D-13).

## 5. Score caps (CRF §4.3)

| ID | Trigger condition | Cap / action | Release condition |
|---|---|---|---|
| CAP-00 | Any GATE-01/02/03/04 unresolved | No score publication; suspend & escalate | gate pass/disposition |
| CAP-01 | Financial statements do not reconcile to bank/tax/source systems (material per 21 §4) | Published total ≤ 59 (Developing) unless clearly immaterial and disclosed | reconciliation verified |
| CAP-02 | No credible repayment source / schedule | Published total ≤ 59 regardless of documentation quality | repayment source evidenced (G3/B1) |
| CAP-03 | Material debt, liens, litigation, tax arrears, or guarantees not disclosed (discovered ≠ declared) | No certification/publication until independently resolved | independent resolution + disclosure |
| CAP-04 | Missing essential financing-module evidence (per 25 module manifests) | Module-specific cap: affected G/H controls floor at 0 **and** assessment flagged "module incomplete — max band Conditionally Ready (74)"; exact missing item disclosed | module evidence verified |
| CAP-05 | Evidence Confidence D | No publication (22 §7) | grade ≥ C |
| CAP-06 | Evidence Confidence C | Conditional publication only | grade ≥ B for unconditional |

Engine mechanics: 20 §4 step 6 and §7. All triggered caps visible internally with pre-cap value; borrower sees the cap reason and release path (SCR-B12); providers see disclosed caps as key risks.

## 6. Visibility & disclosure rules (D-12)

| Audience | Sees |
|---|---|
| Borrower | All flags except `restricted`; plain language; required resolution evidence; conditions on approval. Hidden flags have **no placeholder** (no "1 hidden issue"). |
| Internal | Everything, including restricted. |
| Capital provider | Disclosed set chosen at packaging (55): all S2+ flags affecting the published result **must** be disclosed or publication is blocked (disclosure lint FR-OPP-01); S3 conditions disclosed; S4 optional. Restricted flags never disclosed. |
| Auditor | Everything in scope, read-only. |

**Restricted class** (legally/investigatively hidden — e.g., active fraud investigation, sanctions disposition in progress, law-enforcement request): creation requires R-CR + R-SA co-sign; auto-review every 30 days; while a restricted S1 exists, the assessment cannot be approved or published (so restriction never enables hidden-risk publication).

## 7. Override governance (CRF §4.3, §18)

Scope: an override may (a) adjust a control maturity with justification, (b) accept an exception (e.g., declare CAP-01 delta immaterial), (c) waive a **conditional** gate outcome (never GATE-03/04 substance), or (d) attach/remove conditions. An override may never: edit weights/formulas/bands, clear a sanctions match, fabricate evidence status, or move the published band by more than one band vs system result (ASM-09; beyond that requires founder-level approval recorded as exceptional).

Workflow (SOP 56): proposer (reviewer) → reason code (taxonomy: evidence-nuance, timing, jurisdiction-specific, immateriality, other-documented) + supporting evidence + proposed expiry (≤ assessment expiry) → R-SA approval (proposer ≠ approver) → applied at engine step 7 with **system result retained and visible internally alongside published result** — never silently overwritten (CRF §4.3). Expiry lapses the override → recompute → possible reassessment_required. Monitoring: override rate/direction/reviewer reported monthly (37; CRF §22.1 treats high rates as framework weakness).

## 8. Conditions

Condition = {text, source (flag/cap/confidence/reviewer), owner (borrower action vs provider awareness), due (date or event), status}. `approved_with_conditions` requires every condition enumerated and visible to borrower; provider package lists conditions verbatim (27 §5). Condition breach after approval → material-change evaluation (20 §9).
