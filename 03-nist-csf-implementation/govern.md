# Govern (GV)

## In plain terms

Before any technical control matters, someone has to be accountable for it. **Govern** answers: who owns cybersecurity risk decisions at VitalCore, what's our tolerance for risk, and how do security decisions get made and reviewed? Without this, segmentation and access controls are just isolated technical projects with no one making sure they stay funded, maintained, and aligned with the business.

## VitalCore's governance structure

| Role | Responsibility |
|---|---|
| **Executive Sponsor (CISO)** | Owns overall cyber risk appetite; reports security posture to the Board quarterly |
| **IT Security Lead** | Owns day-to-day implementation of controls (segmentation, IAM, monitoring) |
| **Compliance Officer** | Ensures controls satisfy HIPAA, FDA 21 CFR Part 11, and GxP obligations |
| **Manufacturing OT Owner** | Sole approver for any change affecting the Manufacturing VLAN (change control for GxP-validated systems) |
| **Clinical Data Steward** | Approves any change to Clinical Trials VLAN access; owns the PHI data classification |

## Key governance policies established

1. **Risk appetite statement:** VitalCore accepts low risk tolerance for anything touching Clinical Trials (PHI) or Manufacturing (patient-facing product safety), and moderate risk tolerance for general Corporate IT — this directly justifies why those two VLANs get the strictest controls in [Section 02](../02-network-architecture-and-segmentation/README.md).
2. **Change control:** any firewall rule change to Restricted-classification VLANs (40, 50, 70) requires sign-off from the relevant data/system owner — not just the network engineer making the change. This prevents a well-intentioned but unreviewed change from accidentally opening a hole in the most sensitive zones.
3. **Security review cadence:** the full control set (segmentation rules, access reviews, risk register) is reviewed quarterly, and after any significant incident.
4. **Regulatory alignment ownership:** the Compliance Officer is responsible for confirming that technical controls (e.g., PHI isolation) actually satisfy HIPAA/GxP requirements — closing the loop between "we built a control" and "the control satisfies the legal requirement."

## Why this matters to an employer reviewing this project

Governance is the piece that's easy to skip in a portfolio project but is exactly what separates a junior "I can configure a firewall" candidate from someone who understands that **security decisions are business decisions** — that a control isn't done when it's configured, it's done when someone is accountable for keeping it correct.

---
**Back to:** [03 — NIST CSF Implementation](README.md)
