# 05 — Transport Layer & Traffic Control

This document covers Layer 4 of the OSI Model, the Transport Layer mechanisms, end-to-end communication principles over IP networks, Linux kernel network stack management, and traffic control. The goal of this document is to examine the Transport Layer's operating logic, security dimension, and operational troubleshooting approaches in depth from the perspective of network engineering, system administration, DevSecOps, and SRE.

---

## 1. Overview

The Transport Layer elevates the host-to-host packet delivery service provided by the Network Layer (IP) to a process-to-process logical communication level. While the network layer ensures data reaches the correct IP address, the transport layer determines which application or process on the operating system the incoming data belongs to and manages the transmission quality of this data.

Core responsibilities of the Transport Layer include:

* **Process-to-Process Communication:** Maps network traffic to related processes via port numbers.
* **Multiplexing and Demultiplexing:** Aggregates data from different applications into a single network stream (multiplexing) and distributes incoming packets to the correct applications (demultiplexing).
* **Error Control and Reliability:** Performs checksum calculations to verify data integrity, preserves packet ordering, and retransmits lost packets.
* **Flow and Congestion Control:** Dynamically adjusts data transmission speed based on receiver and network capacity.

There are two primary communication models at the transport layer:

1. **Connection-Oriented Communication:** A logical session is established between the sender and receiver before data transfer begins. Complete, error-free, and ordered delivery of data is guaranteed. The primary protocol is **TCP**.
2. **Connectionless Communication:** Data is sent over the network as independent blocks without any prior handshake or state tracking. No ordering or delivery guarantees are provided. The primary protocol is **UDP**.

---

## 2. Why Is It Important?

The Transport Layer lies at the intersection of system performance and security in modern IT infrastructures:

* **System Administration & SRE:** Memory utilization on servers, open File Descriptors, and kernel backlog queue management directly depend on TCP socket states. Accumulations of `TIME_WAIT` or buffer exhaustion lead directly to service outages.
* **Network Engineering:** Efficient utilization of network bandwidth, optimization of Round-Trip Time (RTT), and congestion management are achieved through parameters at this layer.
* **DevOps & Platform Engineering:** Service-to-service communication in microservice architectures, Load Balancer routing, and latency metrics in Service Mesh setups are shaped by TCP flow control parameters.
* **Cybersecurity & Defensive Security:** Stateful Firewalls and IPS/IDS systems perform state tracking (Connection Tracking - `conntrack`) at Layer 4. DoS/DDoS attacks like SYN Flood and Session Hijacking directly target transport layer vulnerabilities.

---

## 3. Core Concepts and Terminology

### 3.1. Ports and Types

Ports are 16-bit integers (0 – 65535) that identify processes presenting or connecting to network services on an operating system. According to IANA standards, they are divided into three groups:

* **Well-Known Ports (0 – 1023):** Reserved for system-level standard services. In POSIX standards, listening (binding) to these ports requires privileged authority (root/`CAP_NET_BIND_SERVICE`). E.g., SSH (22), DNS (53), HTTP (80), HTTPS (443).
* **Registered Ports (1024 – 49151):** Assigned to specific applications or database services. E.g., MySQL (3306), PostgreSQL (5432), Redis (6379).
* **Ephemeral Ports (49152 – 65535):** Temporary ports dynamically used by client applications when connecting to external servers. On Linux systems, the default range is usually configured under `/proc/sys/net/ipv4/ip_local_port_range` as 32768 – 60999.

### 3.2. Socket and Socket Pair

In network programming and kernel space, communication endpoints are represented by the **Socket** architecture:

    Socket = IP Address + Port Number

A unique TCP/UDP connection over the network is defined by a **4-Tuple (Socket Pair)**:

    Socket Pair = (Source IP, Source Port, Destination IP, Destination Port)

*When protocol information is added, this structure is referred to as a **5-Tuple**.*

### 3.3. Concept Comparison Table

