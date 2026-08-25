# Detection Use Cases & SOAR Playbooks

## In plain terms

A correlation rule (or "detection use case") is a specific pattern the SIEM watches for — "this exact combination of events probably means something bad is happening." A SOAR playbook is what happens automatically the moment that pattern fires, before a human even looks at it. Automating the *first* response step doesn't replace analyst judgment — it just means the obvious, time-critical action (like disabling a clearly-compromised account) happens in seconds instead of whatever time it takes a human to notice the alert, read it, and act.

## Detection use cases (correlation rules)

| Use case | Logic (simplified) | Severity | Maps to |
|---|---|---|---|
| **Cross-VLAN lateral movement attempt** | 5+ denied connections in 10 minutes from a single internal host toward a Restricted VLAN (40/50/70) | High | [detect.md](../03-nist-csf-implementation/detect.md), the ransomware scenario in [Section 06](../06-incident-response-plan/README.md) |
| **Impossible travel** | Same user account authenticates from two geographically distant locations within a time window that makes travel impossible | High | External incident triggers ([external-incident-response-plan.md](../09-csirt-and-incident-response/external-incident-response-plan.md)) |
| **Off-hours Clinical Trials access** | Any access to Clinical Trials app/DB outside documented business hours, by an account not on the on-call roster | High | HIPAA audit control ([Section 07](../07-hipaa-compliance-framework/README.md)) |
| **Privileged account used outside assigned tier** | A Tier 2 (Helpdesk) account attempts authentication to a Tier 0/1 system | Critical | Tiered admin model ([Section 04](../04-least-privilege-and-access-control/README.md)) |
| **Partner API scope violation** | Partner Extranet credential requests data outside its assigned OAuth2 scope | High | Partner Extranet controls ([Section 08](../08-network-protocols-and-segmentation/README.md)) |
| **Unrecognized device on Manufacturing VLAN** | New MAC address appears on VLAN 50 without a matching NAC/asset registration | Critical | OT protection ([protect.md](../03-nist-csf-implementation/protect.md)) |
| **Portal authentication spike** | Login attempt volume against MyVitalCore Portal exceeds baseline by a wide margin (credential stuffing indicator) | High | External Security Plan ([Section 10](../10-security-plans-and-response/README.md)) |
| **Mass data access/export from Clinical DB** | A single account queries or exports an abnormally large volume of PHI records in a short window | Critical | HIPAA technical safeguards ([Section 07](../07-hipaa-compliance-framework/README.md)) |

## SOAR playbooks (automated first response)

Each playbook below executes automatically the moment its matching detection use case fires — a case is simultaneously opened in the CSIRT's case system, so no automated action happens silently without a human record of it.

### Playbook: Phishing Email Triage
**Triggers on:** user-reported or EDR-flagged phishing email.
**Automated steps:** (1) extract sender/links/attachments, (2) check against threat intelligence feeds, (3) if confirmed malicious, automatically search all mailboxes and delete matching emails still in inboxes, (4) open a SEV-3 case for the [Internal Incident Response Plan](../09-csirt-and-incident-response/internal-incident-response-plan.md), (5) notify the reporting user their report was actioned.

### Playbook: Compromised Account Auto-Containment
**Triggers on:** impossible travel or privileged-tier violation.
**Automated steps:** (1) immediately disable the account (per [Section 04](../04-least-privilege-and-access-control/README.md) least-privilege design, a disabled account's blast radius is already limited to its assigned role), (2) revoke active sessions/tokens, (3) page the on-call SOC analyst, (4) open a case pre-populated with the triggering logon events for investigation.

### Playbook: Cross-VLAN Lateral Movement Response
**Triggers on:** the lateral movement detection use case above.
**Automated steps:** (1) automatically disable the source host's switch port (the exact containment action described in the [Section 06](../06-incident-response-plan/README.md) scenario, now automated rather than manual), (2) snapshot current firewall session state for forensics, (3) open a SEV-2 case and page the CSIRT Lead directly, since this pattern is the strongest early signal of a spreading incident.

### Playbook: Partner API Scope Violation
**Triggers on:** partner OAuth2 scope violation.
**Automated steps:** (1) automatically suspend the specific partner's API credentials (not the whole gateway), (2) notify the External Response Coordinator, (3) open a case for [external incident](../09-csirt-and-incident-response/external-incident-response-plan.md) triage to determine if this was a partner-side misconfiguration or a genuine compromise.

### Playbook: Mass PHI Access/Export
**Triggers on:** the mass data export detection use case.
**Automated steps:** (1) automatically suspend the account's access to the Clinical Trials application (not a network-wide action — precisely scoped, per least privilege), (2) immediately page the Compliance & Legal Liaison in addition to the SOC, since this pattern is the single fastest path to a HIPAA breach determination, (3) preserve the exact query/export logs as evidence.

## Why automation is scoped this narrowly

Every playbook above takes the *smallest* action that fully contains the specific risk — disabling one account, one switch port, one partner's credentials — never a broad action like "shut down the VLAN" or "disable all portal logins." This mirrors the least-privilege philosophy from [Section 04](../04-least-privilege-and-access-control/README.md): automation should have exactly enough authority to contain the specific problem it's designed for, and no more, so a false positive doesn't cause a self-inflicted outage at the scale of 100,000+ users.

---
**Back to:** [11 — SIEM & SOAR Implementation](README.md)
