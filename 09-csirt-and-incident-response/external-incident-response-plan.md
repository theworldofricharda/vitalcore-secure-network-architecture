# External Incident Response Plan

## In plain terms

This plan governs incidents that reach — or might have reached — anything outside VitalCore's own walls: the 100,000+ MyVitalCore Portal users, a partner physician or pharmacy system, or public-facing infrastructure. These incidents carry everything the internal plan does, **plus** legal notification obligations, public communication, and reputational stakes that an internal-only incident doesn't have. Speed still matters, but so does getting the *disclosure* right — under-disclosing risks a HIPAA violation; over-disclosing before facts are confirmed causes needless panic and reputational harm.

## When this plan applies

- The incident involves, or might involve, the Public DMZ, Partner Extranet, or Remote Access zone ([Section 08](../08-network-protocols-and-segmentation/README.md)).
- Any data belonging to a portal user, patient, trial participant, or partner may have been accessed, exposed, or affected.
- The incident is classified **SEV-1 or SEV-2** (see [CSIRT charter](README.md#severity-classification)).

## Process

### 1. Detect & Triage (target: within 10 minutes)
Alerts specific to external-facing systems — WAF blocks, abnormal API Gateway traffic, portal availability drops — are prioritized at the top of the SOC queue given the scale of potential impact (100,000+ users).

### 2. Declare & Activate Full CSIRT (target: within 15 minutes)
Unlike internal incidents, an external declaration activates the **full CSIRT roster** immediately, including the Compliance & Legal Liaison and Communications Coordinator — these roles are needed from minute one, not added later, because notification-clock decisions depend on facts gathered from the very start of the investigation.

### 3. Contain (target: within 30 minutes)
The External Response Coordinator leads containment specific to the affected zone:
- **Public DMZ:** isolate the affected portal component behind the WAF; if necessary, take the affected feature (not the whole portal) offline.
- **Partner Extranet:** revoke the specific partner's API Gateway credentials/OAuth scope rather than shutting down the gateway for all partners.
- **Remote Access:** disable the specific compromised VPN account; because Remote Access users are still bound by internal RBAC ([Section 08](../08-network-protocols-and-segmentation/README.md)), scope is usually limited to what that one account could reach.

### 4. Scope Determination — the critical decision point
The Forensics & Detection Analyst, working with the Compliance & Legal Liaison, must answer one specific question as fast as possible: **did this incident expose PHI or personal data belonging to identifiable individuals?** This single determination drives everything that follows:
- If **yes**, the HIPAA Breach Notification Rule clock starts (see [Section 07](../07-hipaa-compliance-framework/README.md#how-the-breach-notification-rule-connects-to-incident-response)) — notification without unreasonable delay, no later than 60 days, and (for 500+ individuals) concurrent notification to HHS and, in some cases, media.
- If **no** (e.g., a contained WAF-blocked attack with no data access confirmed), the incident is documented but does not trigger breach notification — though it may still warrant a transparency post on the status page if it caused visible downtime.

### 5. External Communication
Handled exclusively by the Communications Coordinator, following a strict internal-approval-before-external-release rule (Legal + CSIRT Lead sign off on any external statement before it goes out):

| Audience | Channel | Timing |
|---|---|---|
| All portal users (if service-impacting) | Status page banner | As soon as impact is confirmed, updated hourly |
| Affected individuals (if PHI exposed) | Direct written notification (mail/email per HIPAA requirements) | Within the regulatory window, per [Section 07](../07-hipaa-compliance-framework/README.md) |
| HHS Office for Civil Rights | Regulatory filing | Per HIPAA Breach Notification Rule timeline |
| Partner organizations (if their integration was the entry point or was affected) | Direct outreach from the External Response Coordinator | Within 24 hours of confirmed involvement |
| Media (only for large-scale confirmed breaches) | Prepared statement, Legal-approved | Only if legally required or reputationally necessary |

### 6. Eradicate & Recover
Same technical process as the [Internal plan](internal-incident-response-plan.md) and [recover.md](../03-nist-csf-implementation/recover.md), with one addition: before any externally-facing system is reconnected, the External Response Coordinator confirms with the Communications Coordinator that any promised "fixed by" time in the status page will actually be met — external commitments, once made publicly, are treated as real deadlines.

### 7. Post-Incident Review & Regulatory Follow-Up
In addition to the standard review (per [recover.md](../03-nist-csf-implementation/recover.md)), the Compliance & Legal Liaison confirms all notification obligations were fully met and documents the determination made in Step 4 — this record is what VitalCore would produce if HHS ever audited the response.

## Why the internal/external split matters at this scale

Before the portal reached 100,000+ users, an informal "figure it out together" approach was workable. At this scale, a single external incident can mean tens of thousands of individuals with a legal right to notification, on a strict regulatory clock — decisions can't wait for the right person to happen to be in the room. Pre-defining exactly who does what, and exactly which fact (PHI exposure: yes/no) triggers which obligation, is what makes a fast, legally sound response possible under real time pressure.

---
**Back to:** [09 — CSIRT & Incident Response](README.md)
