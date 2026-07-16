# 03 — Source of Truth and Assumptions

| Field | Value |
|---|---|
| Purpose | Define the authority hierarchy, record the framework interpretation, list contradictions/gaps found, and register all labeled assumptions |
| Audience | All contributors; reviewers resolving documentation conflicts |
| Status | Draft v1.0 |
| Version | 1.0.0 |
| Owner | Head of Product |
| Dependencies | 05 (decisions), 06 (open questions) |
| Source references | CRF v1.0 (entire document); Master prompt §2, §19 |
| Approval required | Product owner; counsel for ASM items marked (legal) |
| Last updated | 2026-07-16 |

## 1. Authority hierarchy

1. **CRF v1.0** (archived at `source/`) — readiness methodology.
2. **Resolved MVP product decisions** (05_DECISION_LOG.md) — MVP scope.
3. **Sound practice** (private credit, product, security, AI governance, software engineering) — gap filler only.
4. **Assumptions** — explicitly labeled `ASM-xx` here and cross-referenced where used.
5. **Jurisdictional statements** — provisional until validated by local counsel (64).

Conflict rule: if any document in this suite contradicts the CRF's methodology, the CRF wins; the deviation must be recorded in §3 below with a proposed refinement — never silently rewritten.

## 2. Framework interpretation (concise)

The CRF is a **six-layer pre-underwriting standard**: (1) eligibility & integrity gates, (2) an 80-point core readiness score across six dimensions (A–F), (3) a 20-point financing module across two dimensions (G–H), (4) an A–D evidence-confidence grade computed separately, (5) a 1–5 external-risk-context profile reported but never blended into the score, (6) a red-flag/conditions/override register with human sign-off. Scoring is deterministic: 45 controls (Appendix A) each rated on a 0–4 maturity scale; `contribution = weight × maturity ÷ 4`; bands 90–100/75–89/60–74/40–59/0–39. Evidence is tiered E0–E3 with freshness, contradiction, attestation, N/A, and reallocation rules (Appendix B). Certification is time-limited and expires on staleness or material change. AI reads, reconciles, explains, prioritizes, and coaches; deterministic services calculate and gate; humans own exceptions, adverse decisions, legal interpretation, sanctions matches, and approval (§16). Downstream underwriting always belongs to the capital provider.

**Note on dimension totals:** the master prompt lists the same eight dimensions and identical point totals as CRF §4; the CRF additionally splits them core (80) vs module (20). Both are preserved.

## 3. Contradictions, gaps, and refinements identified

Per the master prompt, original CRF rules are preserved; refinements are proposed separately.

| # | Issue | CRF position | Proposed refinement | Reason | Operational impact | Approval needed |
|---|---|---|---|---|---|---|
| REF-01 | H-dimension weights sum to 6.0 using fractional weights (1.5/1.5/1.5/1.0/0.5) | Appendix A | Keep fractional weights; scoring engine must support decimals; display rounding only at final score | Fidelity to CRF | Engine uses decimal arithmetic (20 §5) | Framework owner |
| REF-02 | CRF defines borrower "certification"; MVP decisions define admin-approved "assessment" + publication | CRF §14 stage 10 | MVP uses "Approved readiness assessment" language; "CRF Certificate" reserved for post-pilot once validation exists (CRF Appendix C note) | CRF itself says commercial claims need pilot validation | Certificate artifact ships as "Readiness Report" in MVP (27) | Product + counsel |
| REF-03 | CRF §14 includes automated matching/introductions; MVP decisions require manual publication to authorized providers | CRF §14 stages 11–12 | MVP: manual curation (55); matching engine deferred to post-MVP (80) | Human-supervised MVP (D-07) | No matching service in MVP scope | Product |
| REF-04 | CRF roadmap (Phases 0–7) vs prompt roadmap (Phases 0–4) | CRF §22 | Prompt phases control MVP sequencing; CRF phases mapped inside 75 §2 | Prompt is controlling for MVP scope | None — mapping table provided | Product |
| REF-05 | CRF gate "Minimum core dataset" allows "preliminary" assessment without certification | CRF §3.1 | Adopted: assessments below dataset threshold are labeled **Preliminary** and cannot be approved for publication | Consistency with D-08 | New assessment sub-status (14 §3) | Product |
| REF-06 | CRF does not define numeric thresholds for evidence-confidence grades | CRF §4.2 qualitative | Numeric computation model proposed in 22 §4 with provisional thresholds | Engineers need computable rules | Thresholds are scoring-model config, tunable in Phase 0 pilot | Framework owner (ASM-08) |
| REF-07 | CRF silent on multi-user borrower organizations, invitations, suspension | — (gap) | Defined in 11/12 per prompt §9 | Required for MVP | Standard org/RBAC model | Product |
| REF-08 | CRF silent on complaint/appeal SLA specifics | §18 requires appeal/correction | SLAs proposed in 57 | Operability | Ops staffing | Ops lead |

