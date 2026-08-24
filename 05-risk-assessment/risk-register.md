# Risk Register

## In plain terms

A risk register is the bridge between "here's a technical problem" and "here's why the business should care and pay to fix it." Each row below follows the same logic a real security team uses: what could go wrong, how likely and how bad is it, and what specifically was done about it. This is also the file that ties every other section of this project together — every control built elsewhere in this repo traces back to a risk listed here.

## How risk is scored

**Risk Score = Likelihood × Impact**, each rated 1 (low) to 5 (high). A score of 15+ is treated as a priority requiring immediate remediation.

## Register

| ID | Risk description | Likelihood | Impact | Score | Control implemented | Residual risk |
|---|---|---|---|---|---|---|
| R1 | Flat network allows lateral movement from a compromised Corporate device to Clinical Trials (PHI) or Manufacturing (OT) systems | 4 | 5 | 20 | VLAN segmentation + default-deny ACL matrix ([Section 02](../02-network-architecture-and-segmentation/README.md)) | Low — direct paths eliminated; residual risk limited to social engineering of a user with legitimate cross-zone access |
| R2 | Shared local administrator accounts prevent accountability and let one compromised credential unlock every workstation | 4 | 4 | 16 | Individual accounts + tiered admin model ([Section 04](../04-least-privilege-and-access-control/README.md)) | Low — remaining risk is credential theft of an individual admin account, mitigated by MFA |
| R3 | Clinical Trials PHI accessible to staff outside the Clinical division ("access creep") | 3 | 5 | 15 | RBAC scoping + quarterly access reviews ([access-review-process.md](../04-least-privilege-and-access-control/access-review-process.md)) | Low — caught and corrected via recurring review cycle |
| R4 | No monitoring on denied cross-VLAN traffic — malware could probe internal boundaries undetected | 3 | 4 | 12 | SIEM logging + alert thresholds ([detect.md](../03-nist-csf-implementation/detect.md)) | Medium — detection is reactive; still depends on timely analyst response |
| R5 | Manufacturing OT devices (PLCs/SCADA) are rarely patched and highly sensitive to downtime, making them attractive, fragile targets | 3 | 5 | 15 | OT isolation, no direct internet route, jump-host-only patching ([Section 02](../02-network-architecture-and-segmentation/README.md), [protect.md](../03-nist-csf-implementation/protect.md)) | Medium — legacy OT devices remain inherently harder to patch; compensating network controls are the primary mitigation |
| R6 | No documented incident response process — response actions would be improvised during an active incident | 4 | 4 | 16 | Formal IR plan with defined roles ([Section 06](../06-incident-response-plan/README.md)) | Low — plan tested via tabletop scenario |
| R7 | Ransomware encrypting production data could also destroy backups if backups are reachable from production | 2 | 5 | 10 | Isolated backup VLAN, unreachable from production zones ([protect.md](../03-nist-csf-implementation/protect.md)) | Low |
| R8 | Uncontrolled/unreviewed firewall rule changes could silently reopen a path into a Restricted VLAN | 2 | 4 | 8 | Formal change-control requiring data/system owner sign-off ([govern.md](../03-nist-csf-implementation/govern.md)) | Low |
| R9 | Missed HIPAA breach notification deadline following a PHI-involving incident | 2 | 5 | 10 | Compliance Officer explicitly assigned breach-determination role in IR process ([07 — HIPAA Compliance Framework](../07-hipaa-compliance-framework/README.md)) | Low |

## What this register drives

- **Prioritization:** R1, R3, and R5 (scores of 15+) were treated as the founding priorities of this entire project — they are the direct reason Sections 02 and 04 exist at all.
- **NIST CSF traceability:** every control column above links back to a specific CSF function, formalized in the [control mapping table](../03-nist-csf-implementation/README.md#control-mapping-summary).
- **Ongoing review:** per [govern.md](../03-nist-csf-implementation/govern.md), this register is reviewed quarterly and after any incident — a risk's score isn't fixed forever; R4's "Medium" residual risk, for example, is a candidate for further reduction via automated response (not just alerting) in a future iteration.

---
**Previous:** [← 04 — Least Privilege & Access Control](../04-least-privilege-and-access-control/README.md)
**Next:** [06 — Incident Response Plan →](../06-incident-response-plan/README.md)
