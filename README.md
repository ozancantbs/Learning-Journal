# 🏆 Systems, DevOps & Platform Engineering Learning Roadmap

## EN

This repository is an ongoing learning journal and practical roadmap created to build a strong technical foundation in system administration, networking, cybersecurity, DevSecOps, cloud computing, Site Reliability Engineering, and platform engineering.

The roadmap combines technical documentation, security-focused analysis, practical validation exercises, and progressively advanced portfolio projects. Content is prepared in both Turkish and English and is updated throughout the learning process.

## TR

Bu depo; sistem yönetimi, ağ teknolojileri, siber güvenlik, DevSecOps, bulut bilişim, Site Reliability Engineering ve platform mühendisliği alanlarında güçlü bir teknik temel oluşturmak amacıyla hazırladığım, devam eden bir öğrenme günlüğü ve uygulama yol haritasıdır.

Yol haritası; teknik dokümantasyonlar, güvenlik odaklı değerlendirmeler, uygulamalı doğrulama çalışmaları ve seviyeler ilerledikçe geliştirilecek kapsamlı portföy projelerinden oluşmaktadır. İçerikler Türkçe ve İngilizce olarak hazırlanmakta ve öğrenme süreci boyunca düzenli olarak güncellenmektedir.

## 📍 Current Progress

| Field | Current Status |
|---|---|
| **Current Level** | Level 1 — Foundations of Systems & Security |
| **Current Module** | Module 01 — Advanced Networking for Platforms |
| **Repository Status** | 🚧 Actively Maintained |
| **Documentation Languages** | Turkish and English |

### Status Legend

- ✅ Completed
- 🚧 In Progress
- ⏳ Planned

> Completion indicators represent the current learning progress. Direct links will be added as the corresponding topic documents are committed to the repository.

# 🗺️ Learning Roadmap — Öğrenme Yol Haritası

## 🌐 Level 1: Foundations of Systems & Security — 🚧 In Progress
---

## 🌐 Module 01: Advanced Networking for Platforms — ✅ In Progress

