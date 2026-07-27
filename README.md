🏆 Systems, DevOps & Platform Engineering Learning Roadmap

A bilingual, hands-on learning journal focused on systems, security, automation, cloud infrastructure, reliability, and platform engineering.

   

📖 About This Repository

English

This repository is an ongoing learning journal and practical roadmap created to build a strong technical foundation in system administration, networking, cybersecurity, DevSecOps, cloud computing, Site Reliability Engineering, and platform engineering.

The roadmap combines technical documentation, security-focused analysis, practical validation exercises, and progressively advanced portfolio projects. Content is prepared in both Turkish and English and is updated throughout the learning process.

Türkçe

Bu depo; sistem yönetimi, ağ teknolojileri, siber güvenlik, DevSecOps, bulut bilişim, Site Reliability Engineering ve platform mühendisliği alanlarında güçlü bir teknik temel oluşturmak amacıyla hazırladığım, devam eden bir öğrenme günlüğü ve uygulama yol haritasıdır.

Yol haritası; teknik dokümantasyonlar, güvenlik odaklı değerlendirmeler, uygulamalı doğrulama çalışmaları ve seviyeler ilerledikçe geliştirilecek kapsamlı portföy projelerinden oluşmaktadır. İçerikler Türkçe ve İngilizce olarak hazırlanmakta ve öğrenme süreci boyunca düzenli olarak güncellenmektedir.

📍 Current Progress

Field

Current Status

Current Level

Level 1 — Foundations of Systems & Security

Current Module

Module 01 — Advanced Networking for Platforms

Repository Status

🚧 Actively Maintained

Documentation Languages

Turkish and English

[!NOTE]Completion indicators represent the current learning progress. Direct links are added as the corresponding documents are committed to the repository.

Status Legend

Status

Meaning

✅

Completed

🚧

In Progress

⏳

Planned

🧭 Navigation

Roadmap Overview · Detailed Roadmap · Milestone Projects · Documentation Approach · Project Status

🗺️ Roadmap Overview

Level

Main Focus

Modules

Status

1

Systems, Networking & Security Foundations

01–03

🚧 In Progress

2

Application Infrastructure, Containers & IaC

04–07

⏳ Planned

3

Software Engineering, Automation, CI/CD & Observability

08–11

⏳ Planned

4

Cloud, Reliability, Performance & Intelligent Operations

12–15

⏳ Planned

5

AI Systems Security, MLSecOps & Governance

16–25

⏳ Planned

📚 Detailed Learning Roadmap

<details open>
<summary><strong>🌐 Level 1 — Foundations of Systems & Security</strong> · 🚧 In Progress</summary>

<br>

🌐 Module 01 — Advanced Networking for Platforms · 🚧 In Progress

✅ Physical & Data Link Layers — <sub>OSI ve TCP/IP modelleri, encapsulation, fiziksel sinyaller, Ethernet frame yapısı, MAC addressing, switching, MTU, interface counters ve Layer 1/2 troubleshooting.</sub>

✅ ARP & Local Network Dynamics — <sub>ARP çalışma mantığı, neighbor table, ARP cache durumları, gratuitous ARP, proxy ARP, ARP spoofing riskleri ve Dynamic ARP Inspection.</sub>

✅ VLAN & VXLAN — <sub>Layer 2 segmentasyonu, access ve trunk portlar, IEEE 802.1Q tagging, VLAN hopping riskleri, VXLAN overlay mimarisi, VTEP ve VNI kavramları.</sub>

✅ Network Routing & Subnetting — <sub>IPv4 adresleme, CIDR, subnetting, routing table, default gateway, static ve dynamic routing temelleri, OSPF/BGP genel mantığı ve IP fragmentation.</sub>

✅ Transport Layer & Traffic Control — <sub>TCP ve UDP, portlar ve socket'ler, TCP three-way handshake, sequence ve acknowledgment numbers, flow control, congestion control, retransmission ve connection termination.</sub>

✅ Application Layer Services — <sub>DNS kayıt türleri, DNS resolution, DNS spoofing riskleri, DHCP DORA süreci, DHCP Snooping, HTTP/HTTPS ve TLS handshake.</sub>

✅ Load Balancing & Proxy Concepts — <sub>Forward proxy ve reverse proxy, Layer 4 ve Layer 7 load balancing, health checks, session persistence, TLS termination ve TLS passthrough.</sub>

🐧 Module 02 — Enterprise Linux System Administration · 🚧 In Progress

✅ Linux Architecture, Boot Process & FHS — <sub>Kernel, initramfs, GRUB, systemd, boot süreci ve Linux dizin hiyerarşisi; /etc, /var, /home, /opt, /tmp, /proc, /sys ve /dev.</sub>

✅ Shell, CLI & Stream Processing — <sub>Standard input/output, pipes, redirection, environment variables ve grep, awk, sed, cut, sort, uniq, find, xargs gibi araçlar.</sub>

✅ User & Permission Management — <sub>User ve group yönetimi, UID/GID, chmod, chown, umask, POSIX ACLs, sudoers ve least-privilege yaklaşımı.</sub>

✅ Process & Resource Management — <sub>Process lifecycle, signals, foreground/background jobs, ps, top, htop, kill, file descriptors, memory ve CPU resource takibi.</sub>

✅ systemd, Services & Scheduled Tasks — <sub>Unit files, service dependencies, systemctl, journalctl, restart policies, systemd timers ve cron.</sub>

✅ Storage, Filesystems & LVM — <sub>Partitioning, block devices, ext4, XFS, mount işlemleri, /etc/fstab, inode yapısı, swap, LVM, RAID ve disk kapasite yönetimi.</sub>

✅ Linux Networking & Troubleshooting — <sub>Interface yönetimi, routing, DNS çözümleme, /etc/hosts, systemd-resolved, ip, ss, tcpdump, dig, curl, nc ve network namespaces.</sub>

✅ Package & Repository Management — <sub>apt, dpkg, dnf, rpm, repository yapıları, package signatures, dependency management ve güvenli güncelleme süreçleri.</sub>

✅ Logging, Time & Operational Maintenance — <sub>/var/log, systemd journal, rsyslog, log rotation, NTP, chrony, backup, restore validation ve bakım planları.</sub>

🛡️ Module 03 — Security Engineering & Incident Management · ⏳ Planned

⏳ Security Foundations & Risk Management — <sub>CIA triad, authentication, authorization, accountability, assets, threats, vulnerabilities, risk, attack surface ve defense in depth.</sub>

⏳ Threat Modeling & Attack Lifecycle — <sub>Trust boundaries, data-flow diagrams, STRIDE, attack trees, Cyber Kill Chain, MITRE ATT&CK ve saldırı yaşam döngüsü.</sub>

⏳ Network and Availability Threats — <sub>MITM, spoofing, session hijacking, DNS ve ARP saldırıları, DoS/DDoS, SYN flood ve hizmet kesintisi senaryoları.</sub>

⏳ Malware & Endpoint Defense — <sub>Worm, Trojan, ransomware, rootkit, botnet, fileless malware, sandboxing, hashes, EDR ve endpoint hardening.</sub>

⏳ Firewalling & Network Defense — <sub>Stateful ve stateless filtering, Netfilter, iptables, nftables, connection tracking, NAT, rate limiting ve segmentation.</sub>

⏳ Cryptography, PKI & SSH Security — <sub>Symmetric ve asymmetric cryptography, hashing, digital signatures, certificates, SSH key pairs, host keys ve sshd_config hardening.</sub>

⏳ Linux Hardening & Vulnerability Management — <sub>Attack-surface reduction, CIS Benchmarks, CVE/CVSS, patching, service inventory, permissions, PAM, SELinux ve AppArmor temelleri.</sub>

⏳ System Auditing, Logging & SIEM — <sub>/var/log, systemd journal, auditd, log bütünlüğü, merkezi loglama, SIEM, detection rules ve security-event analysis.</sub>

⏳ Log-Based Abuse Mitigation — <sub>Fail2Ban çalışma mantığı, authentication log analizi, geçici engelleme, rate limiting ve aracın güvenlik sınırlamaları.</sub>

⏳ Incident Response Fundamentals — <sub>Preparation, detection, triage, containment, eradication, recovery, evidence preservation, communication ve post-incident review.</sub>

</details>

<details>
<summary><strong>📦 Level 2 — Application Infrastructure, Containers & Infrastructure as Code</strong> · ⏳ Planned</summary>

<br>

Uygulama, Veri, Otomasyon ve Konteyner Altyapıları

Web uygulamalarının, API'lerin ve veri katmanlarının çalışma prensiplerinin öğrenildiği; konteyner tabanlı platformların ve kodla tanımlanan altyapıların oluşturulduğu seviyedir.

🌍 Module 04 — Web, API & Database Foundations · ⏳ Planned

⏳ Web Architecture Fundamentals — <sub>Client-server modeli, request-response akışı, stateless ve stateful application yaklaşımları.</sub>

⏳ HTTP & HTTPS — <sub>HTTP methods, status codes, headers, content types, caching, HTTP/1.1, HTTP/2, HTTP/3 ve HTTPS.</sub>

⏳ TLS & Certificate Validation — <sub>TLS handshake, X.509 certificates, Certificate Authorities, SNI, certificate chains ve renewal süreçleri.</sub>

⏳ Cookies, Sessions & Authentication — <sub>Cookies, session management, authentication state, secure cookie attributes ve session security.</sub>

⏳ REST API Fundamentals — <sub>Resources, endpoints, HTTP methods, idempotency, pagination, versioning, rate limiting ve API error handling.</sub>

