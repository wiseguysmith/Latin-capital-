# 60 — Costa Rica MVP Overlay

| Field | Value |
|---|---|
| Purpose | Costa Rica jurisdictional overlay v1: identity/evidence specifics, operational requirements vs legal assumptions, and the counsel-validation queue |
| Audience | Product, engineers (overlay config), operations, counsel |
| Status | Draft v1.0 — **contains legal assumptions pending Costa Rican counsel validation; nothing herein is settled law or legal advice** |
| Version | 1.0.0 (overlay bundle cr-1.0.0-draft) |
| Owner | Compliance lead; counsel validation owner per 64 |
| Dependencies | 63 (template/schema), 64 (validation checklist), 26 (evidence), 51–55 (SOPs) |
| Source references | CRF §19, §19.1 (Costa Rica row, ref R9 — SUGEF resources), §19.2 (launch posture); D-01 |
| Assumptions | ASM-04, ASM-13, ASM-14; every LEGAL-tagged row below |
| Open questions | OQ-02, OQ-04, OQ-05, OQ-06, OQ-11, OQ-14 |
| Approval required | Counsel sign-off before activation (GATE-07 discipline; 64) |
| Last updated | 2026-07-16 (rule dates/sources recorded per row) |

## 0. Epistemic rule for this document

Rows are tagged **[OPS]** (operational requirement the team can act on: what to collect, which registry to query) or **[LEGAL]** (assumption requiring written counsel confirmation before reliance). Each rule records **source + date recorded**. Per master prompt §14, unverified interpretations are never presented as settled law.

## 1. Launch posture (CRF §19.2 applied to CR)

Business-purpose borrowers only; no consumer lending; sole proprietors excluded [ASM-13]. Platform performs no lending, custody, settlement, intermediation of funds, or public offering activity [02 §2]. Borrower-controlled introductions only. Counsel must deliver a **regulatory-perimeter memorandum** (CRF §19: whether platform activity constitutes arranging/brokerage/intermediation/crowdfunding/credit-bureau activity in CR, per fee model chosen in OQ-01) before pilot publication to providers — tracked as CR-L1 in 64.

## 2. Identity & entity configuration [OPS]

| Item | CR value (source: public registry conventions; recorded 2026-07-16) |
|---|---|
| Entity identifier | Cédula jurídica (format `3-###-######`); validator in overlay config |
| Person identifiers | Cédula de identidad (nationals), DIMEX (residents), passport (foreigners) |
| Entity types (intake list) | S.A. (sociedad anónima), S.R.L./Ltda, sucursal de sociedad extranjera, others via "other + describe" (counsel to confirm treatment of fideicomisos as borrowers — CR-L8) |
| Corporate registry | Registro Nacional — Registro de Personas Jurídicas; literal certification (certificación literal) as EV-COR-02, ≤30d |
| Property registry | Registro Nacional — Registro Inmobiliario; folio real for EV-PRO-01 |
| Movable-collateral registry | Sistema de Garantías Mobiliarias (Registro Nacional) for lien searches/perfection path evidence (EV-COL-03) [LEGAL: perfection mechanics + priority rules — CR-L5] |
| Language / currency | Spanish primary; USD + CRC (ASM-04) |
| Time zone | America/Costa_Rica (UTC-6, no DST) |

## 3. Evidence overlay additions (extends 26)

| Item | Detail | Tag |
|---|---|---|
| EV-LAB-01-CR | CCSS (Caja) employer status certificate — social-security compliance for F5; "al día" status expected; ≤90d | [OPS] |
| EV-TAX-01-CR | Ministerio de Hacienda filings (income tax, IVA); "al día" tax-status verification where obtainable | [OPS] |
| EV-COR-05-CR | Registro de Transparencia y Beneficiarios Finales (RTBF) filing evidence — CR maintains a UBO declaration regime; borrower's RTBF compliance supports GATE-02/E2c. Platform access to RTBF data is restricted; expect borrower-provided filing receipts + declaration copies | [OPS collection / LEGAL access scope — CR-L4] |
| EV-LIC-01-CR | Municipal patente + sector permits (e.g., SETENA environmental viability for development projects — feeds EV-ESG-02/M-CONST) | [OPS] |
| EV-INS-01-CR | INS or private insurer policies; riesgos del trabajo coverage check under F5/labor | [OPS] |
| Bank statement verification | CR bank statements often verifiable via issuing-bank digital stamps/QR — verification protocol per bank documented in ops runbook | [OPS] |

