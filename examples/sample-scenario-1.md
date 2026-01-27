# Worked Example: Exporting PHI Records Under Elevated Session Risk

## Scenario

A customer support analyst accesses a cloud application containing Protected Health Information (PHI). The analyst is authenticated and operating within a valid session.

The analyst attempts to **export a large set of PHI records**.

---

## Step 1: Action Identification

- Action type: Bulk export
- Data classification: PHI
- Action sensitivity: High

---

## Step 2: Risk Context Evaluation

At the time of the action, the following signals are evaluated:

- Device posture: Previously unseen device
- Session duration: Long-running session
- Velocity: Sudden increase in access volume
- Behavioral pattern: Deviates from historical usage
- Identity assurance: Standard (no recent step-up)

Overall risk tier: **Elevated**

---

## Step 3: Policy Evaluation

Policy mapping:
- High-sensitivity action
- PHI data
- Elevated risk tier

Policy requires:
- Increased assurance for export actions under elevated risk

---

## Step 4: Authorization Decision

Decision outcome: **Challenge**

The export action is temporarily blocked pending additional assurance.

---

## Step 5: Evidence Generation

The authorization decision produces audit evidence capturing:
- Action type (bulk export)
- Decision outcome (challenge)
- Risk tier (elevated)
- Policy reference applied

No PHI data or query contents are logged.

---

## Outcome

- Access is not permanently denied
- Session is not terminated
- Privileges are scoped appropriately based on risk
- The decision is explainable and auditable

This demonstrates how continuous authorization balances usability, security, and compliance in a regulated environment.