| Concept | Technical Description | System Role | Related Concepts |
| :--- | :--- | :--- | :--- |
| **TCP (Transmission Control Protocol)** | Connection-oriented, reliable, ordered byte-stream protocol (RFC 793 / RFC 9293). | Critical data transfers with zero fault tolerance. | Handshake, Sliding Window, Congestion Control |
| **UDP (User Datagram Protocol)** | Connectionless, minimalist, low-latency datagram protocol (RFC 768). | Speed-focused communication tolerant to packet loss. | Datagram, Checksum, Stateless |
| **MSS (Maximum Segment Size)** | Maximum amount of raw application data (payload) a TCP segment can carry (Bytes). | Prevents packet fragmentation by excluding IP/TCP header overhead. | MTU, IP Header, TCP Header |
| **Byte-Stream Model** | TCP handling data as a continuous stream of bytes without message boundaries. | Allows application to read data in chunks or streams. | Sequence Number, Buffer, Stream |
| **TCP Reset (RST)** | Flag sent in response to an invalid packet or during forced termination. | Instantly terminates invalid or unauthorized connections. | TCP Flags, Abort, Firewall |

### 3.4. Packet Header Structures (TCP and UDP Headers)

#### UDP Header Structure
The UDP header is fixed and only **8 Bytes (64 bits)** long:

    0                   16                  31 bit
   +-------------------+-------------------+
   |    Source Port    |  Destination Port |  (4 Bytes)
   +-------------------+-------------------+
   |      Length       |     Checksum      |  (4 Bytes)
   +-------------------+-------------------+
   |              DATA (Payload)           |
   +---------------------------------------+

#### TCP Header Structure
The TCP header is fixed at **20 Bytes** excluding options. It can extend up to **60 Bytes** with options:

    0                   16                  31 bit
   +-------------------+-------------------+
   |    Source Port    |  Destination Port |  (4 Bytes)
   +---------------------------------------+
   |            Sequence Number            |  (4 Bytes)
   +---------------------------------------+
   |         Acknowledgment Number         |  (4 Bytes)
   +-------+-------+---+-------------------+
   | Data  |Reser- |F  |    Window Size    |  (4 Bytes)
   |Offset |ved    |lags|                  |
   +-------+-------+---+-------------------+
   |     Checksum      |   Urgent Pointer  |  (4 Bytes)
   +-------------------+-------------------+
   |        Options (0 - 40 Bytes)         |
   +---------------------------------------+
   |              DATA (Payload)           |
   +---------------------------------------+

#### TCP Flags
* **SYN (Synchronize):** Initiates a connection and synchronizes Initial Sequence Numbers (ISN).
* **ACK (Acknowledgment):** Confirms receipt of data or control packets.
* **FIN (Finish):** Indicates sender has no more data to transmit and requests connection closure.
* **RST (Reset):** Forces immediate termination of a connection under abnormal conditions.
* **PSH (Push):** Instructs the receiving kernel to deliver data directly to the application without buffering.
* **URG (Urgent):** Indicates that the Urgent Pointer field is active for processing urgent data.

### 3.5. TCP State Machine

A TCP connection transitions through specific states throughout its lifecycle:

* **LISTEN:** Server is ready to accept incoming connection requests on the specified port.
* **SYN_SENT:** Client sent a `SYN` packet and is awaiting a response from the server.
* **SYN_RCVD:** Server received `SYN`, sent `SYN-ACK`, and is awaiting an `ACK` from the client.
* **ESTABLISHED:** 3-Way Handshake complete; ready for bidirectional data transfer.
* **FIN_WAIT_1:** Active closer sent `FIN` and is awaiting an acknowledgment/response.
* **FIN_WAIT_2:** Active closer received `ACK` and is awaiting a `FIN` from the remote endpoint.
* **CLOSE_WAIT:** Passive closer received `FIN` and is waiting for local application to issue `close()`.
* **CLOSING:** Both endpoints initiated closure simultaneously (Simultaneous Close).
* **LAST_ACK:** Passive closer sent its `FIN` and is awaiting the final `ACK`.
* **TIME_WAIT:** Active closer waits for 2MSL duration to ensure in-flight packets expire.
* **CLOSED:** Connection is fully closed; socket resources are freed.

---

## 4. Operational Logic

### 4.1. Connection Establishment: TCP Three-Way Handshake

A TCP session is initiated between a client and a server via a three-step handshake:

    Client                                             Server
       |                                                    |
       | --- 1. SYN (Seq = ISN_c) ------------------------> | (SYN_RCVD)
       |                                                    |
       | <-- 2. SYN-ACK (Seq = ISN_s, Ack = ISN_c + 1) ---- |
       |                                                    |
       | --- 3. ACK (Ack = ISN_s + 1) --------------------> | (ESTABLISHED)
       v                                                    v

