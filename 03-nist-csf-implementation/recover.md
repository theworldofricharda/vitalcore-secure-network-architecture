# Recover (RC)

## In plain terms

**Recover** is about getting back to normal business operations safely, and then making sure the same incident is harder to repeat. It's easy to treat "the fire is out" as the finish line — Recover insists on two more steps: restoring properly (not just quickly), and learning from what happened.

## Recovery priorities

1. **Restore from isolated backups.** Backups for Clinical Trials and Manufacturing systems live in a separate backup VLAN with no live production access (see [protect.md](protect.md)) — specifically so that ransomware which encrypts production data cannot also reach and encrypt the backups. Recovery restores from these untouched backups.
2. **Validate before reconnecting.** Any restored system — especially Manufacturing OT (GxP-validated systems) — is verified and revalidated before being reconnected to its VLAN, since FDA-regulated manufacturing systems must maintain documented integrity.
3. **Staged reconnection.** Systems are brought back online zone by zone (e.g., Corporate first, then Servers, then only Manufacturing/Clinical last and only after full validation) rather than all at once, to avoid reintroducing the same issue.

## Post-incident review

After every incident (or major test/simulation), VitalCore runs a **lessons-learned review** covering:

- What allowed the initial compromise (e.g., a phishing email bypassing training)?
- Did segmentation perform as designed, or did it reveal a gap (e.g., an ACL rule that was broader than necessary)?
- Were detection alerts timely, or did they fire too late?
- Does the Risk Register ([Section 05](../05-risk-assessment/risk-register.md)) need a new entry, or does an existing risk's likelihood/impact rating need updating?

## Example outcome from the worked scenario

In the ransomware scenario documented in [Section 06](../06-incident-response-plan/README.md), the post-incident review recommended two concrete improvements: (1) reducing the number of Corporate users with local admin rights (tightening [least privilege](../04-least-privilege-and-access-control/README.md) further), and (2) adding a stricter alert threshold for the Servers VLAN specifically, since that was where lateral movement was first attempted.

## Why this matters

Recovery isn't just "turn it back on" — treating it as a distinct, deliberate phase is what prevents an organization from restoring the exact same vulnerable configuration that got compromised in the first place. Closing the loop back into the Risk Register and Govern function is what makes this a continuous cycle rather than a one-time fix.

---
**Back to:** [03 — NIST CSF Implementation](README.md)
