# Inter-VLAN Access Control Matrix

## In plain terms

This table is the "who can talk to whom" rulebook for the whole network. Read it like a permissions grid: find the row (source zone), find the column (destination zone), and the cell tells you whether that connection is allowed, and if so, under what restriction. Anything not explicitly marked "Allow" is **denied by default** — this is the core idea behind **default-deny / Zero Trust networking**: nothing is trusted just because it's "inside" the network.

## The matrix

✅ = Allowed (specific ports only) · ⛔ = Denied · 🔸 = Allowed one-way only

| Source ↓ / Destination → | Corporate (10) | Servers (20) | R&D (30) | Clinical (40) | Manufacturing (50) | Guest (60) | Mgmt (70) | DMZ (80) |
|---|---|---|---|---|---|---|---|---|
| **Corporate (10)** | — | ✅ 443/445 (intranet, file shares) | ⛔ | ⛔ | ⛔ | ⛔ | ⛔ | ✅ 443 |
| **Servers (20)** | 🔸 responses only | — | ⛔ | ⛔ | ⛔ | ⛔ | ⛔ | ⛔ |
| **R&D (30)** | ⛔ | ✅ 443 (specific research app) | — | ⛔ | ⛔ | ⛔ | ⛔ | ⛔ |
| **Clinical (40)** | ⛔ | ⛔ | ⛔ | — | ⛔ | ⛔ | ⛔ | ⛔ |
| **Manufacturing (50)** | ⛔ | ✅ 443 (via jump host only, to patch server) | ⛔ | ⛔ | — | ⛔ | ⛔ | ⛔ |
| **Guest (60)** | ⛔ | ⛔ | ⛔ | ⛔ | ⛔ | Internet only | ⛔ | ⛔ |
| **Mgmt (70)** | ⛔ (admin jump host only) | ⛔ (admin jump host only) | ⛔ | ⛔ | ⛔ (admin jump host only) | ⛔ | — | ⛔ |
| **DMZ (80)** | ⛔ | ✅ 443 (app data pull, one specific server) | ⛔ | ⛔ | ⛔ | ⛔ | ⛔ | — |

## Key rules explained

1. **Clinical Trials (VLAN 40) is a dead end.** Nothing initiates a connection *into* it except the specific application server that runs the trial software, and it initiates *nothing* outbound except logging traffic to the SIEM. This is the highest-sensitivity zone (PHI/HIPAA) and it is treated accordingly — total isolation except for its one legitimate function.

2. **Manufacturing (VLAN 50) never talks to the general Corporate network.** The only path out of Manufacturing is to a single patch-management server in Servers (20), and only through a hardened jump host — never a direct route, and never to the internet. This protects fragile, rarely-patched industrial control systems from both external threats and accidental exposure to office malware.

3. **Management (VLAN 70) is reachable by nobody except through a jump host with MFA.** Even a fully compromised Corporate laptop cannot reach a switch or firewall's admin panel directly — an attacker would first need to compromise the jump host itself, which is monitored and requires multi-factor authentication.

4. **Guest Wi-Fi (VLAN 60) is a true island.** It can reach the internet and absolutely nothing else on VitalCore's internal network — this is standard practice and prevents a compromised or malicious guest device from ever touching internal systems.

5. **DMZ (VLAN 80) never initiates connections inward beyond one narrow exception.** Public-facing systems are assumed to be the most likely to be attacked first, so the DMZ is architecturally prevented from being a stepping stone deeper into the network.

## Why this matters for the business

This matrix directly implements the **"Protect" function** of the NIST CSF (see [Section 03](../../03-nist-csf-implementation/protect.md)) and enforces the **data classification** defined in [Section 01](../../01-company-profile-and-scope/README.md) — Restricted zones (Clinical, Manufacturing, Management) are the most locked-down, exactly matching their business risk.

---
**Back to:** [02 — Network Architecture & Segmentation](../README.md)
