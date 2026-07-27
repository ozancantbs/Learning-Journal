# Systems, DevOps & Platform Engineering Roadmap

<p align="center">
  <img src="https://img.shields.io/badge/Status-Actively%20Maintained-orange?style=for-the-badge" alt="Status" />
  <img src="https://img.shields.io/badge/Current%20Level-Level%201-blue?style=for-the-badge" alt="Current Level" />
  <img src="https://img.shields.io/badge/Language-TR%20%7C%20EN-success?style=for-the-badge" alt="Language" />
</p>

<p align="center">
  <strong>Systems · Security · DevOps · Cloud · SRE · Platform Engineering</strong>
</p>
<img width="203" height="141" alt="image" src="https://github.com/user-attachments/assets/0baa1ebc-3822-4d82-a9b4-8faec5cbc55f" />


> A long-term, hands-on learning journal focused on building practical skills in system administration, networking, cybersecurity, DevSecOps, cloud infrastructure, reliability, and platform engineering.

## About

This repository documents my technical learning journey from systems and networking foundations to cloud-native platforms, Site Reliability Engineering, and AI systems security.

Each completed area is supported with technical notes, practical exercises, troubleshooting records, security considerations, configuration examples, and milestone projects.

- **Current focus:** Systems, Networking & Security Foundations
- **Current module:** Advanced Networking for Platforms
- **Documentation language:** Turkish & English
- **Repository status:** 🚧 Actively maintained

## Roadmap