1. **SYN:** Client generates a cryptographically random **Initial Sequence Number (ISN)**, sends a `SYN` packet, and enters `SYN_SENT` state.
2. **SYN-ACK:** Server accepts the request, generates its own `ISN`, sends `ACK = ISN_c + 1` to confirm the client's `SYN`, and sets `SYN` flag. Server enters `SYN_RCVD` state.
3. **ACK:** Client sends `ACK = ISN_s + 1` to acknowledge the server's `SYN`. Both sides transition to `ESTABLISHED` state.

> **Initial Sequence Number (ISN) Security:** ISN values are generated cryptographically at random rather than starting from 0 to prevent delayed stale packets from corrupting new connections and to mitigate **TCP Sequence Prediction (Session Hijacking)** attacks.

### 4.2. Data Verification and Byte Ordering (Sequence & ACK Math)

TCP tracks data on a **byte basis**, not a packet basis:
* **Sequence Number (Seq):** Specifies the sequence number of the first byte carried in the segment.
* **Acknowledgment Number (Ack):** Represents the next byte sequence number expected by the receiver (Cumulative ACK principle).

    MSS = MTU - (IP_Header + TCP_Header)

In a standard Ethernet network (MTU = 1500 Bytes), subtracting default headers (20 Bytes IP + 20 Bytes TCP) results in a standard MSS = 1460 Bytes.

### 4.3. Connection Termination: Four-Way Termination

Because TCP connections are full-duplex, each direction must be terminated independently:

    Client (Active Closer)                            Server (Passive Closer)
       |                                                    |
       | --- 1. FIN --------------------------------------> | (CLOSE_WAIT)
       | <-- 2. ACK --------------------------------------- |
       |                                                    | (App calls close())
       | <-- 3. FIN --------------------------------------- | (LAST_ACK)
       | --- 4. ACK (TIME_WAIT) --------------------------> | (CLOSED)

#### Critical States: `TIME_WAIT` and `CLOSE_WAIT`
* **`TIME_WAIT`:** Entered by the active closer to handle potential loss of the final `ACK` and to ensure stale packets in the network expire during **2MSL (Maximum Segment Lifetime — typically 60 seconds on Linux)**.
* **`CLOSE_WAIT`:** Entered by the passive closer when kernel receives `FIN`, but the application layer has not yet called `close()`. A high number of `CLOSE_WAIT` sockets indicates a **socket leak at the application layer**.

### 4.4. Flow Control and Window Management

Flow control prevents a fast sender from overflowing a slow receiver's buffer (Receive Buffer).

* **Receive Window (rwnd):** Advertised in the TCP header by the receiver to indicate available buffer space.
* **Sliding Window:** A dynamic mechanism controlling how much unacknowledged data the sender can transmit.
* **Window Scaling (RFC 1323):** Standard 16-bit window size field supports up to 65,535 Bytes (64 KB). Window Scaling in `TCP Options` scales this up to 1 GB for high-bandwidth-delay-product networks.

### 4.5. Congestion Control

Congestion control prevents network collapse by detecting bottlenecks (routers/switches) along the path. The sender maintains a **Congestion Window (`cwnd`)**. The maximum transmission limit is bounded by:

    Transmission Limit = min(rwnd, cwnd)

#### Algorithm Phases and Timers
1. **Slow Start:** `cwnd` begins small (e.g., 10 MSS). For every successful RTT cycle, `cwnd` doubles **exponentially** (1 -> 2 -> 4 -> 8).
2. **Congestion Avoidance:** Once `cwnd` reaches **`ssthresh` (Slow Start Threshold)**, growth transitions to a **linear** rate (+1 MSS per RTT).
3. **Fast Retransmit & Fast Recovery:** Receiver sends duplicate ACKs upon detecting a missing segment. Upon receiving **3 duplicate ACKs**, sender retransmits the missing segment immediately without waiting for **RTO (Retransmission Timeout)** (Fast Retransmit), halves `ssthresh`, and continues linear growth (Fast Recovery).
4. **Round-Trip Time (RTT) and RTO Calculation:** Sender continuously measures RTT. RTO timer dynamically adjusts based on RTT variance. If RTO expires, packet is assumed lost and `cwnd` collapses to 1 MSS.
5. **Selective Acknowledgment (SACK - RFC 2018):** Allows receiver to inform sender about non-contiguous blocks received, enabling retransmission of only lost segments.
6. **TCP Keepalive:** Periodically transmits probe packets during idle connections to detect if the peer has crashed.