## 4. Assumption register

Conservative assumptions made to avoid blocking architecture. Each is used only where cited and is reversible.

| ID | Assumption | Basis / conservatism | Used in | Validation path |
|---|---|---|---|---|
| ASM-01 | Working product name is "Capital Readiness Platform"; brand TBD | Neutral; no marketing claim | all | Branding decision (OQ-03) |
| ASM-02 | Pilot team: 1 PM, 1 designer, 3–4 engineers, 1 AI engineer, 2–3 reviewers/ops, fractional compliance+counsel | Smallest team that satisfies segregation of duties (12 §6) | 75, 50 | Hiring plan |
| ASM-03 | MVP language: Spanish-first UI with English internal/admin; documents accepted in Spanish and English | Costa Rica-first reality | 10, 17, 60 | Pilot feedback |
| ASM-04 | Currency handling: USD and CRC captured natively; scores currency-agnostic; FX context reported in External Risk Context | CRF §8 currency row | 20, 33, 60 | Counsel/pilot |
| ASM-05 | Cloud deployment on a major provider with LatAm-acceptable data-residency posture; region selection pending counsel (data-localization) | Conservative: architecture keeps residency configurable | 40, 44, 46 | Counsel (OQ-04) |
| ASM-06 | MVP identity verification of individuals via a KYC vendor supporting CR/PA/SV documents; manual fallback allowed in Phase 1 | CRF §17 lists vendor classes | 43, 47, 60 | Vendor evaluation (47) |
| ASM-07 | Assessment validity: 6 months default expiry; bank/financial evidence ≤ 90 days old at approval; identity/sanctions re-screened at approval and publication | CRF Appendix B freshness row is qualitative; values chosen conservatively | 20, 22, 26 | Framework owner in Phase 0 |
| ASM-08 | Evidence-confidence numeric model and thresholds (22 §4) | Computable stand-in for CRF qualitative grades | 22, 20 | Phase 0 calibration |
| ASM-09 | Overrides expire at assessment expiry at the latest; max one level band movement per override without founder-level approval | CRF §4.3 requires expiry; magnitude limit is added conservatism | 23, 56 | Framework owner |
| ASM-10 | MVP billing handled manually (invoices) with fee disclosure; no in-product payments | Prompt §15 permits manual | 10, 15 | Commercial decision (OQ-01) |
| ASM-11 | Document maximum sizes 100 MB/file; PDF, DOCX, XLSX, CSV, PNG/JPG accepted; originals immutable | Ops practicality; CRF §16 immutable original | 26, 35 | Pilot |
| ASM-12 | Provider access expires 90 days after grant unless renewed; downloads watermarked with provider identity | CRF §15 stage 5 watermark/download controls | 12, 44, 55 | Product/counsel |
| ASM-13 | (legal) Business-purpose financing only; sole proprietors excluded from MVP to avoid consumer-protection perimeter | CRF §19 consumer/SME protections + §19.2 launch posture | 10, 60 | Counsel per country |
| ASM-14 | (legal) Platform contracts under a single operating entity; local entity requirements unresolved | Conservative: no country-specific promises | 60–62 | Counsel (OQ-02) |
| ASM-15 | AI stack: hosted LLM API with no-training/no-retention terms + dedicated OCR/document-AI vendor; PII redaction before LLM calls where feasible | CRF §16 controls; conservative data posture | 30–32, 47 | Vendor evaluation |
| ASM-16 | Readiness score displayed to borrower only after first human-approved assessment (provisional AI-stage scores are labeled internal-only) | D-06 says borrower sees score; conservatism on unverified numbers | 13, 18, 20 | Product confirmation (OQ-09) |

## 5. How to add an assumption

1. Confirm no controlling source answers the question. 2. Choose the least-committal workable position. 3. Register here with basis and validation path. 4. Cross-reference `ASM-xx` at every point of use. 5. If it materially changes legal boundaries, scoring, access, security, or revenue — also open an OQ and notify the owner of 06.