⏳ Tokens, JWT & OAuth 2.0 — <sub>Access tokens, refresh tokens, JWT yapısı, OAuth 2.0 akışları ve OpenID Connect temelleri.</sub>

⏳ Browser & API Security — <sub>CORS, CSRF, XSS, Host header risks, input validation, authentication ve authorization ayrımı.</sub>

⏳ Relational Database Fundamentals — <sub>PostgreSQL ve MySQL genel mimarisi, schemas, tables, primary/foreign keys ve normalization.</sub>

⏳ Transactions & Concurrency — <sub>ACID, transaction isolation levels, locks, deadlocks ve consistency kavramları.</sub>

⏳ Indexes & Query Performance — <sub>Index yapıları, query plans, full-table scans, latency ve temel query optimization.</sub>

⏳ Database Availability & Protection — <sub>Replication, backup, restore testing, encryption, access control ve credential management.</sub>

⏳ Caching & In-Memory Data Stores — <sub>Redis temelleri, cache-aside yaklaşımı, expiration, persistence ve cache invalidation problemleri.</sub>

📦 Module 05 — Container Technologies — Docker Deep Dive · ⏳ Planned

⏳ Containerization vs Virtualization — <sub>Hypervisor tabanlı virtual machines ile operating-system-level containers arasındaki mimari farklar.</sub>

⏳ Linux Kernel Namespaces & Cgroups — <sub>Container isolation ve resource-control mekanizmalarının Linux kernel içindeki temelleri.</sub>

⏳ Container Runtime Internals — <sub>Docker Engine, containerd, runc, OCI specifications ve Kubernetes Container Runtime Interface ilişkisi.</sub>

⏳ Docker Core — <sub>Docker CLI, image ve container lifecycle, Dockerfile yazım kuralları, layers, build cache ve multi-stage builds.</sub>

⏳ Docker Storage — <sub>Writable layers, volumes, bind mounts, tmpfs, UID/GID sorunları ve veri kalıcılığı.</sub>

⏳ Docker Networking — <sub>Network namespaces, veth pairs, bridge networks, port publishing, NAT, container DNS ve service discovery.</sub>

⏳ Docker Compose — <sub>Çok konteynerli yerel geliştirme, test ve doğrulama ortamlarının tanımlanması.</sub>

⏳ Registries & Image Lifecycle — <sub>Docker Hub, GHCR, Amazon ECR, tags, digests, retention, signing ve image provenance.</sub>

⏳ Container Security & Image Hardening — <sub>Non-root containers, reduced capabilities, read-only filesystems, seccomp, minimal images ve Trivy/Grype taramaları.</sub>

⏳ Docker Operations & Troubleshooting — <sub>Logs, inspect, events, health checks, networking, storage, OOM ve safe-cleanup yaklaşımları.</sub>

☸️ Module 06 — Kubernetes Orchestration & Platform Fundamentals · ⏳ Planned

⏳ Kubernetes Architecture — <sub>API Server, etcd, Scheduler, kube-controller-manager, cloud-controller-manager, kubelet, kube-proxy ve container runtime görevleri.</sub>

⏳ Kubernetes Core Objects — <sub>Pods, ReplicaSets, Deployments, StatefulSets, DaemonSets, Jobs ve CronJobs.</sub>

⏳ Scheduling & Resources — <sub>Requests, limits, QoS classes, affinity, anti-affinity, taints, tolerations ve pod placement.</sub>

⏳ Services & Service Discovery — <sub>ClusterIP, NodePort, LoadBalancer Services, EndpointSlices, kube-proxy ve CoreDNS.</sub>

⏳ Configuration & Secrets — <sub>ConfigMaps, Secrets, environment variables, mounted volumes ve Kubernetes Secrets nesnelerinin güvenlik sınırlamaları.</sub>

⏳ Kubernetes Storage — <sub>PersistentVolumes, PersistentVolumeClaims, StorageClasses, dynamic provisioning, access modes ve backup considerations.</sub>

⏳ Kubernetes Security & RBAC — <sub>ServiceAccounts, Roles, ClusterRoles, RoleBindings, Security Contexts, Pod Security Standards ve least privilege.</sub>

⏳ Traffic Management — <sub>Ingress Controllers, Gateway API, NetworkPolicy, service mesh, mTLS, retries ve circuit breaking.</sub>

⏳ Workload Health & Recovery — <sub>Startup, readiness ve liveness probes; restart behavior ve controller'ların desired state'i koruma yaklaşımı.</sub>

⏳ Scaling & Reliability — <sub>HPA, VPA kavramları, Cluster Autoscaler, PodDisruptionBudget, graceful shutdown ve controlled rollout stratejileri.</sub>

⏳ Cluster Lifecycle & Packaging — <sub>kubeadm, managed Kubernetes, upgrades, Helm, Kustomize, certificates ve etcd backup temelleri.</sub>

⏳ Kubernetes Observability & Troubleshooting — <sub>Events, logs, Metrics Server, Prometheus, CrashLoopBackOff, ImagePullBackOff, DNS ve volume failures.</sub>

⏳ GitOps & Declarative Delivery — <sub>Kubernetes kaynaklarının Argo CD veya Flux ile deklaratif biçimde yönetilmesi.</sub>

⚙️ Module 07 — Infrastructure as Code & Configuration Management · ⏳ Planned

⏳ Infrastructure as Code Fundamentals — <sub>Declarative ve imperative yaklaşımlar, desired state, idempotency, drift, convergence ve repeatability.</sub>

⏳ Terraform Language & Workflow — <sub>Providers, resources, data sources, HCL, dependencies, init, plan, apply ve destroy.</sub>

⏳ Terraform Variables & Data Modeling — <sub>Variables, outputs, locals, lists, maps, objects, conditionals, count ve for_each.</sub>

⏳ Terraform Modules — <sub>Root ve child modules, reusable architecture, module boundaries, version pinning ve environment separation.</sub>

⏳ Terraform State Security — <sub>terraform.tfstate güvenliği, Amazon S3 remote state, native S3 state locking, encryption, versioning ve restricted IAM access.</sub>

⏳ Legacy State Locking Considerations — <sub>DynamoDB tabanlı locking yönteminin mevcut eski yapıların geçişi veya uyumluluk ihtiyaçları kapsamında değerlendirilmesi.</sub>

⏳ Terraform Lifecycle & Testing — <sub>Lifecycle rules, import, moved blocks, static analysis, Policy as Code, security scanning ve destructive-change protection.</sub>

⏳ Ansible Fundamentals — <sub>Agentless architecture, SSH connections, inventories, modules, facts, ad-hoc commands ve privilege escalation.</sub>

⏳ Ansible Playbooks & Roles — <sub>Playbooks, handlers, templates, variables, loops, conditionals, roles ve idempotent service management.</sub>

⏳ Ansible Vault & Operational Safety — <sub>Hassas değerlerin şifrelenmesi, secret injection, least privilege, serial changes ve rollback limitations.</sub>

⏳ Git & Version Control Integration — <sub>Commit disiplini, pull requests, protected branches, code review, GitFlow ve trunk-based development.</sub>

⏳ Infrastructure Validation — <sub>Formatting, validation, linting, policy checks, controlled plan/apply ve CI/CD entegrasyonu.</sub>

⏳ Terraform & Ansible Integration — <sub>Provisioning ve configuration-management sorumluluklarının ayrılması, dynamic inventory ve doğrulama süreçleri.</sub>

</details>

<details>
<summary><strong>🚀 Level 3 — Software Engineering, Automation, CI/CD & Observability</strong> · ⏳ Planned</summary>

<br>

Yazılım Teslimi, Otomasyon ve Gözlemlenebilirlik

Altyapı ve platform yazılımlarının sürdürülebilir biçimde geliştirildiği; teslim süreçlerinin otomatikleştirildiği ve sistemlerin metrics, logs ve traces kullanılarak gözlemlendiği seviyedir.

🔄 Module 08 — CI/CD Pipelines & GitOps · ⏳ Planned

⏳ CI/CD Core Concepts — <sub>Continuous Integration, Continuous Delivery ve Continuous Deployment arasındaki farklar.</sub>

⏳ Git Workflows & Pipeline Triggers — <sub>Branching strategies, pull requests, protected branches, tags, releases ve event-based triggers.</sub>

⏳ Pipeline Platforms — <sub>GitHub Actions veya GitLab CI ile workflows, jobs, runners, stages, artifacts ve environment yönetimi.</sub>

⏳ Build, Test & Artifact Pipelines — <sub>Unit, integration ve end-to-end tests, caching, matrices, reports ve reproducible builds.</sub>

⏳ Container Build & Delivery — <sub>Docker/BuildKit, image tagging, commit SHA kullanımı, registry authentication, scanning, signing ve provenance.</sub>

⏳ DevSecOps & Security Gates — <sub>SonarQube, Gitleaks, TruffleHog, Trivy, SAST, SCA, IaC scanning ve controlled policy enforcement.</sub>

⏳ Artifact Management — <sub>Docker Hub, GitHub Container Registry, Amazon ECR ve JFrog Artifactory gibi registry ve artifact-repository çözümleri.</sub>

⏳ Secure Pipeline Authentication — <sub>Uzun ömürlü cloud credentials yerine OpenID Connect ve kısa ömürlü kimlik bilgilerinin kullanılması.</sub>

⏳ Software Supply-Chain Security — <sub>SBOM, dependency trust, signing, Sigstore, SLSA ve artifact-integrity yaklaşımı.</sub>

⏳ Deployment Strategies — <sub>Rolling update, blue/green, canary, feature flags, progressive delivery ve rollback yaklaşımları.</sub>

