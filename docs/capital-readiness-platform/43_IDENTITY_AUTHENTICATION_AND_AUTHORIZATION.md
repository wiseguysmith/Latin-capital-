# 43 — Identity, Authentication, and Authorization

| Field | Value |
|---|---|
| Purpose | Technical identity architecture: authentication, session, MFA, user identity verification, and authorization enforcement |
| Audience | Engineers, security |
| Status | Draft v1.0 |
| Version | 1.0.0 |
| Owner | Security lead |
| Dependencies | 12 (permission model), 44 (security baseline) |
| Source references | Master prompt §13; CRF §16 layer 1 |
| Assumptions | ASM-06 (KYC vendor phased) |
| Open questions | OQ-06 (e-sign), OQ-07 (vendor) |
| Approval required | Security lead |
| Last updated | 2026-07-16 |

## 1. Authentication

- Managed IdP (40 §3): email+password with complexity + breach-list check; **MFA (TOTP or WebAuthn) mandatory** for all internal, provider, partner, auditor roles at login; for borrower roles before first sensitive read/write (12 §5). SMS OTP only as fallback.
- Sessions: short-lived access tokens (≤15 min) + rotating refresh (≤8h absolute for internal/provider; ≤24h borrower); revocation ≤5 min (NFR-09); device/new-location notice (SEC-02).
- Magic links limited to: application save-resume (SCR-P2) and status view (SCR-P8) — never grant workspace access.
- Service credentials: workload identity, no static secrets in code (44 §7); vendor keys vaulted + rotated ≤90d.

## 2. User identity verification (person-level)

| Phase | Method |
|---|---|
| 1 | Manual: R-BA + signatories upload ID docs (P4); R-CR verifies against corporate authority docs (GATE-01 signatory check); videocall verification allowed per SOP 51 for doubt cases |
| 2+/4 | KYC vendor (ASM-06/OQ-07): doc authenticity + liveness for R-BA, UBOs, guarantors; vendor evidence stored as EV-CMP-01 with E2 tier |

Verification status is per-person, per-role-context; unverified R-BA cannot sign attestations or consents (GATE-05 dependency).

## 3. Authorization enforcement (implements 12)

- Layer 1: route middleware — role×resource-type matrix (12 §2 compiled to policy).
- Layer 2: module object checks — workspace/opportunity scoping, assignment checks (†-scopes), document class/sharing rules (12 §3).
- Layer 3: data layer — PostgreSQL RLS keyed by org/grant tables (NFR-22) as backstop.
- Segregation checks (12 §6) evaluated at decision endpoints (42 §3) using Membership + action history; violations 403 with `code: segregation_conflict` (audited).
- Policy is versioned config; permission changes require security-lead review (PR-based).

## 4. Attestations & e-signature

MVP: in-product attestation = authenticated + MFA-fresh (≤5 min re-auth) click-through with full text versioning, hash of attested content, IP/device record (ENT-30 attestation.signed) **plus** e-sign vendor envelope for the formal attestation letter (CRF §14 stage 9). Legal sufficiency per country pending OQ-06; wet-ink fallback supported (signed PDF upload, R-LR verifies).

## 5. Emergency & administrative access

Break-glass per 12 §7 (dual-approval, ≤24h, fully audited); no standing superuser in production; infrastructure admin access via SSO + hardware MFA + session recording (44 §8).
