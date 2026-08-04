# 02. Threat Modeling & Attack Lifecycle

## 🇬🇧 English Documentation

### Overview
To effectively defend a system, security engineers must adopt an attacker mindset. Threat modeling frameworks help structure security analysis, identify entry points, and break the attack lifecycle before damage occurs.

---

### 1. STRIDE Threat Model
Developed by Microsoft, STRIDE categorizes threats based on the security property they violate:

| Category | Threat | Violated Principle | Example Scenario |
| :--- | :--- | :--- | :--- |
| **S** | **Spoofing** | Authentication | Stealing SSH keys to impersonate a legitimate system administrator. |
| **T** | **Tampering** | Integrity | Modifying system logs or changing a web page source HTML. |
| **R** | **Repudiation** | Accountability | Deleting audit trails to deny performing a malicious action. |
| **I** | **Information Disclosure** | Confidentiality | Exposing .env files containing plain-text database credentials. |
| **D** | **Denial of Service** | Availability | Exhausting server resources (CPU/RAM/Sockets) to crash a service. |
| **E** | **Elevation of Privilege** | Authorization | Exploiting a misconfigured SUID binary to gain root access. |

---

### 2. The Cyber Kill Chain
Developed by Lockheed Martin, this 7-phase model describes the sequence of steps involved in an advanced cyber attack:

[1. Reconnaissance] -> [2. Weaponization] -> [3. Delivery] -> [4. Exploitation] -> [5. Installation] -> [6. Command & Control] -> [7. Actions on Objectives]

1. **Reconnaissance:** Gathering intelligence on the target (nmap port scans, service enumeration).
2. **Weaponization:** Coupling an exploit with a malicious payload tailored to the vulnerability.
3. **Delivery:** Transmitting the payload to the target (Phishing, open exposed service).
4. **Exploitation:** Triggering the exploit code to take advantage of the system flaw.
5. **Installation:** Establishing persistence on the host (Installing backdoors, creating unauthorized users).
6. **Command & Control (C2):** Opening a covert communication channel back to the attacker server.
7. **Actions on Objectives:** Fulfilling the primary goal (Data exfiltration, ransomware encryption).

> **Blue Team Strategy:** Breaking any single link in the chain completely neutralizes the attack.

---

### 3. MITRE ATT&CK Framework
A globally accessible knowledge base of adversary tactics, techniques, and procedures (TTPs) based on real-world observations.
* **Tactics (The 'Why'):** The tactical goal of the attacker (e.g., Privilege Escalation).
* **Techniques (The 'How'):** How an attacker achieves a tactic (e.g., Exploiting SUID/SGID Bits).

---