⏳ GitOps Delivery — <sub>Application code ile deployment state'inin ayrılması, declarative manifests, reconciliation ve drift correction.</sub>

⏳ Pipeline Observability & Troubleshooting — <sub>Pipeline metrics, runner utilization, flaky tests, failures, retries, concurrency ve runbooks.</sub>

🐍 Module 09 — Systems Automation & Scripting — Bash & Python · ⏳ Planned

⏳ Bash Scripting Fundamentals — <sub>Variables, conditionals, loops, functions, arguments, arrays, exit codes ve debugging.</sub>

⏳ Shell Safety Practices — <sub>Quoting, input validation, temporary files, cleanup traps ve set -Eeuo pipefail kullanımının bağlama bağlı değerlendirilmesi.</sub>

⏳ Regular Expressions & Text Processing — <sub>Regex, grep, sed, awk, cut, sort, uniq, tr, find, xargs, jq ve yq.</sub>

⏳ System Administration Automation — <sub>Backup, restore validation, disk monitoring, log rotation, service-health checks ve raporlama.</sub>

⏳ Python Fundamentals for Operations — <sub>Data types, functions, exceptions, virtual environments, type hints, logging ve configuration.</sub>

⏳ Python for Filesystems & Processes — <sub>os, pathlib, sys, shutil, subprocess, signals, permissions ve safe command execution.</sub>

⏳ API Automation — <sub>Authentication, *pagination, timeout, retry, exponential backoff, rate limiting ve schema validation.</sub>

⏳ Network & Security Automation — <sub>DNS checks, TCP/UDP connectivity, certificate inspection, log parsing, firewall validation ve alert enrichment.</sub>

⏳ Secure Automation — <sub>Secrets yönetimi, least privilege, auditability, idempotency ve destructive-action protections.</sub>

⏳ Automation Quality & Packaging — <sub>Unit tests, mocking, linting, type checking, dependency pinning, CLI design ve CI integration.</sub>

💻 Module 10 — Software Engineering for Infrastructure & Platform Teams · ⏳ Planned

⏳ Software Engineering Fundamentals — <sub>Requirements, maintainability, readability, testability ve technical-debt yönetimi.</sub>

⏳ Clean Code Principles — <sub>Naming, small functions, separation of concerns, error handling ve understandable abstractions.</sub>

⏳ SOLID Principles — <sub>Single Responsibility, Open/Closed, Liskov Substitution, Interface Segregation ve Dependency Inversion.</sub>

⏳ Design Patterns — <sub>Factory, Strategy, Adapter, Observer, Dependency Injection ve infrastructure automation kullanım örnekleri.</sub>

⏳ Testing Strategies — <sub>Unit, integration, end-to-end, contract ve smoke tests; mocks, stubs ve test doubles.</sub>

⏳ Refactoring & Code Review — <sub>Safe refactoring, code smells, pull-request reviews ve incremental improvement.</sub>

⏳ Application Architecture — <sub>Monolith, modular monolith, microservices, event-driven architecture ve architecture trade-offs.</sub>

⏳ Distributed-System Fundamentals — <sub>Partial failures, timeouts, retries, idempotency, consistency ve eventual consistency.</sub>

⏳ API & Service Design — <sub>Versioning, backward compatibility, error contracts, rate limits ve graceful degradation.</sub>

⏳ Configuration & Dependency Management — <sub>Configuration separation, feature flags, dependency pinning ve environment consistency.</sub>

⏳ Secure Software Development — <sub>Input validation, secrets, dependency riskleri, threat modeling ve secure defaults.</sub>

⏳ Engineering Documentation — <sub>Architecture Decision Records, diagrams, runbooks, API documentation ve operational handover.</sub>

📊 Module 11 — Observability, Logging & Monitoring · ⏳ Planned

⏳ Observability Fundamentals — <sub>Metrics, logs, traces, events ve profiles arasındaki farklar ve birlikte kullanım biçimleri.</sub>

⏳ Monitoring Approaches — <sub>White-box, black-box, synthetic monitoring, health checks ve baselines.</sub>

⏳ The Four Golden Signals — <sub>Latency, traffic, errors ve saturation.</sub>

⏳ RED & USE Methods — <sub>Request-driven services ve infrastructure resources için gözlem yöntemleri.</sub>

⏳ Metrics & Prometheus — <sub>Prometheus architecture, service discovery, scraping, exporters, PromQL, recording ve alerting rules.</sub>

⏳ Grafana & Dashboard Engineering — <sub>Dashboards, panels, variables, annotations, thresholds, provisioning ve dashboard anti-patterns.</sub>

⏳ Centralized Logging — <sub>Elasticsearch tabanlı çözümler veya Grafana Loki ile log aggregation, parsing, retention ve analysis.</sub>

⏳ Distributed Tracing — <sub>OpenTelemetry instrumentation ve Collector; Jaeger veya Grafana Tempo gibi tracing backend'leri.</sub>

⏳ Alerting & Notification Engineering — <sub>Prometheus Alertmanager ile routing, grouping, deduplication, inhibition, silences ve escalation.</sub>

⏳ SLI, SLO & Reliability Monitoring — <sub>Availability, latency, error budgets, burn rate ve SLO-based alerting.</sub>

⏳ Platform & Kubernetes Observability — <sub>Host, container, node, pod, workload ve control-plane telemetry.</sub>

⏳ Security Observability — <sub>Authentication, authorization, audit, network, DNS, firewall ve cloud-security telemetry.</sub>

⏳ Operational Reliability — <sub>Actionable alerts, alert fatigue, missing telemetry, runbooks ve incident-response bağlantısı.</sub>

⏳ Observability Troubleshooting — <sub>Scrape failures, missing logs, trace gaps, cardinality explosions, storage ve query-performance sorunları.</sub>

</details>

<details>
<summary><strong>☁️ Level 4 — Cloud Computing, Reliability & Intelligent Operations</strong> · ⏳ Planned</summary>

<br>

Bulut, Dayanıklı Sistemler ve Akıllı Operasyon

Yüksek erişilebilirlik, dayanıklılık, ölçeklenebilirlik, güvenli erişim, performans, felaket kurtarma ve yapay zekâ destekli mühendislik ilkelerine dayalı sistemlerin ele alındığı seviyedir.

☁️ Module 12 — Cloud Computing — AWS Focus · ⏳ Planned

⏳ Cloud Computing Foundations — <sub>On-premises ve cloud farkları, IaaS, PaaS, SaaS, elasticity, scalability ve Shared Responsibility Model.</sub>

⏳ AWS Global Infrastructure — <sub>Regions, Availability Zones, edge locations, regional ve global services.</sub>

⏳ Identity & Access Management — <sub>IAM users, groups, roles, policies, trust policies, STS ve Principle of Least Privilege.</sub>

⏳ AWS Networking & VPC Architecture — <sub>VPC, CIDR planning, public/private subnets, route tables, Internet Gateway, NAT Gateway ve VPC endpoints.</sub>

⏳ Network Security — <sub>Security Groups, Network ACLs, VPC Flow Logs, segmentation ve egress-control yaklaşımları.</sub>

⏳ Compute Services — <sub>Amazon EC2, AMIs, launch templates, user data, metadata service, Auto Scaling Groups ve instance lifecycle.</sub>

⏳ Load Balancing — <sub>Application Load Balancer, Network Load Balancer, listeners, target groups, health checks ve TLS.</sub>

⏳ Storage Services — <sub>Amazon S3, EBS ve EFS arasındaki kullanım, availability, durability, performance ve cost trade-off'ları.</sub>

⏳ Managed Databases — <sub>Amazon RDS, Aurora, DynamoDB ve ElastiCache genel mimarileri.</sub>

⏳ Serverless & Event-Driven Architecture — <sub>AWS Lambda, Amazon SQS, SNS, EventBridge, retries, DLQs ve idempotency.</sub>

⏳ Secrets & Key Management — <sub>AWS Secrets Manager, Systems Manager Parameter Store, AWS KMS ve envelope encryption.</sub>

⏳ Cloud Security Services — <sub>CloudTrail, Config, GuardDuty, Security Hub, Inspector, WAF, Shield ve Macie temelleri.</sub>

⏳ Monitoring & Auditing — <sub>CloudWatch metrics, logs, alarms, dashboards, CloudTrail events ve centralized logging.</sub>

⏳ High Availability & Disaster Recovery on AWS — <sub>Multi-AZ, Multi-Region, backup, snapshots, replicas, Route 53 failover, RTO ve RPO.</sub>

⏳ Governance & Cost Management — <sub>AWS Organizations, OUs, SCPs, tagging, budgets, Cost Explorer, quotas ve guardrails.</sub>

⏳ AWS & Infrastructure as Code Integration — <sub>Terraform, remote state, OIDC identities, environment promotion ve drift detection.</sub>

🏛️ Module 13 — Site Reliability Engineering & High Availability · ⏳ Planned

⏳ SRE Foundations & Service Ownership — <sub>SRE ve DevOps ilişkisi, service boundaries, ownership, operational readiness ve reliability as a product feature.</sub>

⏳ SLI, SLO, SLA & Error Budgets — <sub>Service indicators, objectives, external commitments, measurement windows ve burn rate.</sub>

⏳ Toil Reduction & Reliability Automation — <sub>Repetitive operational work, safe automation, self-service, guardrails ve human-in-the-loop.</sub>

⏳ Monitoring, Alerting & On-Call — <sub>Golden Signals, actionable alerts, severity, routing, escalation, handoffs ve on-call health.</sub>

⏳ Incident Management — <sub>Incident detection, declaration, triage, Incident Commander, stabilization, communication ve recovery.</sub>

