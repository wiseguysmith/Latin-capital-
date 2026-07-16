# 46 — Environments, Deployment, and Configuration

| Field | Value |
|---|---|
| Purpose | Environment strategy, CI/CD, release management, and runtime configuration governance |
| Audience | Engineers, ops |
| Status | Draft v1.0 |
| Version | 1.0.0 |
| Owner | Backend lead |
| Dependencies | 40, 44 (SB-08), 45 |
| Source references | NFR-14; ADR-003 |
| Assumptions | ASM-05 |
| Open questions | OQ-04 (region) |
| Approval required | Engineering lead |
| Last updated | 2026-07-16 |

## 1. Environments

| Env | Purpose | Data | Access |
|---|---|---|---|
| dev | development | synthetic fixtures only | engineers |
| staging | pre-release validation, E2E suites, pilot rehearsals | masked/synthetic (never production data — SB-08) | team |
| prod | live | real | least-privilege; no direct DB access (break-glass only) |

Separate cloud accounts/projects per env; IaC (Terraform) as the only mutation path; drift detection weekly.

## 2. CI/CD

Pipeline: lint (incl. prohibited-terms copy lint 73 §6 + dependency rules 41 §2) → unit → scoring golden vectors (20 §11) → integration → E2E (staging) → security scans (SAST, deps, secrets) → deploy gate. Trunk-based with short-lived branches; prod deploys via tagged releases; blue-green or rolling with health checks; automatic rollback on SLO breach post-deploy. DB migrations backward-compatible (expand/contract), reviewed.

## 3. Release management

Weekly release train during build; release notes reference FR/US ids (traceability 79); feature flags for incomplete surfaces (flags are config-registry entries, audited); scoring-model and overlay releases are **data releases** with their own approval workflow (20 §8) decoupled from code deploys.

## 4. Configuration governance (ADR-003)

| Config class | Store | Change control |
|---|---|---|
| Scoring model bundles (ENT-31) | config-registry | R-IA draft → validation suite → R-SA publish (dual-control) |
| Jurisdiction overlays (ENT-32) | config-registry | same + counsel sign-off for legal texts (64) |
| AI bundles/thresholds | model registry (31 §7) | AI lead (+R-SA if material) |
| Notification templates | config-registry | product; counsel for legal templates (19 §4) |
| Feature flags / ops toggles | config service | engineering; audited |
| Secrets | vault (SB-07) | security |

All hot-reloadable (NFR-14), versioned, and pinned per assessment where results-relevant (NFR-15). No environment-specific business logic in code.

## 5. Region posture

Single region (pending OQ-04) + cross-region backup; region endpoints/config abstracted (ADR-006) so residency changes are config+migration, not rewrites.