## 4. Compliance configuration

| Item | Detail | Tag |
|---|---|---|
| Sanctions lists | UN, OFAC, EU + local designations; screening protocol 51 §2.6 | [OPS] |
| AML registration | Law 7786 (as amended) requires registration with SUGEF for certain non-financial activities (notably lending/leasing/factoring/remittances and other "actividades y profesiones no financieras designadas"). **Whether any platform or pilot-adjacent activity triggers registration is a counsel question (CR-L2). The platform does not lend; posture assumes no registrable activity, unvalidated.** Source: CRF §19.1/R9 (SUGEF publishes Law 7786 registered-subjects resources); recorded 2026-07-16 | [LEGAL] |
| Credit information | SUGEF operates the CIC (Centro de Información Crediticia); access is limited to authorized entities — platform access unlikely; MVP relies on borrower-supplied debt evidence + lien searches (OQ-05). Providers who are SUGEF-supervised may use their own CIC access downstream | [LEGAL — CR-L6] |
| FX-exposed debtor context | SUGEF frameworks require supervised lenders to analyze FX-exposed debtors (CRF §19.1/R9) → External Risk Context library includes currency-mismatch factor + B6 stress expectation for USD-borrowing CRC-earners | [OPS (as methodology); LEGAL (regulatory detail) ] |
| Data protection | Ley 8968 (Protección de la Persona frente al Tratamiento de sus Datos Personales) + PRODHAB registration questions: consent regime is consent-centric; database-registration duties and cross-border transfer conditions require counsel confirmation (CR-L3, OQ-04, OQ-11) | [LEGAL] |
| E-signature | Ley 8454 (firma digital) recognizes certified digital signatures (national PKI); enforceability of foreign e-sign vendors (DocuSign-class) for attestations/consents needs counsel confirmation (CR-L7, OQ-06); wet-ink fallback supported | [LEGAL] |
| Consumer/SME protection | Ley 7472 consumer-protection applicability to micro/small business borrowers to be confirmed; ASM-13 exclusion reduces exposure | [LEGAL — CR-L9] |

## 5. Overlay parameter values (63 schema instantiation)

Required-evidence set: universal core (26 §2) + CR rows (§3 above). Identity fields per §2. UBO threshold: 25% default pending counsel (GATE-02). Consent/privacy notice texts: `cr-es-1.0-draft` (counsel-validated versions required before real data — OQ-14). Retention: RC-* defaults (35 §3) pending CR statutes (OQ-11). Disclaimers: limitation statement ES draft + "plataforma no es prestamista ni intermediario financiero regulado" statement [LEGAL wording — CR-L1]. Approved partners: register per 58 (colegio-verified attorneys/CPAs/appraisers). Regulatory-disclaimer footer on provider packages.

## 6. External Risk Context library (CR pilot; sources: central bank/IMF-class public data — refresh each quarter, 37)

Factors catalog: FX (CRC/USD dynamics, dollarized debt), rates environment, sector cyclicality (tourism, construction, agri exports), climate/disaster exposure (hurricane-adjacent, flood, seismic), supply-chain (port/logistics), political/policy (stable relative to region — factor levels set per assessment, not hardcoded). Library entries carry source + date; reviewers select applicable factors (53 §3).

## 7. Counsel-validation queue (feeds 64)

| ID | Question | Blocking |
|---|---|---|
| CR-L1 | Regulatory-perimeter memo: platform activities + fee model vs licensing/intermediation/brokerage/crowdfunding perimeters | pilot publication |
| CR-L2 | Law 7786 registration applicability | pilot |
| CR-L3 | Ley 8968/PRODHAB: consent texts, database registration, cross-border transfers | real borrower data |
| CR-L4 | RTBF: lawful access scope + borrower evidence expectations | GATE-02 config |
| CR-L5 | Garantías mobiliarias: lien search reliability, perfection/priority mechanics for H3 evidence standards | H-dimension anchors |
| CR-L6 | CIC/bureau access with consent (OQ-05) | Phase 4 |
| CR-L7 | E-signature validity for attestations/consents (Ley 8454) | attestation flow |
| CR-L8 | Fideicomiso/trust borrowers & guarantee structures treatment | intake config |
| CR-L9 | Ley 7472 scope re small-business borrowers | intake config |
| CR-L10 | Withholding/stamp/registration taxes affecting typical private-debt closings (disclosure content for providers) | provider package notes |
