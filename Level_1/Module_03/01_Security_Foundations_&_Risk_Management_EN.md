# 01. Security Foundations & Risk Management

## 🇬🇧 English Documentation

### Overview
Security Engineering begins with fundamental principles that govern how systems are architected, defended, and audited. Every security control, firewall rule, or access permission is designed to uphold these core concepts.

---

### 1. The CIA Triad
The core pillar of Information Security consists of three key goals:
* **Confidentiality:** Ensuring data is accessible only to authorized entities (e.g., SSH key authentication, disk encryption).
* **Integrity:** Guaranteeing that data has not been altered or tampered with in transit or at rest (e.g., SHA-256 checksums, digital signatures).
* **Availability:** Ensuring systems and data remain operational and accessible when needed (e.g., Redundancy, DDoS mitigation, web server uptime).

---

### 2. The AAA Model
Every user or process interacting with a system undergoes three distinct phases:
1. **Authentication (Who are you?):** Verifying identity using credentials or SSH keys.
2. **Authorization (What can you do?):** Granting or denying access based on permissions (sudo, file modes like 644).
3. **Accountability (What did you do?):** Tracking actions via audit logs (/var/log/auth.log).

---

### 3. Fundamental Risk Equation
Risk management evaluates potential loss using the following formula:

Risk = Threat * Vulnerability * Asset Value

* **Asset:** The item being protected (e.g., Customer Database, Web Server).
* **Vulnerability:** A flaw or weak point in the system (e.g., Unpatched OS, overly permissive file permissions).
* **Threat:** Any potential danger that could exploit a vulnerability (e.g., Attackers, Malware).
* **Attack Surface:** The sum of all possible entry points an attacker could attempt to exploit.

---

### 4. Defense in Depth
Security must never rely on a single defensive line. A layered defense approach ("The Onion Model") ensures that if one layer fails, subsequent layers halt the attacker:
1. **Perimeter / Cloud Layer:** AWS Security Groups & Network ACLs.
2. **Host Network Layer:** Local Firewalls (iptables / ufw).
3. **System Layer:** OS Hardening, SSH configuration (sshd_config).
4. **Application Layer:** Web server configs, least-privilege service accounts.
5. **Data Layer:** File permissions, encryption at rest and in transit.

---
---
