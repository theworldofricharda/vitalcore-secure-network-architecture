# Detect (DE)

## In plain terms

Locks and walls (Protect) will eventually be tested or bypassed — **Detect** is about noticing when that happens, ideally within minutes, not months. A 2024 industry average shows breaches often go undetected for weeks; the goal of this function is to make VitalCore's network "loud" when something abnormal happens, rather than silent.

## What VitalCore monitors and why

| Signal | Where it's collected | What it would indicate |
|---|---|---|
| Denied inter-VLAN connection attempts | Firewall logs → central SIEM | Malware or an attacker trying to move laterally (e.g., Corporate → Manufacturing) |
| Failed authentication spikes | Active Directory logs → SIEM | Password spraying, brute force, or a compromised credential being tested |
| Off-hours access to Clinical Trials (VLAN 40) | Application + network logs | Potential insider threat or compromised credential misuse |
| New/unrecognized device on Manufacturing VLAN | Network Access Control (NAC) + DHCP logs | Unauthorized device connected to OT network — a serious safety/compliance concern |
| Privileged account usage outside its assigned tier | Tiered admin logging (see [Section 04](../04-least-privilege-and-access-control/README.md)) | Privilege escalation or credential misuse |
| Data volume spikes leaving Clinical or R&D VLANs | Network flow monitoring | Possible data exfiltration |

## How it ties back to segmentation

The default-deny ACL matrix from [Section 02](../02-network-architecture-and-segmentation/README.md) isn't just a *preventive* control — every denied packet it generates is a **detection signal**. A single denied connection from Corporate to Manufacturing might be a fat-fingered config. Fifty denied connections in ten minutes from the same Corporate host is a strong early indicator of malware trying to spread — and it's something a flat network could never have surfaced, because on a flat network that traffic would never have hit a boundary at all.

## Alerting thresholds (example)

- **Low priority:** 1–4 denied cross-VLAN connection attempts per hour from a single host → logged, reviewed daily.
- **High priority:** 5+ denied attempts in 10 minutes from a single host, or *any* denied attempt targeting Manufacturing or Clinical Trials VLANs → immediate SIEM alert to the on-call security analyst, feeding directly into [Respond](respond.md).

## Why this matters

Detection is what turns segmentation from a static wall into an active alarm system. Without monitoring the denies, VitalCore would have the *prevention* benefit of segmentation but miss the equally valuable *early-warning* benefit — this is a distinction that shows a deeper understanding of defense-in-depth than segmentation alone.

---
**Back to:** [03 — NIST CSF Implementation](README.md)
