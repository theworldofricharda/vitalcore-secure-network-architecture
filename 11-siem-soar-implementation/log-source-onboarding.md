# Log Source Onboarding

## In plain terms

A SIEM is only as good as what feeds it. This file lists exactly which systems send logs to Splunk, in what format, and — importantly — what question each source is there to help answer. Onboarding a log source isn't just "point it at the SIEM and forget it"; each one needs a defined parsing method and a reason it's there, or it just becomes noise nobody looks at.

## Onboarded sources

| Source | Method | Format | Primary detection purpose |
|---|---|---|---|
| Perimeter/internal firewalls ([Section 02](../02-network-architecture-and-segmentation/README.md), [08](../08-network-protocols-and-segmentation/README.md)) | Syslog over TLS | CEF | Denied cross-VLAN attempts, ACL violations (feeds [detect.md](../03-nist-csf-implementation/detect.md)) |
| Active Directory Domain Controllers | Splunk Universal Forwarder (Windows Event Log) | XML/EVTX | Failed logons, privilege escalation, Tier 0/1/2 admin misuse ([Section 04](../04-least-privilege-and-access-control/README.md)) |
| EDR agents (Corporate, Servers, R&D endpoints) | Vendor API/HEC (HTTP Event Collector) | JSON | Malware execution, suspicious process behavior |
| MyVitalCore Portal application & WAF | HEC | JSON | Authentication anomalies, WAF blocks, API abuse against the 100K+ user base |
| Partner API Gateway | HEC | JSON, tagged by OAuth2 scope | Out-of-scope access attempts, partner credential misuse |
| Manufacturing OT/SCADA historian | Syslog (via the jump host only — no direct internet-facing forwarder, consistent with [Section 02](../02-network-architecture-and-segmentation/README.md)) | Syslog | Unauthorized configuration changes, unrecognized device connections |
| Clinical Trials application & database audit logs | HEC | JSON | Every read/write to PHI, per HIPAA audit control requirements ([Section 07](../07-hipaa-compliance-framework/README.md)) |
| VPN concentrator (Remote Access) | Syslog | Syslog | Impossible-travel logons, MFA bypass attempts |
| DNS resolvers | Syslog | Syslog | Beaconing/C2 domain lookups, DNS tunneling indicators |

## Onboarding checklist (applied to every new source)

1. **Confirm transport is encrypted** (TLS syslog or HTTPS HEC) — an unencrypted log feed defeats the purpose of protecting the data it describes.
2. **Confirm time sync** — every source must use the authenticated NTP service from [Section 08](../08-network-protocols-and-segmentation/README.md); mismatched clocks break correlation across sources during an investigation.
3. **Map fields to the Common Information Model (CIM)** — so a correlation search written once (e.g., "failed authentication") works across AD, VPN, and the portal without three separate rules.
4. **Assign a data classification and retention tier** — Clinical/PHI-touching sources go into the 6-year retention tier described in the [main README](README.md#sizing-for-100000-users--50-staff); most others use the standard 90-day/1-year tiers.
5. **Validate with a test alert** before considering the source "live" — confirming the SOC actually receives and can act on data from it, not just that data is arriving.

---
**Back to:** [11 — SIEM & SOAR Implementation](README.md)