---

## 5. Architecture and Components

The Linux kernel network stack socket management and queue architecture are modeled as follows:

    [ Incoming TCP SYN Packet ]
               |
               v
    +-----------------------------------+
    |            SYN QUEUE              |  <-- Half-Open Connections (SYN_RCVD)
    |  (SYN-ACK sent, awaiting ACK)     |  (Limit: net.ipv4.tcp_max_syn_backlog)
    +-----------------------------------+
               |
           ACK Received
               |
               v
    +-----------------------------------+
    |           ACCEPT QUEUE            |  <-- Fully Established Connections (ESTABLISHED)
    | (Until application calls accept())|  (Limit: min(somaxconn, backlog))
    +-----------------------------------+
               |
        accept() syscall
               v
    [ User Space Application (NGINX/Go) ]

### 5.1. Linux TCP Backlog Queues

1. **SYN Queue (Half-Open Queue):** Kernel queue holding connections in the first phase of 3-way handshake (`SYN_RCVD`). Bounded by `net.ipv4.tcp_max_syn_backlog`.
2. **Accept Queue (Completely Established Queue):** Queue holding fully established connections (`ESTABLISHED`) awaiting retrieval by user-space applications via `accept()`. Bounded by `min(net.core.somaxconn, application_backlog)`.

### 5.2. Connection Tracking (conntrack) and NAT

Netfilter's core component `conntrack` maintains state tracking for IP packets passing through the system (`NEW`, `ESTABLISHED`, `RELATED`, `INVALID`).

Stateful firewalls and Network Address Translation (NAT) rely on this table. NAT devices store dynamic mappings between private internal IP/Ports and public IP/Ports inside `conntrack`.

---

## 6. Step-by-Step Technical Flow

Step-by-step flow when a client requests data from a web server:

    Client (192.168.1.50)                               Server (10.0.0.10:443)
       |                                                         |
       | --- 1. [SYN] Seq=1000, MSS=1460, WinScale=7 ----------> | (Accept/SYN Queue)
       | <-- 2. [SYN, ACK] Seq=5000, Ack=1001, MSS=1420 -------- |
       | --- 3. [ACK] Seq=1001, Ack=5001 ----------------------> | (Accept Queue -> accept())
       |                                                         |
       | --- 4. [PSH, ACK] Seq=1001, Ack=5001, Data (500B) ----> | (TLS/HTTP Request)
       | <-- 5. [ACK] Seq=5001, Ack=1501, Win=64240 ------------ |
       |                                                         |
       | <-- 6. [DATA] Seq=5001, Ack=1501, Data (1420B) -------- | (Response Part 1)
       | <-- 7. [DATA] Seq=6421, Ack=1501, Data (1420B) -------- | (Response Part 2 - LOST!)
       |                                                         |
       | --- 8. [ACK] Seq=1501, Ack=6421, SACK=[7841-9260] ----> | (Duplicate ACK / SACK)
       | <-- 9. [DATA] Seq=6421 (Fast Retransmit) -------------- | (Lost Packet Recovered)

1. **Connection Setup:** Client sends `SYN` from `192.168.1.50:51234` to `10.0.0.10:443`. MSS and Window Scale are negotiated.
2. **Queue Processing:** Server generates `SYN-ACK` and adds entry to `SYN Queue`. Upon receiving client `ACK`, connection moves to `Accept Queue`.
3. **Application Accept:** Web server calls `accept()` to consume the socket from the queue, allocating a process-specific File Descriptor (FD).
4. **Data Transfer & Loss Recovery:** Client sends HTTP request. Server splits payload into MSS segments. Upon segment loss (Step 7), client sends Duplicate ACK with SACK info (Step 8). Server uses Fast Retransmit to resend missing segment immediately without waiting for RTO (Step 9).

---

## 7. Real-World Use Cases

### Scenario 1: High-Traffic Web API Gateway (E-Commerce Platform)
* **Problem:** Encountering `Cannot assign requested address` and `connection refused` errors when opening upstream microservice connections during flash sales.
* **Root Cause:** Ephemeral Port exhaustion on the client side caused by short-lived HTTP connections, alongside exceeding `Accept Queue` capacity (`somaxconn`) on backend servers.
* **Solution:** Enable HTTP Keep-Alive / Connection Pooling on API Gateway, expand `net.ipv4.ip_local_port_range`, and increase `somaxconn` at kernel level.