⏳ Blameless Postmortems — <sub>Timeline, impact, contributing factors, root-cause analysis, corrective actions ve learning culture.</sub>

⏳ Resilience Patterns — <sub>Timeouts, retries, exponential backoff, jitter, circuit breakers, bulkheads, rate limiting ve load shedding.</sub>

⏳ High Availability Architectures — <sub>Redundancy, single points of failure, active-passive, active-active, quorum, replication ve DNS failover.</sub>

⏳ Disaster Recovery & Business Continuity — <sub>RTO, RPO, backup/restore, Pilot Light, Warm Standby, Active-Active ve recovery testing.</sub>

⏳ Chaos Engineering — <sub>Yetkili ortamlarda kontrollü, hipotez tabanlı failure experiments, abort conditions ve blast-radius management.</sub>

⏳ Release Engineering & Change Reliability — <sub>Rolling, blue/green, canary, feature flags, migration safety ve rollback.</sub>

⏳ Operational Readiness — <sub>Runbooks, dashboards, alerts, deployment validation, capacity ownership ve recovery plans.</sub>

⚡ Module 14 — Performance Engineering & Capacity Analysis · ⏳ Planned

⏳ Performance Engineering Fundamentals — <sub>Latency, throughput, concurrency, utilization, saturation ve bottleneck kavramları.</sub>

⏳ CPU Performance Analysis — <sub>CPU utilization, load average, run queues, context switches, user/system time ve CPU profiling.</sub>

⏳ Memory Performance Analysis — <sub>Memory allocation, cache, swap, page faults, memory pressure, leaks ve OOM behavior.</sub>

⏳ Storage & I/O Performance — <sub>IOPS, throughput, latency, queue depth, filesystem overhead ve disk saturation.</sub>

⏳ Network Performance Analysis — <sub>Bandwidth, packet loss, jitter, retransmissions, connection limits ve socket queues.</sub>

⏳ Application Profiling — <sub>Hot paths, function-level profiling, memory allocation, lock contention ve flame graphs.</sub>

⏳ Database Performance — <sub>Query latency, indexes, execution plans, connection pools, locks ve slow-query analysis.</sub>

⏳ Load, Stress & Soak Testing — <sub>Normal load, peak load, breaking points, long-duration tests ve test-environment limitations.</sub>

⏳ Benchmarking Methodology — <sub>Baselines, controlled experiments, repeatability, warm-up effects ve misleading benchmark risks.</sub>

⏳ Capacity Planning — <sub>Growth forecasting, resource headroom, seasonality, quotas ve scaling decisions.</sub>

⏳ Backpressure & Queue Management — <sub>Queue depth, concurrency limits, rate limiting ve overload behavior.</sub>

⏳ Performance Troubleshooting — <sub>Metrics, logs, traces ve profiles kullanılarak katmanlı bottleneck analizi.</sub>

⏳ Performance vs Cost Trade-offs — <sub>Vertical ve horizontal scaling, overprovisioning, efficiency ve cloud-cost etkileri.</sub>

🤖 Module 15 — AI-Assisted Engineering & Operational Intelligence · ⏳ Planned

⏳ AI-Assisted Engineering Fundamentals — <sub>Yapay zekâ araçlarının yazılım, sistem, cloud ve güvenlik mühendisliğinde doğru kullanım alanları.</sub>

⏳ Prompt Design for Engineers — <sub>Gereksinim, bağlam, kısıt, doğrulama kriteri ve beklenen çıktı formatının açık tanımlanması.</sub>

⏳ AI-Assisted Code Generation — <sub>Bash, Python, Terraform, Ansible ve Kubernetes manifest taslaklarının kontrollü üretilmesi.</sub>

⏳ AI-Assisted Code Review — <sub>Logic errors, security risks, maintainability issues, race conditions ve missing error handling kontrolleri.</sub>

⏳ Infrastructure Review — <sub>Terraform plans, IAM policies, Kubernetes manifests ve network rules için AI destekli inceleme.</sub>

⏳ Log & Incident Analysis — <sub>Log özetleme, timeline oluşturma, correlation, hypothesis generation ve incident triage desteği.</sub>

⏳ AI-Assisted Troubleshooting — <sub>Hata mesajları, metrics, traces ve system outputs üzerinden olası nedenlerin önceliklendirilmesi.</sub>

⏳ Security Analysis & Threat Hunting — <sub>Detection-rule taslakları, IOC enrichment, suspicious-pattern analysis ve false-positive değerlendirmesi.</sub>

⏳ Documentation & Knowledge Management — <sub>README, runbook, postmortem, architecture documentation ve technical-summary üretimi.</sub>

⏳ Output Verification — <sub>AI tarafından üretilen komut, kod, policy ve teknik iddiaların bağımsız olarak doğrulanması.</sub>

⏳ Hallucination & Context Risks — <sub>Uydurulmuş komutlar, eski bilgiler, yanlış varsayımlar ve eksik bağlam nedeniyle oluşabilecek riskler.</sub>

⏳ Sensitive Data Protection — <sub>Credentials, logs, source code, personal data ve şirket bilgilerinin AI araçlarına aktarılma riskleri.</sub>

⏳ Human Approval & Safety Boundaries — <sub>Production değişiklikleri, destructive commands, security decisions ve automated actions için insan onayı.</sub>

⏳ AI-Augmented Operations — <sub>Alert enrichment, anomaly summarization, runbook suggestions ve controlled remediation yaklaşımları.</sub>

⏳ Responsible AI Use — <sub>Privacy, auditability, intellectual property, bias, accountability ve organizational-policy gereksinimleri.</sub>

</details>

<details>
<summary><strong>🤖 Level 5 — AI Systems Security, MLSecOps & Governance</strong> · ⏳ Planned</summary>

<br>

Yapay Zekâ Sistemleri Güvenliği, Güvenli Model Yaşam Döngüsü ve AI Yönetişimi

Yapay zekâ ve makine öğrenmesi sistemlerinin mimarisinin, tehdit yüzeylerinin ve yaşam döngüsünün incelendiği; LLM, RAG ve agent tabanlı uygulamaların güvenli biçimde geliştirildiği, dağıtıldığı, izlendiği ve yönetildiği ileri seviye uzmanlık alanıdır.

🧠 Module 16 — AI, Machine Learning & LLM Foundations for Security Engineers · ⏳ Planned

⏳ AI & Machine Learning Foundations — <sub>Artificial Intelligence, Machine Learning, Deep Learning ve Generative AI arasındaki farklar; supervised, unsupervised, reinforcement ve self-supervised learning yaklaşımları.</sub>

⏳ AI System Lifecycle — <sub>Veri toplama, veri hazırlama, training, validation, testing, fine-tuning, deployment, inference, monitoring, retraining ve retirement aşamaları.</sub>

⏳ Model Architecture Fundamentals — <sub>Neural networks, parameters, weights, layers, loss functions, optimization, checkpoints ve model formats kavramları.</sub>

⏳ Large Language Model Fundamentals — <sub>Transformer mimarisi, attention, tokens, tokenization, context window, temperature, sampling ve hallucination kavramları.</sub>

⏳ Embeddings & Semantic Search — <sub>Embedding modelleri, similarity search, vector representations, indexing ve semantic-retrieval çalışma mantığı.</sub>

⏳ Retrieval-Augmented Generation — <sub>RAG ingestion, chunking, embedding, retrieval, reranking, context construction ve generation akışı.</sub>

⏳ Vector Databases — <sub>Vector indexes, metadata filtering, namespaces, document permissions, multi-tenancy ve vector-store erişim kontrolü.</sub>

⏳ AI Agents & Tool Use — <sub>Planning, reasoning loops, memory, tool calling, function calling, external APIs ve autonomous-action riskleri.</sub>

⏳ Model Context Protocol Fundamentals — <sub>MCP servers, clients, tools, resources, prompts, trust boundaries ve third-party integration riskleri.</sub>

⏳ Training, Fine-Tuning & Inference — <sub>Pre-training, supervised fine-tuning, instruction tuning, adapters, LoRA, quantization ve inference-serving farkları.</sub>

⏳ AI Security Terminology — <sub>Model, dataset, prompt, system prompt, guardrail, agent, inference endpoint, model registry ve evaluation kavramları.</sub>

🧭 Module 17 — AI Threat Modeling & Security Architecture · ⏳ Planned

⏳ AI System Asset Identification — <sub>Models, datasets, prompts, embeddings, vector stores, APIs, agents, tools, credentials, logs ve model artifacts gibi korunması gereken varlıkların belirlenmesi.</sub>

⏳ AI Data-Flow Diagrams — <sub>Kullanıcı, uygulama, model provider, RAG pipeline, agent tools, databases ve external services arasındaki veri akışlarının çıkarılması.</sub>

⏳ Trust Boundaries — <sub>User input, retrieved content, model output, agent memory, tool responses ve third-party models arasındaki güven sınırlarının belirlenmesi.</sub>

⏳ AI Attack-Surface Analysis — <sub>Training, supply chain, model storage, inference API, application, RAG, agent ve monitoring katmanlarının saldırı yüzeyleri.</sub>

⏳ Threat Actors & Capabilities — <sub>External attackers, malicious users, insiders, compromised suppliers ve automated-abuse actors.</sub>

⏳ AI Misuse & Abuse Cases — <sub>Modelin yetkisiz, zararlı, maliyet artırıcı veya kurum politikasına aykırı amaçlarla kullanılması.</sub>

⏳ Threat Modeling Methods — <sub>STRIDE, attack trees, abuse cases ve AI sistemlerine uyarlanmış threat-modeling yaklaşımları.</sub>

