# 09. Log-Based Abuse Mitigation

## 🇬🇧 English Documentation

### Overview
Log-based abuse mitigation is a reactive security paradigm designed to detect and block malicious automated traffic (such as SSH brute-force attacks, credential stuffing, and directory enumeration) by continuously parsing system and service logs. By identifying repeated authentication failures or anomalous request patterns, log-monitoring daemons dynamically update firewall rules to temporarily or permanently quarantine offending IP addresses.

---

### 1. Fail2Ban Architecture & Behavioral Mechanics

Fail2Ban is the standard open-source framework for log-based intrusion prevention on Linux systems. It operates using three core conceptual components:

* **Filters:** Regular expression (regex) pattern definitions stored in `/etc/fail2ban/filter.d/`. Filters parse incoming log streams to match failed authentication attempts (e.g., "Failed password for invalid user").
* **Actions:** Scripted response triggers stored in `/etc/fail2ban/action.d/`. Once a filter threshold is reached, actions invoke local security utilities (such as `iptables`, `nftables`, `ufw`, or `tcpwrappers`) to execute dynamic blocking rules or dispatch alert notifications.
* **Jails:** Operational units defined in `/etc/fail2ban/jail.local` that bind a specific log file (e.g., `/var/log/auth.log`), a corresponding filter, and one or more actions together under specific threshold parameters.

#### Key Operational Parameters:
* `findtime`: The rolling time window (e.g., 600s) during which failed attempts are counted.
* `maxretry`: The maximum number of failed attempts allowed within the `findtime` window before triggering an action.
* `bantime`: The duration (e.g., 3600s or `-1` for permanent) for which the offending IP remains blocked.

---

### 2. Authentication-Log Analysis & Dynamic Blocking Flow

Log-based mitigation relies on real-time log stream ingestion to alter active network state:

1. **Log Generation:** The target daemon (e.g., `sshd`, Nginx) writes authentication failure entries to `/var/log/auth.log` or the systemd journal.
2. **Pattern Matching:** Fail2Ban's worker thread reads new log lines, matching them against compiled regex rules.
3. **Threshold Breached:** An offending IP exceeds `maxretry` within `findtime`.
4. **Firewall Rule Insertion:** Fail2Ban executes an `action.d` command, inserting a dynamic drop rule into the active firewall chain (e.g., `f2b-sshd` chain in `iptables` or `nftables` sets).
5. **Automated Expiration:** Once `bantime` expires, an automated cleanup task removes the firewall rule to restore access.

---

### 3. Operational Limitations & Risks of Log-Based Controls

While log-based abuse mitigation provides an effective layer of defense against low-rate noise attacks, it suffers from intrinsic architectural limitations:

* **Reactive Delay (Window of Exposure):** Block rules are created *after* bad attempts occur. An attacker sending thousands of requests concurrently (e.g., multi-threaded distributed attacks) can succeed before log parsing triggers a block.
* **Log Tampering & Evasion:** Adversaries may format payload inputs to evade regex patterns or execute Log Injection attacks to insert false entries, potentially tricking Fail2Ban into banning legitimate administrative IPs.
* **Resource Exhaustion & Disk I/O Overhead:** High-volume attacks can saturate disk I/O and CPU utilization as the log-parsing daemon continuously processes heavy log streams.
* **Distributed Attacks (DDoS / Botnets):** Fail2Ban tracks single IP addresses. Distributed botnets where each node sends only 1 or 2 attempts completely bypass single-IP threshold detection.
* **State Table Bloat:** Inserting tens of thousands of individual IP drop rules into legacy firewall tables like `iptables` degrades packet processing performance.

---

### 🛠️ Hands-On Execution & Configuration Commands

* **Inspect Overall Fail2Ban Service Status and Active Jails:**  
  `sudo fail2ban-client status`

* **Check Detailed Status and Banned IPs for the SSH Jail:**  
  `sudo fail2ban-client status sshd`

* **Manually Unban an IP Address from a Specific Jail:**  
  `sudo fail2ban-client set sshd unbanip 192.168.1.50`

* **Manually Ban a Malicious IP Address across a Jail:**  
  `sudo fail2ban-client set sshd banip 203.0.113.10`

* **Test a Custom Regex Filter Against an Auth Log File:**  
  `fail2ban-regex /var/log/auth.log /etc/fail2ban/filter.d/sshd.conf`

---

### 📊 Abuse Mitigation & Control Comparison Matrix

| Control Aspect | Reactive Log Parsing (Fail2Ban) | Proactive Rate Limiting (Netfilter/iptables) |
| :--- | :--- | :--- |
| **Inspection Layer** | Application / User-Space Logs | Kernel-Space Network Packets |
| **Latency** | High (Delayed until log write & parse) | Extremely Low (Instant packet-level action) |
| **Evasion Vulnerability**| Regex evasion, Log Injection, Botnets | Connection state spoofing |
| **Performance Impact** | Higher Disk I/O & CPU overhead | Negligible CPU impact, fast packet drops |
