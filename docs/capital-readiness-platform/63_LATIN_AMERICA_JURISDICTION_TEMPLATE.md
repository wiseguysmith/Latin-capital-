# 63 — Latin America Jurisdiction Template

| Field | Value |
|---|---|
| Purpose | The reusable overlay schema and activation playbook for any new country — jurisdiction as configuration, never as forks |
| Audience | Product, engineers, compliance, counsel |
| Status | Draft v1.0 |
| Version | 1.0.0 |
| Owner | Compliance lead + Backend lead (schema) |
| Dependencies | ENT-32 (storage), 46 §4 (publish control), 60–62 (instances), 64 (validation) |
| Source references | Master prompt §14 (overlay-changeable list); CRF §19, §19.2 (jurisdiction rule engine + counsel sign-off) |
| Assumptions | — |
| Open questions | — |
| Approval required | Backend lead (schema changes); counsel per instance |
| Last updated | 2026-07-16 |

## 1. Overlay schema (versioned bundle, ENT-32)

```yaml
overlay:
  country_code: ISO-3166 alpha-2
  version: semver
  status: predraft | draft | counsel_review | active | suspended
  identity:
    entity_id_fields: [{name, format_regex, validator}]
    person_id_fields: [{name, format_regex}]
    entity_types: [{code, label_es, label_en, enhanced_review_default: bool}]
  registries:
    corporate: {name, evidence_types, verification_protocol_ref}
    property: {...}   movable_collateral: {...}   ubo_regime: {...}
  evidence:
    additions: [EvidenceType]            # extends 26 registry
    field_variants: [{ev_id, field_overrides}]
    freshness_overrides: [{ev_id, max_age_days}]
  compliance:
    ubo_threshold_pct: number            # GATE-02 parameter
    sanctions_lists_additional: [source]
    aml_notes_ref: doc section           # counsel-validated notes
    credit_information: {access_model, consent_requirements}
  privacy:
    notice_text_key: versioned string    # counsel-validated
    consent_text_keys: {processing, screening, sharing, retention}
    transfer_mechanism_note: ref
    retention_overrides: [{record_class, years}]
    dsr_rules: {erasure_carveouts_ref}
  esign: {accepted_methods: [], qualified_provider_notes}
  language: {primary, supported[]}
  currency: {primary, supported[]}
  collateral:
    perfection_notes_ref: doc            # H3 anchor guidance
    lien_search_protocol_ref: runbook
  disclaimers:
    limitation_statement_key, platform_role_statement_key, provider_package_footer_key
  partners: {register_ref}               # 58 approved partners per country
  risk_context_library: [{factor, description, source, recorded_date}]
  counsel:
    validation_items: [{id, question, status, memo_ref, date}]
    signoff: {firm, date, scope, memo_ref}   # required for status=active
```

## 2. What an overlay may change (master prompt §14 — complete)

Required evidence; identity fields; entity types; beneficial-ownership requirements (threshold, regime evidence); tax evidence; registry verification protocols; currency; language; privacy notices; consent language; e-signature expectations; credit-information access; AML review requirements; collateral evidence + perfection guidance; disclosure requirements; retention; approved partners; regulatory disclaimers.

**What an overlay may never change:** scoring weights/bands/formula, gate existence (only parameters like UBO threshold), AI governance rules, transaction boundaries (02), audit requirements. Methodology stays comparable across countries (CRF §3.2); jurisdiction affects evidence and process, not the meaning of the score.

## 3. Enforcement in code

Core services read overlay values via config-registry (ADR-003); CI test asserts no country-code literals in core modules (73); every assessment pins overlay version (ENT-16); activation requires `counsel.signoff` present — the publish workflow **blocks** `status: active` without it (GATE-07 in config form; CRF §19.2 "require counsel sign-off before activating a new product-country combination").

## 4. New-country activation playbook

| Step | Owner | Output |
|---|---|---|
| 1. Market rationale + provider demand check | Product | go/no-go memo |
| 2. Instantiate overlay predraft from this template | Compliance | xx-0.1.0 |
| 3. Counsel engagement: perimeter memo + validation items (64 template) | Counsel | written memo |
| 4. Localize evidence registry, verification protocols, screening lists | Ops+Compliance | draft bundle |
| 5. Translate/validate legal texts (notices, consents, disclaimers) | Counsel | text keys |
| 6. Partner register seeding (58) | Ops | ≥1 legal, 1 accounting partner |
| 7. Risk-context library seeding with sources/dates | R-FR | library entries |
| 8. Vendor coverage validation (47 §2) if integrations used | Eng | register entries |
| 9. Dual-control publish (46 §4) + golden checklist/scoring vectors for a local profile | Eng | xx-1.0.0 active |
| 10. Guarded pilot (≤5 borrowers, elevated sampling) | Ops | pilot report → full open |
