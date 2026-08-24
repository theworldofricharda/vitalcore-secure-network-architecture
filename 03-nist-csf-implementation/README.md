# 03 — NIST Cybersecurity Framework (CSF) 2.0 Implementation

## In plain terms

The **NIST Cybersecurity Framework** is like a checklist created by the U.S. National Institute of Standards and Technology that helps organizations answer one question: *"Are we covering all the bases when it comes to security?"* Rather than inventing security priorities from scratch, this project organizes every control around NIST CSF's six core functions — a structure that's recognized and trusted across the security industry, so any employer reviewing this project can immediately see how the technical work maps to an established standard.

## The six functions (CSF 2.0)

CSF 2.0 (released 2024) added **Govern** as a new core function alongside the original five, reflecting the industry's shift toward treating cybersecurity as a business risk decision, not just a technical task.

| Function | Plain-English question it answers | VitalCore implementation |
|---|---|---|
| 🏛️ **Govern** | Who is accountable for security decisions, and what's our risk appetite? | [govern.md](govern.md) |
| 🔍 **Identify** | What do we have, and what's most at risk? | [identify.md](identify.md) |
| 🛡️ **Protect** | What safeguards stop a bad thing from happening? | [protect.md](protect.md) |
| 👁️ **Detect** | How do we notice when something's going wrong? | [detect.md](detect.md) |
| 🚨 **Respond** | What do we do the moment we notice a problem? | [respond.md](respond.md) |
| 🔁 **Recover** | How do we get back to normal, and learn from it? | [recover.md](recover.md) |

## Why organize the whole project this way?

Segmentation ([Section 02](../02-network-architecture-and-segmentation/README.md)) and least privilege ([Section 04](../04-least-privilege-and-access-control/README.md)) are both, on their own, just good technical practices. Mapping them to NIST CSF functions shows something more valuable to an employer: **the ability to connect hands-on technical work to a governance framework that executives, auditors, and regulators already recognize.** That's the difference between "I set up VLANs" and "I designed a segmentation strategy that fulfills the Protect function of our security program and directly supports our HIPAA and GxP compliance obligations."

## Control mapping summary

| Business Risk (from [Risk Register](../05-risk-assessment/risk-register.md)) | CSF Function(s) addressed | Control implemented |
|---|---|---|
| Flat network allows lateral movement to Clinical/Manufacturing | Protect | VLAN segmentation + ACL matrix ([Section 02](../02-network-architecture-and-segmentation/README.md)) |
| Excessive standing admin access | Protect | Tiered admin model + PoLP ([Section 04](../04-least-privilege-and-access-control/README.md)) |
| No visibility into cross-VLAN attack attempts | Detect | Firewall logging + SIEM alerting ([detect.md](detect.md)) |
| No documented incident process | Respond, Recover | Incident Response Plan ([Section 06](../06-incident-response-plan/README.md)) |
| No formal ownership of security risk decisions | Govern | Governance structure ([govern.md](govern.md)) |
| Incomplete asset/data inventory | Identify | Asset & data classification ([identify.md](identify.md)) |

---
**Previous:** [← 02 — Network Architecture & Segmentation](../02-network-architecture-and-segmentation/README.md)
**Next:** [04 — Least Privilege & Access Control →](../04-least-privilege-and-access-control/README.md)
