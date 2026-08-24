# 04 — Least Privilege & Access Control

## In plain terms

**The Principle of Least Privilege (PoLP)** means every person and every system account gets *only* the access it needs to do its specific job — nothing more, and only for as long as it's needed. Think of it like hotel key cards: a housekeeper's card opens the rooms on their assigned floor, not every room in the hotel, and not the manager's office. Before this project, VitalCore had the network equivalent of a master key that opened every door being handed out to far more people than needed one — this section fixes that.

## The problem this fixes

VitalCore's original environment had:
- IT staff sharing a single local administrator account across all corporate laptops (no individual accountability — if something went wrong, there was no way to tell *which* admin did it).
- General staff accounts with standing access to file shares and systems well beyond what their job required, "just in case."
- No formal process for removing access when someone changed roles or left the company.

## The fix: a tiered administration model

VitalCore now uses a **three-tier admin model** (a widely recognized Microsoft/industry pattern) that separates *what* an admin account can touch based on sensitivity:

| Tier | Scope | Example accounts |
|---|---|---|
| **Tier 0** | Domain Controllers, Active Directory itself, Management VLAN (70) | Domain Admins (2 named individuals only, MFA + hardware key required) |
| **Tier 1** | Servers, applications (VLAN 20), Clinical Trials systems (VLAN 40) | Server Admins, Clinical Systems Admins |
| **Tier 2** | End-user workstations (VLAN 10, 30) | Helpdesk/Desktop Support |

**The core rule:** a Tier 2 (helpdesk) admin account can never be used to log into a Tier 0 or Tier 1 system, and a Tier 0 (Domain Admin) account is never used to log into an everyday workstation — this stops a helpdesk laptop compromise (the most common entry point) from ever escalating into full domain control, which is exactly how many real-world ransomware attacks spread from "one laptop" to "the whole company."

## Role-Based Access Control (RBAC)

Full role matrix: [rbac-roles-matrix.md](rbac-roles-matrix.md)

Instead of assigning permissions to individual people one-by-one (which becomes unmanageable and inconsistent over time), access is assigned to **roles**, and people are assigned to roles:

- `Role: Clinical_Trial_Researcher` → read/write access to the Clinical Trials application only, no access to the underlying database or server
- `Role: RD_Scientist` → access to R&D lab systems and file shares only
- `Role: Manufacturing_Operator` → access to view (not modify) production dashboards, no access to PLC configuration
- `Role: Finance_Staff` → access to Finance systems only, no access to R&D, Clinical, or Manufacturing zones at all

## Active Directory structure

Full design: [active-directory-ou-group-design.md](active-directory-ou-group-design.md)

Organizational Units (OUs) and security groups mirror the VLAN/data-classification structure from [Section 02](../02-network-architecture-and-segmentation/README.md), so that network segmentation and identity access control reinforce each other rather than being two disconnected systems.

## Access reviews

Full process: [access-review-process.md](access-review-process.md)

Access isn't "set and forget" — every Restricted-classification role (Clinical, Manufacturing, Tier 0/1 admin) is reviewed **quarterly**, and every account is reviewed immediately upon role change or termination.

## How this connects to the rest of the project

- Least privilege is the **Protect**-function control described in [protect.md](../03-nist-csf-implementation/protect.md).
- A privileged account being misused outside its assigned tier is a **Detect**-function signal described in [detect.md](../03-nist-csf-implementation/detect.md).
- The Manufacturing OT Owner and Clinical Data Steward roles referenced in [govern.md](../03-nist-csf-implementation/govern.md) are the people who approve changes to these access roles.

---
**Previous:** [← 03 — NIST CSF Implementation](../03-nist-csf-implementation/README.md)
**Next:** [05 — Risk Assessment →](../05-risk-assessment/risk-register.md)
