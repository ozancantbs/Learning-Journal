# 05. Firewalling & Network Defense

## 🇬🇧 English Documentation

### Overview
Network defense at the host and perimeter level relies on filtering incoming, outgoing, and routed traffic based on pre-defined security policies. Linux leverages the kernel-space **Netfilter** framework to execute packet filtering, Network Address Translation (NAT), and packet mangling, providing granular control over network communications.

---

### 1. Filtering Paradigms & Connection Tracking

Traffic inspection relies on two fundamental architecture models:

* **Stateless Filtering:** Inspects individual packets in isolation without context regarding previous packets. Rules match static header fields (Source IP, Destination IP, Source Port, Destination Port, Protocol). Highly performant but vulnerable to spoofing and out-of-order state bypasses.
* **Stateful Filtering:** Tracks the state of active network connections using the Linux kernel's **Conntrack (Connection Tracking)** module. Evaluates packets based on their context within an established session.

#### Conntrack State Classifications:
* **NEW:** Packets initiating a fresh connection (e.g., incoming TCP SYN).
* **ESTABLISHED:** Packets belonging to an already approved, bi-directional connection.
* **RELATED:** Packets initiating a secondary connection associated with an existing session (e.g., FTP data channels, ICMP error messages).
* **INVALID:** Packets that do not match any known connection state or contain corrupted headers (dropped by default in secure policies).

---

### 2. Linux Firewall Ecosystem: Netfilter, iptables & nftables

#### Netfilter Architecture
The Linux kernel subsystem providing hooks (`PREROUTING`, `INPUT`, `FORWARD`, `OUTPUT`, `POSTROUTING`) for packet interception, inspection, and modification.

#### `iptables` Framework
Legacy userspace utility operating on specific tables and chains:
* **Tables:** `filter` (default packet filtering), `nat` (address translation), `mangle` (packet header modification), `raw` (connection tracking bypass).
* **Default Chains:** `INPUT` (destined for host), `OUTPUT` (originating from host), `FORWARD` (routed through host).

#### `nftables` Framework
Modern successor replacing `iptables`. Features unified dual-stack (IPv4/IPv6) syntax, faster rule lookup lookup trees, lower memory overhead, and dynamic payload matching within a single framework.

---

### 3. NAT, Rate Limiting & Network Segmentation

* **Network Address Translation (NAT):**
  * **SNAT (Source NAT / Masquerade):** Modifies the source IP of outgoing packets, enabling internal private networks to access public networks.
  * **DNAT (Destination NAT / Port Forwarding):** Modifies the destination IP/port of incoming packets, routing external traffic to internal services.
* **Rate Limiting:** Restricting connection rates or packet frequency to mitigate brute-force attempts and DoS floods.
* **Network Segmentation:** Dividing networks into distinct isolated zones (VLANs, Subnets, DMZs) to enforce strict inter-zone firewall policies and restrict lateral movement.

---

### 🛠️ Defense Execution & Inspection Commands

* **Apply Stateful Rule to Allow Established Connections (iptables):**  
  `sudo iptables -A INPUT -m conntrack --ctstate ESTABLISHED,RELATED -j ACCEPT`

* **Rate-Limit Incoming SSH Connections to Mitigate Brute-Force (iptables):**  
  `sudo iptables -A INPUT -p tcp --dport 22 -m state --state NEW -m recent --set`  
  `sudo iptables -A INPUT -p tcp --dport 22 -m state --state NEW -m recent --update --seconds 60 --hitcount 4 -j DROP`

* **List Active Conntrack State Table Entries:**  
  `sudo conntrack -L`

* **View Active `nftables` RuleSet Structure:**  
  `sudo nft list ruleset`

---

### 📊 Filtering & Defense Summary Matrix

| Defense Vector | Mechanistic Purpose | Key Parameter / Syntax | Operational Benefit |
| :--- | :--- | :--- | :--- |
| **Stateful Tracking** | Validates session context | `-m conntrack --ctstate` | Blocks out-of-state probes & spoofed packets |
| **Rate Limiting** | Throttles connection volume | `-m recent` or `limit rate` | Prevents TCP connection exhaustion / floods |
| **Destination NAT** | Port Forwarding to DMZ | `PREROUTING -j DNAT` | Hides internal network infrastructure |
| **Segmentation** | Limits lateral movement | Inter-VLAN dropping rules | Contains breaches within isolated zones |
