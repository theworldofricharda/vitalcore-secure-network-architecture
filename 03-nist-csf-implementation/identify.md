# Identify (ID)

## In plain terms

You can't protect what you don't know you have. **Identify** is the "inventory and map" stage — cataloging every system, every category of data, and understanding which ones matter most to the business, so that later protection efforts (segmentation, access control) are pointed at the right targets instead of applied evenly and inefficiently everywhere.

## Asset inventory (summary)

| Asset category | Examples | Location (VLAN) | Classification |
|---|---|---|---|
| End-user devices | Laptops, desktops | Corporate (10) | Confidential |
| Business applications | Intranet, HR system, file shares | Servers (20) | Confidential |
| Research systems | Lab workstations, formulation software | R&D (30) | Restricted |
| Clinical systems | Trial database, trial web app | Clinical Trials (40) | Restricted |
| Industrial control systems | PLCs, SCADA, HMIs | Manufacturing (50) | Restricted |
| Network infrastructure | Switches, firewalls, wireless controllers | Management (70) | Restricted |
| Public-facing systems | Corporate website, email gateway | DMZ (80) | Confidential |

This inventory is what directly produced the VLAN design in [Section 02](../02-network-architecture-and-segmentation/README.md) — each row above became its own segmentation zone once we identified it as a distinct category with its own sensitivity level.

## Data flow mapping

Understanding *how* data moves matters as much as knowing where it sits. Key data flows identified:

- **Clinical trial results** flow from the trial application (VLAN 40) → compliance reporting system (VLAN 20) → Compliance Officer, on a scheduled, logged, one-way basis.
- **Manufacturing production data** flows from PLCs (VLAN 50) → SCADA historian (VLAN 50, same zone) → occasionally summarized and exported to Servers (VLAN 20) via the controlled jump host, never in real time and never bidirectionally.
- **R&D formulas** never leave VLAN 30 in unencrypted form, and are never accessible from Corporate (VLAN 10) at all.

Mapping these flows revealed the original flat network's biggest weakness: PHI and manufacturing control data had *no enforced path* — they were simply reachable from anywhere, because nothing constrained the flow to only the connections above.

## Risk identification

The formal output of this function feeds directly into the [Risk Register](../05-risk-assessment/risk-register.md), where each identified asset and data flow is assessed for likelihood and impact of compromise.

## Why this matters

Skipping straight to "let's add firewalls" without first identifying what you actually have is how organizations end up over-protecting low-value assets and under-protecting critical ones. This step is the reason VitalCore's Manufacturing and Clinical zones get materially stricter controls than Corporate — the identification work justified the difference.

---
**Back to:** [03 — NIST CSF Implementation](README.md)
