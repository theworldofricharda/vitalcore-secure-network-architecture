# Role-Based Access Control (RBAC) Matrix

## In plain terms

This table shows exactly what each job role can and cannot access. If a hiring manager wants to sanity-check "does this design actually enforce least privilege," this is the file that proves it — every role has a clearly bounded set of permissions, and nothing is granted "just in case."

## The matrix

| Role | Corporate (10) | Servers (20) | R&D (30) | Clinical (40) | Manufacturing (50) | Admin access |
|---|---|---|---|---|---|---|
| **Finance_Staff** | Read/Write (own files) | Read (Finance app only) | None | None | None | None |
| **HR_Staff** | Read/Write (own files) | Read/Write (HR system only) | None | None | None | None |
| **RD_Scientist** | Read/Write (own files) | None | Read/Write | None | None | None |
| **Clinical_Trial_Researcher** | Read/Write (own files) | None | None | Read/Write (app only, no DB) | None | None |
| **Clinical_Systems_Admin** | Read/Write (own files) | Read/Write (Clinical support only) | None | Full (Tier 1) | None | Tier 1 |
| **Manufacturing_Operator** | Read/Write (own files) | None | None | None | Read-only (dashboards) | None |
| **Manufacturing_Engineer** | Read/Write (own files) | None | None | None | Read/Write (PLC config, change-controlled) | Tier 1 (Manufacturing only) |
| **Helpdesk_Support** | Full (workstation support) | None | None | None | None | Tier 2 |
| **Server_Admin** | None | Full | None | None | None | Tier 1 |
| **Domain_Admin** | (only via Tier 0 jump host) | (only via Tier 0 jump host) | (only via Tier 0 jump host) | (only via Tier 0 jump host) | (only via Tier 0 jump host) | Tier 0 |

## Design rules behind this matrix

1. **No role has standing access to more than one Restricted zone.** A Clinical Systems Admin has full access within Clinical Trials but zero access to Manufacturing, and vice versa — even though both are "Tier 1 admin" roles, they are scoped to a single zone each.
2. **Read vs. write is explicit.** Manufacturing Operators can *view* production dashboards (needed for their daily job) but cannot modify PLC configuration — that requires the distinct Manufacturing_Engineer role, and even then, changes go through the change-control process defined in [govern.md](../03-nist-csf-implementation/govern.md).
3. **Every admin tier maps to the model in the main [README](README.md)** — a Helpdesk_Support account is Tier 2 and physically cannot authenticate to Tier 0/1 systems, enforced through Active Directory group membership (see [active-directory-ou-group-design.md](active-directory-ou-group-design.md)).
4. **Domain Admin is never used directly.** Even the two named individuals with Domain Admin rights access it only through a dedicated, monitored Tier 0 jump host with hardware-key MFA — not from their everyday workstation.

## How access is requested and granted

1. A manager submits an access request specifying the **role** needed (not raw permissions) — reinforcing that access is always granted through a defined role, never ad hoc.
2. The relevant data/system owner (e.g., Clinical Data Steward for any `Clinical_*` role) approves.
3. IT provisions the Active Directory group membership tied to that role.
4. The request and approval are logged for the next [access review](access-review-process.md).

---
**Back to:** [04 — Least Privilege & Access Control](README.md)
