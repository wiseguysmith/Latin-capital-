# 44 — Security, Privacy, and Data Protection

| Field | Value |
|---|---|
| Purpose | Minimum security baseline, data classification, privacy controls, and vendor/third-party risk for highly sensitive financial and corporate documentation |
| Audience | Security, engineering, compliance, ops |
| Status | Draft v1.0 |
| Version | 1.0.0 |
| Owner | Security lead |
| Dependencies | 43, 35, 36, 45, 46, 12 |
| Source references | Master prompt §13; CRF §16 layer 1/12, §17 (cybersecurity domain), §21 (cross-border privacy) |
| Assumptions | ASM-05, ASM-12, ASM-15 |
| Open questions | OQ-04 (residency), OQ-11 (retention) |
| Approval required | Security lead + compliance; counsel for §6 |
| Last updated | 2026-07-16 |

**No certification or legal-compliance claim in this document is made or implied; SOC 2/ISO alignment is a design target until independently validated (02).**

## 1. Threat posture

Assets: borrower financial/corporate documents, UBO identities, assessment results, provider activity. Principal threats: credential compromise, cross-tenant leakage, insider misuse, document exfiltration by provider users, vendor breach (AI/OCR), tampering with scores/audit, prompt-injection via documents, social-engineered sharing. Controls below map to these.

## 2. Minimum security baseline (gate for any real borrower data — NFR-07)

| # | Control |
|---|---|
| SB-01 | TLS 1.2+ everywhere; HSTS; modern cipher policy |
| SB-02 | AES-256 at rest (DB, objects, backups); KMS-managed keys; separate key for P4; annual rotation |
| SB-03 | MFA per 43; SSO+hardware MFA for infra admin |
| SB-04 | RBAC deny-default + RLS backstop (12/43); quarterly access review |
| SB-05 | Signed URLs ≤15 min, audience-bound, single-purpose (35 §1) |
| SB-06 | Malware scanning on every upload before availability; quarantine flow |
| SB-07 | Secrets in vault; no secrets in code/CI logs; rotation ≤90d |
| SB-08 | Environment separation w/ no production data in non-prod (46; masked fixtures only) |
| SB-09 | Audit immutability + daily chain verification (36) |
| SB-10 | Security logging to SIEM-class store: authn events, authz denials, admin actions, downloads, DLP hits |
| SB-11 | Privileged-user monitoring: all internal-role access to borrower content logged and sampled monthly; break-glass reviewed (12 §7) |
| SB-12 | Vulnerability management: dependency scanning in CI, monthly patch cadence, critical CVE ≤72h |
| SB-13 | Pen test before pilot with real data; remediation of highs before launch (77) |
| SB-14 | DLP checks: no P3/P4 in logs, analytics, or AI telemetry (NFR-10); CI + runtime scanners |
| SB-15 | Download tracking + watermarking for provider access (ASM-12); bulk-download alerts |
| SB-16 | Provider access expiry (ASM-12) + auto-revocation sweeps (14 §6) |
| SB-17 | Backup encryption + quarterly restore drills (35 §6) |
| SB-18 | Incident response plan tested via tabletop pre-launch (45 §5) |

## 3. Data classification & handling

| Class | Contents | Handling highlights |
|---|---|---|
| P1 Public/low | published band definitions, marketing | — |
| P2 Internal/commercial | corporate docs, licenses, governance docs, config | role-scoped |
| P3 Financial-sensitive | financials, bank/tax data, debt, scores, requests | approved-room sharing rules (26); no P3 in emails (19 §1) |
| P4 PII/UBO-sensitive | personal IDs, UBO identities, screening data, guarantor personal data | dedicated key (SB-02); field-level access logging; never in provider rooms raw (12 §3); LLM redaction rules (31 §6); export only via dual-approval |

## 4. Privacy program (jurisdiction specifics in 60–63; counsel validation OQ-04/11/14)

- **Lawful basis & consent:** consent ledger (ENT-24, GATE-05); purpose-bound scopes (processing, screening, sharing-per-provider, retention); consent texts versioned + counsel-validated per country.
- **Notices:** privacy notice per jurisdiction overlay; changes versioned and re-acknowledged.
- **Data-subject rights:** access/export, correction (ties to appeal process 57), erasure with AML-retention carve-outs (35 §4), objection routing — SLA 30 days (NFR-11).
- **Minimization:** collect per checklist need only; screening data segregated; analytics de-identified (34 §4).
- **Cross-border:** transfer mechanism per counsel (OQ-04); vendor DPAs mandatory; regional processing preference for P4 (ASM-15).
- **Automated-decision transparency:** AI-assisted steps disclosed in notices; humans make material decisions (D-07) — this is also the privacy-law posture for automated-decision rules (CRF §19).

## 5. Application security

OWASP ASVS L2 target; CSRF/XSS/injection standard controls; SSRF-safe fetchers for any URL-based ingestion; file-type sniffing beyond extension; upload sandbox rendering (previews generated in isolated workers); anti-automation on public forms (rate limits + captcha); prompt-injection defenses per 31 §6 (documents are data, not instructions; adversarial suite 74).

## 6. Third-party / vendor risk (CRF §17.1; procurement checklist 47)

Vendor classes: cloud, LLM, doc-AI/OCR, e-sign, KYC/sanctions, email. Requirements: security questionnaire + certifications review, DPA + no-training/no-retention clauses for AI vendors (ASM-15), sub-processor transparency, breach-notification ≤72h, data-residency disclosure, exit/data-return terms. Vendor register reviewed semi-annually; single-vendor dependency for critical path requires documented fallback (30 §8).

## 7. Secrets & key management

KMS-backed envelope encryption; key usage audit; separation: storage keys ≠ backup keys ≠ P4 key; quarterly access review of key policies; disaster key-recovery procedure (dual-control).

## 8. Insider risk & monitoring

Least privilege + segregation (12 §6); privileged access reviews quarterly; watermarked internal exports; anomaly alerts (mass downloads, off-hours admin, cross-workspace sweeps); support role sees metadata only (11). All monitoring disclosed in internal policy (worker privacy compliance per country — counsel note in 64).
