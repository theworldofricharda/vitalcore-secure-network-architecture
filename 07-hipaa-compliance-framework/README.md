# 07 — HIPAA Compliance Framework

## In plain terms

**HIPAA** (the Health Insurance Portability and Accountability Act) is the U.S. law that governs how Protected Health Information (PHI) — things like a patient's medical history, treatment records, or trial results — must be handled. VitalCore's Clinical Trials division stores exactly this kind of data on trial participants, which means HIPAA isn't optional background reading here; it's a legal obligation that directly shapes how the Clinical Trials VLAN (40) was designed in [Section 02](../02-network-architecture-and-segmentation/README.md).

This section exists to show something specific: not just "we built good security controls," but "here is exactly which HIPAA requirement each control satisfies" — the kind of traceability an auditor, compliance officer, or security-minded employer actually wants to see.

## Which parts of HIPAA apply here

| HIPAA Rule | What it covers | Relevance to VitalCore |
|---|---|---|
| **Privacy Rule** | Who may access/use PHI, and for what purpose | Governs which roles (see [RBAC matrix](../04-least-privilege-and-access-control/rbac-roles-matrix.md)) may touch trial participant data |
| **Security Rule** | Required administrative, physical, and technical safeguards for *electronic* PHI (ePHI) | The main focus of this section — maps directly to network and access controls already built |
| **Breach Notification Rule** | Requirements for notifying affected individuals/HHS after a breach involving PHI | Feeds directly into the [Incident Response Plan](../06-incident-response-plan/README.md) |

## The HIPAA Security Rule's three safeguard categories, mapped to VitalCore's controls

### 1. Administrative Safeguards (the policies and people side)

| HIPAA requirement | VitalCore control |
|---|---|
| Designated security official responsible for HIPAA compliance | Compliance Officer role, defined in [govern.md](../03-nist-csf-implementation/govern.md) |
| Workforce access authorization/clearance procedures | RBAC provisioning process in [rbac-roles-matrix.md](../04-least-privilege-and-access-control/rbac-roles-matrix.md) — only `Clinical_*` roles, approved by the Clinical Data Steward, ever gain PHI access |
| Periodic access review | Quarterly access review cycle in [access-review-process.md](../04-least-privilege-and-access-control/access-review-process.md) |
| Security awareness training | Quarterly phishing-simulation and security training, noted in [protect.md](../03-nist-csf-implementation/protect.md) |
| Contingency plan (backup, disaster recovery) | Isolated backup VLAN for Clinical Trials systems, described in [protect.md](../03-nist-csf-implementation/protect.md) and [recover.md](../03-nist-csf-implementation/recover.md) |
| Business Associate Agreements (BAAs) | Any third party (e.g., a cloud analytics vendor) processing trial data must sign a BAA before any Clinical VLAN (40) connectivity is provisioned — enforced as a Governance gate, not just a legal formality |

### 2. Physical Safeguards (the "locked door" side)

| HIPAA requirement | VitalCore control |
|---|---|
| Facility access controls for systems housing ePHI | Clinical Trials Data Center requires badge access + visitor log, restricted to Clinical Systems Admins and authorized Clinical staff |
| Workstation security | Clinical Trials workstations (VLAN 40) are physically located in access-controlled areas and auto-lock after 5 minutes of inactivity |
| Device/media controls (disposal, reuse) | Any drive or device that ever touched Clinical VLAN data is wiped per NIST 800-88 media sanitization guidelines before reuse or disposal |

### 3. Technical Safeguards (the "in the network/systems" side — where this project's depth lives)

| HIPAA requirement | VitalCore control |
|---|---|
| **Access control** — unique user IDs, automatic logoff, encryption | Individual (non-shared) accounts enforced via the [tiered admin model](../04-least-privilege-and-access-control/README.md); no shared/local admin accounts remain |
| **Audit controls** — record and examine activity in systems with ePHI | Every access to the Clinical Trials application and database is logged and forwarded to the central SIEM, as described in [detect.md](../03-nist-csf-implementation/detect.md) |
| **Integrity controls** — protect ePHI from improper alteration/destruction | Clinical Trials database changes are logged with before/after values; backups are isolated (see above) so ransomware cannot alter both production and backup copies |
| **Transmission security** — protect ePHI in transit | All traffic to/from VLAN 40 is encrypted (TLS); the [ACL matrix](../02-network-architecture-and-segmentation/firewall-rules/inter-vlan-acl-matrix.md) ensures ePHI never transits an unencrypted or unnecessary network path |
| **Network isolation of ePHI systems** *(not an explicit line item in the Rule's text, but the clearest way to satisfy the "minimum necessary" standard)* | Clinical Trials VLAN (40) segmentation — see [Section 02](../02-network-architecture-and-segmentation/README.md) — is the architectural backbone that makes every safeguard above enforceable in the first place |

## How the Breach Notification Rule connects to Incident Response

If a confirmed incident involves unauthorized access to or disclosure of PHI, the [Respond](../03-nist-csf-implementation/respond.md) process explicitly routes the decision to the Compliance Officer, who determines:

1. Whether the exposure meets HIPAA's definition of a "breach" (vs. a permitted disclosure or a low-probability-of-compromise event).
2. Whether notification to affected individuals, HHS, and (for large breaches) media is required, and on what timeline (HIPAA requires notification without unreasonable delay, and no later than 60 days).

This is worked through concretely in the [Incident Response Plan](../06-incident-response-plan/README.md)'s ransomware scenario.

## Why HIPAA gets its own section instead of just being a footnote

Frameworks like NIST CSF ([Section 03](../03-nist-csf-implementation/README.md)) tell you *how to organize* a security program. HIPAA tells you *what the law specifically requires* for one particular category of data VitalCore holds. Showing both — a general framework **and** a specific regulatory mapping — demonstrates the ability to translate technical controls into the language a compliance officer, auditor, or legal team actually needs to hear.

---
**Previous:** [← 06 — Incident Response Plan](../06-incident-response-plan/README.md)
**Back to:** [Main README](../README.md)
