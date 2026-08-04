# 08. System Auditing, Logging & SIEM

## 🇬🇧 English Documentation

### Overview
System auditing and logging form the backbone of security operations, threat detection, and post-incident digital forensics. Without centralized, tamper-resistant logging and real-time event correlation, security teams cannot establish visibility over host-level anomalies, lateral movement, or unauthorized privilege escalation. This module covers Linux system logging (`/var/log`, `journalctl`), kernel auditing via `auditd`, log integrity mechanisms, centralized log forwarding, and SIEM integration.

---

### 1. Linux Logging Architecture: `/var/log` & Systemd Journal

Linux operating systems utilize a dual logging paradigm consisting of traditional flat-file logs and binary systemd journal structures.

#### Traditional Log Files (`/var/log/`)
* `/var/log/auth.log` (Debian/Ubuntu) or `/var/log/secure` (RHEL/CentOS): Tracks authentication attempts, SSH logins, `sudo` usage, and PAM activities.
* `/var/log/syslog` or `/var/log/messages`: Stores general system events, daemon activity, and non-authentication kernel messages.
* `/var/log/audit/audit.log`: Stores structured event logs generated directly by the Linux kernel audit framework.

#### Systemd Journal (`journalctl`)
`systemd-journald` collects volatile or persistent binary logs from the kernel, initrd, standard output/error of services, and syslog events.
* **Indexed Metadata:** Log messages are enriched with process IDs (PID), user IDs (UID), systemd unit names, and microsecond timestamps.
* **Tamper Vulnerability:** While binary formatting prevents simple text editing, journal logs must be persisted to non-volatile disk (`Storage=persistent` in `/etc/systemd/journald.conf`) and forwarded to remote collectors to prevent destruction during an intrusion.

---

### 2. Kernel-Level Auditing with `auditd`

The Linux Audit Subsystem (`auditd`) operates directly at the kernel level to record detailed security-relevant events without relying on user-space applications.

* **Audit Hooks:** Intercepts system calls (syscalls), file access, process execution, and network socket operations.
* **Rule Definitions (`/etc/audit/rules.d/audit.rules`):**
  * **File System Rules (Watches):** Monitors specific files or directories for read (`r`), write (`w`), execute (`x`), or attribute change (`a`) access.
  * **System Call Rules:** Monitors specific kernel syscalls (e.g., `execve`, `chmod`, `ptrace`) filtered by UID, GID, or process context.
* **Forensic Utility:** `auditd` captures exact execution paths, command-line arguments, parent process IDs (PPID), and true user identities even if privilege escalation occurred.

---

### 3. Log Integrity, Centralized Logging & SIEM Integration

Local log files on a compromised host cannot be trusted. Adversaries routinely wipe or tamper with local logs (`/var/log/*`) to hide their operational footprint.

#### Log Integrity & Remote Forwarding
* **Secure Transport:** Local log daemons (`rsyslog`, `syslog-ng`, `vector`, `fluentbit`) must forward logs in real-time over encrypted channels (TLS) to a remote log collector.
* **Append-Only Attributes:** Critical local log files can be protected using file system attributes (`chattr +a /var/log/custom.log`), allowing appends while blocking deletion or modification.

#### SIEM & Detection Engineering
* **SIEM Platforms:** Solutions like Wazuh, Elastic Security (ELK), Splunk, and Microsoft Sentinel ingest, parse, and index log telemetry from across the environment.
* **Detection Rules:** Normalizing log events into standard schemas (Elastic Common Schema / ECS) enables automated detection rules (e.g., Sigma rules) for identifying brute-force attacks, persistence creation, and privilege escalation patterns.

---

### 🛠️ Hands-On Execution & Inspection Commands

* **Filter SSH Authentication Failures in Real-Time via `journalctl`:**  
  `sudo journalctl -u ssh -f -p err`

* **Set a Kernel Audit Watch Rule on the `/etc/shadow` Password File:**  
  `sudo auditctl -w /etc/shadow -p wa -k shadow_tampering`

* **Search `auditd` Logs for Specific Keyed Events:**  
  `sudo ausearch -k shadow_tampering --format raw`

* **Generate an Executive Audit Report of Failed System Calls:**  
  `sudo aureport --syscall --failed`

* **Configure Rsyslog Remote Forwarding over TLS (`/etc/rsyslog.d/50-default.conf`):**  
  `*.* @@siem.company.internal:6514`

---

### 📊 Telemetry & Auditing Summary Matrix

| Log Subsystem | Log Storage Path | Primary Use Case | Security Value |
| :--- | :--- | :--- | :--- |
| **Auth Log** | `/var/log/auth.log` | SSH & Sudo tracking | Detects brute-force & unauthorized privilege escalation |
| **Systemd Journal** | `/run/log/journal/` | Service & daemon health | Real-time structured process stdout/stderr collection |
| **Auditd** | `/var/log/audit/audit.log` | Syscall & file watch | Forensics-grade kernel execution & file modification tracking |
| **SIEM / Remote Log** | Remote TCP/6514 (TLS) | Correlation & Triage | Provides tamper-proof immutable log storage & detection |
