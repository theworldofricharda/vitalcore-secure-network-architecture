# 09 — CSIRT Charter & Incident Response

## In plain terms

[Section 06](../06-incident-response-plan/README.md) walked through one incident scenario end-to-end. As VitalCore grew to support 100,000+ external portal users and a 50-person IT/Security organization, incident response had to grow from "the IT Security Lead coordinates informally" into a **formally chartered team with defined authority, roles, and — critically — separate playbooks for incidents that stay internal versus incidents that touch the outside world.** A ransomware infection on one employee's laptop and a data breach exposing patient portal accounts require very different people, timelines, and communication rules, even though both are "security incidents."

This section establishes the **Computer Security Incident Response Team (CSIRT)**: who's on it, what authority it has, how incidents are classified, and — the key addition here — a distinct plan for **internal-only** incidents versus **external-facing** incidents.

## CSIRT charter

**Mission:** Detect, contain, eradicate, and recover from security incidents affecting VitalCore's internal systems, external-facing services, and third-party integrations — while meeting all regulatory notification obligations (HIPAA, state breach law, FDA reporting where applicable).

**Authority:** During a declared incident, the CSIRT Lead has the authority to isolate any network segment, disable any account, and take any system offline — including production Manufacturing or Clinical systems — without needing prior sign-off from the normal change-control process defined in [govern.md](../03-nist-csf-implementation/govern.md). This emergency authority is deliberately pre-approved by leadership *before* an incident, so it never has to be negotiated for the first time during one.

**Scope:** All VitalCore-owned systems (internal VLANs 10–70), all external-facing systems established in [Section 08](../08-network-protocols-and-segmentation/README.md) (Public DMZ, Partner Extranet, Remote Access), and any third-party system with access to VitalCore data (pharmacy/physician partner integrations).

## Team structure

| Role | Responsibility | Drawn from |
|---|---|---|
| **CSIRT Lead** | Overall incident commander; declares incident severity; has emergency authority above | IT Security Lead ([govern.md](../03-nist-csf-implementation/govern.md)) |
| **Internal Response Coordinator** | Leads containment/eradication for internal-only incidents (endpoints, AD, internal servers) | Server_Admin / Tier 1 Admin pool |
| **External Response Coordinator** | Leads containment for incidents touching the Public DMZ, Partner Extranet, or portal users | Network Engineering |
| **Forensics & Detection Analyst** | Log analysis, scope determination, evidence preservation | SOC Tier 2/3 analysts (see [Section 11](../11-siem-soar-implementation/README.md)) |
| **Compliance & Legal Liaison** | Determines HIPAA/regulatory notification obligations; coordinates with Legal | Compliance Officer |
| **Communications Coordinator** | Owns all external messaging — status page updates, customer/patient notifications, media | Communications/Legal |
| **Manufacturing OT Owner** (as-needed) | Approves any containment action touching production systems | Manufacturing OT Owner ([govern.md](../03-nist-csf-implementation/govern.md)) |

## Severity classification

Severity now explicitly accounts for whether external users are affected — a distinction that matters more as the portal user base has grown past 100,000.

| Severity | Definition | Example | Notification clock starts? |
|---|---|---|---|
| **SEV-1 (Critical)** | Confirmed or suspected breach of PHI, or portal-wide outage affecting all external users | Clinical database accessed by unauthorized party; portal down for all 100K+ users | Yes — immediately |
| **SEV-2 (High)** | Contained compromise with potential for external impact, or partial external outage | Ransomware detected before reaching Clinical/Manufacturing; portal degraded for a subset of users | Yes — pending scope confirmation |
| **SEV-3 (Medium)** | Internal-only incident with no external exposure | Phishing-delivered malware contained to Corporate VLAN (as in [Section 06](../06-incident-response-plan/README.md)) | No |
| **SEV-4 (Low)** | Policy violation or low-risk anomaly, no active compromise | A single denied cross-VLAN connection attempt, later confirmed as misconfiguration | No |

## Internal vs. external response — why they're split

- **[Internal Incident Response Plan](internal-incident-response-plan.md)** governs incidents contained to VitalCore's own internal VLANs and staff — the priority is fast containment and minimal business disruption, with internal-only communication (IT, affected department, leadership).
- **[External Incident Response Plan](external-incident-response-plan.md)** governs incidents touching the Public DMZ, Partner Extranet, or any of the 100,000+ portal users — the priority is the same containment work, *plus* a much stricter communication and regulatory-notification obligation, because people outside VitalCore are directly affected and often legally entitled to be told.

## Tabletop exercises

The CSIRT runs a tabletop exercise (a walk-through simulation, not a live test) **quarterly**, alternating between an internal-only scenario and an external-facing scenario, so both plans stay rehearsed rather than theoretical. Findings feed back into the [Risk Register](../05-risk-assessment/risk-register.md), exactly as described in [recover.md](../03-nist-csf-implementation/recover.md).

---
**Previous:** [← 08 — Network Protocols & Segmentation](../08-network-protocols-and-segmentation/README.md)
**Next:** [10 — Security Plans & Response →](../10-security-plans-and-response/README.md)
