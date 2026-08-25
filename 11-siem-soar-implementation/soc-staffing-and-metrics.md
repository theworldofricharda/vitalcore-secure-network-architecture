# SOC Staffing Model & Metrics

## In plain terms

A SIEM/SOAR platform doesn't run itself — it needs people watching the alerts it produces and making judgment calls the automation isn't built to make. This file describes how VitalCore's 50-person IT/Security organization staffs the Security Operations Center (SOC) that operates the platform described in [Section 11](README.md), and how VitalCore measures whether the whole detection-and-response system is actually working.

## SOC tiered staffing model

| Tier | Role | Headcount (of the 50-person org) | Responsibility |
|---|---|---|---|
| **Tier 1** | SOC Analyst (Alert Triage) | 12 | First review of SIEM alerts; confirms real vs. false positive; escalates confirmed incidents |
| **Tier 2** | SOC Analyst (Investigation) | 6 | Deeper log analysis, scope determination — this is the CSIRT's Forensics & Detection Analyst pool ([Section 09](../09-csirt-and-incident-response/README.md)) |
| **Tier 3** | Senior Analyst / Threat Hunter | 3 | Proactive hunting for threats that didn't trip an existing correlation rule; writes new detection use cases |
| **SOAR/Detection Engineer** | Platform Engineer | 2 | Builds and tunes correlation searches and playbooks ([detection-and-playbooks.md](detection-and-playbooks.md)) |
| **Network Engineering** | — | 8 | Owns segmentation, firewall rules ([Section 02](../02-network-architecture-and-segmentation/README.md), [08](../08-network-protocols-and-segmentation/README.md)) |
| **Identity & Access Management** | — | 6 | Owns RBAC, AD, access reviews ([Section 04](../04-least-privilege-and-access-control/README.md)) |
| **GRC & Compliance** | — | 5 | HIPAA/GxP compliance mapping, audits ([Section 07](../07-hipaa-compliance-framework/README.md)) |
| **CSIRT Leadership & Coordinators** | — | 4 | Named roles from [Section 09](../09-csirt-and-incident-response/README.md) |
| **Management/Leadership** | CISO + team leads | 4 | Governance ([govern.md](../03-nist-csf-implementation/govern.md)) |

**Total: ~50**, consistent with the organizational scale established in [Section 01](../01-company-profile-and-scope/README.md#digital-footprint--organizational-scale).

## Shift coverage

Because MyVitalCore Portal serves 100,000+ users around the clock, Tier 1 SOC coverage is **24/7/365**, staffed across three 8-hour shifts (4 analysts per shift, rotating), rather than a business-hours-only model — an external incident (e.g., credential stuffing against the portal) doesn't wait for 9am.

## Key metrics tracked

| Metric | Definition | Target |
|---|---|---|
| **MTTD** (Mean Time to Detect) | Time from an incident's start to SIEM alert generation | < 15 minutes for High/Critical use cases |
| **MTTR** (Mean Time to Respond/Contain) | Time from alert to first containment action | < 30 minutes internal, < 30 minutes external (matches the targets in both [incident response plans](../09-csirt-and-incident-response/README.md)) |
| **False positive rate** | Percentage of Tier 1-triaged alerts that turn out not to be real incidents | Tracked per correlation rule; rules exceeding 30% false-positive rate are sent back to the SOAR/Detection Engineer for tuning |
| **Playbook automation rate** | Percentage of eligible alerts where the SOAR playbook executed its automated first step successfully | > 95% |
| **Access review completion** | Percentage of quarterly access reviews ([Section 04](../04-least-privilege-and-access-control/access-review-process.md)) completed on schedule | 100% |
| **Tabletop exercise completion** | Quarterly internal/external tabletop exercises ([Section 09](../09-csirt-and-incident-response/README.md#tabletop-exercises)) completed as scheduled | 100% |

These metrics are surfaced on the **Executive Security Posture** dashboard described in the [main SIEM/SOAR README](README.md#dashboards), giving leadership a continuously updated view of whether the entire system built across this portfolio — segmentation, least privilege, CSIRT, and this platform — is actually performing, not just documented.

---
**Back to:** [11 — SIEM & SOAR Implementation](README.md)
