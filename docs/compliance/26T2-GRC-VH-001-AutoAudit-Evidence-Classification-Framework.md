# 26T2-GRC-VH-001 — AutoAudit Evidence Classification Framework

**Author:** Viet Huy Thai  
**Trimester:** T2 2026  
**Project:** AutoAudit  
**Team:** GRC  
**Artefact Type:** Evidence governance framework  
**Status:** Draft v0.1

---

## 1. Purpose

This framework defines a consistent approach for classifying evidence used during compliance assessment in AutoAudit.

AutoAudit evaluates technical controls across compliance frameworks such as CIS Microsoft 365, Essential Eight, ISO 27001 and related standards. Different controls may rely on different forms of evidence, including data collected automatically from cloud platforms, policy or configuration documentation, screenshots, review records, or manual confirmation.

Without a consistent classification model, it can be difficult to understand how evidence was obtained, how reliable it is, and how it should influence a compliance result.

The purpose of this framework is therefore to give AutoAudit a reusable governance model for identifying the source, collection method and confidence of compliance evidence.

---

## 2. Scope

This framework applies to evidence used to support, validate or interpret compliance checks in AutoAudit.

It is intended to support:

- automated compliance checks
- manual compliance reviews
- CIS Microsoft 365 control assessments
- Essential Eight assessments
- framework mapping and remediation work
- future compliance frameworks supported by AutoAudit

This document defines evidence classification from a GRC perspective.

It does not:

- define scanner implementation logic
- replace official framework requirements
- determine final compliance status by itself
- prescribe storage architecture for evidence
- replace technical validation performed by Compliance Engine

---

## 3. Evidence Classification Model

AutoAudit evidence can be classified according to how the evidence is obtained and how directly it represents the system or process being assessed.

### 3.1 Direct Automated Evidence

**Code:** AUTO-DIRECT

Evidence retrieved directly from an authoritative technical source through an automated collector or API.

Examples may include:

- Microsoft Graph configuration data
- Entra ID settings
- Microsoft Defender configuration
- Intune configuration
- tenant-level security settings

This type of evidence generally provides strong traceability because the result is collected directly from the assessed environment.

---

### 3.2 Derived Automated Evidence

**Code:** AUTO-DERIVED

Evidence produced when AutoAudit interprets, transforms or evaluates data obtained from an authoritative source.

For example, a Microsoft Graph response may be collected first and then evaluated against policy logic to determine whether a control passes or fails.

The original data remains important because the derived result depends on the accuracy of both the collector and the assessment logic.

---

### 3.3 Manual Documentary Evidence

**Code:** MANUAL-DOC

Evidence supplied through documentation rather than direct technical collection.

Examples may include:

- security policies
- procedures
- configuration standards
- governance records
- review documents

This evidence can be useful where a compliance requirement cannot be fully verified through automated technical checks.

---

### 3.4 Manual Observational Evidence

**Code:** MANUAL-OBS

Evidence produced through human observation or manual verification of a system or configuration.

Examples may include:

- configuration screenshots
- administrator portal review
- manual inspection of a security setting
- documented review results

This evidence may be necessary where automated collection is not available, but additional validation may be required because interpretation depends on the reviewer.

---

### 3.5 Attestation Evidence

**Code:** ATTEST

Evidence based primarily on confirmation provided by a responsible person or control owner.

Examples may include:

- control-owner confirmation
- management attestation
- confirmation that a procedure is followed

Attestation can provide useful governance context, but it should not normally be treated as equivalent to direct technical evidence where stronger evidence is available.

---

### 3.6 External Evidence

**Code:** EXTERNAL

Evidence produced by a third party or external assurance source.

Examples may include:

- external audit reports
- certification reports
- vendor assurance documentation
- independent assessment records

The reliability of external evidence depends on factors such as scope, date, source and applicability to the assessed control.

---

## 4. Evidence Metadata

Evidence should be accompanied by enough metadata to allow another reviewer to understand its origin and relevance.

Recommended metadata fields include:

| Attribute | Purpose |
|---|---|
| Control ID | Identifies the control supported by the evidence |
| Framework | Identifies the relevant compliance framework |
| Evidence classification | Identifies the evidence type defined in this framework |
| Evidence source | Records where the evidence originated |
| Collection method | Identifies whether collection was automated or manual |
| Collection timestamp | Records when the evidence was obtained |
| Evidence owner | Identifies the responsible role or source |
| Freshness | Indicates whether the evidence remains current |
| Verification status | Indicates whether the evidence has been reviewed or validated |
| Reference/location | Identifies where the supporting artefact can be found |

These fields are intended as governance guidance. The exact implementation format may vary depending on future AutoAudit design decisions.

---

## 5. Evidence Confidence Model

Evidence classification describes the type of evidence, but evidence type alone does not determine whether evidence is sufficient.

A simple confidence model can help reviewers interpret evidence quality.

### High Confidence