### Scenario 2: Live Video Streaming Platform (IPTV / Video Streaming)
* **Problem:** Video buffering issues on client side and high CPU overhead on server side during high-definition live broadcasts.
* **Root Cause:** TCP retransmission and flow control latencies degrading real-time stream performance.
* **Solution:** Migrate from TCP to UDP-based protocols (RTP/RTSP or QUIC/HTTP3) at the transport layer, prioritizing lower latency by dropping lost frames instead of requesting retransmissions.

---

## 8. Security Perspective

### 8.1. SYN Flood Attack
* **Technical Problem:** Attacker floods server with `SYN` packets using spoofed IP addresses, exhausting `SYN Queue` memory.
* **Mechanism:** Spoofed IPs do not respond with `ACK`, keeping connections in `SYN Queue` until timeout.
* **Impact:** Denial of Service (DoS/DDoS — Availability loss). Legitimate users cannot connect.
* **Detection:** `dmesg` warnings (`Possible SYN flooding on port...`) or abnormal spikes in `ss -ant state syn-recv`.
* **Defense (SYN Cookies - RFC 4987):** Enabling `net.ipv4.tcp_syncookies = 1` instructs kernel to stop allocating state memory when `SYN Queue` overflows. A cryptographic hash is embedded into the `ISN` of the `SYN-ACK`. When legitimate `ACK` returns, hash is validated and socket is directly placed into `Accept Queue`.

### 8.2. UDP Flood Attack
* **Technical Problem:** Attacker floods random UDP ports with high volume traffic without connection state overhead.
* **Impact:** Saturates server Network Interface Card (NIC) bandwidth and exhausts CPU due to ICMP "Port Unreachable" generation.
* **Defense:** Cannot be mitigated via host firewall alone; requires upstream rate limiting, BGP Blackholing, or DDoS Scrubbing centers.

### 8.3. Port Scanning & Session Hijacking
* **TCP SYN Scan (Stealth Scan):** Attacker sends `SYN`, detects open port upon receiving `SYN-ACK`, but sends `RST` to avoid completing handshake. Leaves minimal traces in application logs.
* **TCP Connect Scan:** Completes full handshake, leaving application log trails.
* **TCP Session Hijacking:** Attacker injects malicious data into an active session by predicting Sequence Numbers. Modern Linux kernel cryptographic ISN generation mitigates this risk.

---

## 9. Troubleshooting and Operational Approach

### 9.1. Resource Exhaustion

1. **Ephemeral Port Exhaustion:** Thousands of rapidly opened/closed sockets stuck in `TIME_WAIT` state on the client side.
   * *Detection:* `ss -tan state time-wait | wc -l`
   * *Solution:* `net.ipv4.tcp_tw_reuse = 1` and Connection Pooling.
2. **File Descriptor Exhaustion:** Every socket in Linux is represented as a File Descriptor (FD).
   * *Process Limit:* Exceeding `ulimit -n` triggers `EMFILE (Too many open files)` in application logs.
   * *System Limit:* Exceeding `/proc/sys/fs/file-max` triggers `ENFILE (Too many open files in system)` in kernel logs.

### 9.2. Backlog Analysis with `ss` Command

Output of `ss -lnt` for listening sockets (`LISTEN`) carries specific meanings:

    $ ss -lnt
    State      Recv-Q Send-Q Local Address:Port  Peer Address:Port
    LISTEN     129    128    0.0.0.0:80          0.0.0.0:*

* **`Send-Q`:** Maximum configured **Accept Queue capacity** (`backlog`).
* **`Recv-Q`:** Number of connections currently waiting in **Accept Queue** not yet `accept()`ed by the application.
* **Critical Heuristic:** If `Recv-Q > Send-Q`, application is either locked or unable to process incoming connections fast enough due to CPU starvation. Incoming connections will be dropped.

### 9.3. Packet and Traffic Analysis Tools

