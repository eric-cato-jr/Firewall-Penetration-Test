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

## Default LAN Rule Removed

![Default Allow LAN](default-allow-lan.png)

The overly permissive **“Default allow LAN to any”** rule was deleted to prevent unrestricted outbound traffic and reduce lateral movement risk.

Additional remediation steps included:

- Removing unnecessary WAN exposure rules  
- Deleting port 80 (HTTP) access  
- Removing Windows Server public exposure rule  
- Restricting inbound services to only what was required  

These changes significantly reduced the externally reachable attack surface.

---

# 5️⃣ Post-Remediation Validation

After implementing firewall hardening, re-scanning was conducted to verify effectiveness.

## Greenbone Validation Scan

An independent vulnerability scan was performed using Greenbone to validate remediation.

![Greenbone Scan](greenbone-scan.png)

The results showed a significant reduction in previously identified vulnerabilities after firewall rule modifications.

---

## Final Nessus Scan Summary

A final Nessus scan was conducted to confirm remediation success.

![Final Nessus Scan](nessus-final.png)

The results showed that previously identified vulnerabilities were eliminated or substantially reduced, confirming that firewall hardening was effective.

---

# 🚀 Conclusion

This lab demonstrates a complete firewall penetration testing and hardening lifecycle. Through reconnaissance, vulnerability identification, firewall rule modification, and validation scanning, the overall attack surface was successfully reduced.

The exercise reinforces the importance of reviewing firewall exposure, enforcing least-privilege configurations, and validating remediation using multiple assessment tools to ensure measurable security improvement.
