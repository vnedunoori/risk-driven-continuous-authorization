# Risk-Driven Continuous Authorization for Privacy-Regulated Cloud Data (PII/PHI)
## Fraud-Resistant, Audit-Verifiable Decision Architecture

**Version:** v1.0 (A-Lite)  
**Author:** Venkata Nedunoori  
**Status:** Public Specification (Clean-Room, Field-Reusable)

---

## 1. Purpose and Scope

### 1.1 Purpose

This specification defines a **risk-driven continuous authorization method** for systems that handle **privacy-regulated cloud data**, including Personally Identifiable Information (PII) and Protected Health Information (PHI).

The method governs **authorization decisions at the moment of sensitive actions during an active session**, rather than relying solely on authentication or access granted at login time. It enables systems to:

- Perform **action-level authorization**
- Dynamically **expand or contract privileges mid-session** based on risk
- Integrate **fraud and abuse containment** directly into authorization decisions
- Produce **audit-verifiable access evidence** while minimizing exposure of regulated data

This document defines a **decision methodology, reference architecture, and minimal policy and evidence model**. It does **not** prescribe or require a specific implementation.

---

### 1.2 Explicit Non-Scope

This specification does **not** define or require:

- A commercial or open-source product
- An executable service, control plane, or enforcement engine
- A healthcare or clinical application
- An MFA system, identity provider, or authentication workflow
- Employer-specific architecture, metrics, configurations, or workflows
- Proprietary fraud detection algorithms or scoring models

The contribution is a **method and evidentiary framework**, not software.

---

## 2. Problem Statement

Cloud systems handling PII and PHI face a structural gap between:

- **Authentication-centric access models**, which assume trust once a session begins
- **Regulatory expectations**, which require justification for each sensitive data access

Common limitations in regulated environments include:

1. **Static authorization** — privileges granted at login persist even as risk changes  
2. **Coarse-grained control** — access is authorized at resource level rather than action level  
3. **Delayed fraud response** — abuse is detected after exposure occurs  
4. **Over-logging** — audit logs capture sensitive data unnecessarily  
5. **Weak evidentiary posture** — logs show *what* happened but not *why access was justified*

In practice, these gaps are often discovered during audits or incident reviews rather than during initial system design.  
// [Added]

In regulated contexts, terminating sessions in response to elevated risk is often operationally disruptive. This method intentionally prioritizes **scoped privilege contraction** over full session invalidation.

---

## 3. Core Contribution Overview

### 3.1  Summary

This specification introduces a **risk-to-decision authorization loop** that operates continuously at the **action-decision layer**, with four defining characteristics:

1. **Action-level authorization**  
   Each sensitive action (e.g., view, export, bulk query) is evaluated independently.

2. **Continuous, risk-driven decisioning**  
   Authorization decisions adapt to evolving session and behavioral risk signals.

3. **Dynamic privilege contraction**  
   Privileges may be reduced or restricted mid-session without requiring logout.

4. **Audit-verifiable evidence with PII/PHI minimization**  
   Decisions generate regulator-defensible evidence without logging regulated data itself.

---

### 3.2 Distinction from Prior Art

This method is **not**:

- Zero-trust authentication
- Session revalidation or reauthentication
- Role-based or attribute-based access control alone

Unlike authentication-centric or perimeter-based models, this method governs **authorization at the moment of each sensitive action**, explicitly linking:

> **Risk context → authorization decision → audit evidence**

This approach prioritizes decision explainability and auditability over minimal authorization latency, a tradeoff that is acceptable in regulated environments.  
// [Added]

---

## 4. Regulated Data Threat Model

### 4.1 Sensitive Action Classes

The method applies to actions involving privacy-regulated data, including but not limited to:

- Record or attribute view
- Bulk query or aggregation
- Export or download
- Re-identification or data linkage
- Administrative override
- Cryptographic key or token access

Each action is evaluated independently, even within an authenticated session.

---

### 4.2 Abuse and Misuse Paths

The threat model assumes realistic scenarios, including:

- Account takeover (ATO)
- Session hijacking
- Credential recovery abuse
- Insider misuse
- Excessive access via legitimate credentials

The model assumes attackers may operate **within valid sessions**, necessitating continuous authorization.

---

## 5. Risk-to-Decision Control Loop (The Method)

