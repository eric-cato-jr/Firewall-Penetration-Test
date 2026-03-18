# 🔥 Firewall Penetration Test & Attack Surface Reduction

## 📌 Overview

This lab demonstrates a structured firewall penetration test followed by configuration hardening and validation.

Objectives:

- Identify exposed services
- Assess vulnerabilities
- Reduce firewall attack surface
- Validate remediation effectiveness

---

# 🖥️ Lab Environment

## Infrastructure

- pfSense Firewall
- Windows Server (172.30.0.15)
- Linux Target Host
- Kali Linux Attacker Machine

## Network Topology

The lab consists of an internal network protected by a pfSense firewall with public NAT exposure.

![Network Topology](networktop.png)

The Windows server was externally reachable through 1:1 NAT mapping.

---

# 1️⃣ Initial Attack Surface Review

Before scanning, firewall rules and NAT exposure were examined.

## WAN Rules (Pre-Hardening)

The firewall allowed inbound HTTP and HTTPS traffic to the internal server.

![WAN Rules Before](Wan-Rules-Before.png)

This configuration exposed internal services to the public network.

---

## NAT Configuration

A 1:1 NAT rule mapped the internal Windows host to a public IP address.

![NAT Mapping](nat-mapping.png)

This effectively made the internal server directly accessible externally.

---

## Virtual IP Alias

A WAN IP alias was configured to support the public mapping.

![Virtual IP](virtual-ip.png)

This confirmed the firewall was intentionally exposing internal infrastructure.

---

## Default LAN Rule

The firewall contained a “Default allow LAN to any” rule.

![Default Allow LAN](default-allow-lan.png)

This rule allowed unrestricted outbound access, increasing lateral movement risk if compromised.

⚠️ At this stage, the firewall had multiple exposure points increasing the attack surface.

---

# 2️⃣ Service Enumeration (Nmap)

Active reconnaissance was conducted using Nmap with:

- Service detection (-sV)
- OS detection (-O)
- Version scanning

## Nmap Scan Output

![Nmap Service & OS Detection](namp filtered.png)

The scan identified externally accessible services:

- FTP (21/tcp)
- SSH (22/tcp)
- HTTP (80/tcp)

This confirmed that multiple services were publicly reachable through the firewall.

---

# 3️⃣ Vulnerability Assessment (Nessus)

After confirming exposed services, a vulnerability assessment was performed.

## Nessus Detailed Findings

The scan identified multiple medium-severity findings across the hosts.

![Nessus Vulnerabilities](nessus-vulns.png)

Key findings included:

- Weak TLS configuration
- Self-signed certificates
- Outdated services
- Insecure SMB settings

These vulnerabilities were directly tied to exposed services and permissive firewall rules.

---

## Nessus Scan Summary

The summary view showed the total vulnerability count across scanned hosts.

![Nessus Summary](nessus-summary.png)

This provided a high-level overview confirming measurable security risk.

---

# 4️⃣ Firewall Hardening & Attack Surface Reduction

To reduce risk exposure, the following remediation steps were implemented:

- Removed unnecessary WAN rules
- Restricted inbound service exposure
- Reduced 1:1 NAT exposure
- Eliminated overly permissive LAN rule
- Applied least-privilege firewall policy

These changes significantly reduced publicly reachable services.

---

# 5️⃣ Post-Remediation Validation

After hardening, re-scanning was performed to verify improvements.

## Nmap Post-Hardening Scan

Previously open ports were now filtered or blocked.

![Nmap Filtered](nmap-filtered.png)

This confirmed external services were no longer accessible.

---

## Final Nessus Scan

A final vulnerability scan showed that previously identified findings were eliminated or significantly reduced.

![Final Nessus Scan](nessus-final.png)

The firewall modifications successfully reduced the attack surface.

---

# 📊 Before vs After

| Category | Before | After |
|----------|--------|--------|
| NAT Exposure | Enabled | Restricted |
| Open Services | Multiple exposed | Filtered |
| LAN Rule | Allow Any | Restricted |
| Vulnerabilities | Medium findings | Reduced / Removed |

---

# 🧠 Skills Demonstrated

- Network reconnaissance (Nmap)
- Vulnerability assessment (Nessus)
- Firewall configuration analysis (pfSense)
- NAT exposure review
- Security hardening implementation
- Remediation validation

---

# 🔐 Key Takeaways

- Firewall misconfiguration directly increases attack surface.
- NAT exposure must be tightly controlled.
- Vulnerability assessment validates configuration risk.
- Hardening must always be followed by re-testing.
- Security requires both offensive testing and defensive correction.

---

# 🚀 Conclusion

This lab demonstrates a complete firewall penetration testing and hardening lifecycle. Through reconnaissance, vulnerability identification, rule modification, and validation scanning, the firewall’s attack surface was successfully reduced.
