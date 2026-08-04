# 10. Incident Response Fundamentals

## 🇬🇧 English Documentation

### Overview
Incident Response (IR) is a structured, systematic methodology for identifying, containing, mitigating, and recovering from cybersecurity breaches and operational compromises. The primary goal of incident response is to minimize business disruption, preserve digital evidence for forensic analysis, limit financial and reputational damage, and harden infrastructure against recurrent exploitation vectors.

---

### 1. The Incident Response Lifecycle (PICERL Framework)

Modern security operations align incident management processes with standardized frameworks (such as NIST SP 800-61 and SANS IR):

1. **Preparation:** Establishing incident response policies, building playbooks, training the Incident Response Team (CIRT/CSIRT), provisioning forensic tooling, and securing out-of-band communication channels before an incident occurs.
2. **Identification, Detection & Triage:** Analyzing security alerts, SIEM telemetries, and user reports to verify a breach. Categorizing severity levels, establishing the scope of compromise (blast radius), and triaging affected assets.
3. **Containment:** Executing immediate measures to prevent the spread of an active compromise while preserving critical evidence.
   * *Short-Term Containment:* Isolating compromised hosts from the network (VLAN isolation, dynamic firewall drops) without powering them off.
   * *Long-Term Containment:* Applying temporary patches, disabling compromised user accounts, or blocking Command and Control (C2) domains/IPs.
4. **Eradication:** Locating and removing root causes, malicious artifacts, persistence mechanisms (crontabs, systemd services, web shells), and unauthorized accounts from the environment.
5. **Recovery:** Restoring affected systems to production state from verified clean backups, applying security patches, validating operational integrity, and implementing heightened monitoring controls.
6. **Post-Incident Review (Lessons Learned):** Conducting a retrospective meeting with stakeholders, producing a formal Incident Response Report, documenting timeline events, and updating playbooks to prevent reoccurrence.

---

### 2. Evidence Preservation & Forensics Integrity

During a security incident, maintaining the legal and operational integrity of digital evidence is critical for root-cause analysis and potential legal proceedings.

* **Order of Volatility (RFC 3227):** Evidence must be acquired starting from the most volatile memory components to the least volatile storage media:
  1. CPU registers & cache
  2. Main memory (RAM)
  3. Network state & active socket tables
  4. Running process state
  5. Disk storage (swap, file systems)
  6. Remote logging & archived backups
* **Volatile Memory Acquisition:** Capturing live RAM using kernel modules (e.g., LiME) before powering down or rebooting a host. Shutting down a host destroys volatile artifacts like running malware payloads, encryption keys, and active network connections.
* **Disk Imaging & Hash Verification:** Creating raw bit-stream copies (e.g., using `dd` or `dc3dd`) of physical storage media and computing cryptographic hashes (SHA-256) immediately after acquisition to verify evidence integrity.
* **Chain of Custody:** Documenting every individual who handled, transferred, or analyzed evidence, complete with timestamps, storage locations, and cryptographic checksums.

---

### 3. Communication Pathways & Escalation Strategies

Structured communication prevents information leakage, reduces chaos, and ensures legal/regulatory compliance during a crisis:

* **Out-of-Band (OOB) Communication:** Utilizing isolated, encrypted communication channels (e.g., dedicated Signal groups, out-of-band VoIP) that do not rely on potentially compromised enterprise infrastructure (email, corporate messaging).
* **Stakeholder Escalation:** Establishing defined escalation paths for C-suite executives, legal counsel, public relations (PR), regulatory bodies (e.g., GDPR notifications), and law enforcement.
* **Minimizing Operational Disclosure:** Restricting detailed incident disclosures strictly to necessary response personnel on a need-to-know basis.

---

### 🛠️ Hands-On Forensic & Incident Response Commands

* **Isolate Compromised Host via Network IPTables Rule (Allow Loopback Only):**  
  `sudo iptables -P INPUT DROP`  
  `sudo iptables -P OUTPUT DROP`  
  `sudo iptables -A INPUT -i lo -j ACCEPT`  
  `sudo iptables -A OUTPUT -o lo -j ACCEPT`

* **Dump Volatile Memory (RAM) using LiME Kernel Module:**  
  `sudo insmod lime-$(uname -r).ko "path=/tmp/memory_dump.lime format=raw"`

* **Create a Forensic Bit-Stream Disk Image with SHA-256 Hash Verification:**  
  `sudo dd if=/dev/sdb of=/mnt/forensics/disk_image.raw bs=64K status=progress`  
  `sha256sum /dev/sdb /mnt/forensics/disk_image.raw`

* **Inspect File Timestamps (MACB - Modified, Accessed, Changed, Birth):**  
  `stat /path/to/suspicious_binary`

* **Reconstruct File System Timeline using Autopsy / TSK (Sleuth Kit):**  
  `fls -r -m / /mnt/forensics/disk_image.raw > bodyfile`  
  `mactime -b bodyfile 2026-08-01..2026-08-04`

---

### 📊 Incident Response Lifecycle Summary Matrix

| IR Lifecycle Phase | Primary Objective | Key Deliverables / Artifacts | Operational Risk |
| :--- | :--- | :--- | :--- |
| **Identification & Triage** | Scope breach & assess blast radius | Compromise indicators (IoCs), Triage report | Misdiagnosing scope leads to incomplete isolation |
| **Containment** | Halt adversary movement & secure RAM | Network isolation rules, RAM dump (`.lime`) | Rebooting destroys volatile RAM evidence |
| **Eradication** | Remove root cause & persistence | Cleared backdoors, patched vulnerabilities | Incomplete eradication allows rapid re-entry |
| **Recovery** | Restore trusted operational state | Verified system backups, monitored hosts | Restoring compromised or unpatched backups |
| **Lessons Learned** | Update playbooks & prevent repeat | Formal Incident Report, root-cause analysis | Repeating procedural errors in future breaches |
