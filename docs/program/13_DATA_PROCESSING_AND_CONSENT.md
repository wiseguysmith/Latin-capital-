# 13 — Data Processing & Consent (Notice, Consent & DPA Framework)

| Field | Value |
|---|---|
| Purpose | Define the privacy notice, consent capture, controller/processor roles, and cross-border processing controls so the program complies with Costa Rica's Ley 8968 |
| Audience | Compliance, counsel, product/engineering, partner lender |
| Status | **DRAFT TEMPLATE — requires counsel + PRODHAB registration analysis; Spanish operative text needs certified legal drafting** |
| Version | 1.0.0 |
| Owner | Compliance / DPO + counsel |
| Dependencies | `03_...` §9 (Ley 8968), `05_...`, `07_...` (consent schema), CRP `44_SECURITY_PRIVACY_AND_DATA_PROTECTION.md`, `35_DOCUMENT_STORAGE_VERSIONING_AND_RETENTION.md`; decision D-23 (US cloud + consent) |
| Last updated | 2026-07-19 |

---

## 1. Privacy notice — required content (Ley 8968)

Provided **before** collection, in clear Spanish (`03_...` §9.1): the existence and identity of the **database and controller**; **purpose** of collection; **recipients** of the data; how it is **processed**; **consequences of refusal**; the data subject's **rights** (access, rectification, cancellation, objection); and the process to exercise them.

## 2. Consent (express, precise, recorded)

- Consent is **express, informed, and scoped**; captured and stored per the consent schema (`07` §4.4) with scope, purpose, timestamp, language, and expiry.
- **Locked UI requirement (`21` D-P3):** the *Consentimiento Informado* is a mandatory, **non-pre-checked** tick-box form in Spanish; each scope is a separate unchecked box; the US-cloud transmission/processing/storage disclosure is explicit in the consent text itself, not only in the linked notice.
- **International-transfer consent is separate and explicit** — required for US-cloud processing; identifies recipient categories and processing countries (`03_...` §9.1). Transfer without valid consent is a serious violation.
- Consent is **withdrawable**; withdrawal revokes active document links (`06` §7) and triggers retention/deletion handling (§6).
- Sharing with a lender is a **distinct consent scope** (`share_with_lender:[LENDER]`); a lender sees only the consented scope for the consented purpose.

## 3. Controller / processor roles (DPA)

- Determine, per data set, who is **controller** vs **processor** among capitalYA, CRP, the lender, and cloud subprocessors (`03_...` §17 Q13; open item).
- **Controller–processor agreement** with each processor; documented **subprocessor list**; flow-down of security and confidentiality obligations.
- The lender is controller of the data it uses for its regulated KYC/underwriting of record; capitalYA/CRP are controllers/processors for intake and assessment as determined by counsel.

## 4. Cross-border / US-cloud processing controls (`03_...` §9.1)

US cloud is **permitted** (no general CR localization mandate) when structured with: express international-transfer consent; recipient/country disclosure; controller–processor agreement; subprocessor list; **encryption in transit and at rest**; access-control policy; incident-response obligations; retention/deletion schedule; DSAR procedures; purpose limitation; **PRODHAB registration analysis**; and a **prohibition on placing personal data on any public blockchain**.

## 5. PRODHAB registration

Databases administered for distribution, diffusion, or commercialization must be registered with PRODHAB (Ley 8968 Art. 21). **Locked decision (`21` D-P3): formal PRODHAB registration filings are executed upfront in San José, concurrent with Phase 1 entity incorporation** — not deferred pending analysis. Counsel confirms *which* databases and filing content (`03_...` §9.1, §17 Q13), but the posture is register-first, not analyze-first.

## 6. Retention, deletion & data-subject rights

- Defined **retention schedule** and secure deletion (aligned to CRP `35`); note AML record-retention obligations sit with the lender of record.
- **DSAR handling:** access, rectification, cancellation, objection — with identity verification and response timeframes.
- All access to personal data is **logged** (CRP `36`, `44`).

## 7. Special constraints

- **No personal or financial data on any public blockchain**, ever (`03_...` §9.1; `07` §5).
- **ZK proofs** (Horizon 2) reduce disclosure but do not replace consent, accuracy, correction rights, controller accountability, security, or lawful collection of source data (`03_...` §9.1).
- Protected-class attributes are not used in scoring/matching; where law requires collection for compliance, they are stored segregated (CRP `44`; `05` §3).

## 8. Documents this framework produces

| Document | Status |
|---|---|
| Borrower privacy notice (ES) | Draft → counsel |
| Consent form(s) incl. international-transfer & lender-sharing (ES) | Draft → counsel |
| Controller–processor agreement(s) / DPA | Draft → counsel |
| Subprocessor register | Maintained by DPO |
| PRODHAB registration analysis & filings | Counsel + DPO |
| Retention & deletion schedule | Aligned to CRP `35` |

## 9. Open items → counsel / DPO

`[Controller vs processor determination per data set]`, `[PRODHAB registration applicability]`, `[cross-border consent wording]`, `[retention periods vs. AML retention held by lender]`, `[DSAR response timeframes]`, `[certified Spanish text]`.
