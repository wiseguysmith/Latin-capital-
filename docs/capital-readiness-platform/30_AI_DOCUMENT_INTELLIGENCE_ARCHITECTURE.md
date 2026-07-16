# 30 — AI Document Intelligence Architecture

| Field | Value |
|---|---|
| Purpose | Technical architecture of the two-stage AI analysis system: pipelines, components, queues, retries, fallbacks, cost/latency controls |
| Audience | AI engineers, backend engineers |
| Status | Draft v1.0 |
| Version | 1.0.0 |
| Owner | AI lead |
| Dependencies | 31 (governance — binding), 32 (schemas), 36 (events), 41 (service boundaries) |
| Source references | CRF §16 (12-layer architecture), §16.1 (use-case matrix); D-04, D-13 |
| Assumptions | ASM-15 (hosted LLM + document-AI vendor, no-training terms) |
| Open questions | vendor selection per 47 |
| Approval required | AI lead + security |
| Last updated | 2026-07-16 |

## 1. Position in the system

The document-intelligence subsystem implements CRF §16 layers 2 (document intelligence), 3 (financial normalization, jointly with deterministic services), and 8 (LLM reasoning) — and feeds layers 5–7 and 10. It is **advisory**: every output lands either in a deterministic validator or a human queue; it holds no state-machine authority (31 §3).

## 2. Components

| Component | Function | Type |
|---|---|---|
| Ingestion worker | hash, store immutable original, metadata, malware scan orchestration | deterministic |
| OCR/layout service | text + tables + layout, per-page; vendor doc-AI (ASM-15) with self-hosted OCR fallback | ML vendor |
| Classifier | document type (26 registry) + language detection | LLM/ML w/ confidence |
| Extractor | schema-constrained field extraction per document type (32) with field confidence + page/bbox provenance | LLM structured output |
| Normalizer | units, currency, dates, chart-of-accounts mapping to canonical financial model (32 §3) | deterministic + ML-assist mapping proposals |
| Reconciler | cross-document comparators (FR-AI-03): statements↔bank↔tax↔debt↔ownership | deterministic rules |
| Defect detector | missing pages, wrong period, unsigned, illegibility, entity mismatch (26 §5 taxonomy) | rules + LLM assist |
| Contradiction/staleness detector | material conflicts + freshness breaches | rules + LLM assist |
| Clarification drafter | borrower questions from defects/exceptions | LLM draft → human approve |
| Narrative drafter | explanations, summaries, reports (27), action cards (28) — always with citations | LLM draft → human approve (MVP) |
| Verification router | thresholds → human queues (52) | deterministic |

## 3. Stage 1 — per-document pipeline (D-04; SLO ≤10 min P95, NFR-04)

```
upload → scan/quarantine → OCR/layout → classify
  → conf ≥ T_class(0.85): auto-accept type | else: human classification task
→ extract (schema for type) → validate against schema + sanity rules
→ normalize → map fields to controls (21 evidence links)
→ defect detection → provisional document confidence (22 §4.1 inputs)
→ route: material fields or conf < T_field(0.90) or defects → verification queue
→ borrower feedback event (recognized type, period, issues, next steps)
```

All thresholds are config (31 §5). Borrower feedback is generated from a fixed template + validated fields only — no free generation toward borrowers at this stage.

## 4. Stage 2 — full-package analysis (D-04)

Trigger: checklist coverage ≥ config (default 85% of required items verified/uploaded) or manual (FR-SCORE / 14 §3). Steps: assemble verified field graph → run reconciler suite → contradiction register (exceptions ENT-15) → staleness sweep → completeness vs checklist → **hand off to deterministic scoring engine (20)** → AI drafts: internal reviewer report sections, borrower explanations, provider summary drafts, action plan cards → all queued for human review (53). The scoring engine never waits on AI: if drafting fails, numbers still publish to internal queues.

## 5. Human-in-the-loop integration

Queues (SCR-A5/A6) receive: classification fallbacks, low-confidence/material fields, defects, exceptions, staleness, AI-proposed flags. Verification writes authoritative values with provenance `human_verified`; corrections are captured as labeled pairs (model feedback set, 31 §8). No AI output reaches a provider surface without human approval upstream (packaging 55).

## 6. Provenance model

Every extracted field: `{value, source_doc_id, page, bbox, extractor_version, model_id, confidence, verified_by?, verified_at?}` (CRF §16 layer 2 "field-level confidence and bounding-box provenance"). Narrative artifacts store citation lists (field refs + doc refs); render layer turns them into evidence chips (17 §3).

## 7. Cost & latency controls (NFR-20)

Per-document budget: 1 OCR pass + ≤2 extraction attempts + ≤1 repair attempt. Model tiering: small/cheap model for classification & routine extraction; large model for complex tables, contradictions, narratives. Caching by document hash (re-upload of identical file reuses results). Monthly cost cap → circuit breaker degrades to manual queues with ops alert (never silent). Batch (non-urgent) lanes for re-analysis after model upgrades.

## 8. Retry & failure handling

| Failure | Policy |
|---|---|
| OCR/vendor 5xx | retry ×3 exponential (1/4/16 min) → fallback OCR → manual task |
| Schema-invalid LLM output | 1 repair prompt w/ validator errors → fail to manual entry task |
| Low classification confidence | human task (never guess-accept) |
| Timeout (>10 min pipeline) | park + notify borrower "still processing", ops alert at 30 min |
| Hallucination signals (value absent from source per verifier spot-check) | field rejected, incident logged (31 §8), extractor version flagged |

## 9. Model/prompt versioning

Prompts, schemas, model IDs, and thresholds are a versioned bundle (`doc-intel vX.Y.Z`) in the model registry (31 §7); every stored output records its bundle version. Upgrades run shadow-mode on the regression set (31 §8) before activation; re-extraction of live documents only by explicit ops action (versioned, never destructive).
