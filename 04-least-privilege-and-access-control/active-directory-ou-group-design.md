# Active Directory OU & Group Design

## In plain terms

Active Directory (AD) is the system that manages who can log in and what they can access across the company. This file shows how VitalCore's AD is organized so that it mirrors the network segmentation and RBAC design already established — meaning identity and network security work as one system instead of two disconnected ones.

## Organizational Unit (OU) structure

```
vitalcore.local
├── OU=Tier0-Admins            (Domain Admins — 2 accounts, hardware-key MFA)
├── OU=Tier1-Admins
│   ├── OU=Server-Admins
│   ├── OU=Clinical-Systems-Admins
│   └── OU=Manufacturing-Engineers
├── OU=Tier2-Admins
│   └── OU=Helpdesk-Support
├── OU=Corporate
│   ├── OU=Finance
│   ├── OU=HR
│   ├── OU=Legal
│   └── OU=Sales
├── OU=RD-Lab
├── OU=Clinical-Trials
├── OU=Manufacturing
│   ├── OU=Operators
│   └── OU=Engineers
├── OU=Service-Accounts
└── OU=Computers
    ├── OU=Corporate-Workstations
    ├── OU=RD-Workstations
    ├── OU=Clinical-Workstations
    └── OU=Manufacturing-Devices
```

## Why this structure, specifically

- **Tier 0/1/2 admin OUs are separate from every other OU** so that Group Policy Objects (GPOs) enforcing strict security settings (MFA, logon restrictions, session timeouts) can be applied precisely to admin accounts without affecting regular staff, and vice versa.
- **Business-unit OUs mirror the RBAC roles matrix** ([rbac-roles-matrix.md](rbac-roles-matrix.md)) — a user placed in `OU=Clinical-Trials` is automatically eligible for `Clinical_Trial_Researcher`-scoped security groups, keeping identity management aligned with the access model rather than being a separate, easy-to-drift system.
- **Computer objects are separated by zone**, matching the VLAN design in [Section 02](../02-network-architecture-and-segmentation/README.md) — a workstation in `OU=Manufacturing-Devices` receives GPOs appropriate for OT/GxP-validated systems (e.g., no USB storage, no unsigned software), which are very different from the GPOs applied to Corporate workstations.

## Key security groups (tied to the RBAC matrix)

| AD Security Group | Grants | Members drawn from OU |
|---|---|---|
| `SG_Clinical_ReadWrite` | Clinical Trials application access | `OU=Clinical-Trials` |
| `SG_Clinical_Tier1_Admin` | Clinical Trials server/DB admin | `OU=Clinical-Systems-Admins` |
| `SG_Manufacturing_Operator` | Read-only production dashboard access | `OU=Manufacturing\Operators` |
| `SG_Manufacturing_Engineer` | PLC configuration access (change-controlled) | `OU=Manufacturing\Engineers` |
| `SG_Tier0_DomainAdmin` | Full domain admin, jump-host-only login | `OU=Tier0-Admins` |
| `SG_Helpdesk_Tier2` | Local admin on Corporate/R&D workstations only | `OU=Helpdesk-Support` |

## Key Group Policy enforcement

- **Tier separation enforced technically, not just by policy:** GPO logon restrictions ("Deny log on locally/through Remote Desktop") prevent Tier 2 (`SG_Helpdesk_Tier2`) accounts from ever authenticating to any system in the Tier 0 or Tier 1 OUs, even if someone tried — the tiering model in [the main README](README.md) is enforced by AD itself, not just documented as a policy.
- **Manufacturing and Clinical workstation OUs** have GPOs disabling removable storage and restricting installed software to a signed allow-list, appropriate for GxP-validated, regulated environments.
- **Password and lockout policy** is stricter for Tier 0/1 admin OUs (15-character minimum, 3 failed attempts = lockout) than for general staff, reflecting the higher damage potential of a compromised admin credential.

---
**Back to:** [04 — Least Privilege & Access Control](README.md)