- [🚧 Level 1 — Systems, Networking & Security Foundations](#level-1--systems-networking--security-foundations)
- [⏳ Level 2 — Application Infrastructure, Containers & IaC](#level-2--application-infrastructure-containers--iac)
- [⏳ Level 3 — Engineering, CI/CD & Observability](#level-3--engineering-cicd--observability)
- [⏳ Level 4 — Cloud, Reliability & Intelligent Operations](#level-4--cloud-reliability--intelligent-operations)
- [⏳ Level 5 — AI Systems Security & MLSecOps](#level-5--ai-systems-security--mlsecops)
- [🏆 Milestone Projects](#milestone-projects)
- [📝 Documentation Approach](#documentation-approach)

## Level 1 — Systems, Networking & Security Foundations

### Module 01 — Advanced Networking for Platforms

**Status:** ✅ Completed

- [x] **Physical & Data Link Layers**
  - OSI and TCP/IP models
  - Ethernet, MAC addressing, switching and MTU
  - Layer 1 and Layer 2 troubleshooting

- [x] **ARP & Local Network Dynamics**
  - ARP cache and neighbor tables
  - Gratuitous ARP and proxy ARP
  - ARP spoofing risks and Dynamic ARP Inspection

- [x] **VLAN & VXLAN**
  - VLAN segmentation and IEEE 802.1Q tagging
  - Access ports and trunk ports
  - VXLAN overlay networks, VTEP and VNI

- [x] **Network Routing & Subnetting**
  - IPv4 addressing, CIDR and subnetting
  - Routing tables and default gateways
  - Static routing and OSPF/BGP fundamentals

- [x] **Transport Layer & Traffic Control**
  - TCP and UDP
  - Three-way handshake and connection termination
  - Flow control, congestion control and retransmission

- [x] **Application Layer Services**
  - DNS records and resolution flow
  - DHCP and DORA process
  - HTTP/HTTPS and TLS handshake

- [x] **Load Balancing & Proxy Concepts**
  - Forward proxy and reverse proxy
  - Layer 4 and Layer 7 load balancing
  - Health checks, TLS termination and TLS passthrough

### Module 02 — Enterprise Linux System Administration

**Status:** 🚧 In Progress

- [x] **Linux Architecture, Boot Process & FHS**
  - Kernel, GRUB, initramfs and systemd
  - Linux filesystem hierarchy
  - `/etc`, `/var`, `/home`, `/proc`, `/sys` and `/dev`

- [x] **Shell, CLI & Stream Processing**
  - Standard input/output and redirection
  - Pipes and environment variables
  - `grep`, `awk`, `sed`, `find`, `xargs`, `sort` and `uniq`

- [x] **User & Permission Management**
  - Users, groups, UID and GID
  - `chmod`, `chown`, `umask` and POSIX ACLs
  - `sudoers` and least-privilege access

- [x] **Process & Resource Management**
  - Process lifecycle and signals
  - `ps`, `top`, `htop` and `kill`
  - CPU, memory and file descriptor monitoring

- [x] **systemd, Services & Scheduled Tasks**
  - Unit files and service dependencies
  - `systemctl` and `journalctl`
  - systemd timers and cron jobs

- [x] **Storage, Filesystems & LVM**
  - Partitions, block devices and mount points
  - ext4, XFS, swap, LVM and RAID
  - `/etc/fstab`, inode structure and capacity management

- [x] **Linux Networking & Troubleshooting**
  - Interfaces, routing and DNS resolution
  - `ip`, `ss`, `tcpdump`, `dig`, `curl` and `nc`
  - Network namespaces and troubleshooting workflow

- [x] **Package & Repository Management**
  - APT, DPKG, DNF and RPM
  - Repository structures and package signatures
  - Dependency management and secure updates

- [x] **Logging, Time & Operational Maintenance**
  - `/var/log`, rsyslog and systemd journal
  - Log rotation
  - NTP, chrony, backup and restore validation

### Module 03 — Security Engineering & Incident Management

**Status:** ⏳ Planned

- [ ] Security foundations and risk management
- [ ] Threat modeling with STRIDE and MITRE ATT&CK
- [ ] Network threats, MITM, spoofing and DDoS
- [ ] Malware and endpoint defense
- [ ] Firewalling with iptables and nftables
- [ ] Cryptography, PKI and SSH hardening
- [ ] Linux hardening with CIS Benchmarks, SELinux and AppArmor
- [ ] System auditing, centralized logging and SIEM fundamentals
- [ ] Log-based abuse mitigation with Fail2Ban and rate limiting
- [ ] Incident detection, triage, containment, recovery and post-incident review

## Level 2 — Application Infrastructure, Containers & IaC

**Status:** ⏳ Planned

### Module 04 — Web, API & Database Foundations

- HTTP/HTTPS and TLS
- REST APIs and OAuth 2.0
- Database fundamentals, transactions and indexes
- PostgreSQL, Redis, backup and recovery concepts

### Module 05 — Container Technologies

- Docker architecture, namespaces and cgroups
- Dockerfile best practices and multi-stage builds
- Container networking, volumes and registries
- Image lifecycle and container security hardening

### Module 06 — Kubernetes Orchestration & Platform Fundamentals

- Kubernetes architecture and core resources
- Workloads, services, ingress and storage
- RBAC, NetworkPolicy and cluster security
- Scaling, troubleshooting, Helm and GitOps foundations

### Module 07 — Infrastructure as Code & Configuration Management

- Terraform fundamentals, modules and remote state
- Terraform state security and validation
- Ansible inventories, playbooks and roles
- Vault, secrets management and Git integration

## Level 3 — Engineering, CI/CD & Observability

**Status:** ⏳ Planned

### Module 08 — CI/CD Pipelines & GitOps

- GitHub Actions and pipeline design
- Build, test and deployment workflows
- DevSecOps security gates
- OIDC authentication and supply-chain security
- Argo CD and GitOps deployment workflows

### Module 09 — Systems Automation & Scripting

- Bash automation
- Python for infrastructure and operations
- API automation and network checks
- Testing and secure automation practices

### Module 10 — Software Engineering for Platform Teams

- Clean code and SOLID principles
- Design patterns and refactoring
- Testing strategies
- Distributed systems and API design
- Technical documentation practices

### Module 11 — Observability, Logging & Monitoring

- Metrics, logs and traces
- Prometheus and Grafana
- Loki and centralized log analysis
- OpenTelemetry
- Alerting, SLO monitoring and troubleshooting

## Level 4 — Cloud, Reliability & Intelligent Operations

**Status:** ⏳ Planned

### Module 12 — Cloud Computing

- AWS global infrastructure
- IAM, VPC, EC2 and load balancers
- S3, EBS, EFS and RDS
- Lambda, CloudTrail and cloud security services
- Disaster recovery, governance and cloud cost awareness

### Module 13 — Site Reliability Engineering

- SLI, SLO, SLA and error budgets
- Toil reduction and operational readiness
- Incident management
- High availability and disaster recovery
- Chaos engineering and resilience patterns

### Module 14 — Performance Engineering & Capacity Analysis

- CPU, memory, I/O and network performance
- Profiling and benchmarking
- Capacity planning
- Backpressure and bottleneck analysis
- Cost-performance trade-offs

### Module 15 — AI-Assisted Engineering

- Prompt design for engineering workflows
- AI-assisted code and infrastructure review
- Incident investigation support
- Hallucination risks and verification methods
- Responsible AI use in operations

## Level 5 — AI Systems Security & MLSecOps

**Status:** ⏳ Planned

- [ ] AI, ML and LLM foundations for security engineers
- [ ] AI threat modeling and security architecture
- [ ] LLM, RAG and agentic application security
- [ ] Adversarial machine learning and model security
- [ ] AI data security, privacy and knowledge protection
- [ ] Secure AI infrastructure and model serving
- [ ] MLSecOps and AI supply-chain security
- [ ] Secure AI software development lifecycle
- [ ] AI security testing, evaluation and red teaming
- [ ] AI monitoring, threat detection and incident response
- [ ] AI governance, standards, assurance and compliance

## Milestone Projects

### Project 01 — Hardened & Audited Linux Server Infrastructure

**Target:** After Level 1

- Hardened Linux server configuration
- SSH security and access controls
- Firewall policies with nftables
- Fail2Ban and authentication abuse mitigation
- auditd and centralized logging
- Backup and restore validation
- Bash-based security and operational checks

### Project 02 — Automated Provisioning of a Production-Oriented Kubernetes Environment

**Target:** After Level 2

- AWS VPC and EC2 infrastructure with Terraform
- Server configuration with Ansible
- Kubernetes cluster installation with kubeadm
- containerd runtime configuration
- RBAC and NetworkPolicy implementation
- Rebuild and validation documentation

### Project 03 — Resilient Cloud-Native GitOps & Observability Platform

**Target:** After Levels 3 and 4

- GitHub Actions CI pipelines
- Kubernetes deployment with Argo CD
- Prometheus, Grafana and Loki
- OpenTelemetry-based tracing
- SLOs, alerting and incident workflows
- Disaster recovery and resilience testing

## Documentation Approach

Each completed topic may include:

- Turkish and English technical notes
- Architecture and data-flow diagrams
- Security risks and defensive controls
- Hardening recommendations
- Commands and configuration examples
- Troubleshooting notes
- Practical validation steps
- Production and lab environment distinctions
- Official documentation and standards references

> Small exercises are used when they strengthen understanding. Larger implementations are consolidated into milestone projects to demonstrate related technologies within realistic system boundaries.

## Status Notice

This repository represents an ongoing learning process.

- ✅ **Completed:** Topics studied and reviewed
- 🚧 **In Progress:** Areas currently being worked on
- ⏳ **Planned:** Future learning objectives and project scope

Technologies, architectures and implementation details may evolve as systems are built, tested, measured and documented.

<p align="center">
  <sub>Built as a long-term technical learning journal.</sub>
</p>
