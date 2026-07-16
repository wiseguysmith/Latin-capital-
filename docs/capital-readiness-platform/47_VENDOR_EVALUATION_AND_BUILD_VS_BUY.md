# 47 — Vendor Evaluation and Build-vs-Buy

| Field | Value |
|---|---|
| Purpose | Build-vs-buy positions per capability, vendor evaluation criteria, and LatAm-coverage validation checklist |
| Audience | Engineering, ops, compliance, finance |
| Status | Draft v1.0 |
| Version | 1.0.0 |
| Owner | Engineering lead + Ops |
| Dependencies | 42 §5, 44 §6, 30 |
| Source references | CRF §17 (vendor examples "not endorsements"), §17.1 |
| Assumptions | ASM-06, ASM-15 |
| Open questions | OQ-05, OQ-07 |
| Approval required | Engineering lead; compliance for KYC/sanctions vendors |
| Last updated | 2026-07-16 |

## 1. Build-vs-buy positions

| Capability | Position | Rationale |
|---|---|---|
| Scoring engine, rules/gates, workflow, checklists | **Build** | the defensible core; must be deterministic, versioned, auditable (20) |
| Evidence taxonomy/checklist logic | Build (config) | CRF-specific IP |
| Document storage | Buy (cloud object store) | commodity + object-lock |
| OCR/document AI | Buy (ASM-15) | vendor quality >> self-hosted at pilot scale; dual-vendor fallback for critical extraction (CRF §17 "dual validation for critical data") |
| LLM reasoning | Buy (hosted API) | no-training terms mandatory |
| AuthN/IdP | Buy | commodity, security-critical |
| E-signature | Buy | legal validity per OQ-06; local qualified providers considered |
| KYC/KYB + sanctions screening | Buy (Phase 2+/4; manual protocol first) | list coverage + CR/PA/SV document support decisive (OQ-07) |
| Email delivery | Buy | commodity |
| Malware scanning | Buy | commodity |
| Analytics warehouse | Buy (managed) | P4-free stream only |
| Audit hash-chain | Build (thin) | simple, integrity-critical, no vendor lock |
| Matching engine | Defer (post-MVP) | D-18 |

## 2. Evaluation criteria (all vendors)

Weighted scorecard: LatAm coverage (CR/PA/SV specifically — documents, ID types, lists, language); accuracy benchmarks on our golden set (31 §8) for AI vendors; security posture + certifications; DPA terms (no-training/no-retention for AI; sub-processors; residency options — 44 §6); reliability SLA; pricing at pilot and 10× scale; exit path (data export, format portability); support quality; sanctions-list refresh cadence (screening vendors); breach history.

**Country-coverage validation (CRF §17.1 rule):** before naming any integration "supported": test real CR documents/IDs end-to-end; verify lawful-use rights for the data source in that country; verify data-residency handling; record results + date in the vendor register. Same process repeats per country activation (61/62).

## 3. Procurement process

Shortlist ≥2 per category → PoC on golden fixtures → security/DPA review (44 §6) → decision memo (ADR-linked, cost model) → contract with exit terms → vendor register entry + fallback documented (30 §8). Single-source acceptance requires explicit risk-register entry (78).

## 4. MVP procurement list (sequenced)

Phase 1: cloud, IdP, email, malware scan, e-sign. Phase 2: doc-AI/OCR + LLM (dual OCR fallback), analytics. Phase 3: watermarking/render tooling if not built. Phase 4: KYC/KYB, sanctions screening, open-finance (Belvo/Prometeo-class), accounting connectors, bureau access per OQ-05 — each gated by country-coverage validation.