⏳ OWASP LLM & Generative AI Risks — <sub>LLM uygulamalarına özgü güvenlik risklerinin mimari ve uygulama seviyesinde değerlendirilmesi.</sub>

⏳ MITRE ATLAS — <sub>Predictive AI, Generative AI ve Agentic AI sistemlerine yönelik saldırı taktikleri, teknikleri ve mitigations.</sub>

⏳ NIST AI Risk Management Framework — <sub>Govern, Map, Measure ve Manage fonksiyonlarının AI güvenlik risklerine uygulanması.</sub>

⏳ Security Requirements Engineering — <sub>Authentication, authorization, confidentiality, integrity, availability, privacy, auditability ve human-oversight gereksinimleri.</sub>

⏳ AI Security Architecture Patterns — <sub>Isolation, least privilege, defense in depth, zero trust, secure gateways ve controlled tool execution.</sub>

⏳ AI Risk Register — <sub>Risk owner, likelihood, impact, control, residual risk, acceptance ve remediation tracking.</sub>

🔐 Module 18 — LLM, RAG & Agentic Application Security · ⏳ Planned

⏳ Direct Prompt Injection — <sub>Kullanıcının model davranışını veya sistem talimatlarını doğrudan değiştirmeye çalıştığı saldırılar.</sub>

⏳ Indirect Prompt Injection — <sub>Web pages, documents, emails, retrieved content veya external tool outputs içine yerleştirilmiş kötü amaçlı talimatlar.</sub>

⏳ System-Prompt Extraction — <sub>System prompt, hidden instructions, internal policies ve application logic bilgilerinin model üzerinden elde edilmesi.</sub>

⏳ Jailbreak Attacks — <sub>Modelin güvenlik ve kullanım sınırlamalarının crafted prompts, encoding, role-play veya multi-step interactions ile aşılması.</sub>

⏳ Sensitive Information Disclosure — <sub>Credentials, personal data, proprietary data, internal documents ve confidential context bilgilerinin model çıktılarında açığa çıkması.</sub>

⏳ Insecure Output Handling — <sub>Model çıktılarının doğrulanmadan shell, database, browser, HTML renderer veya downstream application tarafından çalıştırılması.</sub>

⏳ Excessive Agency — <sub>Agentlara gereğinden fazla sistem, dosya, network, database veya cloud yetkisi verilmesi.</sub>

⏳ Agent Identity & Authorization — <sub>Kullanıcı kimliği, agent identity, delegated permissions, service accounts ve end-to-end authorization kontrolleri.</sub>

⏳ Tool-Calling Security — <sub>Tool allowlists, parameter validation, schema enforcement, confirmation gates ve destructive-action restrictions.</sub>

⏳ MCP Server & Tool Security — <sub>Malicious MCP servers, tool impersonation, poisoned tool descriptions, overprivileged tools ve untrusted resources.</sub>

⏳ Agent Memory Security — <sub>Persistent memory, conversation history, long-term state ve stored context bilgilerinin zehirlenmesi veya sızdırılması.</sub>

⏳ Context Poisoning — <sub>Model context'inin yanlış, kötü amaçlı veya yanıltıcı bilgilerle manipüle edilmesi.</sub>

⏳ RAG Poisoning — <sub>Knowledge base, document store veya retrieved content içine manipüle edilmiş içerik yerleştirilmesi.</sub>

⏳ Retrieval Authorization — <sub>Kullanıcının yalnızca erişim yetkisi bulunan dokümanların RAG pipeline tarafından getirilmesi.</sub>

⏳ Vector Database Security — <sub>Authentication, encryption, tenant isolation, metadata authorization, backup ve audit logging.</sub>

⏳ Cross-Tenant Data Leakage — <sub>Çok kiracılı sistemlerde kullanıcı veya kurum verilerinin başka tenant'lara sızması.</sub>

⏳ SSRF & External-Resource Abuse — <sub>Agent veya tool mekanizmalarının internal services, metadata endpoints veya yetkisiz URL'lere erişmek için kullanılması.</sub>

⏳ Code-Execution & Sandbox Risks — <sub>Model tarafından üretilen kodun kontrollü olmayan ortamlarda çalıştırılması ve sandbox escape riskleri.</sub>

⏳ Output Validation & Policy Enforcement — <sub>Structured output, schema validation, content filtering, allowlists ve deterministic security controls.</sub>

⏳ Human-in-the-Loop Controls — <sub>Para transferi, veri silme, production değişikliği, email gönderme ve credential işlemlerinde insan onayı.</sub>

⏳ Rate Limiting & Cost Controls — <sub>Token, request, concurrency, tool-call ve spending limitleriyle abuse ve denial-of-wallet risklerinin azaltılması.</sub>

⏳ Secure Conversation Design — <sub>Session isolation, context boundaries, history management ve conversation-state güvenliği.</sub>

🧬 Module 19 — Adversarial Machine Learning & Model Security · ⏳ Planned

⏳ Adversarial ML Foundations — <sub>Model davranışının kötü amaçlı girdiler, training data veya model artifacts üzerinden manipüle edilmesi.</sub>

⏳ Evasion Attacks — <sub>Model inference sırasında girdilerin sınıflandırma veya karar mekanizmasını yanıltacak biçimde değiştirilmesi.</sub>

⏳ Adversarial Examples — <sub>İnsan için küçük görünen ancak model sonucunu önemli ölçüde değiştiren crafted inputs.</sub>

⏳ Training-Data Poisoning — <sub>Training veya fine-tuning verilerinin model davranışını bozmak amacıyla manipüle edilmesi.</sub>

⏳ Backdoor & Trojan Attacks — <sub>Belirli trigger girdilerinde saldırganın istediği sonucu üreten gizli model davranışları.</sub>

⏳ Fine-Tuning Attacks — <sub>Zararlı dataset, adapter veya fine-tuning süreçleri üzerinden model davranışının değiştirilmesi.</sub>

⏳ Model Extraction — <sub>Query-response interactions üzerinden hedef modelin davranışının veya işlevinin kopyalanması.</sub>

⏳ Model Inversion — <sub>Model outputs veya gradients üzerinden training data özelliklerinin yeniden oluşturulmaya çalışılması.</sub>

⏳ Membership Inference — <sub>Belirli bir verinin modelin training dataset'i içinde bulunup bulunmadığının tahmin edilmesi.</sub>

⏳ Model Fingerprinting — <sub>Model family, version, architecture veya provider bilgilerinin davranışsal yöntemlerle belirlenmesi.</sub>

⏳ Model Theft — <sub>Model weights, checkpoints, adapters, architecture bilgileri veya private endpoints'in yetkisiz biçimde ele geçirilmesi.</sub>

⏳ Privacy Attacks — <sub>Training data memorization, personal-data leakage ve reconstruction saldırıları.</sub>

⏳ Model Integrity Verification — <sub>Hashes, digital signatures, artifact validation ve trusted model-source kontrolleri.</sub>

⏳ Robustness Evaluation — <sub>Modelin adversarial, noisy, out-of-distribution ve manipulated inputs karşısındaki dayanıklılığının ölçülmesi.</sub>

⏳ Mitigation Limitations — <sub>Adversarial training, filtering, detection ve guardrail yöntemlerinin sınırlamalarının değerlendirilmesi.</sub>

🗃️ Module 20 — AI Data Security, Privacy & Knowledge Protection · ⏳ Planned

⏳ AI Data Classification — <sub>Public, internal, confidential, restricted, personal ve regulated data türlerinin sınıflandırılması.</sub>

⏳ Dataset Inventory & Ownership — <sub>Dataset owner, source, purpose, access, retention ve approved-use bilgilerinin kayıt altına alınması.</sub>

⏳ Data Lineage & Provenance — <sub>Verinin kaynağı, dönüşümleri, training süreçleri ve model üzerindeki kullanımının izlenebilir olması.</sub>

⏳ Data Quality & Integrity — <sub>Eksik, yanlış, manipüle edilmiş, outdated veya malicious data'nın tespit edilmesi.</sub>

⏳ Training-Data Access Control — <sub>Dataset storage, notebooks, pipelines ve processing systems için least-privilege erişim.</sub>

⏳ Secrets & Credential Detection — <sub>API keys, passwords, certificates, tokens ve connection strings'in dataset, prompt veya notebook içinde bulunmasının engellenmesi.</sub>

⏳ Personal-Data Protection — <sub>PII detection, masking, anonymization, pseudonymization ve data minimization.</sub>

⏳ Encryption — <sub>Data at rest, data in transit, model artifacts, embeddings, backups ve logs için şifreleme.</sub>

⏳ RAG Knowledge-Base Protection — <sub>Document permissions, ingestion approval, integrity validation ve source trust değerlendirmesi.</sub>

⏳ Prompt & Response Privacy — <sub>Kullanıcı promptları, model outputs, feedback verileri ve conversation history'nin güvenli saklanması.</sub>

⏳ Logging & Retention Controls — <sub>Prompt, response ve tool-call loglarının içerik, erişim, saklama süresi ve silme politikaları.</sub>

⏳ Tenant Isolation — <sub>Dataset, vector namespace, memory ve retrieval context'in tenant bazında ayrılması.</sub>

⏳ Data-Poisoning Detection — <sub>Anomalous records, duplicate content, malicious instructions ve unexpected distribution değişimlerinin tespiti.</sub>

⏳ Data Usage Rights — <sub>Dataset licensing, consent, intellectual property ve permitted-use gereksinimleri.</sub>

⏳ Privacy-Enhancing Technologies — <sub>Differential privacy, federated learning, secure aggregation ve confidential-computing kavramları.</sub>

