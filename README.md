# VitalCore Pharmaceuticals — Secure Network Architecture Portfolio

**A hands-on cybersecurity portfolio project demonstrating network segmentation, NIST Cybersecurity Framework (CSF) implementation, HIPAA compliance mapping, and the Principle of Least Privilege (PoLP) for a fictional pharmaceutical & medical device company.**

> ⚠️ **Disclaimer:** VitalCore Pharmaceuticals is a fictional company created solely for this portfolio. All names, IP addresses, employees, incidents, and data described here are invented for demonstration purposes. No real organization, patient, or dataset is represented.

---

## Why this project exists

I built this project to show — not just tell — how I would design and secure a network for an organization that handles some of the most sensitive data and equipment there is: patient/clinical trial records, proprietary drug research, and physical manufacturing equipment — and that also operates a public-facing portal serving over 100,000 external users, supported by a 50+ person IT/Security organization. Every section below walks through a real-world security problem, explains it in plain English first, and then backs it up with the technical implementation a hiring manager or engineering lead would expect to see.

**Each section is written in two layers:**
1. **"In plain terms"** — what the problem is and why it matters to the business, written for a non-technical reader (e.g., a hiring manager, recruiter, or executive).
2. **"How it was implemented"** — the actual technical design, configuration, and reasoning, for a technical reviewer.

---

## Table of Contents

| # | Section | What it demonstrates |
|---|---|---|
| 01 | [Company Profile & Scope](01-company-profile-and-scope/README.md) | Business context, regulatory drivers (HIPAA, FDA 21 CFR Part 11, GxP), data classification |
| 02 | [Network Architecture & Segmentation](02-network-architecture-and-segmentation/README.md) | VLAN design, Zero Trust zoning, firewall rule matrix, before/after network diagrams |
| 03 | [NIST CSF 2.0 Implementation](03-nist-csf-implementation/README.md) | Govern, Identify, Protect, Detect, Respond, Recover mapped to real controls |
| 04 | [Least Privilege & Access Control](04-least-privilege-and-access-control/README.md) | RBAC design, tiered admin model, Active Directory OU structure, access reviews |
| 05 | [Risk Assessment](05-risk-assessment/risk-register.md) | Risk register connecting business risk to technical controls |
| 06 | [Incident Response Plan](06-incident-response-plan/README.md) | A worked ransomware scenario showing segmentation and IR working together |
| 07 | [HIPAA Compliance Framework](07-hipaa-compliance-framework/README.md) | HIPAA Security Rule (Administrative/Physical/Technical safeguards) mapped line-by-line to VitalCore's controls |
| 08 | [Network Protocols & Segmentation (Internal & External)](08-network-protocols-and-segmentation/README.md) | Internal protocols (802.1X, Kerberos, IPsec), external/perimeter protocols (TLS 1.3, mTLS, WAF, BGP+RPKI), and the extended zone model for a 100K+ user public portal |
| 09 | [CSIRT & Incident Response](09-csirt-and-incident-response/README.md) | Formal CSIRT charter, severity classification, and separate Internal / External incident response plans |
| 10 | [Security Plans & Response](10-security-plans-and-response/README.md) | Proactive internal (patching, EDR, awareness) and external (bug bounty, partner risk, trust page) security programs |
| 11 | [SIEM & SOAR Implementation](11-siem-soar-implementation/README.md) | Splunk ES + SOAR architecture, log source onboarding, detection use cases, automated playbooks, and a 50-person SOC staffing model sized for 100K+ users |

---

## Skills demonstrated

`Network Segmentation` `VLAN Design` `Zero Trust Architecture` `Firewall Rule Engineering` `NIST CSF 2.0` `HIPAA Security Rule Compliance` `Risk Management` `Principle of Least Privilege` `Role-Based Access Control (RBAC)` `Active Directory Design` `Tiered Administration Model` `Incident Response Planning` `CSIRT Operations` `Regulatory Compliance (HIPAA, FDA 21 CFR Part 11, GxP)` `Internal & External Network Protocols` `Perimeter Security (WAF, DDoS, mTLS)` `SIEM (Splunk Enterprise Security)` `SOAR & Security Automation` `SOC Design & Staffing` `Security Documentation`

---

## About VitalCore Pharmaceuticals (the fictional company)

VitalCore Pharmaceuticals is a mid-sized pharmaceutical and medical device company (~1,200 employees) with four physical sites:

- **Corporate HQ** — finance, HR, legal, sales, general office staff
- **R&D Laboratory** — drug research, formulation, and lab equipment
- **Manufacturing Plant** — production lines controlled by industrial equipment (OT/SCADA/PLCs)
- **Clinical Trials Data Center** — patient health data and trial results (HIPAA-regulated)

It also operates **MyVitalCore Portal**, a public web/mobile platform used by over **100,000** patients, referring physicians, and pharmacy partners — supported by a **50+ person IT & Security organization** including a formally chartered CSIRT and a 24/7 SOC (see [Section 09](09-csirt-and-incident-response/README.md) and [Section 11](11-siem-soar-implementation/README.md)).

Because VitalCore handles regulated patient data, controls physical manufacturing equipment, and exposes services directly to the public internet, a security failure here isn't just an IT problem — it can mean a HIPAA violation, an FDA audit finding, a halted production line, a breach notification to 100,000+ people, or in the worst case, a patient safety issue. That context is what shapes every design decision in this repo.

Read the full profile: [01-company-profile-and-scope/README.md](01-company-profile-and-scope/README.md)

---

## How to navigate this repo

Start at Section 01 and read in order — each section builds on the last (the segmentation design in Section 02 is what makes the access control and monitoring in Sections 03–04 possible; Section 08 extends that segmentation to the external-facing portal, which Sections 09–11 then monitor and respond to). If you only have five minutes, read the **"In plain terms"** box at the top of each section's README.
