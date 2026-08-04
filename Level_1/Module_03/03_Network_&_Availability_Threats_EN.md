# 03. Network & Availability Threats

## 🇬🇧 English Documentation

### Overview
Network-level threats primarily aim to disrupt service availability (Denial of Service) or intercept unencrypted network traffic (Man-in-the-Middle). Understanding protocol mechanisms like TCP handshakes is vital for server defense.

---

### 1. Availability Threats (DoS, DDoS, SYN Flood)

* **DoS (Denial of Service):** Originating from a single IP source. Can be easily mitigated via local firewall rules (iptables).
* **DDoS (Distributed Denial of Service):** Originating from thousands of distributed botnet nodes simultaneously. Requires perimeter protection (AWS Shield, Cloudflare).

#### 🚨 Technical Deep Dive: SYN Flood Attack
Standard TCP communication relies on the 3-Way Handshake:

[Client]  --- (1) SYN --->  [Server]  (Initiate Connection)
[Client]  <-- (2) SYN-ACK - [Server]  (Acknowledge & Reserve Resource)
[Client]  --- (3) ACK --->  [Server]  (Connection Established)

* **The Exploit Mechanism:** An attacker sends thousands of spoofed SYN requests. The server responds with SYN-ACK and keeps the socket in a HALF-OPEN state awaiting the final ACK. The attacker never sends the final ACK, exhausting the server's connection backlog table.
* **Linux Kernel Mitigation:** Enabling TCP SYN Cookies via kernel parameter:
  sysctl -w net.ipv4.tcp_syncookies=1

---

### 2. Man-in-the-Middle (MITM) & Spoofing Attacks

* **ARP Poisoning / Spoofing:** Occurs on the local network (LAN). The attacker broadcasts malicious ARP responses linking their MAC address with the default gateway's IP address, intercepting local traffic.
* **DNS Poisoning:** Injecting false DNS entries into a resolver, redirecting users from legitimate domains to malicious server IPs.
* **Session Hijacking:** Stealing a user's active session token/cookie to impersonate an authenticated session without credentials.

---

### 3. Mitigation Matrix

| Attack Vector | Target Principle | Defensive Measure |
| :--- | :--- | :--- |
| **SYN Flood** | Availability | tcp_syncookies = 1, Rate limiting via iptables. |
| **ARP Spoofing** | Integrity / Confidentiality | Dynamic ARP Inspection (DAI), Static ARP tables. |
| **DNS Spoofing** | Integrity | DNSSEC implementation, forcing HTTPS/TLS. |
| **Session Hijacking**| Confidentiality | Flagging cookies as Secure & HttpOnly. |

---
