# Sample Firewall Ruleset (pfSense-style syntax)

## In plain terms

The table in [inter-vlan-acl-matrix.md](inter-vlan-acl-matrix.md) describes *what* is allowed between zones. This file shows *how* that policy would actually be written as firewall rules, using pfSense-style syntax (a widely-used open-source firewall platform) as the example. Rules are processed top-to-bottom, and the **last rule is always "deny everything else"** — this is what makes the network default-deny instead of default-allow.

## Rule order matters

Firewalls read rules from top to bottom and stop at the first match — like a bouncer checking a guest list in order. That's why the most specific "allow" rules come first, and the broadest "deny all" rule always comes last, as a safety net.

## Sample rules — Clinical Trials VLAN (VLAN 40)

```
# Interface: VLAN40_CLINICAL
# Purpose: Isolate PHI systems — HIPAA scope

# Rule 1 — Allow the Clinical Trials app server to reach the database server only
pass in on VLAN40_CLINICAL proto tcp from 10.10.40.10 to 10.10.40.20 port 5432 \
     label "Clinical app -> Clinical DB (Postgres)"

# Rule 2 — Allow logging traffic out to the central SIEM in the Management VLAN
pass out on VLAN40_CLINICAL proto tcp from 10.10.40.0/24 to 10.10.70.50 port 514 \
     label "Clinical VLAN -> SIEM syslog"

# Rule 3 — Allow patch management traffic from the hardened jump host only
pass in on VLAN40_CLINICAL proto tcp from 10.10.70.5 to 10.10.40.0/24 port 443 \
     label "Jump host -> Clinical patch management"

# Rule 4 (final, catch-all) — Deny everything else, log the attempt
block in log on VLAN40_CLINICAL from any to any \
     label "DEFAULT DENY — Clinical VLAN"
```

## Sample rules — Manufacturing OT VLAN (VLAN 50)

```
# Interface: VLAN50_MANUFACTURING
# Purpose: Protect industrial control systems (PLCs/SCADA/HMIs) — GxP scope

# Rule 1 — Allow PLCs to talk to the local SCADA server only (control traffic)
pass in on VLAN50_MANUFACTURING proto tcp from 10.10.50.0/24 to 10.10.50.5 port 502 \
     label "PLC -> SCADA server (Modbus)"

# Rule 2 — Allow the hardened jump host to reach the patch server in Servers VLAN
pass out on VLAN50_MANUFACTURING proto tcp from 10.10.70.5 to 10.10.20.30 port 443 \
     label "Jump host -> Patch server"

# Rule 3 — Explicitly block any direct-to-internet traffic (belt-and-suspenders,
# even though this VLAN has no default gateway to the internet at all)
block out log on VLAN50_MANUFACTURING from 10.10.50.0/24 to !10.10.0.0/16 \
     label "Block Manufacturing -> Internet"

# Rule 4 (final, catch-all)
block in log on VLAN50_MANUFACTURING from any to any \
     label "DEFAULT DENY — Manufacturing VLAN"
```

## Sample rules — Guest Wi-Fi VLAN (VLAN 60)

```
# Interface: VLAN60_GUEST
# Purpose: Internet-only access for visitor devices

# Rule 1 — Allow DNS and web browsing out to the internet only
pass out on VLAN60_GUEST proto { tcp, udp } from 10.10.60.0/24 to any port { 53, 80, 443 } \
     label "Guest -> Internet (web/DNS only)"

# Rule 2 — Explicitly block all access to internal RFC1918 ranges
block out log on VLAN60_GUEST from 10.10.60.0/24 to 10.0.0.0/8 \
     label "Block Guest -> Internal networks"

# Rule 3 (final, catch-all)
block in log on VLAN60_GUEST from any to any \
     label "DEFAULT DENY — Guest VLAN"
```

## Why the "log" keyword matters

Every deny rule above includes `log` — meaning every blocked attempt is recorded. This isn't just for troubleshooting: a spike in denied connection attempts *from* Corporate *to* Manufacturing, for example, is one of the clearest early signs of malware trying to spread internally, and feeds directly into the **Detect** function of the NIST CSF (see [detect.md](../../03-nist-csf-implementation/detect.md)).

---
**Back to:** [02 — Network Architecture & Segmentation](../README.md)
