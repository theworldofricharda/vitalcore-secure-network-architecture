# 10 — Security Plans & Response (Internal & External)

## In plain terms

[Section 09](../09-csirt-and-incident-response/README.md) covers what happens *during* an incident — this section covers everything that happens *before* one, to make incidents less likely and less damaging in the first place. Think of it as the difference between a fire department's response plan (Section 09) and a building's fire-prevention program — sprinklers, inspections, fire drills (this section). Both matter; neither replaces the other. As with the incident response plans, VitalCore's proactive security work splits naturally into **internal** (protecting VitalCore's own people and systems) and **external** (protecting and reassuring the 100,000+ portal users, patients, and partners who depend on VitalCore but aren't VitalCore employees).

## Internal Security Plan

Focused on reducing the likelihood and blast radius of incidents affecting VitalCore's own environment.

| Program | What it does | Cadence |
|---|---|---|
| **Vulnerability management** | Scans all internal VLANs for known vulnerabilities; Restricted-zone systems (Clinical, Manufacturing) get prioritized remediation SLAs | Weekly scans; Critical findings patched within 72 hours (Corporate/Servers) or via the jump-host-only process within 14 days (Manufacturing OT, per [protect.md](../03-nist-csf-implementation/protect.md)) |
| **Patch management** | Structured deployment of OS/application patches, tiered by system criticality | Monthly patch cycle; emergency out-of-cycle patches for actively-exploited vulnerabilities |
| **Endpoint detection & response (EDR)** | Behavioral monitoring agent on all Corporate, Servers, and R&D endpoints, feeding the SIEM (see [Section 11](../11-siem-soar-implementation/README.md)) | Continuous |
| **Security awareness training** | Phishing simulations and role-specific training (e.g., extra module for anyone with Clinical/Manufacturing access) | Quarterly, per [protect.md](../03-nist-csf-implementation/protect.md) |
| **Access reviews** | Recurring least-privilege verification | Quarterly, per [access-review-process.md](../04-least-privilege-and-access-control/access-review-process.md) |
| **Internal penetration testing** | An independent test of whether the segmentation and access controls in Sections 02/04/08 actually hold up against a simulated attacker | Annually, plus after any major network change |

## External Security Plan

Focused on protecting the 100,000+ external portal users, patients, and partner organizations, and on maintaining their trust even when things go wrong.

| Program | What it does | Cadence |
|---|---|---|
| **Perimeter monitoring (WAF/DDoS)** | Continuous filtering of malicious traffic to the Public DMZ, per [Section 08](../08-network-protocols-and-segmentation/README.md) | Continuous, with monthly rule-tuning review |
| **External vulnerability scanning & bug bounty program** | Independent security researchers can responsibly report vulnerabilities in MyVitalCore Portal through a published disclosure policy, with defined response SLAs | Continuous intake; acknowledgment within 3 business days |
| **Third-party/partner risk assessment** | Before any physician EHR system or pharmacy partner is granted Partner Extranet access, their security posture is assessed and an integration agreement (including a Business Associate Agreement where PHI is involved, per [Section 07](../07-hipaa-compliance-framework/README.md)) is signed | Before onboarding; reassessed annually |
| **Public security & trust page** | A published, plain-language page describing VitalCore's security commitments to portal users (encryption, MFA options, how to report a concern) | Reviewed and updated twice yearly |
| **External-facing incident communication readiness** | Pre-drafted (but not pre-dated) status page templates and notification letter templates, so the [External Incident Response Plan](../09-csirt-and-incident-response/external-incident-response-plan.md) isn't drafting communications from a blank page during a live incident | Reviewed quarterly alongside CSIRT tabletop exercises |
| **Uptime/availability commitments** | Defined internal SLO for portal availability, monitored continuously, feeding into external incident severity classification | Continuous |

## How the two plans reinforce each other

A weakness in the Internal Security Plan often becomes an External Security Plan problem. For example: the phishing scenario in [Section 06](../06-incident-response-plan/README.md) started as a purely internal risk, but if awareness training (internal) had failed *and* segmentation (internal) had also failed, the next line of defense would have been the External Security Plan's perimeter controls protecting the actual PHI and portal systems. Neither plan is sufficient by itself — this is [defense-in-depth](../03-nist-csf-implementation/protect.md) applied at the organizational-program level, not just the network level.

## Ownership

Per the governance model in [govern.md](../03-nist-csf-implementation/govern.md), the Internal Security Plan is owned by the IT Security Lead; the External Security Plan is jointly owned by the IT Security Lead and the Compliance Officer, reflecting that external-facing programs carry regulatory weight (HIPAA, partner BAAs) that purely internal programs don't.

---
**Previous:** [← 09 — CSIRT & Incident Response](../09-csirt-and-incident-response/README.md)
**Next:** [11 — SIEM & SOAR Implementation →](../11-siem-soar-implementation/README.md)