⏳ Secure Data Deletion — <sub>Dataset, embeddings, caches, model artifacts ve backups içindeki verilerin lifecycle'a uygun silinmesi.</sub>

🖥️ Module 21 — Secure AI Infrastructure & Model Serving · ⏳ Planned

⏳ AI Infrastructure Architecture — <sub>Model gateway, inference server, vector database, object storage, agent runtime, API ve monitoring bileşenleri.</sub>

⏳ GPU & Accelerator Fundamentals — <sub>GPU resources, drivers, CUDA ecosystem, device access ve accelerator-sharing kavramları.</sub>

⏳ GPU Workload Security — <sub>GPU device permissions, workload isolation, driver security, resource quotas ve multi-tenant accelerator riskleri.</sub>

⏳ Secure Model Serving — <sub>Model loading, inference endpoints, batching, caching, streaming ve request-isolation güvenliği.</sub>

⏳ Containerized AI Workloads — <sub>Non-root containers, minimal images, reduced capabilities, read-only filesystems ve image scanning.</sub>

⏳ Kubernetes for AI Security — <sub>GPU scheduling, ServiceAccounts, RBAC, Pod Security Standards, NetworkPolicy ve workload isolation.</sub>

⏳ Model-Serving Platforms — <sub>vLLM, Triton, KServe, Ray Serve veya benzeri serving çözümlerinin güvenlik mimarisi.</sub>

⏳ Identity & Workload Authentication — <sub>Users, services, agents, workloads ve external APIs için güçlü kimlik doğrulama.</sub>

⏳ Secrets & Key Management — <sub>Model API keys, database credentials, encryption keys ve signing keys'in güvenli yönetimi.</sub>

⏳ Network Segmentation — <sub>Training, inference, management, storage ve monitoring ağlarının ayrılması.</sub>

⏳ API Gateway Security — <sub>Authentication, authorization, request limits, schema validation, TLS termination ve audit logging.</sub>

⏳ Service-to-Service Security — <sub>Mutual TLS, workload identities, certificate rotation ve service-mesh kontrolleri.</sub>

⏳ Multi-Tenancy Isolation — <sub>Compute, memory, network, model, cache ve data katmanlarında tenant ayrımı.</sub>

⏳ Model Registry Security — <sub>Model versioning, access control, signing, approval workflows ve artifact immutability.</sub>

⏳ Artifact Storage Security — <sub>Model weights, checkpoints, adapters, tokenizer files ve configuration dosyalarının korunması.</sub>

⏳ Secure Sandboxing — <sub>Untrusted code execution, interpreters, browser tools ve file-processing workloads için izolasyon.</sub>

⏳ Availability & Resource Exhaustion — <sub>GPU, CPU, memory, token, queue ve concurrency tüketimine karşı koruma.</sub>

⏳ Denial-of-Service & Denial-of-Wallet — <sub>Yüksek maliyetli inference requests, long-context attacks ve automated API abuse.</sub>

⏳ AI Infrastructure Observability — <sub>Inference latency, token usage, errors, resource consumption, model loading ve tool-call telemetry.</sub>

⏳ Backup, Recovery & Resilience — <sub>Model registry, vector database, configuration, prompts ve AI-service dependencies için recovery planları.</sub>

⏳ Cloud AI Shared Responsibility — <sub>Managed model services, customer data, IAM, network, logging ve application-security sorumluluklarının ayrılması.</sub>

🔗 Module 22 — MLSecOps, AI Supply Chain & Secure AI Development Lifecycle · ⏳ Planned

⏳ MLSecOps Foundations — <sub>Machine Learning, MLOps, DevSecOps ve AI Security kontrollerinin tek yaşam döngüsünde birleştirilmesi.</sub>

⏳ Secure AI SDLC — <sub>Requirements, design, development, training, testing, release, deployment, monitoring ve retirement süreçlerine güvenlik eklenmesi.</sub>

⏳ AI Pipeline Security — <sub>Data ingestion, feature engineering, training, evaluation, packaging ve deployment pipeline'larının korunması.</sub>

⏳ Secure Development Environments — <sub>Notebooks, IDEs, experiment platforms, build runners ve development credentials güvenliği.</sub>

⏳ Dataset Supply-Chain Security — <sub>Third-party datasets, provenance, licenses, integrity checks ve poisoning riskleri.</sub>

⏳ Model Supply-Chain Security — <sub>Public models, adapters, checkpoints ve pre-trained artifacts için source-trust değerlendirmesi.</sub>

⏳ Third-Party Model Risk — <sub>Model provider, hosting service, API, license, data usage ve security-assurance kontrolleri.</sub>

⏳ Dependency Security — <sub>Python packages, container images, ML libraries, CUDA dependencies ve transitive-dependency riskleri.</sub>

⏳ Unsafe Model Serialization — <sub>Pickle ve benzeri formats üzerinden arbitrary-code-execution riskleri ve güvenli format değerlendirmesi.</sub>

⏳ Artifact Signing & Verification — <sub>Model, dataset, container ve configuration artifacts için digital signatures, hashes ve provenance.</sub>

⏳ AI Bill of Materials — <sub>Model, dataset, library, container, provider ve service dependencies'in envanterinin oluşturulması.</sub>

⏳ Reproducibility — <sub>Dataset version, code commit, configuration, dependency, model checkpoint ve environment kayıtlarının tutulması.</sub>

⏳ Secure CI/CD for AI — <sub>Automated tests, security scanning, policy checks, approvals ve protected deployment environments.</sub>

⏳ Security Evaluation Gates — <sub>Model veya AI uygulaması production'a alınmadan önce zorunlu güvenlik ve risk testleri.</sub>

⏳ Model Registry Promotion — <sub>Development, testing, staging ve production modelleri arasında kontrollü promotion.</sub>

⏳ Canary & Shadow Deployment — <sub>Yeni model sürümlerinin sınırlı trafik veya shadow mode ile güvenli doğrulanması.</sub>

⏳ Rollback & Model Revocation — <sub>Güvensiz veya bozuk model sürümlerinin hızla geri alınması ve kullanımının engellenmesi.</sub>

⏳ Drift & Unauthorized Change Detection — <sub>Model, dataset, configuration ve infrastructure üzerinde beklenmeyen değişikliklerin tespiti.</sub>

⏳ Vulnerability & Patch Management — <sub>AI frameworks, serving platforms, drivers, dependencies ve containers için güncelleme süreçleri.</sub>

⏳ Vendor & Acquisition Security — <sub>Satın alınan veya dışarıdan kullanılan AI sistemlerinin security requirements ve assurance evidence ile değerlendirilmesi.</sub>

🧪 Module 23 — AI Security Testing, Evaluation & Red Teaming · ⏳ Planned

⏳ AI Security Test Planning — <sub>System scope, assets, threat model, test objectives, rules of engagement ve success criteria.</sub>

⏳ Prompt Fuzzing — <sub>Farklı prompt structures, encodings, languages, obfuscation ve edge cases ile otomatik güvenlik testi.</sub>

⏳ Jailbreak Testing — <sub>Model policy ve guardrail sınırlarının yetkili test ortamlarında değerlendirilmesi.</sub>

⏳ Prompt-Injection Testing — <sub>Direct ve indirect injection senaryolarının application, RAG ve agent katmanlarında denenmesi.</sub>

⏳ RAG Security Testing — <sub>Malicious documents, poisoned context, unauthorized retrieval ve source-manipulation senaryoları.</sub>

⏳ Agentic Security Testing — <sub>Tool abuse, privilege escalation, unsafe planning, memory poisoning ve autonomous-action testleri.</sub>

⏳ MCP Security Testing — <sub>Malicious server, poisoned tool metadata, unauthorized resource ve tool substitution senaryoları.</sub>

⏳ Model Extraction Testing — <sub>Query limits, output precision, rate controls ve model-copying risklerinin değerlendirilmesi.</sub>

⏳ Privacy Leakage Testing — <sub>Memorized data, personal information, training-data exposure ve membership-inference senaryoları.</sub>

⏳ Adversarial Input Testing — <sub>Evasion, adversarial examples, malformed data ve multimodal-input manipülasyonları.</sub>

⏳ Multimodal Security Testing — <sub>Image, audio, video, document ve hidden-instruction içeriklerinin modele etkisi.</sub>

⏳ Tool & Output Exploitation — <sub>Model outputs üzerinden SQL injection, command injection, XSS, SSRF veya unsafe code execution riskleri.</sub>

⏳ Abuse & Cost Testing — <sub>High-token prompts, recursive agents, repeated tool calls ve resource-exhaustion senaryoları.</sub>

⏳ Automated AI Evaluations — <sub>Repeatable test suites, benchmark datasets, attack templates ve regression testing.</sub>

⏳ Human Red Teaming — <sub>Context-dependent, creative ve chained attack paths'in deneyimli test uzmanları tarafından incelenmesi.</sub>

⏳ Security vs Safety Evaluation — <sub>Siber güvenlik açıkları, kullanım politikası ihlalleri ve harmful-output risklerinin ayrı fakat ilişkili değerlendirilmesi.</sub>

⏳ Findings & Severity Assessment — <sub>Exploitability, business impact, affected assets, reproducibility ve remediation priority.</sub>

⏳ Remediation Verification — <sub>Uygulanan kontrollerin tekrar test edilmesi ve yeni bypass yöntemlerinin araştırılması.</sub>

⏳ Responsible Testing Boundaries — <sub>Yalnızca yetkili sistemlerde, kontrollü test verileri ve sınırlı blast radius ile çalışma.</sub>

📡 Module 24 — AI Security Monitoring, Threat Detection & Incident Response · ⏳ Planned

