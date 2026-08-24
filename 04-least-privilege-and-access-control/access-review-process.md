# Access Review Process

## In plain terms

Access control isn't a one-time setup — people change roles, leave the company, or accumulate permissions over time that they no longer need ("access creep"). This document defines how VitalCore regularly checks that everyone's access still matches what their job actually requires, so the least-privilege design stays accurate over time instead of slowly drifting back toward "everyone has access to everything."

## Review cadence

| Trigger | Review scope | Owner |
|---|---|---|
| **Quarterly scheduled review** | All Restricted-classification roles (Clinical, Manufacturing, Tier 0/1 admin) | Relevant data/system owner + IT Security Lead |
| **Semi-annual scheduled review** | All Confidential/Internal roles (Corporate, R&D general access) | Department managers |
| **Immediate — role change** | The specific user's access, re-provisioned to match new role | Manager + IT |
| **Immediate — termination** | Full account disablement within 1 business day, not deletion (for audit trail) | HR + IT Security Lead |
| **Immediate — after any security incident** | Access relevant to the incident scope | IT Security Lead (per [respond.md](../03-nist-csf-implementation/respond.md)) |

## What a review actually checks

1. **Does this person still hold this job role?** (Confirmed against HR records.)
2. **Does their current access match the RBAC role tied to that job?** (Checked against the [RBAC matrix](rbac-roles-matrix.md).)
3. **Is there any access that was granted temporarily and never revoked?** (e.g., a project-based exception that outlived the project.)
4. **For Tier 0/1 admin accounts specifically:** is this still one of the minimum necessary people, and is MFA/hardware-key enrollment still active?

## Example finding from a simulated review

During a simulated quarterly review, one `Clinical_Trial_Researcher` account was found to still hold `SG_Clinical_Tier1_Admin` group membership from a two-month research project that had since ended. This is exactly the kind of "access creep" this process exists to catch — the access was revoked immediately, and the review log noted it as a process gap (the project's access grant had no expiration date set), which fed back into tightening the provisioning process to require an expiration date on all temporary access grants going forward.

## Why this matters

A least-privilege design is only as good as its last review. Documenting a *recurring* process — not just the initial design — demonstrates an understanding that access control is operational, ongoing security work, not a one-time architecture diagram.

---
**Back to:** [04 — Least Privilege & Access Control](README.md)
