# 01 — Company Profile & Scope

> ⚠️ VitalCore Pharmaceuticals is a **fictional company** invented for this portfolio. No real organization or data is represented.

## In plain terms

Before designing any security control, you need to understand *what* you're protecting and *why it matters*. This section lays out who VitalCore is, what kind of data and equipment it has, and which laws/regulations apply — because those regulations are what ultimately justify every technical decision made later in this project (segmentation, access control, monitoring). You can't design good security in a vacuum; it has to match the actual risk to the business.

## The company

**VitalCore Pharmaceuticals** is a fictional mid-sized pharmaceutical and medical device manufacturer with approximately **1,200 employees** across four sites:

| Site | Function | Sensitive assets |
|---|---|---|
| Corporate HQ | Finance, HR, Legal, Sales, general office staff | Employee PII, financial records, contracts |
| R&D Laboratory | Drug formulation & research | Proprietary drug formulas, unpatented research data |
| Manufacturing Plant | Production lines for drugs & medical devices | Industrial control systems (PLCs, SCADA, HMIs) |
| Clinical Trials Data Center | Stores patient trial data and results | Protected Health Information (PHI) |

## Why this company is a good security case study

VitalCore is deliberately designed to have **three very different risk profiles under one roof**, which is what makes network segmentation and least privilege so important here:

1. **Regulated health data (HIPAA)** — the Clinical Trials division holds Protected Health Information (PHI) on trial participants. A breach here isn't just embarrassing — it's a legal and regulatory event.
2. **FDA-regulated manufacturing (21 CFR Part 11 / GxP)** — the Manufacturing Plant runs industrial control systems that must maintain data integrity and validated, auditable processes. These systems are often old, rarely patched, and extremely sensitive to downtime — you cannot treat a PLC on a production line the same way you treat a laptop in Finance.
3. **High-value intellectual property (R&D)** — drug formulas and unpublished research are the company's core competitive asset. If R&D and Corporate share a network with no boundary, a phishing email opened in Finance could become a path to stealing a drug formula.

## Regulatory & compliance drivers referenced throughout this project

| Framework | Why it applies to VitalCore |
|---|---|
| **HIPAA** (Health Insurance Portability and Accountability Act) | Governs how Clinical Trials PHI must be protected, who may access it, and how breaches must be reported |
| **FDA 21 CFR Part 11** | Governs electronic records and signatures for FDA-regulated processes — relevant to Manufacturing and Clinical systems |
| **GxP** (Good Manufacturing/Laboratory/Clinical Practice) | Requires validated, auditable, tamper-evident systems in Manufacturing and R&D |
| **NIST Cybersecurity Framework (CSF) 2.0** | Used as the overarching framework to organize *all* security controls in this project (see [Section 03](../03-nist-csf-implementation/README.md)) |

## Data classification used throughout this project

| Classification | Description | Example |
|---|---|---|
| **Restricted** | Legally protected or business-critical; breach = regulatory/legal event | Clinical trial PHI, drug formulas |
| **Confidential** | Internal-only, damaging if leaked but not independently regulated | Financial reports, HR records |
| **Internal** | General business data, low individual risk | Internal memos, general office files |
| **Public** | Approved for external release | Press releases, marketing material |

This classification is the basis for the network segmentation zones built in [Section 02](../02-network-architecture-and-segmentation/README.md) — data classified as **Restricted** always lives in its own isolated network zone, never sharing a broadcast domain with general office traffic.

## Scope of this project

This portfolio focuses on the **network and identity security layer** — segmentation, framework-driven controls, and access management. It does **not** cover application-level security (e.g., secure coding of VitalCore's internal apps) or physical security of the buildings — those are acknowledged as out of scope so the project stays focused and demonstrable.

---
**Next:** [02 — Network Architecture & Segmentation →](../02-network-architecture-and-segmentation/README.md)
