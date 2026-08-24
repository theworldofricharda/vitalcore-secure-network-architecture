# Respond (RS)

## In plain terms

An alert firing is only useful if there's a clear, rehearsed plan for what happens in the next five minutes. **Respond** defines exactly that: who gets notified, what gets isolated first, and how the team communicates during an active incident — so that the moment of highest stress isn't also the moment decisions are being invented from scratch.

## Response priorities, in order

1. **Contain** — isolate the affected VLAN or host first, before investigating root cause. Segmentation makes this fast: disabling a single switch port or VLAN ACL can fully isolate a compromised zone in seconds, rather than needing to hunt down every affected device on a flat network.
2. **Notify** — the IT Security Lead and, depending on which VLAN is affected, the Compliance Officer (if Clinical Trials/PHI is involved) or the Manufacturing OT Owner (if Manufacturing is involved) are notified within 15 minutes of a confirmed high-priority alert.
3. **Investigate** — determine scope using SIEM logs and the denied-connection data described in [Detect](detect.md).
4. **Eradicate** — remove the malicious presence (malware, unauthorized account, rogue device).
5. **Communicate** — for any incident touching Clinical Trials data, the Compliance Officer determines whether HIPAA breach notification obligations apply.

## Worked example: how segmentation changes the response

A detailed worked scenario (ransomware entering through a Corporate phishing email) is documented in full in [Section 06 — Incident Response Plan](../06-incident-response-plan/README.md). The short version: because Corporate (VLAN 10) has no direct route to Manufacturing (VLAN 50) or Clinical Trials (VLAN 40), containment means isolating one VLAN rather than shutting down the entire company network — production keeps running, and patient trial data is never at risk, because the ACL matrix from [Section 02](../02-network-architecture-and-segmentation/README.md) already prevented any path between them.

## Roles during an incident

| Role | Responsibility during response |
|---|---|
| IT Security Lead | Incident commander; coordinates containment and investigation |
| Network Engineer | Executes VLAN isolation / firewall rule changes |
| Compliance Officer | Determines regulatory notification obligations (HIPAA, FDA) |
| Manufacturing OT Owner | Approves any containment action affecting production systems |
| Communications/Legal | Manages any external disclosure |

## Why this matters

A response plan that has to figure out "can we even isolate this?" *during* an incident is far slower than one built on a network that was already designed to be isolated in pieces. This is the payoff of doing the segmentation work in [Section 02](../02-network-architecture-and-segmentation/README.md) properly — Respond becomes fast because Protect already did the hard architectural work in advance.

---
**Back to:** [03 — NIST CSF Implementation](README.md)
