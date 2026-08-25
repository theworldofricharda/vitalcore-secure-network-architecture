# Internal Incident Response Plan

## In plain terms

This plan governs incidents that start and stay inside VitalCore's own walls — a phishing email, a compromised employee laptop, an insider misusing access. Because no external portal user, patient, or partner is affected, the priorities are speed and minimal disruption to the business, without the added regulatory-notification machinery that an external incident triggers. This plan builds directly on the worked ransomware scenario in [Section 06](../06-incident-response-plan/README.md), formalizing it as a repeatable process now that a full CSIRT exists.

## When this plan applies

- The incident is confined to internal VLANs (10, 20, 30, 50, 70) or internal-only systems.
- No data or system reachable by external portal users, patients, or partners is involved.
- Initial classification is **SEV-3 or SEV-4** (see [CSIRT charter](README.md#severity-classification)) — if scope investigation later reveals external exposure, the incident is immediately escalated to the [External Incident Response Plan](external-incident-response-plan.md).

## Process

### 1. Detect & Triage (target: within 10 minutes of alert)
The SOC (see [Section 11](../11-siem-soar-implementation/README.md)) receives an alert — e.g., the denied cross-VLAN connection spike described in [detect.md](../03-nist-csf-implementation/detect.md) — and the on-call analyst performs initial triage to confirm it's a real incident and estimate scope.

### 2. Declare & Assign (target: within 15 minutes)
The CSIRT Lead declares the incident and assigns the **Internal Response Coordinator** as the lead for hands-on containment. For SEV-4 (policy violations, no active compromise), a single analyst may resolve it without a full CSIRT activation.

### 3. Contain (target: within 30 minutes)
Following [respond.md](../03-nist-csf-implementation/respond.md)'s priority order: isolate the affected host/VLAN first, investigate second. Because internal segmentation ([Section 02](../02-network-architecture-and-segmentation/README.md)) already limits how far an internal-only incident can spread, containment is typically a single switch-port or VLAN-level action, not a company-wide shutdown.

### 4. Investigate & Eradicate
Using SIEM data (see [Section 11](../11-siem-soar-implementation/README.md)), the Forensics & Detection Analyst confirms the full scope, removes the malicious presence, and confirms no lateral movement occurred beyond the initial point of compromise.

### 5. Internal Communication
Communication stays entirely internal:
- **Immediate:** CSIRT Lead → affected department manager + IT Security leadership.
- **If SEV-2 escalation risk exists:** CSIRT Lead → executive leadership (informational, not yet a public matter).
- **No external communication is issued** — internal incidents, by definition, have not reached the point of affecting portal users, patients, or partners.

### 6. Recover
Per [recover.md](../03-nist-csf-implementation/recover.md): restore from clean backups, validate, and reconnect in stages.

### 7. Post-Incident Review
Every internal incident — even SEV-4 — gets a brief written review within 3 business days: what happened, what worked, what should change. Findings update the [Risk Register](../05-risk-assessment/risk-register.md).

## Escalation trigger — when an "internal" incident becomes "external"

If, at any point during investigation, evidence shows the incident reached the Public DMZ, Partner Extranet, Remote Access zone ([Section 08](../08-network-protocols-and-segmentation/README.md)), or any system holding data belonging to portal users or partners, the CSIRT Lead **immediately reclassifies the incident to at least SEV-2** and activates the [External Incident Response Plan](external-incident-response-plan.md) — the two plans run in parallel from that point rather than one replacing the other, since the internal cleanup work still needs to finish.

---
**Back to:** [09 — CSIRT & Incident Response](README.md)
