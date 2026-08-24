# Protect (PR)

## In plain terms

**Protect** is where the actual safeguards live — the locked doors, not just the map of the building. This is the largest function in this project because it's where network segmentation and least privilege — the two headline deliverables of this portfolio — actually live.

## Controls implemented under Protect

### 1. Network segmentation & access control
The full VLAN design, default-deny ACL matrix, and firewall ruleset in [Section 02](../02-network-architecture-and-segmentation/README.md) are this project's primary Protect control. Splitting the network into classification-aligned zones (Corporate, R&D, Clinical, Manufacturing, etc.) and enforcing default-deny between them is what stops a compromise in one zone from automatically becoming a compromise everywhere.

### 2. Identity & access management (Least Privilege)
The tiered administration model, RBAC design, and Active Directory OU structure in [Section 04](../04-least-privilege-and-access-control/README.md) ensure that every account — human or service — has only the access it needs to do its job, and nothing more. This limits the damage a single compromised account (e.g., through phishing) can do.

### 3. Data protection
- Clinical Trials data (PHI) is encrypted at rest and in transit, satisfying HIPAA's technical safeguard requirements.
- R&D formula data is encrypted at rest within VLAN 30 and is never transmitted to Corporate systems.
- Backups of Clinical and Manufacturing systems are stored in a separate, isolated backup VLAN, not reachable from the production zones — this prevents ransomware that encrypts production data from also destroying the backups.

### 4. Hardening baseline
- All Manufacturing OT devices are on a documented patch schedule executed only through the controlled jump host (never patched live from the internet).
- Default/shared local administrator accounts have been eliminated in favor of individual, audited admin accounts (see [Section 04](../04-least-privilege-and-access-control/README.md)).
- Multi-factor authentication (MFA) is enforced for any access to Management (VLAN 70), Clinical Trials (VLAN 40), and all administrative accounts.

### 5. Awareness & training
All Corporate staff receive quarterly phishing-simulation training — because the Corporate VLAN is the most likely initial entry point for an attacker (as modeled in the [Incident Response scenario](../06-incident-response-plan/README.md)), and segmentation only limits *how far* an initial compromise spreads, not whether it happens at all.

## Why this is the centerpiece function

Protect is deliberately where this project puts the most technical depth, because it's the function most directly demonstrable through configuration, diagrams, and design decisions — exactly what a hiring manager wants to see evidence of, rather than just a policy statement.

---
**Back to:** [03 — NIST CSF Implementation](README.md)