⏳ AI Security Telemetry — <sub>Prompt, response, retrieval, model, tool-call, identity, authorization, network ve infrastructure olaylarının kaydı.</sub>

⏳ Secure AI Logging — <sub>Hassas veriyi gereksiz toplamadan olay araştırmasına yetecek seviyede audit trail oluşturulması.</sub>

⏳ Prompt & Response Monitoring — <sub>Injection patterns, data leakage, policy bypass ve suspicious-output davranışlarının tespiti.</sub>

⏳ Agent Activity Monitoring — <sub>Tool selection, parameter values, file access, network requests, external actions ve approval decisions.</sub>

⏳ RAG Monitoring — <sub>Retrieved documents, source trust, unusual retrieval patterns ve authorization failures.</sub>

⏳ Model-Behavior Monitoring — <sub>Unexpected outputs, integrity issues, safety-control degradation ve model-version farklılıkları.</sub>

⏳ Abuse Detection — <sub>Automated scraping, model extraction, credential testing, spam, fraud ve denial-of-wallet davranışları.</sub>

⏳ Drift Monitoring — <sub>Data drift, concept drift, performance drift ve security-control drift arasındaki farklar.</sub>

⏳ AI Security Metrics — <sub>Injection success rate, blocked actions, sensitive-data exposure, anomalous tool calls ve security-test regression results.</sub>

⏳ SIEM Integration — <sub>AI application, model gateway, cloud, Kubernetes ve identity logs'un merkezi güvenlik izleme sistemlerine aktarılması.</sub>

⏳ Detection Engineering — <sub>AI-specific detection rules, behavioral baselines, correlation rules ve alert tuning.</sub>

⏳ Threat Hunting with MITRE ATLAS — <sub>AI sistemlerine yönelik tactics ve techniques üzerinden hipotez tabanlı araştırma.</sub>

⏳ AI Incident Classification — <sub>Prompt injection, data leakage, compromised agent, poisoned RAG source, stolen model ve supply-chain compromise olayları.</sub>

⏳ AI Incident Triage — <sub>Etkilenen model, dataset, users, tools, credentials, infrastructure ve business processes'in belirlenmesi.</sub>

⏳ Containment Strategies — <sub>Agent tools'un kapatılması, model endpoint'in izole edilmesi, retrieval source'un kaldırılması ve access tokens'ın iptali.</sub>

⏳ Eradication & Recovery — <sub>Poisoned content'in temizlenmesi, model veya dataset'in güvenli sürümden yeniden yüklenmesi ve controls'un doğrulanması.</sub>

⏳ Model Rollback & Revocation — <sub>Güvensiz model, adapter veya configuration sürümünün kullanım dışı bırakılması.</sub>

⏳ Credential & Secret Rotation — <sub>Agent, service, model provider ve tool credentials bilgilerinin olay sonrasında yenilenmesi.</sub>

⏳ AI Forensics & Evidence Preservation — <sub>Prompts, outputs, retrieved content, tool executions, model versions ve configuration bilgilerinin korunması.</sub>

⏳ Communication & Reporting — <sub>Teknik ekipler, yönetim, hukuk, privacy ve business stakeholders için olay iletişimi.</sub>

⏳ Post-Incident Review — <sub>Timeline, root causes, control failures, lessons learned ve corrective actions.</sub>

⚖️ Module 25 — AI Governance, Assurance, Standards & Compliance · ⏳ Planned

⏳ AI Governance Foundations — <sub>AI ownership, roles, responsibilities, decision rights ve organizational oversight.</sub>

⏳ NIST AI RMF — <sub>Govern, Map, Measure ve Manage fonksiyonlarıyla AI risk-management programı oluşturulması.</sub>

⏳ NIST Generative AI Profile — <sub>Generative AI sistemlerine özgü risklerin ve risk-treatment yöntemlerinin değerlendirilmesi.</sub>

⏳ NIST Secure AI Development Practices — <sub>AI model ve sistem geliştirme süreçlerinin Secure Software Development Framework ile uyumlu hâle getirilmesi.</sub>

⏳ ISO/IEC 42001 — <sub>Artificial Intelligence Management System kurulumu, policy, objectives, risk management ve continuous improvement.</sub>

⏳ ISO/IEC 23894 — <sub>AI risk-management principles, lifecycle risk identification ve risk-treatment yaklaşımı.</sub>

⏳ EU AI Act — <sub>Prohibited practices, risk classifications, high-risk systems, general-purpose AI ve provider/deployer obligations.</sub>

⏳ CSA AI Controls Matrix — <sub>Cloud tabanlı AI sistemleri için governance, security, privacy, supply chain ve operational controls.</sub>

⏳ OWASP & MITRE Mapping — <sub>OWASP AI risks ve MITRE ATLAS techniques'in security controls ve test cases ile eşleştirilmesi.</sub>

⏳ AI System Inventory — <sub>Kurum içinde geliştirilen, kullanılan veya third party üzerinden alınan AI sistemlerinin kayıt altına alınması.</sub>

⏳ AI Risk Classification — <sub>Use case, autonomy, data sensitivity, user impact, business criticality ve regulatory exposure değerlendirmesi.</sub>

⏳ AI Impact Assessments — <sub>Güvenlik, privacy, safety, fairness, human rights ve operational etkilerin sistem devreye alınmadan incelenmesi.</sub>

⏳ Model Cards & System Cards — <sub>Model purpose, limitations, training information, evaluation results, risks ve intended-use bilgilerinin dokümantasyonu.</sub>

⏳ Transparency & Explainability — <sub>Kullanıcı bilgilendirmesi, AI-generated content disclosure, model limitations ve decision explanations.</sub>

⏳ Human Oversight — <sub>Kritik karar ve eylemlerde insan kontrolü, intervention, override ve escalation mekanizmaları.</sub>

⏳ Accountability & Auditability — <sub>AI kararlarının, model versions'ın, approvals'ın ve configuration değişikliklerinin izlenebilir olması.</sub>

⏳ Third-Party Assurance — <sub>Model providers, datasets, AI APIs ve SaaS services için due diligence ve contractual security requirements.</sub>

⏳ Acceptable AI Use Policies — <sub>Çalışanların AI araçlarıyla hangi veri ve sistemleri kullanabileceğini belirleyen kurallar.</sub>

⏳ AI Exception & Risk Acceptance — <sub>Güvenlik kontrollerinden sapmaların business owner ve risk owner tarafından onaylanması.</sub>

⏳ Continuous Compliance — <sub>AI system, model, dataset, regulation ve control değişikliklerinin sürekli takip edilmesi.</sub>

⏳ Responsible AI Principles — <sub>Privacy, fairness, transparency, reliability, safety, security ve accountability gereksinimlerinin birlikte yönetilmesi.</sub>

🏗️ Level 5 Capstone Projects · ⏳ Planned

⏳ Project 1: Secure RAG Application — <sub>Authentication, document-level authorization, vector database isolation, prompt-injection protection, output validation, logging ve security testing içeren güvenli RAG uygulaması.</sub>

⏳ Project 2: Agentic AI Security Laboratory — <sub>Kontrollü sandbox içinde tool kullanan bir AI agent'ın kurulması; least privilege, approval gates, MCP security, memory isolation, monitoring ve red-team testleri.</sub>

⏳ Project 3: Secure AI Model-Serving Platform — <sub>Docker veya Kubernetes üzerinde model serving; API gateway, TLS, workload identity, secrets management, rate limiting, model registry, monitoring ve rollback.</sub>

⏳ Project 4: AI Supply-Chain Security Pipeline — <sub>Model, dataset, container ve dependency artifacts için provenance, signing, vulnerability scanning, approval gates ve controlled deployment pipeline'ı.</sub>

⏳ Project 5: AI Security Monitoring & Incident Response Lab — <sub>Prompt, RAG, agent ve infrastructure telemetry'sinin SIEM'e aktarılması; detection rules, simulated incidents, containment ve post-incident documentation.</sub>

⏳ Project 6: Production-Grade Cloud AI Security Architecture — <sub>AWS, Terraform, Kubernetes, managed AI services, private networking, IAM, encryption, centralized logging, high availability ve AI governance kontrolleriyle uçtan uca güvenli platform.</sub>

</details>

🏆 Milestone Portfolio Projects

The following three progressively advanced portfolio projects are planned to validate the knowledge acquired throughout the roadmap.

Each project combines concepts from multiple modules and is designed around production-oriented security, automation, reliability, validation, and documentation practices.

Project Map

Project

Planned Milestone

Project 1 — Hardened & Audited Linux Server Infrastructure

Level 1 tamamlandıktan sonra.

Project 2 — Automated Provisioning of a Production-Oriented Kubernetes Environment

Level 2 tamamlandıktan sonra.

Project 3 — Resilient Cloud-Native GitOps & Observability Platform

Level 3 ve Level 4 tamamlandıktan sonra.

Detailed Project Specifications

<details open>
<summary><strong>🛡️ Project 1: Hardened & Audited Private Linux Server Infrastructure — ⏳ Planned</strong></summary>

<br>

Türkçe Özet: Networking, Linux administration, users and permissions, systemd, Linux networking, nftables, SSH hardening, Fail2Ban, auditd, logging, backup, Bash automation, incident validation ve teknik dokümantasyon alanlarını birleştiren kapsamlı bir sunucu güvenliği projesi.

Field

Details

Focus

Host Hardening, Stateful Firewalling, Security Auditing and Log-Based Abuse Mitigation

Roadmap Milestone

Level 1 tamamlandıktan sonra.

Detailed Timeline

At the end of Level 1 — Module 03