* **`ss` (Socket Statistics):** Directly inspects socket information from kernel (`ss -tulpn`).
* **`lsof` (List Open Files):** Maps processes to open network sockets and files (`lsof -i :80`).
* **`tcpdump`:** CLI packet capture tool (`tcpdump -i eth0 port 80 -n`).
* **Wireshark:** GUI tool for inspecting `.pcap` captures, analyzing TCP stream reconstruction (`Follow TCP Stream`), RTT, and retransmissions.

### 9.4. Troubleshooting Command Reference

    # 1. Display summary of socket counts and state distribution
    ss -s

    # 2. Identify process and File Descriptor listening on port 80
    lsof -i :80

    # 3. Live capture SYN packets on a specific interface (detect SYN floods)
    tcpdump -i eth0 -n 'tcp[tcpflags] & (tcp-syn) != 0'

    # 4. Inspect kernel-level dropped packet and overflow statistics
    nstat -az TcpExtListenDrops TcpExtListenOverflows

---

## 10. Production, SRE, and Platform Engineering Perspective

* **Observability:** SRE teams monitor TCP health via Prometheus and `node_exporter` using key metrics:
  * `node_netstat_Tcp_RetransSegs`: Packet retransmission rate (indicator of loss and congestion).
  * `node_netstat_TcpExt_ListenOverflows`: Rejected connection count due to Accept Queue overflows.
  * `node_nf_conntrack_entries` / `node_nf_conntrack_entries_limit`: Connection tracking table saturation level.
* **Kubernetes CNI & Service Mesh:** Container network interfaces (e.g., Cilium, Calico) rely heavily on `conntrack`. A `nf_conntrack: table full` error can disrupt all cluster inter-pod communications.
* **Congestion Control Algorithm Selection:** Modern Linux distributions default to **CUBIC**. For cloud-native environments prioritizing high bandwidth and low latency, Google's **BBR (Bottleneck Bandwidth and RTT)** algorithm is preferred.

---

## 11. Limitations, Alternatives, and Trade-offs

| Layer / Protocol | Advantages | Disadvantages | Use Case / Trade-off |
| :--- | :--- | :--- | :--- |
| **TCP** | 100% Reliability, ordered delivery, flow/congestion control. | High header overhead, Handshake latency, Head-of-Line Blocking. | Mission-critical transfers with zero tolerance for data loss (Web, Databases, SSH). |
| **UDP** | Minimal latency, low header overhead (8B), zero handshake delay. | No reliability, no ordering, no flow control. | Real-time streaming, VoIP, DNS, Gaming. |
| **QUIC (HTTP/3)** | Runs over UDP. Built-in TLS 1.3. Resolves Head-of-Line blocking. Supports Connection Migration. | Higher CPU overhead, potential blocking by legacy firewalls. | Modern web, mobile applications, high-performance content delivery (CDN). |

---

## 12. Validated Parameters and Command Reference

Recommended safe kernel tuning parameters to append to `/etc/sysctl.conf`:

    # Enable SYN Flood protection (Mandatory)
    net.ipv4.tcp_syncookies = 1

    # Increase Accept Queue capacity (Default 128 is often inadequate)
    net.core.somaxconn = 4096

    # Increase half-open connection backlog limit
    net.ipv4.tcp_max_syn_backlog = 8192

    # Expand local ephemeral port range
    net.ipv4.ip_local_port_range = 1024 65535

    # Allow reuse of TIME_WAIT sockets for outgoing connections
    net.ipv4.tcp_tw_reuse = 1

    # Increase conntrack table max capacity
    net.netfilter.nf_conntrack_max = 262144

> ⚠️ **Critical Warning:** The `net.ipv4.tcp_tw_recycle` parameter was completely removed in Linux kernel 4.12. It causes random packet drops for clients behind NAT environments and **must never be used.**

---

## 13. References and Further Reading

* **RFC 793 / RFC 9293:** Transmission Control Protocol (TCP Specification)
* **RFC 768:** User Datagram Protocol (UDP Specification)
* **RFC 2018:** TCP Selective Acknowledgment Options (SACK)
* **RFC 1323 / RFC 7323:** TCP Extensions for High Performance (Window Scaling)
* **RFC 4987:** TCP SYN Flooding Attacks and Common Mitigations (SYN Cookies)
* **Linux Kernel Networking Documentation:** `kernel.org/doc/Documentation/networking/`
* **Linux Man Pages:** `ss(8)`, `lsof(8)`, `tcpdump(8)`, `sysctl(8)`
