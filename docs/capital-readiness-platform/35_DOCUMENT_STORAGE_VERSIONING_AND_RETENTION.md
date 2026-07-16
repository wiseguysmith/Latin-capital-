# 35 — Document Storage, Versioning, and Retention

| Field | Value |
|---|---|
| Purpose | Storage architecture for evidence files, versioning rules, retention schedule, deletion, legal holds, and backup |
| Audience | Backend engineers, security, compliance, ops |
| Status | Draft v1.0 |
| Version | 1.0.0 |
| Owner | Backend lead + Compliance |
| Dependencies | 33 (ENT-11), 44 (security), 14 §4 (document states) |
| Source references | CRF §16 layer 1 (immutable original, hash, source, timestamp, owner); Appendix B |
| Assumptions | ASM-05 (cloud object storage), ASM-11 (formats/sizes); retention values provisional (OQ-11) |
| Open questions | OQ-11 (country retention), OQ-04 (residency) |
| Approval required | Compliance + counsel (retention schedule) |
| Last updated | 2026-07-16 |

## 1. Storage architecture

- Object storage, private buckets, no public access paths; per-environment separation (46).
- Layout: `tenant/{org}/workspace/{ws}/doc/{doc_id}/v{n}/original.<ext>` + derived artifacts (`ocr.json`, `preview/*.png`, `redacted.pdf`) stored as separate derived objects keyed to the version — derived artifacts are reproducible and never replace originals.
- **Immutability:** originals written once (object-lock/WORM where available); SHA-256 recorded at write; hash re-verified on read (NFR-19); any mismatch = Sev-1 incident.
- Access exclusively via short-lived signed URLs (≤15 min, single-purpose, audience-bound) issued by the document service after authz check (44 §2); no direct bucket credentials in app tier.
- Encryption at rest with KMS per 44; P4 documents use a dedicated key.

## 2. Versioning

New upload for the same checklist item/document = version n+1 (14 §4 supersede); versions immutable; version chain queryable; reviewer decisions bind to a specific version; provider approved rooms always expose only the latest **verified, shared** version (older shared versions withdrawn automatically on supersede, audit-logged).

## 3. Retention schedule (provisional — OQ-11; per-record-class, overlay-overridable)

| Class | Contents | Active retention | Post-relationship | Basis |
|---|---|---|---|---|
| RC-AUDIT | Audit events, assessment results, overrides, consent ledger, screening dispositions | life of platform record | 10 years | AML/audit conservatism (CRF §18) |
| RC-EVIDENCE | Borrower documents + verified fields | while workspace active | 5 years after last assessment/relationship end | evidentiary + AML |
| RC-KYC | Identity docs, UBO data, screening records | active | 5–10 years per overlay (AML statutes) | overlay config |
| RC-COMM | Notifications, RFI/EOI threads, clarifications | active | 5 years | dispute defense |
| RC-AI | Raw prompt logs (P4-redacted) | 90 days | — | 31 §6 |
| RC-APP | Rejected/withdrawn applicant records | — | 2 years | reapplication + compliance |
| RC-TELEM | Ops telemetry (no P3/P4) | 13 months | — | ops |

## 4. Deletion & erasure

- Scheduled deletion jobs execute per class with **destruction certificates** (what, when, policy basis) logged to audit (NFR-21).
- Data-subject erasure requests (privacy laws per overlay): honored for data not under retention duty or legal hold; where AML retention prevails, subject is informed of the legal basis (44 §6; NFR-11). Erasure applies tombstoning: content destroyed, minimal skeleton (ids, class, destruction record) retained.
- Borrower off-boarding: workspace archived read-only → clock starts on post-relationship retention.

## 5. Legal holds

ENT-35: placed by R-IA or R-CR (reason mandatory), release requires R-SA; holds override deletion jobs and erasure (documents move to `legally_preserved`, 14 §4); active holds reviewed quarterly.

## 6. Backup & recovery

Cross-region replication for object store + daily DB snapshots (RPO 1h via WAL/point-in-time); restore drills quarterly (NFR-05/06); backups encrypted with separate keys; backup retention aligned to class schedule (backups expire on schedule so deletion is effective in backups within 35 days — documented gap window accepted and disclosed in privacy notice).