### 5.1 Risk Signal Taxonomy

Authorization decisions consider contextual risk signals organized into a reusable taxonomy, which may include:

- **Device context** (posture, environment)
- **Behavioral signals** (consistency, deviation)
- **Session state** (duration, sequencing)
- **Velocity indicators** (rate, volume)
- **Historical patterns** (prior actions, anomalies)
- **Identity assurance level**

This specification does not mandate how signals are collected, scored, or weighted.

---

### 5.2 Decision Outcomes (Action-Level Step-Up Logic)

For each sensitive action, the authorization engine produces one of three outcomes:

- **Allow** — action proceeds  
- **Challenge** — step-up authorization required  
- **Deny** — action blocked  

A **challenge** represents an increase in required assurance at the authorization layer. It does **not** redefine authentication.

---

### 5.3 Dynamic Privilege Adjustment

The method explicitly supports **privilege contraction mid-session**, including:

- Restricting accessible actions
- Reducing data scope
- Increasing assurance requirements

Session termination is optional and not required.

---

### 5.4 Integrated Fraud and Abuse Containment

Fraud and abuse containment is **integrated directly into authorization decisions**, rather than handled as a separate detection or response system.

Elevated risk resulting from suspected account takeover, abuse, or anomalous behavior directly influences authorization outcomes for sensitive actions.

---

## 6. Minimal Policy Language

### 6.1 Policy Model

Policies map three dimensions:

1. **Action sensitivity**
2. **Data classification (PII / PHI)**
3. **Risk tier**

Policies define:

- Required assurance level
- Permitted action scope
- Acceptable decision outcomes

---

### 6.2 Design Principles

The policy language is intentionally:

- **Minimal** — limited primitives
- **Declarative** — no embedded logic
- **Implementation-agnostic** — supports independent reimplementation

This enables adoption without licensing, vendor lock-in, or coordination with the author.

---

## 7. Audit-Verifiable Evidence Model

### 7.1 Evidence Goals

The evidence model is designed to:

- Justify *why* access was permitted or denied
- Support regulatory and audit review
- Minimize exposure of regulated data

---

### 7.2 Logging Principles

Logs should capture:

- Action type (not data content)
- Decision outcome
- Risk tier at decision time
- Policy reference applied

Logs should **exclude**:

- Raw PII or PHI
- Query contents or payloads
- Sensitive field values

---

### 7.3 Evidence Lifecycle

Evidence should support:

- Integrity guarantees
- Regulation-aligned retention
- Controlled auditor access

This transforms logs into **compliance-grade evidence** rather than operational exhaust.

---

## 8. Reference Architecture (Conceptual)

The reference architecture consists of:

1. Action request  
2. Risk context evaluation  
3. Policy lookup  
4. Authorization decision  
5. Evidence generation  

This architecture is **conceptual**, not prescriptive.

---

## 9. Implementation Checklist and Anti-Patterns

### 9.1 Implementation Checklist

- Identify regulated action set
- Classify data as PII or PHI
- Define action sensitivity tiers
- Establish risk tiers
- Map risk tiers to decision outcomes
- Define evidence schema prior to logging
- Validate privilege contraction behavior

---

### 9.2 Common Anti-Patterns

- Authorizing access only at login
- Treating challenge as reauthentication
- Logging full queries or data payloads
- Using static roles for sensitive actions
- Terminating sessions instead of contracting scope
- Relying on opaque risk scores without decision rationale

---

## 10. Reuse and Independent Adoption

This specification is intended for independent use by:

- Cloud security architects
- Compliance and audit professionals
- Security consultancies
- SaaS organizations handling regulated data

Organizations may reimplement or adapt this method without permission, licensing, or coordination with the author.

---

## 11. Versioning and Evolution

This document is **Version 1.0 (A-Lite)**.

Future versions may expand taxonomies, refine policy semantics, and incorporate feedback from independent adopters.

---

## 12. Summary

This specification defines a **risk-driven continuous authorization method** for privacy-regulated cloud data that:

- Operates at the action-decision layer
- Integrates fraud and abuse containment
- Dynamically adjusts privileges during active sessions
- Produces audit-verifiable evidence while minimizing PII/PHI exposure

It is intended as a **field-reusable original contribution** suitable for regulated cloud environments.

---

*End of Specification*