Evidence may be considered high confidence where it:

- comes from an authoritative source
- is collected directly or through a controlled automated process
- is current
- can be traced to the assessed control
- can be independently verified or reproduced

### Medium Confidence

Evidence may be considered medium confidence where it:

- comes from a relevant and credible source
- provides reasonable support for the control
- may require some manual interpretation
- may not be independently reproducible without additional context

### Low Confidence

Evidence may be considered low confidence where it:

- cannot be independently verified
- is outdated or has unclear collection timing
- relies only on self-attestation
- has an unclear source
- only partially demonstrates the control requirement

Evidence confidence should support professional judgement rather than automatically determine compliance status.

---

## 6. Evidence Classification Decision Guide

The following guide can be used to support consistent classification.

| Evidence example | Source | Collection method | Classification | Typical confidence |
|---|---|---|---|---|
| Microsoft Graph tenant configuration | Authoritative platform | Automated | AUTO-DIRECT | High |
| Compliance result derived from Graph data | AutoAudit assessment logic | Automated | AUTO-DERIVED | High, subject to rule validation |
| Current security policy | Internal governance document | Manual | MANUAL-DOC | Medium |
| Admin portal screenshot | Tenant administrator portal | Manual | MANUAL-OBS | Medium |
| Control-owner confirmation | Responsible person | Manual | ATTEST | Low to Medium |
| Independent audit report | External assessor | External | EXTERNAL | Depends on scope and currency |

This table provides general guidance. Classification and confidence may differ depending on the control being assessed.

---

## 7. Relationship to AutoAudit Assessment Flow

The proposed evidence governance flow is:

**Control requirement → Evidence source → Evidence collection → Evidence classification → Evidence validation → Compliance assessment**

This distinction is important because evidence and compliance results are not the same thing.

For example:

- a collector obtains configuration evidence
- AutoAudit evaluates that evidence against policy logic
- GRC defines what the control requires
- the final assessment result is produced from the relationship between the control requirement and the available evidence

This helps keep evidence provenance separate from the final pass/fail decision.

---

## 8. Example Application — CIS Microsoft 365

A CIS Microsoft 365 control may require AutoAudit to verify a tenant security configuration.

If AutoAudit retrieves the relevant configuration directly through Microsoft Graph, the raw response would normally be classified as **AUTO-DIRECT** evidence.

If AutoAudit then evaluates that response against compliance logic and produces a pass/fail result, the evaluated result would be classified as **AUTO-DERIVED**.

If the same requirement cannot be retrieved automatically and an administrator provides a screenshot of the configuration, that evidence would instead be classified as **MANUAL-OBS**.

If the control depends on an organisational policy document, that evidence may be classified as **MANUAL-DOC**.

This demonstrates why the same compliance framework may require different evidence classes depending on the nature of each control.

---

## 9. Governance Principles

The following principles are proposed for evidence handling in AutoAudit:

1. Evidence should be traceable to the control it supports.
2. Evidence should record its source and collection method where practical.
3. Automated evidence should retain enough context to allow the result to be reviewed.
4. Manual evidence should identify the reviewer, source or responsible role where appropriate.
5. Stale or unverifiable evidence should not be silently treated as equivalent to current authoritative evidence.
6. High-impact findings should, where practical, be supported by stronger evidence than unverified attestation alone.
7. Evidence classification should support professional judgement rather than automatically determine compliance.
8. Evidence limitations should be visible when they may affect interpretation of a result.

---

## 10. Relationship to Existing GRC Work

This framework builds on the existing AutoAudit Evidence Quality Checklist.

The existing checklist focuses on the quality and traceability of GRC contribution evidence, including clear task summaries, supporting artefacts, rationale, outcomes and links to Planner work.

This framework addresses a related but different problem: the classification of compliance evidence used during AutoAudit assessments.

The two artefacts therefore serve different purposes:

- **Evidence Quality Checklist:** helps contributors demonstrate and document GRC work clearly.
- **Evidence Classification Framework:** helps AutoAudit describe and interpret evidence associated with compliance controls.

This distinction avoids replacing or duplicating the existing checklist.

---

## 11. Proposed Next Steps

This framework is intended to provide the foundation for two follow-up GRC artefacts:

1. **Compliance Evidence Collection Matrix**
   - applies the classification model to specific compliance controls and evidence sources.

2. **Evidence Validation and Quality Guidelines**
   - defines practical criteria for determining whether collected evidence is sufficiently valid, current, complete and reliable.

Future review with the GRC and Compliance Engine teams may also determine whether evidence classification metadata should be represented directly in AutoAudit policy or assessment structures.

---

## 12. Current Outcome

A first evidence classification model has been defined for AutoAudit, including:

- six evidence classes
- recommended evidence metadata
- a three-level confidence model
- a classification decision guide
- governance principles
- a proposed relationship between evidence collection and compliance assessment

This provides a reusable starting point for further evidence governance work and control-level application.

