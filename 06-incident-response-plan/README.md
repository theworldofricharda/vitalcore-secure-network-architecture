# 06 — Incident Response Plan

## In plain terms

This section answers the question every other section has been building toward: **"Okay, but what actually happens the day something goes wrong?"** Rather than describe the incident response plan abstractly, this walks through one realistic, worked scenario end-to-end — a ransomware infection starting from a phishing email — and shows exactly how the segmentation ([Section 02](../02-network-architecture-and-segmentation/README.md)), least privilege ([Section 04](../04-least-privilege-and-access-control/README.md)), and monitoring ([detect.md](../03-nist-csf-implementation/detect.md)) built elsewhere in this project change the outcome.

## Scenario: Ransomware via phishing email

### Hour 0 — Initial compromise
A Finance employee in **Corporate (VLAN 10)** opens a malicious attachment. Ransomware executes and attempts to encrypt local files, then begins scanning the network for other reachable systems to spread to — standard ransomware behavior.

### Hour 0:05 — Detection
The ransomware's network scan generates a burst of connection attempts from the infected host toward Servers (VLAN 20), and a smaller number toward Manufacturing (VLAN 50) and Clinical Trials (VLAN 40). Because the [ACL matrix](../02-network-architecture-and-segmentation/firewall-rules/inter-vlan-acl-matrix.md) denies almost all of this traffic by default, the firewall logs a spike of denied connections from a single host — tripping the **high-priority alert threshold** defined in [detect.md](../03-nist-csf-implementation/detect.md) (5+ denied attempts in 10 minutes). The on-call security analyst is paged automatically.

> **Compare this to VitalCore's original flat network:** on the old architecture, that same scan would have found *no boundary at all* — the ransomware could have reached the Clinical Trials database or Manufacturing PLCs directly, with no denied-connection signal to alert on, because there was nothing to deny.

### Hour 0:10 — Containment
Following the priority order in [respond.md](../03-nist-csf-implementation/respond.md), the Network Engineer disables the switch port for the infected host and, as a precaution, temporarily tightens the Corporate VLAN's ACL to block all outbound SMB traffic VLAN-wide. Because Corporate (10) has no legitimate path to Manufacturing (50) or Clinical Trials (40) in the first place, **no containment action is even needed on those zones** — they were never reachable to begin with. Production keeps running. Clinical trial data is never at risk.

### Hour 0:15 — Notification
The IT Security Lead is notified (incident commander). Because the scan attempted contact with the Clinical Trials VLAN (even though it was blocked), the **Compliance Officer is notified out of caution**, per the HIPAA-driven process in [07 — HIPAA Compliance Framework](../07-hipaa-compliance-framework/README.md), to assess whether any PHI exposure occurred. (Answer, after investigation: no — the connection was denied at the firewall and never reached the Clinical application or database.)

### Hour 0:30 – 4:00 — Investigation & eradication
SIEM logs (per [detect.md](../03-nist-csf-implementation/detect.md)) are used to confirm the ransomware never successfully reached any system outside the infected host's local files and the immediately adjacent Corporate VLAN share it had legitimate access to. The infected machine is wiped and rebuilt from a known-clean image. The compromised employee's credentials are reset; because that employee's account was a standard `Finance_Staff` role with no admin rights (per the [RBAC matrix](../04-least-privilege-and-access-control/rbac-roles-matrix.md)), the ransomware never had the privileges to disable security tooling or spread via admin shares.

### Day 1 — Recovery
Per [recover.md](../03-nist-csf-implementation/recover.md), the only files actually lost are the ones on the infected host and the one Corporate file share it had access to — restored from the previous night's backup. Because Clinical and Manufacturing backups sit in an isolated backup VLAN never reachable from Corporate, they were never at risk regardless of the outcome above.

### Day 2 — Post-incident review
The lessons-learned review (per [recover.md](../03-nist-csf-implementation/recover.md)) identifies two improvements:
1. Reduce the number of Corporate users with any local admin rights further (tightening [least privilege](../04-least-privilege-and-access-control/README.md)).
2. Add a *medium*-priority alert tier for smaller denied-connection bursts (previously only high-priority bursts of 5+ triggered an alert; this incident's early scanning activity was borderline and worth catching sooner).

Both improvements are logged as updates to the [Risk Register](../05-risk-assessment/risk-register.md).

## Why a worked scenario, not just a policy document

Anyone can write "we have an incident response plan." Walking through one concrete, plausible scenario and showing exactly how each control from every other section of this project changes the outcome is what demonstrates the design actually holds up under something resembling a real attack — not just on paper.

## Incident response roles reference

| Role | Responsibility |
|---|---|
| IT Security Lead | Incident commander |
| Network Engineer | Executes containment (VLAN/port isolation) |
| Compliance Officer | HIPAA/regulatory breach determination |
| Manufacturing OT Owner | Approves any containment action touching Manufacturing |
| Communications/Legal | External disclosure, if required |

---
**Previous:** [← 05 — Risk Assessment](../05-risk-assessment/risk-register.md)
**Next:** [07 — HIPAA Compliance Framework →](../07-hipaa-compliance-framework/README.md)
