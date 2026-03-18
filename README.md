# 🔥 Firewall Penetration Test & Attack Surface Reduction

## 📌 Overview

This lab demonstrates a structured firewall penetration test followed by configuration hardening and remediation validation.

The objective was to:

- Identify externally exposed services
- Assess vulnerabilities
- Reduce firewall attack surface
- Validate security improvements through re-scanning

---

# 🖥️ Lab Environment

## Infrastructure

- pfSense Firewall
- Windows Server (172.30.0.15)
- Linux Target Host
- Kali Linux Attacker Machine

## Network Topology

The environment consists of segmented internal hosts protected by a pfSense firewall with controlled external exposure.

![Network Topology](networktop.png)

The Windows server was publicly reachable through firewall configuration.

---

# 1️⃣ Initial Firewall Exposure Review

Before scanning, firewall rules and public mappings were reviewed.

## WAN Rules (Pre-Hardening)

Inbound HTTP/HTTPS traffic was permitted to the internal server.

![WAN Rules Before](Wan-Rules-Before.png)

This allowed direct exposure of internal services to the external network.

---

## Virtual IP Configuration

A WAN IP alias was configured to map the internal Windows server to a public-facing IP.

![Virtual IP](virtual-ip.png)

This confirmed the host was externally accessible.

⚠️ At this stage, the firewall configuration expanded the attack surface.

---

# 2️⃣ Service Enumeration (Nmap)

Active reconnaissance was conducted using Nmap with:

- Service detection (-sV)
- OS detection (-O)
- Version enumeration

## Nmap Service & OS Detection Output

![Nmap Enumeration](namp-filtered.png)

The scan identified publicly accessible services including:

- FTP (21/tcp)
- SSH (22/tcp)
- HTTP (80/tcp)

This confirmed multiple services were externally exposed through firewall rules.

---

# 3️⃣ Vulnerability Assessment (Nessus)

After confirming exposed services, a vulnerability scan was performed.

## Nessus Scan Summary

![Nessus Summary](nessus-summary.png)

The summary view displayed the total number of vulnerabilities detected across the scanned hosts, indicating measurable security risk.

---

## Nessus Detailed Findings

![Nessus Vulnerabilities](nessus-vulns.png)

Key findings included:

- Weak TLS configuration
- Self-signed certificates
- Outdated service versions
- Insecure protocol configurations

These vulnerabilities were directly associated with exposed services and permissive firewall rules.

---

# 4️⃣ Firewall Hardening & Attack Surface Reduction

To mitigate identified risks, firewall configurations were modified to follow a least-privilege model.

Remediation actions included:

- Removing unnecessary WAN exposure rules
- Deleting port 80 (HTTP) access
- Removing Windows Server exposure rule
- Deleting the “Default allow LAN to any” rule
- Restricting inbound service access

## Default LAN Rule Removed

![Default Allow LAN](default-allow-lan.png)

By eliminating overly permissive rules, external and lateral attack vectors were significantly reduced.

---

# 5️⃣ Post-Remediation Validation

After implementing firewall hardening, re-scanning was conducted to verify effectiveness.

## Greenbone Validation Scan

An additional vulnerability scan was performed using Greenbone to independently validate remediation.

![Greenbone Scan](greenbone-scan.png)

The results confirmed a significant reduction in previously identified vulnerabilities following firewall modifications.

---

## Final Nessus Scan Summary

A final Nessus scan was conducted to confirm remediation success.

![Final Nessus Scan](nessus-final.png)

The results showed that previously identified vulnerabilities were eliminated or substantially reduced, confirming the effectiveness of firewall hardening.

---

# 🚀 Conclusion

This lab demonstrates a complete firewall penetration testing and hardening lifecycle. Through structured reconnaissance, vulnerability assessment, configuration review, rule modification, and validation scanning, the firewall’s attack surface was successfully reduced.

The exercise highlights the importance of reviewing firewall exposure, applying least-privilege configurations, and validating remediation through multiple scanning tools to ensure measurable security improvement.
