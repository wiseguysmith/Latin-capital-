# 31 — AI Governance and Model Risk

| Field | Value |
|---|---|
| Purpose | Binding governance for all AI use: permitted/prohibited matrix, thresholds, validation, hallucination controls, PII rules, evaluation, change management |
| Audience | AI engineers, engineering leads, compliance, auditors |
| Status | Draft v1.0 |
| Version | 1.0.0 |
| Owner | AI lead + Compliance (joint); framework owner for scoring-adjacent rules |
| Dependencies | 30 (architecture), 32 (schemas), 44 (data protection), 74 (AI test plan) |
| Source references | CRF §16 (architectural rule), §16.1 (use-case matrix), §18 (governance/model risk; SR 11-7 + NIST AI RMF anchors R7–R8); master prompt §6; D-13 |
| Assumptions | ASM-15 (vendor posture) |
| Open questions | — |
| Approval required | AI lead + compliance + R-SA |
| Last updated | 2026-07-16 |

## 1. Governing rule (CRF §16)

> Use AI to read, reconcile, explain, prioritize, and coach. Use deterministic services for calculations, gates, scoring, permissions, and audit. Keep humans responsible for exceptions, adverse decisions, legal interpretation, sanctions matches, and lender approval.

## 2. Deterministic-only functions (never AI-influenced at decision time)

Formulas, control weights, score calculations, applicability rules, readiness bands, hard gates, score caps, permissions, workflow state transitions, required-document rules, audit events, model-version selection, approval requirements (master prompt §6). Enforcement is **architectural**: the scoring engine and workflow service have no dependency on AI services (30 §4); CI forbids imports/calls from `scoring-engine`/`workflow` to AI clients (74 TC-AI-01).

## 3. Permitted / prohibited matrix (D-13; CRF §16.1)

| AI activity | Status | Control requirement |
|---|---|---|
| Document classification | Permitted | conf threshold, human fallback |
| Data extraction | Permitted | schema-constrained, provenance, human verification for material/low-conf fields |
| Evidence mapping to controls | Permitted | mapping table is deterministic config; AI proposes only for unmapped types |
| Summaries/explanations/report drafts | Permitted | citations mandatory; human approval before external release; generated-status disclosed |
| Contradiction & missing-info detection | Permitted | outputs are exceptions/tasks, not verdicts |
| Draft clarification questions | Permitted | human approval before send |
| Gap-remediation recommendations | Permitted | template-bounded; reviewer approval (28) |
| Reviewer prioritization | Permitted | ranking only; queues remain complete (nothing hidden) |
| Borrower coaching | Permitted | non-legal/non-investment bounds; template library |
| Financial analysis narration | Permitted w/ controls | calculations deterministic; AI describes drivers/anomalies |
| Fraud/anomaly detection | Assistive only | signals escalate; **never label fraud** without human investigation (GATE-04) |
| Lender/opportunity matching | Post-MVP | hard mandate filters first; explainable; no suitability claims |
| Approve applicant / final assessment / publication | **Prohibited** | human only (D-07) |
| Clear sanctions/PEP match | **Prohibited** | R-CR disposition only (GATE-03) |
| Legal determination / credit decision / pricing | **Prohibited** | outside platform function entirely (02) |
| Change weights, thresholds, gates, caps | **Prohibited** | model-governance process only (20 §10) |
| Override any gate/cap/score | **Prohibited** | human override workflow only (23 §7) |
| Decide who receives capital | **Prohibited** | always |

## 4. Structured output & hallucination controls

1. All extraction/mapping outputs validate against JSON Schemas (32); invalid → repair-once → manual.
2. **Citation-or-silence rule:** narrative claims must cite field/document refs; uncited factual sentences are stripped by the render layer.
3. Numbers in narratives are injected from the result object by template slots — the LLM never writes numerals for score/financial values (27 §1).
4. Verifier spot-check sampling: ≥10% of auto-accepted fields human-sampled weekly during pilot; error rate feeds thresholds (§5).
5. Refusal-to-answer is acceptable and routed to humans; guessing is a defect.

## 5. Confidence thresholds & human-review routing (config, versioned)

| Signal | v1.0 default | Action below threshold |
|---|---|---|
| Classification confidence | 0.85 | human classification |
| Field confidence (non-material) | 0.75 | flag for review batch |
| Field confidence (material fields per 21) | 0.90 | mandatory verification |
| Any GATE-relevant field (identity, UBO, sanctions inputs, debt) | always | mandatory verification regardless of confidence |
| Contradiction materiality | per 21 §4 | exception queue |

## 6. PII & data exposure rules (with 44)

- P4 fields (UBO identity, personal IDs) excluded from LLM prompts where the task permits; where identity extraction requires them, use vendor with no-training/no-retention contract terms + regional processing per OQ-04 (ASM-15).
- Prompts and outputs are logged **with P4 redaction** in the AI audit store; raw prompt logs retention 90 days, redacted logs per 35.
- Prohibited data exposure: never send another tenant's data in context; never include internal notes, restricted flags, or provider identities in borrower-facing generations; no cross-workspace context assembly.
- Prompt-injection posture: documents are untrusted input — system prompts instruct extraction-only; instruction-like content inside documents is flagged (74 adversarial suite tests this).

## 7. Model & prompt management

Model registry (CRF §18): every model/prompt/ruleset/data source registered with owner, purpose, limitations, dependencies, version. Bundles per 30 §9. Changes: shadow evaluation → regression pass (§8) → AI lead + R-SA sign-off (material changes) → staged rollout → post-change monitoring. Emergency rollback runbook in 45.

## 8. Evaluation, regression, monitoring

- **Golden datasets:** ≥50 labeled documents per top-10 evidence type (built during Phase 0/1 from pilot docs with consent), incl. Spanish-language and poor-scan cases; contradiction fixtures; adversarial fixtures (altered documents, injection attempts, look-alike entities).
- **Metrics:** per-type classification accuracy (target ≥95% on golden), field-level extraction precision/recall (≥97% precision on material fields), false-clean rate for defect detection, drafting citation-validity rate (100% enforced), human-correction rate trend (drift signal).
- **Regression gate:** no bundle ships if any material metric degrades >1pt vs current.
- **Drift & bias:** monthly drift report; fairness/access analysis per CRF §22.1 (completion/score/outcome patterns by lawful segments — geography, sector, size); protected-class features prohibited in any model input (CRF §18).
- **Incidents:** hallucination/leak/misroute → AI incident process (45 §5) + registry entry; repeated class of failure blocks the bundle.

## 9. Change governance & effective challenge (CRF §18)

Independent reviewer (not the builder) challenges: conceptual soundness, data quality, performance, stability, limitations — pre-launch and annually (SR 11-7 anchor). NIST AI RMF mapping: Govern (this doc), Map (30 §1–2 inventory), Measure (§8), Manage (§7, 45). Validation records are auditor-visible.

## 10. Cost, latency, retention

Budgets & circuit breaker per 30 §7 / NFR-20; latency SLOs NFR-04; model-output retention: outputs tied to assessments retained with the assessment (NFR-15 reproducibility incl. AI-assisted artifacts); orphan outputs 90 days.