- ✅ [**Physical & Data Link Layers**](https://github.com/ozancantbs/Learning-Journal/tree/main/Level_1/01_Physical_%26_Data_Link_Layers) — <sub>*OSI ve TCP/IP modelleri, encapsulation, fiziksel sinyaller, Ethernet frame yapısı, MAC addressing, switching, MTU, interface counters ve Layer 1/2 troubleshooting.*<sub>
- ✅ [**ARP & Local Network Dynamics**](https://github.com/ozancantbs/Learning-Journal/tree/main/Level_1/02_ARP_%26_Local_Network_Dynamics) — <sub>*ARP çalışma mantığı, neighbor table, ARP cache durumları, gratuitous ARP, proxy ARP, ARP spoofing riskleri ve Dynamic ARP Inspection.*<sub>
- ✅ [**VLAN & VXLAN**](https://github.com/ozancantbs/Learning-Journal/tree/main/Level_1/03_VLAN_%26_VXLAN) — <sub>*Layer 2 segmentasyonu, access ve trunk portlar, IEEE 802.1Q tagging, VLAN hopping riskleri, VXLAN overlay mimarisi, VTEP ve VNI kavramları.*<sub>
- ✅ [**Network Routing & Subnetting**](https://github.com/ozancantbs/Learning-Journal/tree/main/Level_1/04_Network_Routing_%26_Subnetting) — <sub>*IPv4 adresleme, CIDR, subnetting, routing table, default gateway, static ve dynamic routing temelleri, OSPF/BGP genel mantığı ve IP fragmentation.*<sub>
- ✅ [**Transport Layer & Traffic Control**](https://github.com/ozancantbs/Learning-Journal/tree/main/Level_1/05_Transport_Layer_%26_Traffic_Control) — <sub>*TCP ve UDP, portlar ve socket'ler, TCP three-way handshake, sequence ve acknowledgment numbers, flow control, congestion control, retransmission ve connection termination.*<sub>
- ✅ [**Application Layer Services**](https://github.com/ozancantbs/Learning-Journal/tree/main/Level_1/06_Application_Layer_Services) — <sub>*DNS kayıt türleri, DNS resolution, DNS spoofing riskleri, DHCP DORA süreci, DHCP Snooping, HTTP/HTTPS ve TLS handshake.*<sub>
- ✅ [**Load Balancing & Proxy Concepts**](https://github.com/ozancantbs/Learning-Journal/tree/main/Level_1/07_Load_Balancing_%26%20_Proxy_Concepts) — <sub>*Forward proxy ve reverse proxy, Layer 4 ve Layer 7 load balancing, health checks, session persistence, TLS termination ve TLS passthrough.*<sub>

---

## 🐧 Module 02: Enterprise Linux System Administration — ✅ In Progress

- ✅ **Linux Architecture, Boot Process & FHS** — <sub>*Kernel, initramfs, GRUB, systemd, boot süreci ve Linux dizin hiyerarşisi; `/etc`, `/var`, `/home`, `/opt`, `/tmp`, `/proc`, `/sys` ve `/dev`.
- ✅ **Shell, CLI & Stream Processing** — <sub>*Standard input/output, pipes, redirection, environment variables ve `grep`, `awk`, `sed`, `cut`, `sort`, `uniq`, `find`, `xargs` gibi araçlar.
- ✅ **User & Permission Management** — <sub>*User ve group yönetimi, UID/GID, `chmod`, `chown`, `umask`, POSIX ACLs, sudoers ve least-privilege yaklaşımı.
- ✅ **Process & Resource Management** — <sub>*Process lifecycle, signals, foreground/background jobs, `ps`, `top`, `htop`, `kill`, file descriptors, memory ve CPU resource takibi.
- ✅ **systemd, Services & Scheduled Tasks** — <sub>*Unit files, service dependencies, `systemctl`, `journalctl`, restart policies, systemd timers ve cron.
- ✅ **Storage, Filesystems & LVM** — <sub>*Partitioning, block devices, ext4, XFS, mount işlemleri, `/etc/fstab`, inode yapısı, swap, LVM, RAID ve disk kapasite yönetimi.
- ✅ **Linux Networking & Troubleshooting** — <sub>*Interface yönetimi, routing, DNS çözümleme, `/etc/hosts`, `systemd-resolved`, `ip`, `ss`, `tcpdump`, `dig`, `curl`, `nc` ve network namespaces.
- ✅ **Package & Repository Management** — <sub>*`apt`, `dpkg`, `dnf`, `rpm`, repository yapıları, package signatures, dependency management ve güvenli güncelleme süreçleri.
- ✅ **Logging, Time & Operational Maintenance** — <sub>*`/var/log`, systemd journal, rsyslog, log rotation, NTP, chrony, backup, restore validation ve bakım planları.

---

## 🛡️ Module 03: Security Engineering & Incident Management — ⏳ Planned

- ⏳ **Security Foundations & Risk Management** — <sub>*CIA triad, authentication, authorization, accountability, assets, threats, vulnerabilities, risk, attack surface ve defense in depth.
- ⏳ **Threat Modeling & Attack Lifecycle** — <sub>*Trust boundaries, data-flow diagrams, STRIDE, attack trees, Cyber Kill Chain, MITRE ATT&CK ve saldırı yaşam döngüsü.
- ⏳ **Network and Availability Threats** — <sub>*MITM, spoofing, session hijacking, DNS ve ARP saldırıları, DoS/DDoS, SYN flood ve hizmet kesintisi senaryoları.
- ⏳ **Malware & Endpoint Defense** — <sub>*Worm, Trojan, ransomware, rootkit, botnet, fileless malware, sandboxing, hashes, EDR ve endpoint hardening.
- ⏳ **Firewalling & Network Defense** — <sub>*Stateful ve stateless filtering, Netfilter, iptables, nftables, connection tracking, NAT, rate limiting ve segmentation.
- ⏳ **Cryptography, PKI & SSH Security** — <sub>*Symmetric ve asymmetric cryptography, hashing, digital signatures, certificates, SSH key pairs, host keys ve `sshd_config` hardening.
- ⏳ **Linux Hardening & Vulnerability Management** — <sub>*Attack-surface reduction, CIS Benchmarks, CVE/CVSS, patching, service inventory, permissions, PAM, SELinux ve AppArmor temelleri.
- ⏳ **System Auditing, Logging & SIEM** — <sub>*`/var/log`, systemd journal, auditd, log bütünlüğü, merkezi loglama, SIEM, detection rules ve security-event analysis.
- ⏳ **Log-Based Abuse Mitigation** — <sub>*Fail2Ban çalışma mantığı, authentication log analizi, geçici engelleme, rate limiting ve aracın güvenlik sınırlamaları.
- ⏳ **Incident Response Fundamentals** — <sub>*Preparation, detection, triage, containment, eradication, recovery, evidence preservation, communication ve post-incident review.

---

# 📦 Level 2: Application Infrastructure, Containers & Infrastructure as Code — ⏳ Planned

### Uygulama, Veri, Otomasyon ve Konteyner Altyapıları

Web uygulamalarının, API'lerin ve veri katmanlarının çalışma prensiplerinin öğrenildiği; konteyner tabanlı platformların ve kodla tanımlanan altyapıların oluşturulduğu seviyedir.

---

## 🌍 Module 04: Web, API & Database Foundations — ⏳ Planned

- ⏳ **Web Architecture Fundamentals** — <sub>*Client-server modeli, request-response akışı, stateless ve stateful application yaklaşımları.
- ⏳ **HTTP & HTTPS** — <sub>*HTTP methods, status codes, headers, content types, caching, HTTP/1.1, HTTP/2, HTTP/3 ve HTTPS.
- ⏳ **TLS & Certificate Validation** — <sub>*TLS handshake, X.509 certificates, Certificate Authorities, SNI, certificate chains ve renewal süreçleri.
- ⏳ **Cookies, Sessions & Authentication** — <sub>*Cookies, session management, authentication state, secure cookie attributes ve session security.
- ⏳ **REST API Fundamentals** — <sub>*Resources, endpoints, HTTP methods, idempotency, pagination, versioning, rate limiting ve API error handling.
- ⏳ **Tokens, JWT & OAuth 2.0** — <sub>*Access tokens, refresh tokens, JWT yapısı, OAuth 2.0 akışları ve OpenID Connect temelleri.
- ⏳ **Browser & API Security** — <sub>*CORS, CSRF, XSS, Host header risks, input validation, authentication ve authorization ayrımı.
- ⏳ **Relational Database Fundamentals** — <sub>*PostgreSQL ve MySQL genel mimarisi, schemas, tables, primary/foreign keys ve normalization.
- ⏳ **Transactions & Concurrency** — <sub>*ACID, transaction isolation levels, locks, deadlocks ve consistency kavramları.
- ⏳ **Indexes & Query Performance** — <sub>*Index yapıları, query plans, full-table scans, latency ve temel query optimization.
- ⏳ **Database Availability & Protection** — <sub>*Replication, backup, restore testing, encryption, access control ve credential management.
- ⏳ **Caching & In-Memory Data Stores** — <sub>*Redis temelleri, cache-aside yaklaşımı, expiration, persistence ve cache invalidation problemleri.

---

## 📦 Module 05: Container Technologies — Docker Deep Dive — ⏳ Planned

- ⏳ **Containerization vs Virtualization** — Hypervisor tabanlı virtual machines ile operating-system-level containers arasındaki mimari farklar.
- ⏳ **Linux Kernel Namespaces & Cgroups** — Container isolation ve resource-control mekanizmalarının Linux kernel içindeki temelleri.
- ⏳ **Container Runtime Internals** — Docker Engine, containerd, runc, OCI specifications ve Kubernetes Container Runtime Interface ilişkisi.
- ⏳ **Docker Core** — Docker CLI, image ve container lifecycle, Dockerfile yazım kuralları, layers, build cache ve multi-stage builds.
- ⏳ **Docker Storage** — Writable layers, volumes, bind mounts, tmpfs, UID/GID sorunları ve veri kalıcılığı.
- ⏳ **Docker Networking** — Network namespaces, veth pairs, bridge networks, port publishing, NAT, container DNS ve service discovery.
- ⏳ **Docker Compose** — Çok konteynerli yerel geliştirme, test ve doğrulama ortamlarının tanımlanması.
- ⏳ **Registries & Image Lifecycle** — Docker Hub, GHCR, Amazon ECR, tags, digests, retention, signing ve image provenance.
- ⏳ **Container Security & Image Hardening** — Non-root containers, reduced capabilities, read-only filesystems, seccomp, minimal images ve Trivy/Grype taramaları.
- ⏳ **Docker Operations & Troubleshooting** — Logs, inspect, events, health checks, networking, storage, OOM ve safe-cleanup yaklaşımları.

---

## ☸️ Module 06: Kubernetes Orchestration & Platform Fundamentals — ⏳ Planned

- ⏳ **Kubernetes Architecture** — API Server, etcd, Scheduler, kube-controller-manager, cloud-controller-manager, kubelet, kube-proxy ve container runtime görevleri.
- ⏳ **Kubernetes Core Objects** — Pods, ReplicaSets, Deployments, StatefulSets, DaemonSets, Jobs ve CronJobs.
- ⏳ **Scheduling & Resources** — Requests, limits, QoS classes, affinity, anti-affinity, taints, tolerations ve pod placement.
- ⏳ **Services & Service Discovery** — ClusterIP, NodePort, LoadBalancer Services, EndpointSlices, kube-proxy ve CoreDNS.
- ⏳ **Configuration & Secrets** — ConfigMaps, Secrets, environment variables, mounted volumes ve Kubernetes Secrets nesnelerinin güvenlik sınırlamaları.
- ⏳ **Kubernetes Storage** — PersistentVolumes, PersistentVolumeClaims, StorageClasses, dynamic provisioning, access modes ve backup considerations.
- ⏳ **Kubernetes Security & RBAC** — ServiceAccounts, Roles, ClusterRoles, RoleBindings, Security Contexts, Pod Security Standards ve least privilege.
- ⏳ **Traffic Management** — Ingress Controllers, Gateway API, NetworkPolicy, service mesh, mTLS, retries ve circuit breaking.
- ⏳ **Workload Health & Recovery** — Startup, readiness ve liveness probes; restart behavior ve controller'ların desired state'i koruma yaklaşımı.
- ⏳ **Scaling & Reliability** — HPA, VPA kavramları, Cluster Autoscaler, PodDisruptionBudget, graceful shutdown ve controlled rollout stratejileri.
- ⏳ **Cluster Lifecycle & Packaging** — kubeadm, managed Kubernetes, upgrades, Helm, Kustomize, certificates ve etcd backup temelleri.
- ⏳ **Kubernetes Observability & Troubleshooting** — Events, logs, Metrics Server, Prometheus, CrashLoopBackOff, ImagePullBackOff, DNS ve volume failures.
- ⏳ **GitOps & Declarative Delivery** — Kubernetes kaynaklarının Argo CD veya Flux ile deklaratif biçimde yönetilmesi.

---

## ⚙️ Module 07: Infrastructure as Code & Configuration Management — ⏳ Planned

- ⏳ **Infrastructure as Code Fundamentals** — Declarative ve imperative yaklaşımlar, desired state, idempotency, drift, convergence ve repeatability.
- ⏳ **Terraform Language & Workflow** — Providers, resources, data sources, HCL, dependencies, `init`, `plan`, `apply` ve `destroy`.
- ⏳ **Terraform Variables & Data Modeling** — Variables, outputs, locals, lists, maps, objects, conditionals, `count` ve `for_each`.
- ⏳ **Terraform Modules** — Root ve child modules, reusable architecture, module boundaries, version pinning ve environment separation.
- ⏳ **Terraform State Security** — `terraform.tfstate` güvenliği, Amazon S3 remote state, native S3 state locking, encryption, versioning ve restricted IAM access.
- ⏳ **Legacy State Locking Considerations** — DynamoDB tabanlı locking yönteminin mevcut eski yapıların geçişi veya uyumluluk ihtiyaçları kapsamında değerlendirilmesi.
- ⏳ **Terraform Lifecycle & Testing** — Lifecycle rules, import, moved blocks, static analysis, Policy as Code, security scanning ve destructive-change protection.
- ⏳ **Ansible Fundamentals** — Agentless architecture, SSH connections, inventories, modules, facts, ad-hoc commands ve privilege escalation.
- ⏳ **Ansible Playbooks & Roles** — Playbooks, handlers, templates, variables, loops, conditionals, roles ve idempotent service management.
- ⏳ **Ansible Vault & Operational Safety** — Hassas değerlerin şifrelenmesi, secret injection, least privilege, serial changes ve rollback limitations.
- ⏳ **Git & Version Control Integration** — Commit disiplini, pull requests, protected branches, code review, GitFlow ve trunk-based development.
- ⏳ **Infrastructure Validation** — Formatting, validation, linting, policy checks, controlled plan/apply ve CI/CD entegrasyonu.
- ⏳ **Terraform & Ansible Integration** — Provisioning ve configuration-management sorumluluklarının ayrılması, dynamic inventory ve doğrulama süreçleri.

---

# 🚀 Level 3: Software Engineering, Automation, CI/CD & Observability — ⏳ Planned

### Yazılım Teslimi, Otomasyon ve Gözlemlenebilirlik

Altyapı ve platform yazılımlarının sürdürülebilir biçimde geliştirildiği; teslim süreçlerinin otomatikleştirildiği ve sistemlerin metrics, logs ve traces kullanılarak gözlemlendiği seviyedir.

---

## 🔄 Module 08: CI/CD Pipelines & GitOps — ⏳ Planned

- ⏳ **CI/CD Core Concepts** — Continuous Integration, Continuous Delivery ve Continuous Deployment arasındaki farklar.
- ⏳ **Git Workflows & Pipeline Triggers** — Branching strategies, pull requests, protected branches, tags, releases ve event-based triggers.
- ⏳ **Pipeline Platforms** — GitHub Actions veya GitLab CI ile workflows, jobs, runners, stages, artifacts ve environment yönetimi.
- ⏳ **Build, Test & Artifact Pipelines** — Unit, integration ve end-to-end tests, caching, matrices, reports ve reproducible builds.
- ⏳ **Container Build & Delivery** — Docker/BuildKit, image tagging, commit SHA kullanımı, registry authentication, scanning, signing ve provenance.
- ⏳ **DevSecOps & Security Gates** — SonarQube, Gitleaks, TruffleHog, Trivy, SAST, SCA, IaC scanning ve controlled policy enforcement.
- ⏳ **Artifact Management** — Docker Hub, GitHub Container Registry, Amazon ECR ve JFrog Artifactory gibi registry ve artifact-repository çözümleri.
- ⏳ **Secure Pipeline Authentication** — Uzun ömürlü cloud credentials yerine OpenID Connect ve kısa ömürlü kimlik bilgilerinin kullanılması.
- ⏳ **Software Supply-Chain Security** — SBOM, dependency trust, signing, Sigstore, SLSA ve artifact-integrity yaklaşımı.
- ⏳ **Deployment Strategies** — Rolling update, blue/green, canary, feature flags, progressive delivery ve rollback yaklaşımları.
- ⏳ **GitOps Delivery** — Application code ile deployment state'inin ayrılması, declarative manifests, reconciliation ve drift correction.
- ⏳ **Pipeline Observability & Troubleshooting** — Pipeline metrics, runner utilization, flaky tests, failures, retries, concurrency ve runbooks.

---

## 🐍 Module 09: Systems Automation & Scripting — Bash & Python — ⏳ Planned

- ⏳ **Bash Scripting Fundamentals** — Variables, conditionals, loops, functions, arguments, arrays, exit codes ve debugging.
- ⏳ **Shell Safety Practices** — Quoting, input validation, temporary files, cleanup traps ve `set -Eeuo pipefail` kullanımının bağlama bağlı değerlendirilmesi.
- ⏳ **Regular Expressions & Text Processing** — Regex, `grep`, `sed`, `awk`, `cut`, `sort`, `uniq`, `tr`, `find`, `xargs`, `jq` ve `yq`.
- ⏳ **System Administration Automation** — Backup, restore validation, disk monitoring, log rotation, service-health checks ve raporlama.
- ⏳ **Python Fundamentals for Operations** — Data types, functions, exceptions, virtual environments, type hints, logging ve configuration.
- ⏳ **Python for Filesystems & Processes** — `os`, `pathlib`, `sys`, `shutil`, `subprocess`, signals, permissions ve safe command execution.
- ⏳ **API Automation** — Authentication, pagination, timeout, retry, exponential backoff, rate limiting ve schema validation.
- ⏳ **Network & Security Automation** — DNS checks, TCP/UDP connectivity, certificate inspection, log parsing, firewall validation ve alert enrichment.
- ⏳ **Secure Automation** — Secrets yönetimi, least privilege, auditability, idempotency ve destructive-action protections.
- ⏳ **Automation Quality & Packaging** — Unit tests, mocking, linting, type checking, dependency pinning, CLI design ve CI integration.

---

## 💻 Module 10: Software Engineering for Infrastructure & Platform Teams — ⏳ Planned

- ⏳ **Software Engineering Fundamentals** — Requirements, maintainability, readability, testability ve technical-debt yönetimi.
- ⏳ **Clean Code Principles** — Naming, small functions, separation of concerns, error handling ve understandable abstractions.
- ⏳ **SOLID Principles** — Single Responsibility, Open/Closed, Liskov Substitution, Interface Segregation ve Dependency Inversion.
- ⏳ **Design Patterns** — Factory, Strategy, Adapter, Observer, Dependency Injection ve infrastructure automation kullanım örnekleri.
- ⏳ **Testing Strategies** — Unit, integration, end-to-end, contract ve smoke tests; mocks, stubs ve test doubles.
- ⏳ **Refactoring & Code Review** — Safe refactoring, code smells, pull-request reviews ve incremental improvement.
- ⏳ **Application Architecture** — Monolith, modular monolith, microservices, event-driven architecture ve architecture trade-offs.
- ⏳ **Distributed-System Fundamentals** — Partial failures, timeouts, retries, idempotency, consistency ve eventual consistency.
- ⏳ **API & Service Design** — Versioning, backward compatibility, error contracts, rate limits ve graceful degradation.
- ⏳ **Configuration & Dependency Management** — Configuration separation, feature flags, dependency pinning ve environment consistency.
- ⏳ **Secure Software Development** — Input validation, secrets, dependency riskleri, threat modeling ve secure defaults.
- ⏳ **Engineering Documentation** — Architecture Decision Records, diagrams, runbooks, API documentation ve operational handover.

---

## 📊 Module 11: Observability, Logging & Monitoring — ⏳ Planned

- ⏳ **Observability Fundamentals** — Metrics, logs, traces, events ve profiles arasındaki farklar ve birlikte kullanım biçimleri.
- ⏳ **Monitoring Approaches** — White-box, black-box, synthetic monitoring, health checks ve baselines.
- ⏳ **The Four Golden Signals** — Latency, traffic, errors ve saturation.
- ⏳ **RED & USE Methods** — Request-driven services ve infrastructure resources için gözlem yöntemleri.
- ⏳ **Metrics & Prometheus** — Prometheus architecture, service discovery, scraping, exporters, PromQL, recording ve alerting rules.
- ⏳ **Grafana & Dashboard Engineering** — Dashboards, panels, variables, annotations, thresholds, provisioning ve dashboard anti-patterns.
- ⏳ **Centralized Logging** — Elasticsearch tabanlı çözümler veya Grafana Loki ile log aggregation, parsing, retention ve analysis.
- ⏳ **Distributed Tracing** — OpenTelemetry instrumentation ve Collector; Jaeger veya Grafana Tempo gibi tracing backend'leri.
- ⏳ **Alerting & Notification Engineering** — Prometheus Alertmanager ile routing, grouping, deduplication, inhibition, silences ve escalation.
- ⏳ **SLI, SLO & Reliability Monitoring** — Availability, latency, error budgets, burn rate ve SLO-based alerting.
- ⏳ **Platform & Kubernetes Observability** — Host, container, node, pod, workload ve control-plane telemetry.
- ⏳ **Security Observability** — Authentication, authorization, audit, network, DNS, firewall ve cloud-security telemetry.
- ⏳ **Operational Reliability** — Actionable alerts, alert fatigue, missing telemetry, runbooks ve incident-response bağlantısı.
- ⏳ **Observability Troubleshooting** — Scrape failures, missing logs, trace gaps, cardinality explosions, storage ve query-performance sorunları.

---

# ☁️ Level 4: Cloud Computing, Reliability & Intelligent Operations — ⏳ Planned

### Bulut, Dayanıklı Sistemler ve Akıllı Operasyon

Yüksek erişilebilirlik, dayanıklılık, ölçeklenebilirlik, güvenli erişim, performans, felaket kurtarma ve yapay zekâ destekli mühendislik ilkelerine dayalı sistemlerin ele alındığı seviyedir.

---

## ☁️ Module 12: Cloud Computing — AWS Focus — ⏳ Planned

- ⏳ **Cloud Computing Foundations** — On-premises ve cloud farkları, IaaS, PaaS, SaaS, elasticity, scalability ve Shared Responsibility Model.
- ⏳ **AWS Global Infrastructure** — Regions, Availability Zones, edge locations, regional ve global services.
- ⏳ **Identity & Access Management** — IAM users, groups, roles, policies, trust policies, STS ve Principle of Least Privilege.
- ⏳ **AWS Networking & VPC Architecture** — VPC, CIDR planning, public/private subnets, route tables, Internet Gateway, NAT Gateway ve VPC endpoints.
- ⏳ **Network Security** — Security Groups, Network ACLs, VPC Flow Logs, segmentation ve egress-control yaklaşımları.
- ⏳ **Compute Services** — Amazon EC2, AMIs, launch templates, user data, metadata service, Auto Scaling Groups ve instance lifecycle.
- ⏳ **Load Balancing** — Application Load Balancer, Network Load Balancer, listeners, target groups, health checks ve TLS.
- ⏳ **Storage Services** — Amazon S3, EBS ve EFS arasındaki kullanım, availability, durability, performance ve cost trade-off'ları.
- ⏳ **Managed Databases** — Amazon RDS, Aurora, DynamoDB ve ElastiCache genel mimarileri.
- ⏳ **Serverless & Event-Driven Architecture** — AWS Lambda, Amazon SQS, SNS, EventBridge, retries, DLQs ve idempotency.
- ⏳ **Secrets & Key Management** — AWS Secrets Manager, Systems Manager Parameter Store, AWS KMS ve envelope encryption.
- ⏳ **Cloud Security Services** — CloudTrail, Config, GuardDuty, Security Hub, Inspector, WAF, Shield ve Macie temelleri.
- ⏳ **Monitoring & Auditing** — CloudWatch metrics, logs, alarms, dashboards, CloudTrail events ve centralized logging.
- ⏳ **High Availability & Disaster Recovery on AWS** — Multi-AZ, Multi-Region, backup, snapshots, replicas, Route 53 failover, RTO ve RPO.
- ⏳ **Governance & Cost Management** — AWS Organizations, OUs, SCPs, tagging, budgets, Cost Explorer, quotas ve guardrails.
- ⏳ **AWS & Infrastructure as Code Integration** — Terraform, remote state, OIDC identities, environment promotion ve drift detection.

---

## 🏛️ Module 13: Site Reliability Engineering & High Availability — ⏳ Planned

- ⏳ **SRE Foundations & Service Ownership** — SRE ve DevOps ilişkisi, service boundaries, ownership, operational readiness ve reliability as a product feature.
- ⏳ **SLI, SLO, SLA & Error Budgets** — Service indicators, objectives, external commitments, measurement windows ve burn rate.
- ⏳ **Toil Reduction & Reliability Automation** — Repetitive operational work, safe automation, self-service, guardrails ve human-in-the-loop.
- ⏳ **Monitoring, Alerting & On-Call** — Golden Signals, actionable alerts, severity, routing, escalation, handoffs ve on-call health.
- ⏳ **Incident Management** — Incident detection, declaration, triage, Incident Commander, stabilization, communication ve recovery.
- ⏳ **Blameless Postmortems** — Timeline, impact, contributing factors, root-cause analysis, corrective actions ve learning culture.
- ⏳ **Resilience Patterns** — Timeouts, retries, exponential backoff, jitter, circuit breakers, bulkheads, rate limiting ve load shedding.
- ⏳ **High Availability Architectures** — Redundancy, single points of failure, active-passive, active-active, quorum, replication ve DNS failover.
- ⏳ **Disaster Recovery & Business Continuity** — RTO, RPO, backup/restore, Pilot Light, Warm Standby, Active-Active ve recovery testing.
- ⏳ **Chaos Engineering** — Yetkili ortamlarda kontrollü, hipotez tabanlı failure experiments, abort conditions ve blast-radius management.
- ⏳ **Release Engineering & Change Reliability** — Rolling, blue/green, canary, feature flags, migration safety ve rollback.
- ⏳ **Operational Readiness** — Runbooks, dashboards, alerts, deployment validation, capacity ownership ve recovery plans.

---

## ⚡ Module 14: Performance Engineering & Capacity Analysis — ⏳ Planned

- ⏳ **Performance Engineering Fundamentals** — Latency, throughput, concurrency, utilization, saturation ve bottleneck kavramları.
- ⏳ **CPU Performance Analysis** — CPU utilization, load average, run queues, context switches, user/system time ve CPU profiling.
- ⏳ **Memory Performance Analysis** — Memory allocation, cache, swap, page faults, memory pressure, leaks ve OOM behavior.
- ⏳ **Storage & I/O Performance** — IOPS, throughput, latency, queue depth, filesystem overhead ve disk saturation.
- ⏳ **Network Performance Analysis** — Bandwidth, packet loss, jitter, retransmissions, connection limits ve socket queues.
- ⏳ **Application Profiling** — Hot paths, function-level profiling, memory allocation, lock contention ve flame graphs.
- ⏳ **Database Performance** — Query latency, indexes, execution plans, connection pools, locks ve slow-query analysis.
- ⏳ **Load, Stress & Soak Testing** — Normal load, peak load, breaking points, long-duration tests ve test-environment limitations.
- ⏳ **Benchmarking Methodology** — Baselines, controlled experiments, repeatability, warm-up effects ve misleading benchmark risks.
- ⏳ **Capacity Planning** — Growth forecasting, resource headroom, seasonality, quotas ve scaling decisions.
- ⏳ **Backpressure & Queue Management** — Queue depth, concurrency limits, rate limiting ve overload behavior.
- ⏳ **Performance Troubleshooting** — Metrics, logs, traces ve profiles kullanılarak katmanlı bottleneck analizi.
- ⏳ **Performance vs Cost Trade-offs** — Vertical ve horizontal scaling, overprovisioning, efficiency ve cloud-cost etkileri.

---

## 🤖 Module 15: AI-Assisted Engineering & Operational Intelligence — ⏳ Planned

- ⏳ **AI-Assisted Engineering Fundamentals** — Yapay zekâ araçlarının yazılım, sistem, cloud ve güvenlik mühendisliğinde doğru kullanım alanları.
- ⏳ **Prompt Design for Engineers** — Gereksinim, bağlam, kısıt, doğrulama kriteri ve beklenen çıktı formatının açık tanımlanması.
- ⏳ **AI-Assisted Code Generation** — Bash, Python, Terraform, Ansible ve Kubernetes manifest taslaklarının kontrollü üretilmesi.
- ⏳ **AI-Assisted Code Review** — Logic errors, security risks, maintainability issues, race conditions ve missing error handling kontrolleri.
- ⏳ **Infrastructure Review** — Terraform plans, IAM policies, Kubernetes manifests ve network rules için AI destekli inceleme.
- ⏳ **Log & Incident Analysis** — Log özetleme, timeline oluşturma, correlation, hypothesis generation ve incident triage desteği.
- ⏳ **AI-Assisted Troubleshooting** — Hata mesajları, metrics, traces ve system outputs üzerinden olası nedenlerin önceliklendirilmesi.
- ⏳ **Security Analysis & Threat Hunting** — Detection-rule taslakları, IOC enrichment, suspicious-pattern analysis ve false-positive değerlendirmesi.
- ⏳ **Documentation & Knowledge Management** — README, runbook, postmortem, architecture documentation ve technical-summary üretimi.
- ⏳ **Output Verification** — AI tarafından üretilen komut, kod, policy ve teknik iddiaların bağımsız olarak doğrulanması.
- ⏳ **Hallucination & Context Risks** — Uydurulmuş komutlar, eski bilgiler, yanlış varsayımlar ve eksik bağlam nedeniyle oluşabilecek riskler.
- ⏳ **Sensitive Data Protection** — Credentials, logs, source code, personal data ve şirket bilgilerinin AI araçlarına aktarılma riskleri.
- ⏳ **Human Approval & Safety Boundaries** — Production değişiklikleri, destructive commands, security decisions ve automated actions için insan onayı.
- ⏳ **AI-Augmented Operations** — Alert enrichment, anomaly summarization, runbook suggestions ve controlled remediation yaklaşımları.
- ⏳ **Responsible AI Use** — Privacy, auditability, intellectual property, bias, accountability ve organizational-policy gereksinimleri.

---

# 🤖 Level 5: AI Systems Security, MLSecOps & Governance — ⏳ Planned

### Yapay Zekâ Sistemleri Güvenliği, Güvenli Model Yaşam Döngüsü ve AI Yönetişimi

Yapay zekâ ve makine öğrenmesi sistemlerinin mimarisinin, tehdit yüzeylerinin ve yaşam döngüsünün incelendiği; LLM, RAG ve agent tabanlı uygulamaların güvenli biçimde geliştirildiği, dağıtıldığı, izlendiği ve yönetildiği ileri seviye uzmanlık alanıdır.

## 🧠 Module 16: AI, Machine Learning & LLM Foundations for Security Engineers — ⏳ Planned

⏳ AI & Machine Learning Foundations — Artificial Intelligence, Machine Learning, Deep Learning ve Generative AI arasındaki farklar; supervised, unsupervised, reinforcement ve self-supervised learning yaklaşımları.
⏳ AI System Lifecycle — Veri toplama, veri hazırlama, training, validation, testing, fine-tuning, deployment, inference, monitoring, retraining ve retirement aşamaları.
⏳ Model Architecture Fundamentals — Neural networks, parameters, weights, layers, loss functions, optimization, checkpoints ve model formats kavramları.
⏳ Large Language Model Fundamentals — Transformer mimarisi, attention, tokens, tokenization, context window, temperature, sampling ve hallucination kavramları.
⏳ Embeddings & Semantic Search — Embedding modelleri, similarity search, vector representations, indexing ve semantic-retrieval çalışma mantığı.
⏳ Retrieval-Augmented Generation — RAG ingestion, chunking, embedding, retrieval, reranking, context construction ve generation akışı.
⏳ Vector Databases — Vector indexes, metadata filtering, namespaces, document permissions, multi-tenancy ve vector-store erişim kontrolü.
⏳ AI Agents & Tool Use — Planning, reasoning loops, memory, tool calling, function calling, external APIs ve autonomous-action riskleri.
⏳ Model Context Protocol Fundamentals — MCP servers, clients, tools, resources, prompts, trust boundaries ve third-party integration riskleri.
⏳ Training, Fine-Tuning & Inference — Pre-training, supervised fine-tuning, instruction tuning, adapters, LoRA, quantization ve inference-serving farkları.
⏳ AI Security Terminology — Model, dataset, prompt, system prompt, guardrail, agent, inference endpoint, model registry ve evaluation kavramları.

---

## 🧭 Module 17: AI Threat Modeling & Security Architecture — ⏳ Planned

⏳ AI System Asset Identification — Models, datasets, prompts, embeddings, vector stores, APIs, agents, tools, credentials, logs ve model artifacts gibi korunması gereken varlıkların belirlenmesi.
⏳ AI Data-Flow Diagrams — Kullanıcı, uygulama, model provider, RAG pipeline, agent tools, databases ve external services arasındaki veri akışlarının çıkarılması.
⏳ Trust Boundaries — User input, retrieved content, model output, agent memory, tool responses ve third-party models arasındaki güven sınırlarının belirlenmesi.
⏳ AI Attack-Surface Analysis — Training, supply chain, model storage, inference API, application, RAG, agent ve monitoring katmanlarının saldırı yüzeyleri.
⏳ Threat Actors & Capabilities — External attackers, malicious users, insiders, compromised suppliers ve automated-abuse actors.
⏳ AI Misuse & Abuse Cases — Modelin yetkisiz, zararlı, maliyet artırıcı veya kurum politikasına aykırı amaçlarla kullanılması.
⏳ Threat Modeling Methods — STRIDE, attack trees, abuse cases ve AI sistemlerine uyarlanmış threat-modeling yaklaşımları.
⏳ OWASP LLM & Generative AI Risks — LLM uygulamalarına özgü güvenlik risklerinin mimari ve uygulama seviyesinde değerlendirilmesi.
⏳ MITRE ATLAS — Predictive AI, Generative AI ve Agentic AI sistemlerine yönelik saldırı taktikleri, teknikleri ve mitigations.
⏳ NIST AI Risk Management Framework — Govern, Map, Measure ve Manage fonksiyonlarının AI güvenlik risklerine uygulanması.
⏳ Security Requirements Engineering — Authentication, authorization, confidentiality, integrity, availability, privacy, auditability ve human-oversight gereksinimleri.
⏳ AI Security Architecture Patterns — Isolation, least privilege, defense in depth, zero trust, secure gateways ve controlled tool execution.
⏳ AI Risk Register — Risk owner, likelihood, impact, control, residual risk, acceptance ve remediation tracking.

 ---
 
## 🔐 Module 18: LLM, RAG & Agentic Application Security — ⏳ Planned

⏳ Direct Prompt Injection — Kullanıcının model davranışını veya sistem talimatlarını doğrudan değiştirmeye çalıştığı saldırılar.
⏳ Indirect Prompt Injection — Web pages, documents, emails, retrieved content veya external tool outputs içine yerleştirilmiş kötü amaçlı talimatlar.
⏳ System-Prompt Extraction — System prompt, hidden instructions, internal policies ve application logic bilgilerinin model üzerinden elde edilmesi.
⏳ Jailbreak Attacks — Modelin güvenlik ve kullanım sınırlamalarının crafted prompts, encoding, role-play veya multi-step interactions ile aşılması.
⏳ Sensitive Information Disclosure — Credentials, personal data, proprietary data, internal documents ve confidential context bilgilerinin model çıktılarında açığa çıkması.
⏳ Insecure Output Handling — Model çıktılarının doğrulanmadan shell, database, browser, HTML renderer veya downstream application tarafından çalıştırılması.
⏳ Excessive Agency — Agentlara gereğinden fazla sistem, dosya, network, database veya cloud yetkisi verilmesi.
⏳ Agent Identity & Authorization — Kullanıcı kimliği, agent identity, delegated permissions, service accounts ve end-to-end authorization kontrolleri.
⏳ Tool-Calling Security — Tool allowlists, parameter validation, schema enforcement, confirmation gates ve destructive-action restrictions.
⏳ MCP Server & Tool Security — Malicious MCP servers, tool impersonation, poisoned tool descriptions, overprivileged tools ve untrusted resources.
⏳ Agent Memory Security — Persistent memory, conversation history, long-term state ve stored context bilgilerinin zehirlenmesi veya sızdırılması.
⏳ Context Poisoning — Model context'inin yanlış, kötü amaçlı veya yanıltıcı bilgilerle manipüle edilmesi.
⏳ RAG Poisoning — Knowledge base, document store veya retrieved content içine manipüle edilmiş içerik yerleştirilmesi.
⏳ Retrieval Authorization — Kullanıcının yalnızca erişim yetkisi bulunan dokümanların RAG pipeline tarafından getirilmesi.
⏳ Vector Database Security — Authentication, encryption, tenant isolation, metadata authorization, backup ve audit logging.
⏳ Cross-Tenant Data Leakage — Çok kiracılı sistemlerde kullanıcı veya kurum verilerinin başka tenant'lara sızması.
⏳ SSRF & External-Resource Abuse — Agent veya tool mekanizmalarının internal services, metadata endpoints veya yetkisiz URL'lere erişmek için kullanılması.
⏳ Code-Execution & Sandbox Risks — Model tarafından üretilen kodun kontrollü olmayan ortamlarda çalıştırılması ve sandbox escape riskleri.
⏳ Output Validation & Policy Enforcement — Structured output, schema validation, content filtering, allowlists ve deterministic security controls.
⏳ Human-in-the-Loop Controls — Para transferi, veri silme, production değişikliği, email gönderme ve credential işlemlerinde insan onayı.
⏳ Rate Limiting & Cost Controls — Token, request, concurrency, tool-call ve spending limitleriyle abuse ve denial-of-wallet risklerinin azaltılması.
⏳ Model API Security — Authentication, authorization, TLS, request validation, quotas, logging ve abuse prevention.
⏳ Secure Conversation Design — Session isolation, context boundaries, history management ve conversation-state güvenliği.

---

## 🧬 Module 19: Adversarial Machine Learning & Model Security — ⏳ Planned

⏳ Adversarial ML Foundations — Model davranışının kötü amaçlı girdiler, training data veya model artifacts üzerinden manipüle edilmesi.
⏳ Evasion Attacks — Model inference sırasında girdilerin sınıflandırma veya karar mekanizmasını yanıltacak biçimde değiştirilmesi.
⏳ Adversarial Examples — İnsan için küçük görünen ancak model sonucunu önemli ölçüde değiştiren crafted inputs.
⏳ Training-Data Poisoning — Training veya fine-tuning verilerinin model davranışını bozmak amacıyla manipüle edilmesi.
⏳ Backdoor & Trojan Attacks — Belirli trigger girdilerinde saldırganın istediği sonucu üreten gizli model davranışları.
⏳ Fine-Tuning Attacks — Zararlı dataset, adapter veya fine-tuning süreçleri üzerinden model davranışının değiştirilmesi.
⏳ Model Extraction — Query-response interactions üzerinden hedef modelin davranışının veya işlevinin kopyalanması.
⏳ Model Inversion — Model outputs veya gradients üzerinden training data özelliklerinin yeniden oluşturulmaya çalışılması.
⏳ Membership Inference — Belirli bir verinin modelin training dataset'i içinde bulunup bulunmadığının tahmin edilmesi.
⏳ Model Fingerprinting — Model family, version, architecture veya provider bilgilerinin davranışsal yöntemlerle belirlenmesi.
⏳ Model Theft — Model weights, checkpoints, adapters, architecture bilgileri veya private endpoints'in yetkisiz biçimde ele geçirilmesi.
⏳ Privacy Attacks — Training data memorization, personal-data leakage ve reconstruction saldırıları.
⏳ Model Integrity Verification — Hashes, digital signatures, artifact validation ve trusted model-source kontrolleri.
⏳ Robustness Evaluation — Modelin adversarial, noisy, out-of-distribution ve manipulated inputs karşısındaki dayanıklılığının ölçülmesi.
⏳ Mitigation Limitations — Adversarial training, filtering, detection ve guardrail yöntemlerinin sınırlamalarının değerlendirilmesi.

---

## 🗃️ Module 20: AI Data Security, Privacy & Knowledge Protection — ⏳ Planned

⏳ AI Data Classification — Public, internal, confidential, restricted, personal ve regulated data türlerinin sınıflandırılması.
⏳ Dataset Inventory & Ownership — Dataset owner, source, purpose, access, retention ve approved-use bilgilerinin kayıt altına alınması.
⏳ Data Lineage & Provenance — Verinin kaynağı, dönüşümleri, training süreçleri ve model üzerindeki kullanımının izlenebilir olması.
⏳ Data Quality & Integrity — Eksik, yanlış, manipüle edilmiş, outdated veya malicious data'nın tespit edilmesi.
⏳ Training-Data Access Control — Dataset storage, notebooks, pipelines ve processing systems için least-privilege erişim.
⏳ Secrets & Credential Detection — API keys, passwords, certificates, tokens ve connection strings'in dataset, prompt veya notebook içinde bulunmasının engellenmesi.
⏳ Personal-Data Protection — PII detection, masking, anonymization, pseudonymization ve data minimization.
⏳ Encryption — Data at rest, data in transit, model artifacts, embeddings, backups ve logs için şifreleme.
⏳ RAG Knowledge-Base Protection — Document permissions, ingestion approval, integrity validation ve source trust değerlendirmesi.
⏳ Prompt & Response Privacy — Kullanıcı promptları, model outputs, feedback verileri ve conversation history'nin güvenli saklanması.
⏳ Logging & Retention Controls — Prompt, response ve tool-call loglarının içerik, erişim, saklama süresi ve silme politikaları.
⏳ Tenant Isolation — Dataset, vector namespace, memory ve retrieval context'in tenant bazında ayrılması.
⏳ Data-Poisoning Detection — Anomalous records, duplicate content, malicious instructions ve unexpected distribution değişimlerinin tespiti.
⏳ Data Usage Rights — Dataset licensing, consent, intellectual property ve permitted-use gereksinimleri.
⏳ Privacy-Enhancing Technologies — Differential privacy, federated learning, secure aggregation ve confidential-computing kavramları.
⏳ Secure Data Deletion — Dataset, embeddings, caches, model artifacts ve backups içindeki verilerin lifecycle'a uygun silinmesi.

---

## 🖥️ Module 21: Secure AI Infrastructure & Model Serving — ⏳ Planned

⏳ AI Infrastructure Architecture — Model gateway, inference server, vector database, object storage, agent runtime, API ve monitoring bileşenleri.
⏳ GPU & Accelerator Fundamentals — GPU resources, drivers, CUDA ecosystem, device access ve accelerator-sharing kavramları.
⏳ GPU Workload Security — GPU device permissions, workload isolation, driver security, resource quotas ve multi-tenant accelerator riskleri.
⏳ Secure Model Serving — Model loading, inference endpoints, batching, caching, streaming ve request-isolation güvenliği.
⏳ Containerized AI Workloads — Non-root containers, minimal images, reduced capabilities, read-only filesystems ve image scanning.
⏳ Kubernetes for AI Security — GPU scheduling, ServiceAccounts, RBAC, Pod Security Standards, NetworkPolicy ve workload isolation.
⏳ Model-Serving Platforms — vLLM, Triton, KServe, Ray Serve veya benzeri serving çözümlerinin güvenlik mimarisi.
⏳ Identity & Workload Authentication — Users, services, agents, workloads ve external APIs için güçlü kimlik doğrulama.
⏳ Secrets & Key Management — Model API keys, database credentials, encryption keys ve signing keys'in güvenli yönetimi.
⏳ Network Segmentation — Training, inference, management, storage ve monitoring ağlarının ayrılması.
⏳ API Gateway Security — Authentication, authorization, request limits, schema validation, TLS termination ve audit logging.
⏳ Service-to-Service Security — Mutual TLS, workload identities, certificate rotation ve service-mesh kontrolleri.
⏳ Multi-Tenancy Isolation — Compute, memory, network, model, cache ve data katmanlarında tenant ayrımı.
⏳ Model Registry Security — Model versioning, access control, signing, approval workflows ve artifact immutability.
⏳ Artifact Storage Security — Model weights, checkpoints, adapters, tokenizer files ve configuration dosyalarının korunması.
⏳ Secure Sandboxing — Untrusted code execution, interpreters, browser tools ve file-processing workloads için izolasyon.
⏳ Availability & Resource Exhaustion — GPU, CPU, memory, token, queue ve concurrency tüketimine karşı koruma.
⏳ Denial-of-Service & Denial-of-Wallet — Yüksek maliyetli inference requests, long-context attacks ve automated API abuse.
⏳ AI Infrastructure Observability — Inference latency, token usage, errors, resource consumption, model loading ve tool-call telemetry.
⏳ Backup, Recovery & Resilience — Model registry, vector database, configuration, prompts ve AI-service dependencies için recovery planları.
⏳ Cloud AI Shared Responsibility — Managed model services, customer data, IAM, network, logging ve application-security sorumluluklarının ayrılması.

---

## 🔗 Module 22: MLSecOps, AI Supply Chain & Secure AI Development Lifecycle — ⏳ Planned

⏳ MLSecOps Foundations — Machine Learning, MLOps, DevSecOps ve AI Security kontrollerinin tek yaşam döngüsünde birleştirilmesi.
⏳ Secure AI SDLC — Requirements, design, development, training, testing, release, deployment, monitoring ve retirement süreçlerine güvenlik eklenmesi.
⏳ AI Pipeline Security — Data ingestion, feature engineering, training, evaluation, packaging ve deployment pipeline'larının korunması.
⏳ Secure Development Environments — Notebooks, IDEs, experiment platforms, build runners ve development credentials güvenliği.
⏳ Dataset Supply-Chain Security — Third-party datasets, provenance, licenses, integrity checks ve poisoning riskleri.
⏳ Model Supply-Chain Security — Public models, adapters, checkpoints ve pre-trained artifacts için source-trust değerlendirmesi.
⏳ Third-Party Model Risk — Model provider, hosting service, API, license, data usage ve security-assurance kontrolleri.
⏳ Dependency Security — Python packages, container images, ML libraries, CUDA dependencies ve transitive-dependency riskleri.
⏳ Unsafe Model Serialization — Pickle ve benzeri formats üzerinden arbitrary-code-execution riskleri ve güvenli format değerlendirmesi.
⏳ Artifact Signing & Verification — Model, dataset, container ve configuration artifacts için digital signatures, hashes ve provenance.
⏳ AI Bill of Materials — Model, dataset, library, container, provider ve service dependencies'in envanterinin oluşturulması.
⏳ Reproducibility — Dataset version, code commit, configuration, dependency, model checkpoint ve environment kayıtlarının tutulması.
⏳ Secure CI/CD for AI — Automated tests, security scanning, policy checks, approvals ve protected deployment environments.
⏳ Security Evaluation Gates — Model veya AI uygulaması production'a alınmadan önce zorunlu güvenlik ve risk testleri.
⏳ Model Registry Promotion — Development, testing, staging ve production modelleri arasında kontrollü promotion.
⏳ Canary & Shadow Deployment — Yeni model sürümlerinin sınırlı trafik veya shadow mode ile güvenli doğrulanması.
⏳ Rollback & Model Revocation — Güvensiz veya bozuk model sürümlerinin hızla geri alınması ve kullanımının engellenmesi.
⏳ Drift & Unauthorized Change Detection — Model, dataset, configuration ve infrastructure üzerinde beklenmeyen değişikliklerin tespiti.
⏳ Vulnerability & Patch Management — AI frameworks, serving platforms, drivers, dependencies ve containers için güncelleme süreçleri.
⏳ Vendor & Acquisition Security — Satın alınan veya dışarıdan kullanılan AI sistemlerinin security requirements ve assurance evidence ile değerlendirilmesi.

---

## 🧪 Module 23: AI Security Testing, Evaluation & Red Teaming — ⏳ Planned

⏳ AI Security Test Planning — System scope, assets, threat model, test objectives, rules of engagement ve success criteria.
⏳ Prompt Fuzzing — Farklı prompt structures, encodings, languages, obfuscation ve edge cases ile otomatik güvenlik testi.
⏳ Jailbreak Testing — Model policy ve guardrail sınırlarının yetkili test ortamlarında değerlendirilmesi.
⏳ Prompt-Injection Testing — Direct ve indirect injection senaryolarının application, RAG ve agent katmanlarında denenmesi.
⏳ RAG Security Testing — Malicious documents, poisoned context, unauthorized retrieval ve source-manipulation senaryoları.
⏳ Agentic Security Testing — Tool abuse, privilege escalation, unsafe planning, memory poisoning ve autonomous-action testleri.
⏳ MCP Security Testing — Malicious server, poisoned tool metadata, unauthorized resource ve tool substitution senaryoları.
⏳ Model Extraction Testing — Query limits, output precision, rate controls ve model-copying risklerinin değerlendirilmesi.
⏳ Privacy Leakage Testing — Memorized data, personal information, training-data exposure ve membership-inference senaryoları.
⏳ Adversarial Input Testing — Evasion, adversarial examples, malformed data ve multimodal-input manipülasyonları.
⏳ Multimodal Security Testing — Image, audio, video, document ve hidden-instruction içeriklerinin modele etkisi.
⏳ Tool & Output Exploitation — Model outputs üzerinden SQL injection, command injection, XSS, SSRF veya unsafe code execution riskleri.
⏳ Abuse & Cost Testing — High-token prompts, recursive agents, repeated tool calls ve resource-exhaustion senaryoları.
⏳ Automated AI Evaluations — Repeatable test suites, benchmark datasets, attack templates ve regression testing.
⏳ Human Red Teaming — Context-dependent, creative ve chained attack paths'in deneyimli test uzmanları tarafından incelenmesi.
⏳ Security vs Safety Evaluation — Siber güvenlik açıkları, kullanım politikası ihlalleri ve harmful-output risklerinin ayrı fakat ilişkili değerlendirilmesi.
⏳ Findings & Severity Assessment — Exploitability, business impact, affected assets, reproducibility ve remediation priority.
⏳ Remediation Verification — Uygulanan kontrollerin tekrar test edilmesi ve yeni bypass yöntemlerinin araştırılması.
⏳ Responsible Testing Boundaries — Yalnızca yetkili sistemlerde, kontrollü test verileri ve sınırlı blast radius ile çalışma.

---

## 📡 Module 24: AI Security Monitoring, Threat Detection & Incident Response — ⏳ Planned

⏳ AI Security Telemetry — Prompt, response, retrieval, model, tool-call, identity, authorization, network ve infrastructure olaylarının kaydı.
⏳ Secure AI Logging — Hassas veriyi gereksiz toplamadan olay araştırmasına yetecek seviyede audit trail oluşturulması.
⏳ Prompt & Response Monitoring — Injection patterns, data leakage, policy bypass ve suspicious-output davranışlarının tespiti.
⏳ Agent Activity Monitoring — Tool selection, parameter values, file access, network requests, external actions ve approval decisions.
⏳ RAG Monitoring — Retrieved documents, source trust, unusual retrieval patterns ve authorization failures.
⏳ Model-Behavior Monitoring — Unexpected outputs, integrity issues, safety-control degradation ve model-version farklılıkları.
⏳ Abuse Detection — Automated scraping, model extraction, credential testing, spam, fraud ve denial-of-wallet davranışları.
⏳ Drift Monitoring — Data drift, concept drift, performance drift ve security-control drift arasındaki farklar.
⏳ AI Security Metrics — Injection success rate, blocked actions, sensitive-data exposure, anomalous tool calls ve security-test regression results.
⏳ SIEM Integration — AI application, model gateway, cloud, Kubernetes ve identity logs'un merkezi güvenlik izleme sistemlerine aktarılması.
⏳ Detection Engineering — AI-specific detection rules, behavioral baselines, correlation rules ve alert tuning.
⏳ Threat Hunting with MITRE ATLAS — AI sistemlerine yönelik tactics ve techniques üzerinden hipotez tabanlı araştırma.
⏳ AI Incident Classification — Prompt injection, data leakage, compromised agent, poisoned RAG source, stolen model ve supply-chain compromise olayları.
⏳ AI Incident Triage — Etkilenen model, dataset, users, tools, credentials, infrastructure ve business processes'in belirlenmesi.
⏳ Containment Strategies — Agent tools'un kapatılması, model endpoint'in izole edilmesi, retrieval source'un kaldırılması ve access tokens'ın iptali.
⏳ Eradication & Recovery — Poisoned content'in temizlenmesi, model veya dataset'in güvenli sürümden yeniden yüklenmesi ve controls'un doğrulanması.
⏳ Model Rollback & Revocation — Güvensiz model, adapter veya configuration sürümünün kullanım dışı bırakılması.
⏳ Credential & Secret Rotation — Agent, service, model provider ve tool credentials bilgilerinin olay sonrasında yenilenmesi.
⏳ AI Forensics & Evidence Preservation — Prompts, outputs, retrieved content, tool executions, model versions ve configuration bilgilerinin korunması.
⏳ Communication & Reporting — Teknik ekipler, yönetim, hukuk, privacy ve business stakeholders için olay iletişimi.
⏳ Post-Incident Review — Timeline, root causes, control failures, lessons learned ve corrective actions.

---

## ⚖️ Module 25: AI Governance, Assurance, Standards & Compliance — ⏳ Planned

⏳ AI Governance Foundations — AI ownership, roles, responsibilities, decision rights ve organizational oversight.
⏳ NIST AI RMF — Govern, Map, Measure ve Manage fonksiyonlarıyla AI risk-management programı oluşturulması.
⏳ NIST Generative AI Profile — Generative AI sistemlerine özgü risklerin ve risk-treatment yöntemlerinin değerlendirilmesi.
⏳ NIST Secure AI Development Practices — AI model ve sistem geliştirme süreçlerinin Secure Software Development Framework ile uyumlu hâle getirilmesi.
⏳ ISO/IEC 42001 — Artificial Intelligence Management System kurulumu, policy, objectives, risk management ve continuous improvement.
⏳ ISO/IEC 23894 — AI risk-management principles, lifecycle risk identification ve risk-treatment yaklaşımı.
⏳ EU AI Act — Prohibited practices, risk classifications, high-risk systems, general-purpose AI ve provider/deployer obligations.
⏳ CSA AI Controls Matrix — Cloud tabanlı AI sistemleri için governance, security, privacy, supply chain ve operational controls.
⏳ OWASP & MITRE Mapping — OWASP AI risks ve MITRE ATLAS techniques'in security controls ve test cases ile eşleştirilmesi.
⏳ AI System Inventory — Kurum içinde geliştirilen, kullanılan veya third party üzerinden alınan AI sistemlerinin kayıt altına alınması.
⏳ AI Risk Classification — Use case, autonomy, data sensitivity, user impact, business criticality ve regulatory exposure değerlendirmesi.
⏳ AI Impact Assessments — Güvenlik, privacy, safety, fairness, human rights ve operational etkilerin sistem devreye alınmadan incelenmesi.
⏳ Model Cards & System Cards — Model purpose, limitations, training information, evaluation results, risks ve intended-use bilgilerinin dokümantasyonu.
⏳ Transparency & Explainability — Kullanıcı bilgilendirmesi, AI-generated content disclosure, model limitations ve decision explanations.
⏳ Human Oversight — Kritik karar ve eylemlerde insan kontrolü, intervention, override ve escalation mekanizmaları.
⏳ Accountability & Auditability — AI kararlarının, model versions'ın, approvals'ın ve configuration değişikliklerinin izlenebilir olması.
⏳ Third-Party Assurance — Model providers, datasets, AI APIs ve SaaS services için due diligence ve contractual security requirements.
⏳ Acceptable AI Use Policies — Çalışanların AI araçlarıyla hangi veri ve sistemleri kullanabileceğini belirleyen kurallar.
⏳ AI Exception & Risk Acceptance — Güvenlik kontrollerinden sapmaların business owner ve risk owner tarafından onaylanması.
⏳ Continuous Compliance — AI system, model, dataset, regulation ve control değişikliklerinin sürekli takip edilmesi.
⏳ Responsible AI Principles — Privacy, fairness, transparency, reliability, safety, security ve accountability gereksinimlerinin birlikte yönetilmesi.

---
## 🏗️ Level 5 — Capstone Projects
⏳ Project 1: Secure RAG Application — Authentication, document-level authorization, vector database isolation, prompt-injection protection, output validation, logging ve security testing içeren güvenli RAG uygulaması.
⏳ Project 2: Agentic AI Security Laboratory — Kontrollü sandbox içinde tool kullanan bir AI agent'ın kurulması; least privilege, approval gates, MCP security, memory isolation, monitoring ve red-team testleri.
⏳ Project 3: Secure AI Model-Serving Platform — Docker veya Kubernetes üzerinde model serving; API gateway, TLS, workload identity, secrets management, rate limiting, model registry, monitoring ve rollback.
⏳ Project 4: AI Supply-Chain Security Pipeline — Model, dataset, container ve dependency artifacts için provenance, signing, vulnerability scanning, approval gates ve controlled deployment pipeline'ı.
⏳ Project 5: AI Security Monitoring & Incident Response Lab — Prompt, RAG, agent ve infrastructure telemetry'sinin SIEM'e aktarılması; detection rules, simulated incidents, containment ve post-incident documentation.
⏳ Project 6: Production-Grade Cloud AI Security Architecture — AWS, Terraform, Kubernetes, managed AI services, private networking, IAM, encryption, centralized logging, high availability ve AI governance kontrolleriyle uçtan uca güvenli platform.

---

# 🏆 Milestone Portfolio Projects

## Project 1 — Hardened & Audited Linux Server Infrastructure

**Zamanlama:** Level 1 tamamlandıktan sonra.

Networking, Linux administration, users and permissions, systemd, Linux networking, nftables, SSH hardening, Fail2Ban, auditd, logging, backup, Bash automation, incident validation ve teknik dokümantasyon alanlarını birleştiren kapsamlı bir sunucu güvenliği projesi.

---

## Project 2 — Automated Provisioning of a Production-Oriented Kubernetes Environment

**Zamanlama:** Level 2 tamamlandıktan sonra.

Docker, secure images, Kubernetes workloads, Services, RBAC, NetworkPolicy, storage, probes, Terraform, remote state, Ansible, AWS VPC/EC2, automated validation ve operasyonel dokümantasyon alanlarını birleştiren otomatik altyapı projesi.

---

## Project 3 — Resilient Cloud-Native GitOps & Observability Platform

**Zamanlama:** Level 3 ve Level 4 tamamlandıktan sonra.

CI/CD, DevSecOps security gates, software supply-chain security, GitOps, Kubernetes, Prometheus, Grafana, Loki, OpenTelemetry, alerting, SLOs, AWS Multi-AZ, IAM, backup, disaster recovery, resilience testing, runbooks, postmortems, performance analysis ve AI-assisted operational workflows alanlarını birleştiren kapsamlı platform mühendisliği projesi.

## 🏆 Section 2: Milestone Portfolio Projects

The following three progressively advanced portfolio projects are planned to validate the knowledge acquired throughout the roadmap.

Each project combines concepts from multiple modules and is designed around production-oriented security, automation, reliability, validation, and documentation practices.

### 🛡️ Project 1: Hardened & Audited Private Linux Server Infrastructure — ⏳ Planned

> **Focus:** Host Hardening, Stateful Firewalling, Security Auditing and Log-Based Abuse Mitigation  
> **Timeline:** At the end of Level 1 — Module 03

#### Overview

An Ubuntu Server instance running on Amazon EC2 and configured using layered host-hardening practices.

The project focuses on reducing the exposed attack surface, enforcing a default-deny inbound firewall policy, securing administrative access, detecting repeated authentication failures, and recording security-relevant events for auditing and incident analysis.

The project is intended as a security-focused learning environment and portfolio implementation. It is not presented as a highly available or universally production-ready architecture.

#### Core Technology Stack

- **Operating System:** Ubuntu Server on Amazon EC2
- **Firewall Framework:** `nftables`, with `iptables` concepts covered where relevant
- **Automated Abuse Mitigation:** Fail2Ban
- **Audit Framework:** Linux audit subsystem and auditd
- **System Logging:** systemd journal and Linux log files
- **Automation:** Bash reporting and validation scripts
- **Cloud Visibility:** Amazon CloudWatch where appropriate
- **Recovery:** Amazon EBS snapshots and documented restore procedures

#### Planned Security and Operational Controls

- **Default-Deny Inbound Policy:** Unsolicited inbound traffic is denied by default. Only explicitly required services are allowed.
- **Restricted Administrative Access:** SSH is limited to approved source addresses or a dedicated management path.
- **SSH Hardening:** Direct root login and password authentication are disabled. Administrative access uses public-key authentication.
- **Reduced Attack Surface:** Unnecessary services, packages and exposed ports are removed or disabled.
- **Automated Authentication Abuse Mitigation:** Fail2Ban analyzes authentication logs and temporarily blocks addresses that exceed defined failure thresholds.
- **Security Auditing:** auditd rules record selected security-relevant events involving authentication, privilege escalation and sensitive files.
- **Firewall and Authentication Visibility:** Logs are configured and reviewed without assuming that logging alone prevents attacks.
- **Automated Reporting:** Bash scripts summarize authentication failures, firewall events, banned addresses and selected audit records.
- **Recovery Validation:** Snapshot creation and restore procedures are tested and documented.

### 📦 Project 2: Declarative Provisioning of a Production-Oriented Kubernetes Environment — ⏳ Planned

> **Focus:** Cloud Provisioning, Infrastructure as Code, Configuration Management and Kubernetes Orchestration  
> **Timeline:** At the end of Level 2 — Module 06

#### Overview

An end-to-end infrastructure automation project that provisions AWS resources and configures a multi-node Kubernetes learning cluster consisting of one control-plane node and two worker nodes without relying on repeated manual console configuration.

The project demonstrates production-oriented practices such as Infrastructure as Code, automated node configuration, network isolation, secure container builds and declarative orchestration.

A cluster with one control-plane node is not a highly available control-plane architecture. This limitation will be documented explicitly and evaluated separately from the automation objectives.

#### Core Technology Stack

- **Infrastructure as Code:** Terraform
- **Configuration Management:** Ansible
- **Container Runtime:** containerd
- **Cluster Bootstrap:** kubeadm
- **Cloud Infrastructure:** AWS VPC, subnets, EC2 and S3
- **Orchestration:** Kubernetes
- **Container Build:** Docker or another OCI-compatible build workflow
- **Networking:** A selected CNI with documented NetworkPolicy support

#### Planned Deployment Architecture

- **Infrastructure Provisioning:** Terraform provisions a custom AWS VPC, public and private subnets, compute resources and supporting infrastructure through reusable modules.
- **Remote State Management:** Terraform state is stored in Amazon S3 with public access blocked, encryption, versioning, restricted IAM permissions and native state locking.
- **Automated Node Preparation:** Ansible configures kernel modules, required `sysctl` values, packages, Kubernetes prerequisites and containerd.
- **Automated Cluster Bootstrap:** Ansible initializes the control-plane node with kubeadm, handles the join process securely and connects worker nodes to the cluster.
- **Secure Container Builds:** Application workloads use multi-stage builds, minimal base images and non-root execution where supported.
- **Kubernetes Authorization:** RBAC permissions are designed according to least-privilege principles.
- **Network Isolation:** NetworkPolicies restrict unnecessary pod traffic when supported and enforced by the selected CNI.
- **Validation:** Node health, workload scheduling, networking, policy enforcement and recovery behavior are tested and documented.
- **Rebuild Test:** The environment is recreated from version-controlled Terraform and Ansible definitions to identify undocumented manual dependencies.
- **Known Limitations:** Lack of a highly available control plane, single-region scope and learning-environment cost constraints are documented explicitly.

### 🚀 Project 3: Resilient Cloud-Native GitOps & Observability Platform — ⏳ Planned

> **Focus:** DevSecOps, Declarative Delivery, SRE Practices, Observability and Multi-AZ Resilience  
> **Timeline:** At the end of Level 4 — Module 11

#### Overview

The final milestone project implements an end-to-end automated software delivery and operations workflow.

Application code is validated through CI controls, packaged as container images, published to a controlled registry, deployed declaratively through GitOps, observed through metrics, logs and traces, and evaluated through controlled recovery scenarios.

The primary architecture is planned as a single-region Amazon EKS environment distributed across multiple Availability Zones. Multi-Region disaster recovery may be evaluated as a separate extension rather than claimed by default.

#### Core Technology Stack

- **Cloud Infrastructure:** AWS
- **Kubernetes Runtime:** Amazon EKS
- **Infrastructure as Code:** Terraform
- **CI Platform:** GitHub Actions
- **Cloud Authentication:** GitHub Actions OpenID Connect with short-lived AWS credentials
- **Container Registry:** Amazon ECR
- **GitOps Engine:** Argo CD
- **Code Quality & Static Analysis:** SonarQube
- **Secret Scanning:** Gitleaks or TruffleHog
- **Container Image Scanning:** Trivy
- **Policy Enforcement:** Kyverno or an equivalent Kubernetes policy engine
- **Secrets Integration:** External Secrets Operator with AWS Secrets Manager
- **Metrics:** Prometheus
- **Visualization:** Grafana
- **Logging:** Grafana Loki
- **Tracing:** OpenTelemetry with Grafana Tempo or another compatible backend
- **Alerting:** Prometheus Alertmanager
- **Progressive Delivery:** Argo Rollouts where appropriate
- **Backup and Recovery:** Velero and cloud-native data protection mechanisms where applicable

#### Planned Engineering Principles

- **DevSecOps Pipeline:** The CI workflow performs code validation, secret-leak detection, code quality analysis, container image scanning and controlled artifact publishing.
- **Short-Lived Cloud Authentication:** GitHub Actions accesses AWS through OpenID Connect instead of long-lived static credentials.
- **GitOps Architecture:** Argo CD reconciles Kubernetes resources with version-controlled declarative manifests and surfaces configuration drift.
- **Environment Separation:** Staging and production-like environments are logically separated, with stronger account or cluster separation considered as an extension.
- **Secrets Management:** Sensitive values are stored outside Git and synchronized through controlled secret-management integrations.
- **Policy Enforcement:** Admission policies prevent selected insecure workload configurations without being treated as a complete security solution.
- **Golden Signals Observability:** Prometheus and Grafana expose latency, traffic, errors and saturation signals.
- **Centralized Logging:** Loki collects application and platform logs for troubleshooting and incident analysis.
- **Distributed Tracing:** OpenTelemetry captures and exports trace data across supported services.
- **Actionable Alerting:** Alertmanager routes grouped and prioritized alerts through defined notification channels.
- **Progressive Delivery:** Selected workloads use controlled rollout strategies with measurable promotion and rollback conditions.
- **Workload Recovery:** Kubernetes controllers and health checks replace failed workload instances under supported conditions; this does not guarantee recovery from every application or dependency failure.
- **Resilience Validation:** Controlled failure scenarios measure recovery behavior, blast radius and operational limitations.
- **Multi-AZ Deployment:** Workloads and supported cloud resources are distributed across Availability Zones to reduce selected single points of failure.
- **Backup and Restore:** Application state and Kubernetes resources are protected through documented backup and recovery procedures.
- **SRE Practices:** Service Level Indicators, Service Level Objectives, error budgets, incident response and blameless postmortems are incorporated into the project.
- **Rebuild Validation:** Infrastructure and platform components are rebuilt from version-controlled repositories without relying on undocumented deployment steps.

## 📝 Documentation Approach

Each completed topic may include, depending on its requirements:

- Turkish and English technical documentation
- Architecture and data-flow diagrams
- Security risks and defensive controls
- Hardening recommendations and their limitations
- Command and configuration examples
- Troubleshooting methodology
- Practical validation steps
- Production and laboratory environment distinctions
- Official references and standards

The goal is not to create a separate large laboratory project for every topic.

Small exercises are included when they improve understanding. Broader implementations are consolidated into milestone portfolio projects so that related technologies can be demonstrated together within realistic system boundaries.

## ⚠️ Project Status Notice

This repository represents an ongoing learning process.

Completed entries describe topics that have been studied and reviewed. Planned modules and project descriptions define the intended scope of future work.

Technologies, architectures, implementation details and project boundaries may be revised as the systems are built, tested, measured and documented.

A planned feature should not be interpreted as an already implemented or validated capability.