Overview

An Ubuntu Server instance running on Amazon EC2 and configured using layered host-hardening practices.

The project focuses on reducing the exposed attack surface, enforcing a default-deny inbound firewall policy, securing administrative access, detecting repeated authentication failures, and recording security-relevant events for auditing and incident analysis.

The project is intended as a security-focused learning environment and portfolio implementation. It is not presented as a highly available or universally production-ready architecture.

Core Technology Stack

Operating System: Ubuntu Server on Amazon EC2

Firewall Framework: nftables, with iptables concepts covered where relevant

Automated Abuse Mitigation: Fail2Ban

Audit Framework: Linux audit subsystem and auditd

System Logging: systemd journal and Linux log files

Automation: Bash reporting and validation scripts

Cloud Visibility: Amazon CloudWatch where appropriate

Recovery: Amazon EBS snapshots and documented restore procedures

Planned Security and Operational Controls

Default-Deny Inbound Policy: Unsolicited inbound traffic is denied by default. Only explicitly required services are allowed.

Restricted Administrative Access: SSH is limited to approved source addresses or a dedicated management path.

SSH Hardening: Direct root login and password authentication are disabled. Administrative access uses public-key authentication.

Reduced Attack Surface: Unnecessary services, packages and exposed ports are removed or disabled.

Automated Authentication Abuse Mitigation: Fail2Ban analyzes authentication logs and temporarily blocks addresses that exceed defined failure thresholds.

Security Auditing: auditd rules record selected security-relevant events involving authentication, privilege escalation and sensitive files.

Firewall and Authentication Visibility: Logs are configured and reviewed without assuming that logging alone prevents attacks.

Automated Reporting: Bash scripts summarize authentication failures, firewall events, banned addresses and selected audit records.

Recovery Validation: Snapshot creation and restore procedures are tested and documented.

</details>

<details>
<summary><strong>📦 Project 2: Declarative Provisioning of a Production-Oriented Kubernetes Environment — ⏳ Planned</strong></summary>

<br>

Türkçe Özet: Docker, secure images, Kubernetes workloads, Services, RBAC, NetworkPolicy, storage, probes, Terraform, remote state, Ansible, AWS VPC/EC2, automated validation ve operasyonel dokümantasyon alanlarını birleştiren otomatik altyapı projesi.

Field

Details

Focus

Cloud Provisioning, Infrastructure as Code, Configuration Management and Kubernetes Orchestration

Roadmap Milestone

Level 2 tamamlandıktan sonra.

Detailed Timeline

At the end of Level 2 — Module 06

Overview

An end-to-end infrastructure automation project that provisions AWS resources and configures a multi-node Kubernetes learning cluster consisting of one control-plane node and two worker nodes without relying on repeated manual console configuration.

The project demonstrates production-oriented practices such as Infrastructure as Code, automated node configuration, network isolation, secure container builds and declarative orchestration.

A cluster with one control-plane node is not a highly available control-plane architecture. This limitation will be documented explicitly and evaluated separately from the automation objectives.

Core Technology Stack

Infrastructure as Code: Terraform

Configuration Management: Ansible

Container Runtime: containerd

Cluster Bootstrap: kubeadm

Cloud Infrastructure: AWS VPC, subnets, EC2 and S3

Orchestration: Kubernetes

Container Build: Docker or another OCI-compatible build workflow

Networking: A selected CNI with documented NetworkPolicy support

Planned Deployment Architecture

Infrastructure Provisioning: Terraform provisions a custom AWS VPC, public and private subnets, compute resources and supporting infrastructure through reusable modules.

Remote State Management: Terraform state is stored in Amazon S3 with public access blocked, encryption, versioning, restricted IAM permissions and native state locking.

Automated Node Preparation: Ansible configures kernel modules, required sysctl values, packages, Kubernetes prerequisites and containerd.

Automated Cluster Bootstrap: Ansible initializes the control-plane node with kubeadm, handles the join process securely and connects worker nodes to the cluster.

Secure Container Builds: Application workloads use multi-stage builds, minimal base images and non-root execution where supported.

Kubernetes Authorization: RBAC permissions are designed according to least-privilege principles.

Network Isolation: NetworkPolicies restrict unnecessary pod traffic when supported and enforced by the selected CNI.

Validation: Node health, workload scheduling, networking, policy enforcement and recovery behavior are tested and documented.

Rebuild Test: The environment is recreated from version-controlled Terraform and Ansible definitions to identify undocumented manual dependencies.

Known Limitations: Lack of a highly available control plane, single-region scope and learning-environment cost constraints are documented explicitly.

</details>

<details>
<summary><strong>🚀 Project 3: Resilient Cloud-Native GitOps & Observability Platform — ⏳ Planned</strong></summary>

<br>

Türkçe Özet: CI/CD, DevSecOps security gates, software supply-chain security, GitOps, Kubernetes, Prometheus, Grafana, Loki, OpenTelemetry, alerting, SLOs, AWS Multi-AZ, IAM, backup, disaster recovery, resilience testing, runbooks, postmortems, performance analysis ve AI-assisted operational workflows alanlarını birleştiren kapsamlı platform mühendisliği projesi.

Field

Details

Focus

DevSecOps, Declarative Delivery, SRE Practices, Observability and Multi-AZ Resilience

Roadmap Milestone

Level 3 ve Level 4 tamamlandıktan sonra.

Detailed Timeline

At the end of Level 4 — Module 11

Overview

The final milestone project implements an end-to-end automated software delivery and operations workflow.

Application code is validated through CI controls, packaged as container images, published to a controlled registry, deployed declaratively through GitOps, observed through metrics, logs and traces, and evaluated through controlled recovery scenarios.

The primary architecture is planned as a single-region Amazon EKS environment distributed across multiple Availability Zones. Multi-Region disaster recovery may be evaluated as a separate extension rather than claimed by default.

Core Technology Stack

Cloud Infrastructure: AWS

Kubernetes Runtime: Amazon EKS

Infrastructure as Code: Terraform

CI Platform: GitHub Actions

Cloud Authentication: GitHub Actions OpenID Connect with short-lived AWS credentials

Container Registry: Amazon ECR

GitOps Engine: Argo CD

Code Quality & Static Analysis: SonarQube

Secret Scanning: Gitleaks or TruffleHog

Container Image Scanning: Trivy

Policy Enforcement: Kyverno or an equivalent Kubernetes policy engine

Secrets Integration: External Secrets Operator with AWS Secrets Manager

Metrics: Prometheus

Visualization: Grafana

Logging: Grafana Loki

Tracing: OpenTelemetry with Grafana Tempo or another compatible backend

Alerting: Prometheus Alertmanager

Progressive Delivery: Argo Rollouts where appropriate

Backup and Recovery: Velero and cloud-native data protection mechanisms where applicable

Planned Engineering Principles

DevSecOps Pipeline: The CI workflow performs code validation, secret-leak detection, code quality analysis, container image scanning and controlled artifact publishing.

Short-Lived Cloud Authentication: GitHub Actions accesses AWS through OpenID Connect instead of long-lived static credentials.

GitOps Architecture: Argo CD reconciles Kubernetes resources with version-controlled declarative manifests and surfaces configuration drift.

Environment Separation: Staging and production-like environments are logically separated, with stronger account or cluster separation considered as an extension.

Secrets Management: Sensitive values are stored outside Git and synchronized through controlled secret-management integrations.

Policy Enforcement: Admission policies prevent selected insecure workload configurations without being treated as a complete security solution.

Golden Signals Observability: Prometheus and Grafana expose latency, traffic, errors and saturation signals.

Centralized Logging: Loki collects application and platform logs for troubleshooting and incident analysis.

Distributed Tracing: OpenTelemetry captures and exports trace data across supported services.

Actionable Alerting: Alertmanager routes grouped and prioritized alerts through defined notification channels.

Progressive Delivery: Selected workloads use controlled rollout strategies with measurable promotion and rollback conditions.

Workload Recovery: Kubernetes controllers and health checks replace failed workload instances under supported conditions; this does not guarantee recovery from every application or dependency failure.

Resilience Validation: Controlled failure scenarios measure recovery behavior, blast radius and operational limitations.

Multi-AZ Deployment: Workloads and supported cloud resources are distributed across Availability Zones to reduce selected single points of failure.

Backup and Restore: Application state and Kubernetes resources are protected through documented backup and recovery procedures.

SRE Practices: Service Level Indicators, Service Level Objectives, error budgets, incident response and blameless postmortems are incorporated into the project.

Rebuild Validation: Infrastructure and platform components are rebuilt from version-controlled repositories without relying on undocumented deployment steps.

</details>

📝 Documentation Approach

Each completed topic may include, depending on its requirements:

Turkish and English technical documentation

Architecture and data-flow diagrams

Security risks and defensive controls

Hardening recommendations and their limitations

Command and configuration examples

Troubleshooting methodology

Practical validation steps

Production and laboratory environment distinctions

Official references and standards

The goal is not to create a separate large laboratory project for every topic.

Small exercises are included when they improve understanding. Broader implementations are consolidated into milestone portfolio projects so that related technologies can be demonstrated together within realistic system boundaries.

⚠️ Project Status Notice

This repository represents an ongoing learning process.

Completed entries describe topics that have been studied and reviewed. Planned modules and project descriptions define the intended scope of future work.

Technologies, architectures, implementation details and project boundaries may be revised as the systems are built, tested, measured and documented.

A planned feature should not be interpreted as an already implemented or validated capability.

<p align="center"><sub>Built as a long-term technical learning journal. Scope and implementation details evolve through study, testing, measurement, and documentation.</sub></p>
