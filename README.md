<a id="top"></a>

<div align="center">

🏆 Systems, DevOps & Platform Engineering Learning Roadmap

A bilingual, hands-on learning journal for building secure, automated, observable and reliable infrastructure.



</div>

<a id="about"></a>

📘 About This Repository

🇬🇧 English

This repository is an ongoing learning journal and practical roadmap created to build a strong technical foundation in system administration, networking, cybersecurity, DevSecOps, cloud computing, Site Reliability Engineering, and platform engineering.

The roadmap combines technical documentation, security-focused analysis, practical validation exercises, and progressively advanced portfolio projects. Content is prepared in both Turkish and English and is updated throughout the learning process.

🇹🇷 Türkçe

Bu depo; sistem yönetimi, ağ teknolojileri, siber güvenlik, DevSecOps, bulut bilişim, Site Reliability Engineering ve platform mühendisliği alanlarında güçlü bir teknik temel oluşturmak amacıyla hazırladığım, devam eden bir öğrenme günlüğü ve uygulama yol haritasıdır.

Yol haritası; teknik dokümantasyonlar, güvenlik odaklı değerlendirmeler, uygulamalı doğrulama çalışmaları ve seviyeler ilerledikçe geliştirilecek kapsamlı portföy projelerinden oluşmaktadır. İçerikler Türkçe ve İngilizce olarak hazırlanmakta ve öğrenme süreci boyunca düzenli olarak güncellenmektedir.

<a id="current-progress"></a>

📍 Current Progress

Field

Current Status

Current Level

🌐 Level 1 — Foundations of Systems & Security

Current Module

Module 01 — Advanced Networking for Platforms

Repository Status

🚧 Actively Maintained

Documentation Languages

🇹🇷 Turkish · 🇬🇧 English

Status Legend

Status

Meaning

✅

Completed

🚧

In Progress

⏳

Planned

[!NOTE]Completion indicators represent the current learning progress. Direct links will be added as the corresponding topic documents are committed to the repository.

🧭 Quick Navigation

Roadmap

Portfolio & Documentation

🌐 Level 1 — Systems & Security

🏆 Milestone Portfolio Projects

📦 Level 2 — Applications, Containers & IaC

📝 Documentation Approach

🚀 Level 3 — Software, Automation & Observability

⚠️ Project Status Notice

☁️ Level 4 — Cloud, Reliability & Operations

⬆️ Back to Top

🤖 Level 5 — AI Systems Security & MLSecOps



<a id="roadmap-overview"></a>

🗂️ Roadmap Overview

Level

Scope

Modules

Status

1

Systems, Networking, Linux & Security

01–03

🚧

2

Web, Databases, Containers, Kubernetes & IaC

04–07

⏳

3

CI/CD, Automation, Software Engineering & Observability

08–11

⏳

4

AWS, SRE, Performance & Intelligent Operations

12–15

⏳

5

AI Security, MLSecOps, Red Teaming & Governance

16–25

⏳

🗺️ Learning Roadmap — Öğrenme Yol Haritası

<a id="level-1"></a>

<details open>
<summary><strong>🌐 Level 1: Foundations of Systems & Security — 🚧 In Progress</strong></summary>

🌐 Module 01: Advanced Networking for Platforms — 🚧 In Progress

✅ Physical & Data Link Layers — <sub><em>OSI ve TCP/IP modelleri, encapsulation, fiziksel sinyaller, Ethernet frame yapısı, MAC addressing, switching, MTU, interface counters ve Layer 1/2 troubleshooting.</em></sub>

✅ ARP & Local Network Dynamics — <sub><em>ARP çalışma mantığı, neighbor table, ARP cache durumları, gratuitous ARP, proxy ARP, ARP spoofing riskleri ve Dynamic ARP Inspection.</em></sub>

✅ VLAN & VXLAN — <sub><em>Layer 2 segmentasyonu, access ve trunk portlar, IEEE 802.1Q tagging, VLAN hopping riskleri, VXLAN overlay mimarisi, VTEP ve VNI kavramları.</em></sub>

✅ Network Routing & Subnetting — <sub><em>IPv4 adresleme, CIDR, subnetting, routing table, default gateway, static ve dynamic routing temelleri, OSPF/BGP genel mantığı ve IP fragmentation.</em></sub>

✅ Transport Layer & Traffic Control — <sub><em>TCP ve UDP, portlar ve socket'ler, TCP three-way handshake, sequence ve acknowledgment numbers, flow control, congestion control, retransmission ve connection termination.</em></sub>

✅ Application Layer Services — <sub><em>DNS kayıt türleri, DNS resolution, DNS spoofing riskleri, DHCP DORA süreci, DHCP Snooping, HTTP/HTTPS ve TLS handshake.</em></sub>

✅ Load Balancing & Proxy Concepts — <sub><em>Forward proxy ve reverse proxy, Layer 4 ve Layer 7 load balancing, health checks, session persistence, TLS termination ve TLS passthrough.</em></sub>

🐧 Module 02: Enterprise Linux System Administration — 🚧 In Progress

✅ Linux Architecture, Boot Process & FHS — <sub><em>Kernel, initramfs, GRUB, systemd, boot süreci ve Linux dizin hiyerarşisi; /etc, /var, /home, /opt, /tmp, /proc, /sys ve /dev.</em></sub>

✅ Shell, CLI & Stream Processing — <sub><em>Standard input/output, pipes, redirection, environment variables ve grep, awk, sed, cut, sort, uniq, find, xargs gibi araçlar.</em></sub>

✅ User & Permission Management — <sub><em>User ve group yönetimi, UID/GID, chmod, chown, umask, POSIX ACLs, sudoers ve least-privilege yaklaşımı.</em></sub>

✅ Process & Resource Management — <sub><em>Process lifecycle, signals, foreground/background jobs, ps, top, htop, kill, file descriptors, memory ve CPU resource takibi.</em></sub>

✅ systemd, Services & Scheduled Tasks — <sub><em>Unit files, service dependencies, systemctl, journalctl, restart policies, systemd timers ve cron.</em></sub>

✅ Storage, Filesystems & LVM — <sub><em>Partitioning, block devices, ext4, XFS, mount işlemleri, /etc/fstab, inode yapısı, swap, LVM, RAID ve disk kapasite yönetimi.</em></sub>

✅ Linux Networking & Troubleshooting — <sub><em>Interface yönetimi, routing, DNS çözümleme, /etc/hosts, systemd-resolved, ip, ss, tcpdump, dig, curl, nc ve network namespaces.</em></sub>

✅ Package & Repository Management — <sub><em>apt, dpkg, dnf, rpm, repository yapıları, package signatures, dependency management ve güvenli güncelleme süreçleri.</em></sub>

✅ Logging, Time & Operational Maintenance — <sub><em>/var/log, systemd journal, rsyslog, log rotation, NTP, chrony, backup, restore validation ve bakım planları.</em></sub>

🛡️ Module 03: Security Engineering & Incident Management — ⏳ Planned

⏳ Security Foundations & Risk Management — <sub><em>CIA triad, authentication, authorization, accountability, assets, threats, vulnerabilities, risk, attack surface ve defense in depth.</em></sub>

⏳ Threat Modeling & Attack Lifecycle — <sub><em>Trust boundaries, data-flow diagrams, STRIDE, attack trees, Cyber Kill Chain, MITRE ATT&CK ve saldırı yaşam döngüsü.</em></sub>

⏳ Network and Availability Threats — <sub><em>MITM, spoofing, session hijacking, DNS ve ARP saldırıları, DoS/DDoS, SYN flood ve hizmet kesintisi senaryoları.</em></sub>

⏳ Malware & Endpoint Defense — <sub><em>Worm, Trojan, ransomware, rootkit, botnet, fileless malware, sandboxing, hashes, EDR ve endpoint hardening.</em></sub>

⏳ Firewalling & Network Defense — <sub><em>Stateful ve stateless filtering, Netfilter, iptables, nftables, connection tracking, NAT, rate limiting ve segmentation.</em></sub>

⏳ Cryptography, PKI & SSH Security — <sub><em>Symmetric ve asymmetric cryptography, hashing, digital signatures, certificates, SSH key pairs, host keys ve sshd_config hardening.</em></sub>

⏳ Linux Hardening & Vulnerability Management — <sub><em>Attack-surface reduction, CIS Benchmarks, CVE/CVSS, patching, service inventory, permissions, PAM, SELinux ve AppArmor temelleri.</em></sub>

⏳ System Auditing, Logging & SIEM — <sub><em>/var/log, systemd journal, auditd, log bütünlüğü, merkezi loglama, SIEM, detection rules ve security-event analysis.</em></sub>

⏳ Log-Based Abuse Mitigation — <sub><em>Fail2Ban çalışma mantığı, authentication log analizi, geçici engelleme, rate limiting ve aracın güvenlik sınırlamaları.</em></sub>

⏳ Incident Response Fundamentals — <sub><em>Preparation, detection, triage, containment, eradication, recovery, evidence preservation, communication ve post-incident review.</em></sub>

<div align="right">

⬆️ Back to Top

</div>

</details>

<a id="level-2"></a>

<details>
<summary><strong>📦 Level 2: Application Infrastructure, Containers & Infrastructure as Code — ⏳ Planned</strong></summary>

Uygulama, Veri, Otomasyon ve Konteyner Altyapıları

Web uygulamalarının, API'lerin ve veri katmanlarının çalışma prensiplerinin öğrenildiği; konteyner tabanlı platformların ve kodla tanımlanan altyapıların oluşturulduğu seviyedir.

🌍 Module 04: Web, API & Database Foundations — ⏳ Planned

⏳ Web Architecture Fundamentals — <sub><em>Client-server modeli, request-response akışı, stateless ve stateful application yaklaşımları.</em></sub>

⏳ HTTP & HTTPS — <sub><em>HTTP methods, status codes, headers, content types, caching, HTTP/1.1, HTTP/2, HTTP/3 ve HTTPS.</em></sub>

⏳ TLS & Certificate Validation — <sub><em>TLS handshake, X.509 certificates, Certificate Authorities, SNI, certificate chains ve renewal süreçleri.</em></sub>

⏳ Cookies, Sessions & Authentication — <sub><em>Cookies, session management, authentication state, secure cookie attributes ve session security.</em></sub>

⏳ REST API Fundamentals — <sub><em>Resources, endpoints, HTTP methods, idempotency, pagination, versioning, rate limiting ve API error handling.</em></sub>

⏳ Tokens, JWT & OAuth 2.0 — <sub><em>Access tokens, refresh tokens, JWT yapısı, OAuth 2.0 akışları ve OpenID Connect temelleri.</em></sub>

⏳ Browser & API Security — <sub><em>CORS, CSRF, XSS, Host header risks, input validation, authentication ve authorization ayrımı.</em></sub>

⏳ Relational Database Fundamentals — <sub><em>PostgreSQL ve MySQL genel mimarisi, schemas, tables, primary/foreign keys ve normalization.</em></sub>

⏳ Transactions & Concurrency — <sub><em>ACID, transaction isolation levels, locks, deadlocks ve consistency kavramları.</em></sub>

⏳ Indexes & Query Performance — <sub><em>Index yapıları, query plans, full-table scans, latency ve temel query optimization.</em></sub>

⏳ Database Availability & Protection — <sub><em>Replication, backup, restore testing, encryption, access control ve credential management.</em></sub>

⏳ Caching & In-Memory Data Stores — <sub><em>Redis temelleri, cache-aside yaklaşımı, expiration, persistence ve cache invalidation problemleri.</em></sub>

📦 Module 05: Container Technologies — Docker Deep Dive — ⏳ Planned

⏳ Containerization vs Virtualization — <sub><em>Hypervisor tabanlı virtual machines ile operating-system-level containers arasındaki mimari farklar.</em></sub>

⏳ Linux Kernel Namespaces & Cgroups — <sub><em>Container isolation ve resource-control mekanizmalarının Linux kernel içindeki temelleri.</em></sub>

⏳ Container Runtime Internals — <sub><em>Docker Engine, containerd, runc, OCI specifications ve Kubernetes Container Runtime Interface ilişkisi.</em></sub>

⏳ Docker Core — <sub><em>Docker CLI, image ve container lifecycle, Dockerfile yazım kuralları, layers, build cache ve multi-stage builds.</em></sub>

⏳ Docker Storage — <sub><em>Writable layers, volumes, bind mounts, tmpfs, UID/GID sorunları ve veri kalıcılığı.</em></sub>

⏳ Docker Networking — <sub><em>Network namespaces, veth pairs, bridge networks, port publishing, NAT, container DNS ve service discovery.</em></sub>

⏳ Docker Compose — <sub><em>Çok konteynerli yerel geliştirme, test ve doğrulama ortamlarının tanımlanması.</em></sub>

⏳ Registries & Image Lifecycle — <sub><em>Docker Hub, GHCR, Amazon ECR, tags, digests, retention, signing ve image provenance.</em></sub>

⏳ Container Security & Image Hardening — <sub><em>Non-root containers, reduced capabilities, read-only filesystems, seccomp, minimal images ve Trivy/Grype taramaları.</em></sub>

⏳ Docker Operations & Troubleshooting — <sub><em>Logs, inspect, events, health checks, networking, storage, OOM ve safe-cleanup yaklaşımları.</em></sub>

☸️ Module 06: Kubernetes Orchestration & Platform Fundamentals — ⏳ Planned

⏳ Kubernetes Architecture — <sub><em>API Server, etcd, Scheduler, kube-controller-manager, cloud-controller-manager, kubelet, kube-proxy ve container runtime görevleri.</em></sub>

⏳ Kubernetes Core Objects — <sub><em>Pods, ReplicaSets, Deployments, StatefulSets, DaemonSets, Jobs ve CronJobs.</em></sub>

⏳ Scheduling & Resources — <sub><em>Requests, limits, QoS classes, affinity, anti-affinity, taints, tolerations ve pod placement.</em></sub>

⏳ Services & Service Discovery — <sub><em>ClusterIP, NodePort, LoadBalancer Services, EndpointSlices, kube-proxy ve CoreDNS.</em></sub>

⏳ Configuration & Secrets — <sub><em>ConfigMaps, Secrets, environment variables, mounted volumes ve Kubernetes Secrets nesnelerinin güvenlik sınırlamaları.</em></sub>

⏳ Kubernetes Storage — <sub><em>PersistentVolumes, PersistentVolumeClaims, StorageClasses, dynamic provisioning, access modes ve backup considerations.</em></sub>

⏳ Kubernetes Security & RBAC — <sub><em>ServiceAccounts, Roles, ClusterRoles, RoleBindings, Security Contexts, Pod Security Standards ve least privilege.</em></sub>

⏳ Traffic Management — <sub><em>Ingress Controllers, Gateway API, NetworkPolicy, service mesh, mTLS, retries ve circuit breaking.</em></sub>

⏳ Workload Health & Recovery — <sub><em>Startup, readiness ve liveness probes; restart behavior ve controller'ların desired state'i koruma yaklaşımı.</em></sub>

⏳ Scaling & Reliability — <sub><em>HPA, VPA kavramları, Cluster Autoscaler, PodDisruptionBudget, graceful shutdown ve controlled rollout stratejileri.</em></sub>

⏳ Cluster Lifecycle & Packaging — <sub><em>kubeadm, managed Kubernetes, upgrades, Helm, Kustomize, certificates ve etcd backup temelleri.</em></sub>

⏳ Kubernetes Observability & Troubleshooting — <sub><em>Events, logs, Metrics Server, Prometheus, CrashLoopBackOff, ImagePullBackOff, DNS ve volume failures.</em></sub>

⏳ GitOps & Declarative Delivery — <sub><em>Kubernetes kaynaklarının Argo CD veya Flux ile deklaratif biçimde yönetilmesi.</em></sub>

⚙️ Module 07: Infrastructure as Code & Configuration Management — ⏳ Planned

⏳ Infrastructure as Code Fundamentals — <sub><em>Declarative ve imperative yaklaşımlar, desired state, idempotency, drift, convergence ve repeatability.</em></sub>

⏳ Terraform Language & Workflow — <sub><em>Providers, resources, data sources, HCL, dependencies, init, plan, apply ve destroy.</em></sub>

⏳ Terraform Variables & Data Modeling — <sub><em>Variables, outputs, locals, lists, maps, objects, conditionals, count ve for_each.</em></sub>

⏳ Terraform Modules — <sub><em>Root ve child modules, reusable architecture, module boundaries, version pinning ve environment separation.</em></sub>

⏳ Terraform State Security — <sub><em>terraform.tfstate güvenliği, Amazon S3 remote state, native S3 state locking, encryption, versioning ve restricted IAM access.</em></sub>

⏳ Legacy State Locking Considerations — <sub><em>DynamoDB tabanlı locking yönteminin mevcut eski yapıların geçişi veya uyumluluk ihtiyaçları kapsamında değerlendirilmesi.</em></sub>

⏳ Terraform Lifecycle & Testing — <sub><em>Lifecycle rules, import, moved blocks, static analysis, Policy as Code, security scanning ve destructive-change protection.</em></sub>

⏳ Ansible Fundamentals — <sub><em>Agentless architecture, SSH connections, inventories, modules, facts, ad-hoc commands ve privilege escalation.</em></sub>

⏳ Ansible Playbooks & Roles — <sub><em>Playbooks, handlers, templates, variables, loops, conditionals, roles ve idempotent service management.</em></sub>

⏳ Ansible Vault & Operational Safety — <sub><em>Hassas değerlerin şifrelenmesi, secret injection, least privilege, serial changes ve rollback limitations.</em></sub>

⏳ Git & Version Control Integration — <sub><em>Commit disiplini, pull requests, protected branches, code review, GitFlow ve trunk-based development.</em></sub>

⏳ Infrastructure Validation — <sub><em>Formatting, validation, linting, policy checks, controlled plan/apply ve CI/CD entegrasyonu.</em></sub>

⏳ Terraform & Ansible Integration — <sub><em>Provisioning ve configuration-management sorumluluklarının ayrılması, dynamic inventory ve doğrulama süreçleri.</em></sub>

<div align="right">

⬆️ Back to Top

</div>

</details>

<a id="level-3"></a>

<details>
<summary><strong>🚀 Level 3: Software Engineering, Automation, CI/CD & Observability — ⏳ Planned</strong></summary>

Yazılım Teslimi, Otomasyon ve Gözlemlenebilirlik

Altyapı ve platform yazılımlarının sürdürülebilir biçimde geliştirildiği; teslim süreçlerinin otomatikleştirildiği ve sistemlerin metrics, logs ve traces kullanılarak gözlemlendiği seviyedir.

🔄 Module 08: CI/CD Pipelines & GitOps — ⏳ Planned

⏳ CI/CD Core Concepts — <sub><em>Continuous Integration, Continuous Delivery ve Continuous Deployment arasındaki farklar.</em></sub>

⏳ Git Workflows & Pipeline Triggers — <sub><em>Branching strategies, pull requests, protected branches, tags, releases ve event-based triggers.</em></sub>

⏳ Pipeline Platforms — <sub><em>GitHub Actions veya GitLab CI ile workflows, jobs, runners, stages, artifacts ve environment yönetimi.</em></sub>

⏳ Build, Test & Artifact Pipelines — <sub><em>Unit, integration ve end-to-end tests, caching, matrices, reports ve reproducible builds.</em></sub>

⏳ Container Build & Delivery — <sub><em>Docker/BuildKit, image tagging, commit SHA kullanımı, registry authentication, scanning, signing ve provenance.</em></sub>

⏳ DevSecOps & Security Gates — <sub><em>SonarQube, Gitleaks, TruffleHog, Trivy, SAST, SCA, IaC scanning ve controlled policy enforcement.</em></sub>

⏳ Artifact Management — <sub><em>Docker Hub, GitHub Container Registry, Amazon ECR ve JFrog Artifactory gibi registry ve artifact-repository çözümleri.</em></sub>

⏳ Secure Pipeline Authentication — <sub><em>Uzun ömürlü cloud credentials yerine OpenID Connect ve kısa ömürlü kimlik bilgilerinin kullanılması.</em></sub>

⏳ Software Supply-Chain Security — <sub><em>SBOM, dependency trust, signing, Sigstore, SLSA ve artifact-integrity yaklaşımı.</em></sub>

⏳ Deployment Strategies — <sub><em>Rolling update, blue/green, canary, feature flags, progressive delivery ve rollback yaklaşımları.</em></sub>

⏳ GitOps Delivery — <sub><em>Application code ile deployment state'inin ayrılması, declarative manifests, reconciliation ve drift correction.</em></sub>

⏳ Pipeline Observability & Troubleshooting — <sub><em>Pipeline metrics, runner utilization, flaky tests, failures, retries, concurrency ve runbooks.</em></sub>

🐍 Module 09: Systems Automation & Scripting — Bash & Python — ⏳ Planned

⏳ Bash Scripting Fundamentals — <sub><em>Variables, conditionals, loops, functions, arguments, arrays, exit codes ve debugging.</em></sub>

⏳ Shell Safety Practices — <sub><em>Quoting, input validation, temporary files, cleanup traps ve set -Eeuo pipefail kullanımının bağlama bağlı değerlendirilmesi.</em></sub>

⏳ Regular Expressions & Text Processing — <sub><em>Regex, grep, sed, awk, cut, sort, uniq, tr, find, xargs, jq ve yq.</em></sub>

⏳ System Administration Automation — <sub><em>Backup, restore validation, disk monitoring, log rotation, service-health checks ve raporlama.</em></sub>

⏳ Python Fundamentals for Operations — <sub><em>Data types, functions, exceptions, virtual environments, type hints, logging ve configuration.</em></sub>

⏳ Python for Filesystems & Processes — <sub><em>os, pathlib, sys, shutil, subprocess, signals, permissions ve safe command execution.</em></sub>

⏳ API Automation — <sub><em>Authentication, pagination, timeout, retry, exponential backoff, rate limiting ve schema validation.</em></sub>

⏳ Network & Security Automation — <sub><em>DNS checks, TCP/UDP connectivity, certificate inspection, log parsing, firewall validation ve alert enrichment.</em></sub>

⏳ Secure Automation — <sub><em>Secrets yönetimi, least privilege, auditability, idempotency ve destructive-action protections.</em></sub>

⏳ Automation Quality & Packaging — <sub><em>Unit tests, mocking, linting, type checking, dependency pinning, CLI design ve CI integration.</em></sub>

💻 Module 10: Software Engineering for Infrastructure & Platform Teams — ⏳ Planned

⏳ Software Engineering Fundamentals — <sub><em>Requirements, maintainability, readability, testability ve technical-debt yönetimi.</em></sub>

⏳ Clean Code Principles — <sub><em>Naming, small functions, separation of concerns, error handling ve understandable abstractions.</em></sub>

⏳ SOLID Principles — <sub><em>Single Responsibility, Open/Closed, Liskov Substitution, Interface Segregation ve Dependency Inversion.</em></sub>

⏳ Design Patterns — <sub><em>Factory, Strategy, Adapter, Observer, Dependency Injection ve infrastructure automation kullanım örnekleri.</em></sub>

⏳ Testing Strategies — <sub><em>Unit, integration, end-to-end, contract ve smoke tests; mocks, stubs ve test doubles.</em></sub>

⏳ Refactoring & Code Review — <sub><em>Safe refactoring, code smells, pull-request reviews ve incremental improvement.</em></sub>

⏳ Application Architecture — <sub><em>Monolith, modular monolith, microservices, event-driven architecture ve architecture trade-offs.</em></sub>

⏳ Distributed-System Fundamentals — <sub><em>Partial failures, timeouts, retries, idempotency, consistency ve eventual consistency.</em></sub>

⏳ API & Service Design — <sub><em>Versioning, backward compatibility, error contracts, rate limits ve graceful degradation.</em></sub>

⏳ Configuration & Dependency Management — <sub><em>Configuration separation, feature flags, dependency pinning ve environment consistency.</em></sub>

⏳ Secure Software Development — <sub><em>Input validation, secrets, dependency riskleri, threat modeling ve secure defaults.</em></sub>

⏳ Engineering Documentation — <sub><em>Architecture Decision Records, diagrams, runbooks, API documentation ve operational handover.</em></sub>

📊 Module 11: Observability, Logging & Monitoring — ⏳ Planned

⏳ Observability Fundamentals — <sub><em>Metrics, logs, traces, events ve profiles arasındaki farklar ve birlikte kullanım biçimleri.</em></sub>

⏳ Monitoring Approaches — <sub><em>White-box, black-box, synthetic monitoring, health checks ve baselines.</em></sub>

⏳ The Four Golden Signals — <sub><em>Latency, traffic, errors ve saturation.</em></sub>

⏳ RED & USE Methods — <sub><em>Request-driven services ve infrastructure resources için gözlem yöntemleri.</em></sub>

⏳ Metrics & Prometheus — <sub><em>Prometheus architecture, service discovery, scraping, exporters, PromQL, recording ve alerting rules.</em></sub>

⏳ Grafana & Dashboard Engineering — <sub><em>Dashboards, panels, variables, annotations, thresholds, provisioning ve dashboard anti-patterns.</em></sub>

⏳ Centralized Logging — <sub><em>Elasticsearch tabanlı çözümler veya Grafana Loki ile log aggregation, parsing, retention ve analysis.</em></sub>

⏳ Distributed Tracing — <sub><em>OpenTelemetry instrumentation ve Collector; Jaeger veya Grafana Tempo gibi tracing backend'leri.</em></sub>

⏳ Alerting & Notification Engineering — <sub><em>Prometheus Alertmanager ile routing, grouping, deduplication, inhibition, silences ve escalation.</em></sub>

⏳ SLI, SLO & Reliability Monitoring — <sub><em>Availability, latency, error budgets, burn rate ve SLO-based alerting.</em></sub>

⏳ Platform & Kubernetes Observability — <sub><em>Host, container, node, pod, workload ve control-plane telemetry.</em></sub>

⏳ Security Observability — <sub><em>Authentication, authorization, audit, network, DNS, firewall ve cloud-security telemetry.</em></sub>

⏳ Operational Reliability — <sub><em>Actionable alerts, alert fatigue, missing telemetry, runbooks ve incident-response bağlantısı.</em></sub>

⏳ Observability Troubleshooting — <sub><em>Scrape failures, missing logs, trace gaps, cardinality explosions, storage ve query-performance sorunları.</em></sub>

<div align="right">

⬆️ Back to Top

</div>

</details>

<a id="level-4"></a>

<details>
<summary><strong>☁️ Level 4: Cloud Computing, Reliability & Intelligent Operations — ⏳ Planned</strong></summary>

Bulut, Dayanıklı Sistemler ve Akıllı Operasyon

Yüksek erişilebilirlik, dayanıklılık, ölçeklenebilirlik, güvenli erişim, performans, felaket kurtarma ve yapay zekâ destekli mühendislik ilkelerine dayalı sistemlerin ele alındığı seviyedir.

☁️ Module 12: Cloud Computing — AWS Focus — ⏳ Planned

⏳ Cloud Computing Foundations — <sub><em>On-premises ve cloud farkları, IaaS, PaaS, SaaS, elasticity, scalability ve Shared Responsibility Model.</em></sub>

⏳ AWS Global Infrastructure — <sub><em>Regions, Availability Zones, edge locations, regional ve global services.</em></sub>

⏳ Identity & Access Management — <sub><em>IAM users, groups, roles, policies, trust policies, STS ve Principle of Least Privilege.</em></sub>

⏳ AWS Networking & VPC Architecture — <sub><em>VPC, CIDR planning, public/private subnets, route tables, Internet Gateway, NAT Gateway ve VPC endpoints.</em></sub>

⏳ Network Security — <sub><em>Security Groups, Network ACLs, VPC Flow Logs, segmentation ve egress-control yaklaşımları.</em></sub>

⏳ Compute Services — <sub><em>Amazon EC2, AMIs, launch templates, user data, metadata service, Auto Scaling Groups ve instance lifecycle.</em></sub>

⏳ Load Balancing — <sub><em>Application Load Balancer, Network Load Balancer, listeners, target groups, health checks ve TLS.</em></sub>

⏳ Storage Services — <sub><em>Amazon S3, EBS ve EFS arasındaki kullanım, availability, durability, performance ve cost trade-off'ları.</em></sub>

⏳ Managed Databases — <sub><em>Amazon RDS, Aurora, DynamoDB ve ElastiCache genel mimarileri.</em></sub>

⏳ Serverless & Event-Driven Architecture — <sub><em>AWS Lambda, Amazon SQS, SNS, EventBridge, retries, DLQs ve idempotency.</em></sub>

⏳ Secrets & Key Management — <sub><em>AWS Secrets Manager, Systems Manager Parameter Store, AWS KMS ve envelope encryption.</em></sub>

⏳ Cloud Security Services — <sub><em>CloudTrail, Config, GuardDuty, Security Hub, Inspector, WAF, Shield ve Macie temelleri.</em></sub>

⏳ Monitoring & Auditing — <sub><em>CloudWatch metrics, logs, alarms, dashboards, CloudTrail events ve centralized logging.</em></sub>

⏳ High Availability & Disaster Recovery on AWS — <sub><em>Multi-AZ, Multi-Region, backup, snapshots, replicas, Route 53 failover, RTO ve RPO.</em></sub>

⏳ Governance & Cost Management — <sub><em>AWS Organizations, OUs, SCPs, tagging, budgets, Cost Explorer, quotas ve guardrails.</em></sub>

⏳ AWS & Infrastructure as Code Integration — <sub><em>Terraform, remote state, OIDC identities, environment promotion ve drift detection.</em></sub>

🏛️ Module 13: Site Reliability Engineering & High Availability — ⏳ Planned

⏳ SRE Foundations & Service Ownership — <sub><em>SRE ve DevOps ilişkisi, service boundaries, ownership, operational readiness ve reliability as a product feature.</em></sub>

⏳ SLI, SLO, SLA & Error Budgets — <sub><em>Service indicators, objectives, external commitments, measurement windows ve burn rate.</em></sub>

⏳ Toil Reduction & Reliability Automation — <sub><em>Repetitive operational work, safe automation, self-service, guardrails ve human-in-the-loop.</em></sub>

⏳ Monitoring, Alerting & On-Call — <sub><em>Golden Signals, actionable alerts, severity, routing, escalation, handoffs ve on-call health.</em></sub>

⏳ Incident Management — <sub><em>Incident detection, declaration, triage, Incident Commander, stabilization, communication ve recovery.</em></sub>

⏳ Blameless Postmortems — <sub><em>Timeline, impact, contributing factors, root-cause analysis, corrective actions ve learning culture.</em></sub>

⏳ Resilience Patterns — <sub><em>Timeouts, retries, exponential backoff, jitter, circuit breakers, bulkheads, rate limiting ve load shedding.</em></sub>

⏳ High Availability Architectures — <sub><em>Redundancy, single points of failure, active-passive, active-active, quorum, replication ve DNS failover.</em></sub>

⏳ Disaster Recovery & Business Continuity — <sub><em>RTO, RPO, backup/restore, Pilot Light, Warm Standby, Active-Active ve recovery testing.</em></sub>

⏳ Chaos Engineering — <sub><em>Yetkili ortamlarda kontrollü, hipotez tabanlı failure experiments, abort conditions ve blast-radius management.</em></sub>

⏳ Release Engineering & Change Reliability — <sub><em>Rolling, blue/green, canary, feature flags, migration safety ve rollback.</em></sub>

⏳ Operational Readiness — <sub><em>Runbooks, dashboards, alerts, deployment validation, capacity ownership ve recovery plans.</em></sub>

⚡ Module 14: Performance Engineering & Capacity Analysis — ⏳ Planned

⏳ Performance Engineering Fundamentals — <sub><em>Latency, throughput, concurrency, utilization, saturation ve bottleneck kavramları.</em></sub>

⏳ CPU Performance Analysis — <sub><em>CPU utilization, load average, run queues, context switches, user/system time ve CPU profiling.</em></sub>

⏳ Memory Performance Analysis — <sub><em>Memory allocation, cache, swap, page faults, memory pressure, leaks ve OOM behavior.</em></sub>

⏳ Storage & I/O Performance — <sub><em>IOPS, throughput, latency, queue depth, filesystem overhead ve disk saturation.</em></sub>

⏳ Network Performance Analysis — <sub><em>Bandwidth, packet loss, jitter, retransmissions, connection limits ve socket queues.</em></sub>

⏳ Application Profiling — <sub><em>Hot paths, function-level profiling, memory allocation, lock contention ve flame graphs.</em></sub>

⏳ Database Performance — <sub><em>Query latency, indexes, execution plans, connection pools, locks ve slow-query analysis.</em></sub>

⏳ Load, Stress & Soak Testing — <sub><em>Normal load, peak load, breaking points, long-duration tests ve test-environment limitations.</em></sub>

⏳ Benchmarking Methodology — <sub><em>Baselines, controlled experiments, repeatability, warm-up effects ve misleading benchmark risks.</em></sub>

⏳ Capacity Planning — <sub><em>Growth forecasting, resource headroom, seasonality, quotas ve scaling decisions.</em></sub>

⏳ Backpressure & Queue Management — <sub><em>Queue depth, concurrency limits, rate limiting ve overload behavior.</em></sub>

⏳ Performance Troubleshooting — <sub><em>Metrics, logs, traces ve profiles kullanılarak katmanlı bottleneck analizi.</em></sub>

⏳ Performance vs Cost Trade-offs — <sub><em>Vertical ve horizontal scaling, overprovisioning, efficiency ve cloud-cost etkileri.</em></sub>

🤖 Module 15: AI-Assisted Engineering & Operational Intelligence — ⏳ Planned

⏳ AI-Assisted Engineering Fundamentals — <sub><em>Yapay zekâ araçlarının yazılım, sistem, cloud ve güvenlik mühendisliğinde doğru kullanım alanları.</em></sub>

⏳ Prompt Design for Engineers — <sub><em>Gereksinim, bağlam, kısıt, doğrulama kriteri ve beklenen çıktı formatının açık tanımlanması.</em></sub>

⏳ AI-Assisted Code Generation — <sub><em>Bash, Python, Terraform, Ansible ve Kubernetes manifest taslaklarının kontrollü üretilmesi.</em></sub>

⏳ AI-Assisted Code Review — <sub><em>Logic errors, security risks, maintainability issues, race conditions ve missing error handling kontrolleri.</em></sub>

⏳ Infrastructure Review — <sub><em>Terraform plans, IAM policies, Kubernetes manifests ve network rules için AI destekli inceleme.</em></sub>

⏳ Log & Incident Analysis — <sub><em>Log özetleme, timeline oluşturma, correlation, hypothesis generation ve incident triage desteği.</em></sub>

⏳ AI-Assisted Troubleshooting — <sub><em>Hata mesajları, metrics, traces ve system outputs üzerinden olası nedenlerin önceliklendirilmesi.</em></sub>

⏳ Security Analysis & Threat Hunting — <sub><em>Detection-rule taslakları, IOC enrichment, suspicious-pattern analysis ve false-positive değerlendirmesi.</em></sub>

⏳ Documentation & Knowledge Management — <sub><em>README, runbook, postmortem, architecture documentation ve technical-summary üretimi.</em></sub>

⏳ Output Verification — <sub><em>AI tarafından üretilen komut, kod, policy ve teknik iddiaların bağımsız olarak doğrulanması.</em></sub>

⏳ Hallucination & Context Risks — <sub><em>Uydurulmuş komutlar, eski bilgiler, yanlış varsayımlar ve eksik bağlam nedeniyle oluşabilecek riskler.</em></sub>

⏳ Sensitive Data Protection — <sub><em>Credentials, logs, source code, personal data ve şirket bilgilerinin AI araçlarına aktarılma riskleri.</em></sub>

⏳ Human Approval & Safety Boundaries — <sub><em>Production değişiklikleri, destructive commands, security decisions ve automated actions için insan onayı.</em></sub>

⏳ AI-Augmented Operations — <sub><em>Alert enrichment, anomaly summarization, runbook suggestions ve controlled remediation yaklaşımları.</em></sub>

⏳ Responsible AI Use — <sub><em>Privacy, auditability, intellectual property, bias, accountability ve organizational-policy gereksinimleri.</em></sub>

<div align="right">

⬆️ Back to Top

</div>

</details>

<a id="level-5"></a>

<details>
<summary><strong>🤖 Level 5: AI Systems Security, MLSecOps & Governance — ⏳ Planned</strong></summary>

Yapay Zekâ Sistemleri Güvenliği, Güvenli Model Yaşam Döngüsü ve AI Yönetişimi

Yapay zekâ ve makine öğrenmesi sistemlerinin mimarisinin, tehdit yüzeylerinin ve yaşam döngüsünün incelendiği; LLM, RAG ve agent tabanlı uygulamaların güvenli biçimde geliştirildiği, dağıtıldığı, izlendiği ve yönetildiği ileri seviye uzmanlık alanıdır.

🧠 Module 16: AI, Machine Learning & LLM Foundations for Security Engineers — ⏳ Planned

⏳ AI & Machine Learning Foundations — <sub><em>Artificial Intelligence, Machine Learning, Deep Learning ve Generative AI arasındaki farklar; supervised, unsupervised, reinforcement ve self-supervised learning yaklaşımları.</em></sub>

⏳ AI System Lifecycle — <sub><em>Veri toplama, veri hazırlama, training, validation, testing, fine-tuning, deployment, inference, monitoring, retraining ve retirement aşamaları.</em></sub>

⏳ Model Architecture Fundamentals — <sub><em>Neural networks, parameters, weights, layers, loss functions, optimization, checkpoints ve model formats kavramları.</em></sub>

⏳ Large Language Model Fundamentals — <sub><em>Transformer mimarisi, attention, tokens, tokenization, context window, temperature, sampling ve hallucination kavramları.</em></sub>

⏳ Embeddings & Semantic Search — <sub><em>Embedding modelleri, similarity search, vector representations, indexing ve semantic-retrieval çalışma mantığı.</em></sub>

⏳ Retrieval-Augmented Generation — <sub><em>RAG ingestion, chunking, embedding, retrieval, reranking, context construction ve generation akışı.</em></sub>

⏳ Vector Databases — <sub><em>Vector indexes, metadata filtering, namespaces, document permissions, multi-tenancy ve vector-store erişim kontrolü.</em></sub>

⏳ AI Agents & Tool Use — <sub><em>Planning, reasoning loops, memory, tool calling, function calling, external APIs ve autonomous-action riskleri.</em></sub>

⏳ Model Context Protocol Fundamentals — <sub><em>MCP servers, clients, tools, resources, prompts, trust boundaries ve third-party integration riskleri.</em></sub>

⏳ Training, Fine-Tuning & Inference — <sub><em>Pre-training, supervised fine-tuning, instruction tuning, adapters, LoRA, quantization ve inference-serving farkları.</em></sub>

⏳ AI Security Terminology — <sub><em>Model, dataset, prompt, system prompt, guardrail, agent, inference endpoint, model registry ve evaluation kavramları.</em></sub>

🧭 Module 17: AI Threat Modeling & Security Architecture — ⏳ Planned

⏳ AI System Asset Identification — <sub><em>Models, datasets, prompts, embeddings, vector stores, APIs, agents, tools, credentials, logs ve model artifacts gibi korunması gereken varlıkların belirlenmesi.</em></sub>

⏳ AI Data-Flow Diagrams — <sub><em>Kullanıcı, uygulama, model provider, RAG pipeline, agent tools, databases ve external services arasındaki veri akışlarının çıkarılması.</em></sub>

⏳ Trust Boundaries — <sub><em>User input, retrieved content, model output, agent memory, tool responses ve third-party models arasındaki güven sınırlarının belirlenmesi.</em></sub>

⏳ AI Attack-Surface Analysis — <sub><em>Training, supply chain, model storage, inference API, application, RAG, agent ve monitoring katmanlarının saldırı yüzeyleri.</em></sub>

⏳ Threat Actors & Capabilities — <sub><em>External attackers, malicious users, insiders, compromised suppliers ve automated-abuse actors.</em></sub>

⏳ AI Misuse & Abuse Cases — <sub><em>Modelin yetkisiz, zararlı, maliyet artırıcı veya kurum politikasına aykırı amaçlarla kullanılması.</em></sub>

⏳ Threat Modeling Methods — <sub><em>STRIDE, attack trees, abuse cases ve AI sistemlerine uyarlanmış threat-modeling yaklaşımları.</em></sub>

⏳ OWASP LLM & Generative AI Risks — <sub><em>LLM uygulamalarına özgü güvenlik risklerinin mimari ve uygulama seviyesinde değerlendirilmesi.</em></sub>

⏳ MITRE ATLAS — <sub><em>Predictive AI, Generative AI ve Agentic AI sistemlerine yönelik saldırı taktikleri, teknikleri ve mitigations.</em></sub>

⏳ NIST AI Risk Management Framework — <sub><em>Govern, Map, Measure ve Manage fonksiyonlarının AI güvenlik risklerine uygulanması.</em></sub>

⏳ Security Requirements Engineering — <sub><em>Authentication, authorization, confidentiality, integrity, availability, privacy, auditability ve human-oversight gereksinimleri.</em></sub>

⏳ AI Security Architecture Patterns — <sub><em>Isolation, least privilege, defense in depth, zero trust, secure gateways ve controlled tool execution.</em></sub>

⏳ AI Risk Register — <sub><em>Risk owner, likelihood, impact, control, residual risk, acceptance ve remediation tracking.</em></sub>

🔐 Module 18: LLM, RAG & Agentic Application Security — ⏳ Planned

⏳ Direct Prompt Injection — <sub><em>Kullanıcının model davranışını veya sistem talimatlarını doğrudan değiştirmeye çalıştığı saldırılar.</em></sub>

⏳ Indirect Prompt Injection — <sub><em>Web pages, documents, emails, retrieved content veya external tool outputs içine yerleştirilmiş kötü amaçlı talimatlar.</em></sub>

⏳ System-Prompt Extraction — <sub><em>System prompt, hidden instructions, internal policies ve application logic bilgilerinin model üzerinden elde edilmesi.</em></sub>

⏳ Jailbreak Attacks — <sub><em>Modelin güvenlik ve kullanım sınırlamalarının crafted prompts, encoding, role-play veya multi-step interactions ile aşılması.</em></sub>

⏳ Sensitive Information Disclosure — <sub><em>Credentials, personal data, proprietary data, internal documents ve confidential context bilgilerinin model çıktılarında açığa çıkması.</em></sub>

⏳ Insecure Output Handling — <sub><em>Model çıktılarının doğrulanmadan shell, database, browser, HTML renderer veya downstream application tarafından çalıştırılması.</em></sub>

⏳ Excessive Agency — <sub><em>Agentlara gereğinden fazla sistem, dosya, network, database veya cloud yetkisi verilmesi.</em></sub>

⏳ Agent Identity & Authorization — <sub><em>Kullanıcı kimliği, agent identity, delegated permissions, service accounts ve end-to-end authorization kontrolleri.</em></sub>

⏳ Tool-Calling Security — <sub><em>Tool allowlists, parameter validation, schema enforcement, confirmation gates ve destructive-action restrictions.</em></sub>

⏳ MCP Server & Tool Security — <sub><em>Malicious MCP servers, tool impersonation, poisoned tool descriptions, overprivileged tools ve untrusted resources.</em></sub>

⏳ Agent Memory Security — <sub><em>Persistent memory, conversation history, long-term state ve stored context bilgilerinin zehirlenmesi veya sızdırılması.</em></sub>

⏳ Context Poisoning — <sub><em>Model context'inin yanlış, kötü amaçlı veya yanıltıcı bilgilerle manipüle edilmesi.</em></sub>

⏳ RAG Poisoning — <sub><em>Knowledge base, document store veya retrieved content içine manipüle edilmiş içerik yerleştirilmesi.</em></sub>

⏳ Retrieval Authorization — <sub><em>Kullanıcının yalnızca erişim yetkisi bulunan dokümanların RAG pipeline tarafından getirilmesi.</em></sub>

⏳ Vector Database Security — <sub><em>Authentication, encryption, tenant isolation, metadata authorization, backup ve audit logging.</em></sub>

⏳ Cross-Tenant Data Leakage — <sub><em>Çok kiracılı sistemlerde kullanıcı veya kurum verilerinin başka tenant'lara sızması.</em></sub>

⏳ SSRF & External-Resource Abuse — <sub><em>Agent veya tool mekanizmalarının internal services, metadata endpoints veya yetkisiz URL'lere erişmek için kullanılması.</em></sub>

⏳ Code-Execution & Sandbox Risks — <sub><em>Model tarafından üretilen kodun kontrollü olmayan ortamlarda çalıştırılması ve sandbox escape riskleri.</em></sub>

⏳ Output Validation & Policy Enforcement — <sub><em>Structured output, schema validation, content filtering, allowlists ve deterministic security controls.</em></sub>

⏳ Human-in-the-Loop Controls — <sub><em>Para transferi, veri silme, production değişikliği, email gönderme ve credential işlemlerinde insan onayı.</em></sub>

⏳ Rate Limiting & Cost Controls — <sub><em>Token, request, concurrency, tool-call ve spending limitleriyle abuse ve denial-of-wallet risklerinin azaltılması.</em></sub>

⏳ Secure Conversation Design — <sub><em>Session isolation, context boundaries, history management ve conversation-state güvenliği.</em></sub>

🧬 Module 19: Adversarial Machine Learning & Model Security — ⏳ Planned

⏳ Adversarial ML Foundations — <sub><em>Model davranışının kötü amaçlı girdiler, training data veya model artifacts üzerinden manipüle edilmesi.</em></sub>

⏳ Evasion Attacks — <sub><em>Model inference sırasında girdilerin sınıflandırma veya karar mekanizmasını yanıltacak biçimde değiştirilmesi.</em></sub>

⏳ Adversarial Examples — <sub><em>İnsan için küçük görünen ancak model sonucunu önemli ölçüde değiştiren crafted inputs.</em></sub>

⏳ Training-Data Poisoning — <sub><em>Training veya fine-tuning verilerinin model davranışını bozmak amacıyla manipüle edilmesi.</em></sub>

⏳ Backdoor & Trojan Attacks — <sub><em>Belirli trigger girdilerinde saldırganın istediği sonucu üreten gizli model davranışları.</em></sub>

⏳ Fine-Tuning Attacks — <sub><em>Zararlı dataset, adapter veya fine-tuning süreçleri üzerinden model davranışının değiştirilmesi.</em></sub>

⏳ Model Extraction — <sub><em>Query-response interactions üzerinden hedef modelin davranışının veya işlevinin kopyalanması.</em></sub>

⏳ Model Inversion — <sub><em>Model outputs veya gradients üzerinden training data özelliklerinin yeniden oluşturulmaya çalışılması.</em></sub>

⏳ Membership Inference — <sub><em>Belirli bir verinin modelin training dataset'i içinde bulunup bulunmadığının tahmin edilmesi.</em></sub>

⏳ Model Fingerprinting — <sub><em>Model family, version, architecture veya provider bilgilerinin davranışsal yöntemlerle belirlenmesi.</em></sub>

⏳ Model Theft — <sub><em>Model weights, checkpoints, adapters, architecture bilgileri veya private endpoints'in yetkisiz biçimde ele geçirilmesi.</em></sub>

⏳ Privacy Attacks — <sub><em>Training data memorization, personal-data leakage ve reconstruction saldırıları.</em></sub>

⏳ Model Integrity Verification — <sub><em>Hashes, digital signatures, artifact validation ve trusted model-source kontrolleri.</em></sub>

⏳ Robustness Evaluation — <sub><em>Modelin adversarial, noisy, out-of-distribution ve manipulated inputs karşısındaki dayanıklılığının ölçülmesi.</em></sub>

⏳ Mitigation Limitations — <sub><em>Adversarial training, filtering, detection ve guardrail yöntemlerinin sınırlamalarının değerlendirilmesi.</em></sub>

🗃️ Module 20: AI Data Security, Privacy & Knowledge Protection — ⏳ Planned

⏳ AI Data Classification — <sub><em>Public, internal, confidential, restricted, personal ve regulated data türlerinin sınıflandırılması.</em></sub>

⏳ Dataset Inventory & Ownership — <sub><em>Dataset owner, source, purpose, access, retention ve approved-use bilgilerinin kayıt altına alınması.</em></sub>

⏳ Data Lineage & Provenance — <sub><em>Verinin kaynağı, dönüşümleri, training süreçleri ve model üzerindeki kullanımının izlenebilir olması.</em></sub>

⏳ Data Quality & Integrity — <sub><em>Eksik, yanlış, manipüle edilmiş, outdated veya malicious data'nın tespit edilmesi.</em></sub>

⏳ Training-Data Access Control — <sub><em>Dataset storage, notebooks, pipelines ve processing systems için least-privilege erişim.</em></sub>

⏳ Secrets & Credential Detection — <sub><em>API keys, passwords, certificates, tokens ve connection strings'in dataset, prompt veya notebook içinde bulunmasının engellenmesi.</em></sub>

⏳ Personal-Data Protection — <sub><em>PII detection, masking, anonymization, pseudonymization ve data minimization.</em></sub>

⏳ Encryption — <sub><em>Data at rest, data in transit, model artifacts, embeddings, backups ve logs için şifreleme.</em></sub>

⏳ RAG Knowledge-Base Protection — <sub><em>Document permissions, ingestion approval, integrity validation ve source trust değerlendirmesi.</em></sub>

⏳ Prompt & Response Privacy — <sub><em>Kullanıcı promptları, model outputs, feedback verileri ve conversation history'nin güvenli saklanması.</em></sub>

⏳ Logging & Retention Controls — <sub><em>Prompt, response ve tool-call loglarının içerik, erişim, saklama süresi ve silme politikaları.</em></sub>

⏳ Tenant Isolation — <sub><em>Dataset, vector namespace, memory ve retrieval context'in tenant bazında ayrılması.</em></sub>

⏳ Data-Poisoning Detection — <sub><em>Anomalous records, duplicate content, malicious instructions ve unexpected distribution değişimlerinin tespiti.</em></sub>

⏳ Data Usage Rights — <sub><em>Dataset licensing, consent, intellectual property ve permitted-use gereksinimleri.</em></sub>

⏳ Privacy-Enhancing Technologies — <sub><em>Differential privacy, federated learning, secure aggregation ve confidential-computing kavramları.</em></sub>

⏳ Secure Data Deletion — <sub><em>Dataset, embeddings, caches, model artifacts ve backups içindeki verilerin lifecycle'a uygun silinmesi.</em></sub>

🖥️ Module 21: Secure AI Infrastructure & Model Serving — ⏳ Planned

⏳ AI Infrastructure Architecture — <sub><em>Model gateway, inference server, vector database, object storage, agent runtime, API ve monitoring bileşenleri.</em></sub>

⏳ GPU & Accelerator Fundamentals — <sub><em>GPU resources, drivers, CUDA ecosystem, device access ve accelerator-sharing kavramları.</em></sub>

⏳ GPU Workload Security — <sub><em>GPU device permissions, workload isolation, driver security, resource quotas ve multi-tenant accelerator riskleri.</em></sub>

⏳ Secure Model Serving — <sub><em>Model loading, inference endpoints, batching, caching, streaming ve request-isolation güvenliği.</em></sub>

⏳ Containerized AI Workloads — <sub><em>Non-root containers, minimal images, reduced capabilities, read-only filesystems ve image scanning.</em></sub>

⏳ Kubernetes for AI Security — <sub><em>GPU scheduling, ServiceAccounts, RBAC, Pod Security Standards, NetworkPolicy ve workload isolation.</em></sub>

⏳ Model-Serving Platforms — <sub><em>vLLM, Triton, KServe, Ray Serve veya benzeri serving çözümlerinin güvenlik mimarisi.</em></sub>

⏳ Identity & Workload Authentication — <sub><em>Users, services, agents, workloads ve external APIs için güçlü kimlik doğrulama.</em></sub>

⏳ Secrets & Key Management — <sub><em>Model API keys, database credentials, encryption keys ve signing keys'in güvenli yönetimi.</em></sub>

⏳ Network Segmentation — <sub><em>Training, inference, management, storage ve monitoring ağlarının ayrılması.</em></sub>

⏳ API Gateway Security — <sub><em>Authentication, authorization, request limits, schema validation, TLS termination ve audit logging.</em></sub>

⏳ Service-to-Service Security — <sub><em>Mutual TLS, workload identities, certificate rotation ve service-mesh kontrolleri.</em></sub>

⏳ Multi-Tenancy Isolation — <sub><em>Compute, memory, network, model, cache ve data katmanlarında tenant ayrımı.</em></sub>

⏳ Model Registry Security — <sub><em>Model versioning, access control, signing, approval workflows ve artifact immutability.</em></sub>

⏳ Artifact Storage Security — <sub><em>Model weights, checkpoints, adapters, tokenizer files ve configuration dosyalarının korunması.</em></sub>

⏳ Secure Sandboxing — <sub><em>Untrusted code execution, interpreters, browser tools ve file-processing workloads için izolasyon.</em></sub>

⏳ Availability & Resource Exhaustion — <sub><em>GPU, CPU, memory, token, queue ve concurrency tüketimine karşı koruma.</em></sub>

⏳ Denial-of-Service & Denial-of-Wallet — <sub><em>Yüksek maliyetli inference requests, long-context attacks ve automated API abuse.</em></sub>

⏳ AI Infrastructure Observability — <sub><em>Inference latency, token usage, errors, resource consumption, model loading ve tool-call telemetry.</em></sub>

⏳ Backup, Recovery & Resilience — <sub><em>Model registry, vector database, configuration, prompts ve AI-service dependencies için recovery planları.</em></sub>

⏳ Cloud AI Shared Responsibility — <sub><em>Managed model services, customer data, IAM, network, logging ve application-security sorumluluklarının ayrılması.</em></sub>

🔗 Module 22: MLSecOps, AI Supply Chain & Secure AI Development Lifecycle — ⏳ Planned

⏳ MLSecOps Foundations — <sub><em>Machine Learning, MLOps, DevSecOps ve AI Security kontrollerinin tek yaşam döngüsünde birleştirilmesi.</em></sub>

⏳ Secure AI SDLC — <sub><em>Requirements, design, development, training, testing, release, deployment, monitoring ve retirement süreçlerine güvenlik eklenmesi.</em></sub>

⏳ AI Pipeline Security — <sub><em>Data ingestion, feature engineering, training, evaluation, packaging ve deployment pipeline'larının korunması.</em></sub>

⏳ Secure Development Environments — <sub><em>Notebooks, IDEs, experiment platforms, build runners ve development credentials güvenliği.</em></sub>

⏳ Dataset Supply-Chain Security — <sub><em>Third-party datasets, provenance, licenses, integrity checks ve poisoning riskleri.</em></sub>

⏳ Model Supply-Chain Security — <sub><em>Public models, adapters, checkpoints ve pre-trained artifacts için source-trust değerlendirmesi.</em></sub>

⏳ Third-Party Model Risk — <sub><em>Model provider, hosting service, API, license, data usage ve security-assurance kontrolleri.</em></sub>

⏳ Dependency Security — <sub><em>Python packages, container images, ML libraries, CUDA dependencies ve transitive-dependency riskleri.</em></sub>

⏳ Unsafe Model Serialization — <sub><em>Pickle ve benzeri formats üzerinden arbitrary-code-execution riskleri ve güvenli format değerlendirmesi.</em></sub>

⏳ Artifact Signing & Verification — <sub><em>Model, dataset, container ve configuration artifacts için digital signatures, hashes ve provenance.</em></sub>

⏳ AI Bill of Materials — <sub><em>Model, dataset, library, container, provider ve service dependencies'in envanterinin oluşturulması.</em></sub>

⏳ Reproducibility — <sub><em>Dataset version, code commit, configuration, dependency, model checkpoint ve environment kayıtlarının tutulması.</em></sub>

⏳ Secure CI/CD for AI — <sub><em>Automated tests, security scanning, policy checks, approvals ve protected deployment environments.</em></sub>

⏳ Security Evaluation Gates — <sub><em>Model veya AI uygulaması production'a alınmadan önce zorunlu güvenlik ve risk testleri.</em></sub>

⏳ Model Registry Promotion — <sub><em>Development, testing, staging ve production modelleri arasında kontrollü promotion.</em></sub>

⏳ Canary & Shadow Deployment — <sub><em>Yeni model sürümlerinin sınırlı trafik veya shadow mode ile güvenli doğrulanması.</em></sub>

⏳ Rollback & Model Revocation — <sub><em>Güvensiz veya bozuk model sürümlerinin hızla geri alınması ve kullanımının engellenmesi.</em></sub>

⏳ Drift & Unauthorized Change Detection — <sub><em>Model, dataset, configuration ve infrastructure üzerinde beklenmeyen değişikliklerin tespiti.</em></sub>

⏳ Vulnerability & Patch Management — <sub><em>AI frameworks, serving platforms, drivers, dependencies ve containers için güncelleme süreçleri.</em></sub>

⏳ Vendor & Acquisition Security — <sub><em>Satın alınan veya dışarıdan kullanılan AI sistemlerinin security requirements ve assurance evidence ile değerlendirilmesi.</em></sub>

🧪 Module 23: AI Security Testing, Evaluation & Red Teaming — ⏳ Planned

⏳ AI Security Test Planning — <sub><em>System scope, assets, threat model, test objectives, rules of engagement ve success criteria.</em></sub>

⏳ Prompt Fuzzing — <sub><em>Farklı prompt structures, encodings, languages, obfuscation ve edge cases ile otomatik güvenlik testi.</em></sub>

⏳ Jailbreak Testing — <sub><em>Model policy ve guardrail sınırlarının yetkili test ortamlarında değerlendirilmesi.</em></sub>

⏳ Prompt-Injection Testing — <sub><em>Direct ve indirect injection senaryolarının application, RAG ve agent katmanlarında denenmesi.</em></sub>

⏳ RAG Security Testing — <sub><em>Malicious documents, poisoned context, unauthorized retrieval ve source-manipulation senaryoları.</em></sub>

⏳ Agentic Security Testing — <sub><em>Tool abuse, privilege escalation, unsafe planning, memory poisoning ve autonomous-action testleri.</em></sub>

⏳ MCP Security Testing — <sub><em>Malicious server, poisoned tool metadata, unauthorized resource ve tool substitution senaryoları.</em></sub>

⏳ Model Extraction Testing — <sub><em>Query limits, output precision, rate controls ve model-copying risklerinin değerlendirilmesi.</em></sub>

⏳ Privacy Leakage Testing — <sub><em>Memorized data, personal information, training-data exposure ve membership-inference senaryoları.</em></sub>

⏳ Adversarial Input Testing — <sub><em>Evasion, adversarial examples, malformed data ve multimodal-input manipülasyonları.</em></sub>

⏳ Multimodal Security Testing — <sub><em>Image, audio, video, document ve hidden-instruction içeriklerinin modele etkisi.</em></sub>

⏳ Tool & Output Exploitation — <sub><em>Model outputs üzerinden SQL injection, command injection, XSS, SSRF veya unsafe code execution riskleri.</em></sub>

⏳ Abuse & Cost Testing — <sub><em>High-token prompts, recursive agents, repeated tool calls ve resource-exhaustion senaryoları.</em></sub>

⏳ Automated AI Evaluations — <sub><em>Repeatable test suites, benchmark datasets, attack templates ve regression testing.</em></sub>

⏳ Human Red Teaming — <sub><em>Context-dependent, creative ve chained attack paths'in deneyimli test uzmanları tarafından incelenmesi.</em></sub>

⏳ Security vs Safety Evaluation — <sub><em>Siber güvenlik açıkları, kullanım politikası ihlalleri ve harmful-output risklerinin ayrı fakat ilişkili değerlendirilmesi.</em></sub>

⏳ Findings & Severity Assessment — <sub><em>Exploitability, business impact, affected assets, reproducibility ve remediation priority.</em></sub>

⏳ Remediation Verification — <sub><em>Uygulanan kontrollerin tekrar test edilmesi ve yeni bypass yöntemlerinin araştırılması.</em></sub>

⏳ Responsible Testing Boundaries — <sub><em>Yalnızca yetkili sistemlerde, kontrollü test verileri ve sınırlı blast radius ile çalışma.</em></sub>

📡 Module 24: AI Security Monitoring, Threat Detection & Incident Response — ⏳ Planned

⏳ AI Security Telemetry — <sub><em>Prompt, response, retrieval, model, tool-call, identity, authorization, network ve infrastructure olaylarının kaydı.</em></sub>

⏳ Secure AI Logging — <sub><em>Hassas veriyi gereksiz toplamadan olay araştırmasına yetecek seviyede audit trail oluşturulması.</em></sub>

⏳ Prompt & Response Monitoring — <sub><em>Injection patterns, data leakage, policy bypass ve suspicious-output davranışlarının tespiti.</em></sub>

⏳ Agent Activity Monitoring — <sub><em>Tool selection, parameter values, file access, network requests, external actions ve approval decisions.</em></sub>

⏳ RAG Monitoring — <sub><em>Retrieved documents, source trust, unusual retrieval patterns ve authorization failures.</em></sub>

⏳ Model-Behavior Monitoring — <sub><em>Unexpected outputs, integrity issues, safety-control degradation ve model-version farklılıkları.</em></sub>

⏳ Abuse Detection — <sub><em>Automated scraping, model extraction, credential testing, spam, fraud ve denial-of-wallet davranışları.</em></sub>

⏳ Drift Monitoring — <sub><em>Data drift, concept drift, performance drift ve security-control drift arasındaki farklar.</em></sub>

⏳ AI Security Metrics — <sub><em>Injection success rate, blocked actions, sensitive-data exposure, anomalous tool calls ve security-test regression results.</em></sub>

⏳ SIEM Integration — <sub><em>AI application, model gateway, cloud, Kubernetes ve identity logs'un merkezi güvenlik izleme sistemlerine aktarılması.</em></sub>

⏳ Detection Engineering — <sub><em>AI-specific detection rules, behavioral baselines, correlation rules ve alert tuning.</em></sub>

⏳ Threat Hunting with MITRE ATLAS — <sub><em>AI sistemlerine yönelik tactics ve techniques üzerinden hipotez tabanlı araştırma.</em></sub>

⏳ AI Incident Classification — <sub><em>Prompt injection, data leakage, compromised agent, poisoned RAG source, stolen model ve supply-chain compromise olayları.</em></sub>

⏳ AI Incident Triage — <sub><em>Etkilenen model, dataset, users, tools, credentials, infrastructure ve business processes'in belirlenmesi.</em></sub>

⏳ Containment Strategies — <sub><em>Agent tools'un kapatılması, model endpoint'in izole edilmesi, retrieval source'un kaldırılması ve access tokens'ın iptali.</em></sub>

⏳ Eradication & Recovery — <sub><em>Poisoned content'in temizlenmesi, model veya dataset'in güvenli sürümden yeniden yüklenmesi ve controls'un doğrulanması.</em></sub>

⏳ Model Rollback & Revocation — <sub><em>Güvensiz model, adapter veya configuration sürümünün kullanım dışı bırakılması.</em></sub>

⏳ Credential & Secret Rotation — <sub><em>Agent, service, model provider ve tool credentials bilgilerinin olay sonrasında yenilenmesi.</em></sub>

⏳ AI Forensics & Evidence Preservation — <sub><em>Prompts, outputs, retrieved content, tool executions, model versions ve configuration bilgilerinin korunması.</em></sub>

⏳ Communication & Reporting — <sub><em>Teknik ekipler, yönetim, hukuk, privacy ve business stakeholders için olay iletişimi.</em></sub>

⏳ Post-Incident Review — <sub><em>Timeline, root causes, control failures, lessons learned ve corrective actions.</em></sub>

⚖️ Module 25: AI Governance, Assurance, Standards & Compliance — ⏳ Planned

⏳ AI Governance Foundations — <sub><em>AI ownership, roles, responsibilities, decision rights ve organizational oversight.</em></sub>

⏳ NIST AI RMF — <sub><em>Govern, Map, Measure ve Manage fonksiyonlarıyla AI risk-management programı oluşturulması.</em></sub>

⏳ NIST Generative AI Profile — <sub><em>Generative AI sistemlerine özgü risklerin ve risk-treatment yöntemlerinin değerlendirilmesi.</em></sub>

⏳ NIST Secure AI Development Practices — <sub><em>AI model ve sistem geliştirme süreçlerinin Secure Software Development Framework ile uyumlu hâle getirilmesi.</em></sub>

⏳ ISO/IEC 42001 — <sub><em>Artificial Intelligence Management System kurulumu, policy, objectives, risk management ve continuous improvement.</em></sub>

⏳ ISO/IEC 23894 — <sub><em>AI risk-management principles, lifecycle risk identification ve risk-treatment yaklaşımı.</em></sub>

⏳ EU AI Act — <sub><em>Prohibited practices, risk classifications, high-risk systems, general-purpose AI ve provider/deployer obligations.</em></sub>

⏳ CSA AI Controls Matrix — <sub><em>Cloud tabanlı AI sistemleri için governance, security, privacy, supply chain ve operational controls.</em></sub>

⏳ OWASP & MITRE Mapping — <sub><em>OWASP AI risks ve MITRE ATLAS techniques'in security controls ve test cases ile eşleştirilmesi.</em></sub>

⏳ AI System Inventory — <sub><em>Kurum içinde geliştirilen, kullanılan veya third party üzerinden alınan AI sistemlerinin kayıt altına alınması.</em></sub>

⏳ AI Risk Classification — <sub><em>Use case, autonomy, data sensitivity, user impact, business criticality ve regulatory exposure değerlendirmesi.</em></sub>

⏳ AI Impact Assessments — <sub><em>Güvenlik, privacy, safety, fairness, human rights ve operational etkilerin sistem devreye alınmadan incelenmesi.</em></sub>

⏳ Model Cards & System Cards — <sub><em>Model purpose, limitations, training information, evaluation results, risks ve intended-use bilgilerinin dokümantasyonu.</em></sub>

⏳ Transparency & Explainability — <sub><em>Kullanıcı bilgilendirmesi, AI-generated content disclosure, model limitations ve decision explanations.</em></sub>

⏳ Human Oversight — <sub><em>Kritik karar ve eylemlerde insan kontrolü, intervention, override ve escalation mekanizmaları.</em></sub>

⏳ Accountability & Auditability — <sub><em>AI kararlarının, model versions'ın, approvals'ın ve configuration değişikliklerinin izlenebilir olması.</em></sub>

⏳ Third-Party Assurance — <sub><em>Model providers, datasets, AI APIs ve SaaS services için due diligence ve contractual security requirements.</em></sub>

⏳ Acceptable AI Use Policies — <sub><em>Çalışanların AI araçlarıyla hangi veri ve sistemleri kullanabileceğini belirleyen kurallar.</em></sub>

⏳ AI Exception & Risk Acceptance — <sub><em>Güvenlik kontrollerinden sapmaların business owner ve risk owner tarafından onaylanması.</em></sub>

⏳ Continuous Compliance — <sub><em>AI system, model, dataset, regulation ve control değişikliklerinin sürekli takip edilmesi.</em></sub>

⏳ Responsible AI Principles — <sub><em>Privacy, fairness, transparency, reliability, safety, security ve accountability gereksinimlerinin birlikte yönetilmesi.</em></sub>

🏗️ Level 5 — Capstone Projects

⏳ Project 1: Secure RAG Application — <sub><em>Authentication, document-level authorization, vector database isolation, prompt-injection protection, output validation, logging ve security testing içeren güvenli RAG uygulaması.</em></sub>

⏳ Project 2: Agentic AI Security Laboratory — <sub><em>Kontrollü sandbox içinde tool kullanan bir AI agent'ın kurulması; least privilege, approval gates, MCP security, memory isolation, monitoring ve red-team testleri.</em></sub>

⏳ Project 3: Secure AI Model-Serving Platform — <sub><em>Docker veya Kubernetes üzerinde model serving; API gateway, TLS, workload identity, secrets management, rate limiting, model registry, monitoring ve rollback.</em></sub>

⏳ Project 4: AI Supply-Chain Security Pipeline — <sub><em>Model, dataset, container ve dependency artifacts için provenance, signing, vulnerability scanning, approval gates ve controlled deployment pipeline'ı.</em></sub>

⏳ Project 5: AI Security Monitoring & Incident Response Lab — <sub><em>Prompt, RAG, agent ve infrastructure telemetry'sinin SIEM'e aktarılması; detection rules, simulated incidents, containment ve post-incident documentation.</em></sub>

⏳ Project 6: Production-Grade Cloud AI Security Architecture — <sub><em>AWS, Terraform, Kubernetes, managed AI services, private networking, IAM, encryption, centralized logging, high availability ve AI governance kontrolleriyle uçtan uca güvenli platform.</em></sub>

<div align="right">

⬆️ Back to Top

</div>

</details>

<a id="milestone-projects"></a>

🏆 Milestone Portfolio Projects

Project 1 — Hardened & Audited Linux Server Infrastructure

Zamanlama: Level 1 tamamlandıktan sonra.

Networking, Linux administration, users and permissions, systemd, Linux networking, nftables, SSH hardening, Fail2Ban, auditd, logging, backup, Bash automation, incident validation ve teknik dokümantasyon alanlarını birleştiren kapsamlı bir sunucu güvenliği projesi.

Project 2 — Automated Provisioning of a Production-Oriented Kubernetes Environment

Zamanlama: Level 2 tamamlandıktan sonra.

Docker, secure images, Kubernetes workloads, Services, RBAC, NetworkPolicy, storage, probes, Terraform, remote state, Ansible, AWS VPC/EC2, automated validation ve operasyonel dokümantasyon alanlarını birleştiren otomatik altyapı projesi.

Project 3 — Resilient Cloud-Native GitOps & Observability Platform

Zamanlama: Level 3 ve Level 4 tamamlandıktan sonra.

CI/CD, DevSecOps security gates, software supply-chain security, GitOps, Kubernetes, Prometheus, Grafana, Loki, OpenTelemetry, alerting, SLOs, AWS Multi-AZ, IAM, backup, disaster recovery, resilience testing, runbooks, postmortems, performance analysis ve AI-assisted operational workflows alanlarını birleştiren kapsamlı platform mühendisliği projesi.

🔎 Detailed Milestone Project Specifications

The following three progressively advanced portfolio projects are planned to validate the knowledge acquired throughout the roadmap.

Each project combines concepts from multiple modules and is designed around production-oriented security, automation, reliability, validation, and documentation practices.

🛡️ Project 1: Hardened & Audited Private Linux Server Infrastructure — ⏳ Planned

Focus: Host Hardening, Stateful Firewalling, Security Auditing and Log-Based Abuse MitigationTimeline: At the end of Level 1 — Module 03

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

📦 Project 2: Declarative Provisioning of a Production-Oriented Kubernetes Environment — ⏳ Planned

Focus: Cloud Provisioning, Infrastructure as Code, Configuration Management and Kubernetes OrchestrationTimeline: At the end of Level 2 — Module 06

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

🚀 Project 3: Resilient Cloud-Native GitOps & Observability Platform — ⏳ Planned

Focus: DevSecOps, Declarative Delivery, SRE Practices, Observability and Multi-AZ ResilienceTimeline: At the end of Level 4 — Module 11

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

<a id="documentation-approach"></a>

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

<a id="project-status"></a>

⚠️ Project Status Notice

This repository represents an ongoing learning process.

Completed entries describe topics that have been studied and reviewed. Planned modules and project descriptions define the intended scope of future work.

Technologies, architectures, implementation details and project boundaries may be revised as the systems are built, tested, measured and documented.

A planned feature should not be interpreted as an already implemented or validated capability.

<div align="center">

<sub>Built as a living learning journal focused on secure systems, automation, reliability and measurable engineering practice.</sub>

⬆️ Back to Top

</div><a id="top"></a>

<div align="center">

🏆 Systems, DevOps & Platform Engineering Learning Roadmap

A bilingual, hands-on learning journal for building secure, automated, observable and reliable infrastructure.



</div>

<a id="about"></a>

📘 About This Repository

🇬🇧 English

This repository is an ongoing learning journal and practical roadmap created to build a strong technical foundation in system administration, networking, cybersecurity, DevSecOps, cloud computing, Site Reliability Engineering, and platform engineering.

The roadmap combines technical documentation, security-focused analysis, practical validation exercises, and progressively advanced portfolio projects. Content is prepared in both Turkish and English and is updated throughout the learning process.

🇹🇷 Türkçe

Bu depo; sistem yönetimi, ağ teknolojileri, siber güvenlik, DevSecOps, bulut bilişim, Site Reliability Engineering ve platform mühendisliği alanlarında güçlü bir teknik temel oluşturmak amacıyla hazırladığım, devam eden bir öğrenme günlüğü ve uygulama yol haritasıdır.

Yol haritası; teknik dokümantasyonlar, güvenlik odaklı değerlendirmeler, uygulamalı doğrulama çalışmaları ve seviyeler ilerledikçe geliştirilecek kapsamlı portföy projelerinden oluşmaktadır. İçerikler Türkçe ve İngilizce olarak hazırlanmakta ve öğrenme süreci boyunca düzenli olarak güncellenmektedir.

<a id="current-progress"></a>

📍 Current Progress

Field

Current Status

Current Level

🌐 Level 1 — Foundations of Systems & Security

Current Module

Module 01 — Advanced Networking for Platforms

Repository Status

🚧 Actively Maintained

Documentation Languages

🇹🇷 Turkish · 🇬🇧 English

Status Legend

Status

Meaning

✅

Completed

🚧

In Progress

⏳

Planned

[!NOTE]Completion indicators represent the current learning progress. Direct links will be added as the corresponding topic documents are committed to the repository.

🧭 Quick Navigation

Roadmap

Portfolio & Documentation

🌐 Level 1 — Systems & Security

🏆 Milestone Portfolio Projects

📦 Level 2 — Applications, Containers & IaC

📝 Documentation Approach

🚀 Level 3 — Software, Automation & Observability

⚠️ Project Status Notice

☁️ Level 4 — Cloud, Reliability & Operations

⬆️ Back to Top

🤖 Level 5 — AI Systems Security & MLSecOps



<a id="roadmap-overview"></a>

🗂️ Roadmap Overview

Level

Scope

Modules

Status

1

Systems, Networking, Linux & Security

01–03

🚧

2

Web, Databases, Containers, Kubernetes & IaC

04–07

⏳

3

CI/CD, Automation, Software Engineering & Observability

08–11

⏳

4

AWS, SRE, Performance & Intelligent Operations

12–15

⏳

5

AI Security, MLSecOps, Red Teaming & Governance

16–25

⏳

🗺️ Learning Roadmap — Öğrenme Yol Haritası

<a id="level-1"></a>

<details open>
<summary><strong>🌐 Level 1: Foundations of Systems & Security — 🚧 In Progress</strong></summary>

🌐 Module 01: Advanced Networking for Platforms — 🚧 In Progress

✅ Physical & Data Link Layers — <sub><em>OSI ve TCP/IP modelleri, encapsulation, fiziksel sinyaller, Ethernet frame yapısı, MAC addressing, switching, MTU, interface counters ve Layer 1/2 troubleshooting.</em></sub>

✅ ARP & Local Network Dynamics — <sub><em>ARP çalışma mantığı, neighbor table, ARP cache durumları, gratuitous ARP, proxy ARP, ARP spoofing riskleri ve Dynamic ARP Inspection.</em></sub>

✅ VLAN & VXLAN — <sub><em>Layer 2 segmentasyonu, access ve trunk portlar, IEEE 802.1Q tagging, VLAN hopping riskleri, VXLAN overlay mimarisi, VTEP ve VNI kavramları.</em></sub>

✅ Network Routing & Subnetting — <sub><em>IPv4 adresleme, CIDR, subnetting, routing table, default gateway, static ve dynamic routing temelleri, OSPF/BGP genel mantığı ve IP fragmentation.</em></sub>

✅ Transport Layer & Traffic Control — <sub><em>TCP ve UDP, portlar ve socket'ler, TCP three-way handshake, sequence ve acknowledgment numbers, flow control, congestion control, retransmission ve connection termination.</em></sub>

✅ Application Layer Services — <sub><em>DNS kayıt türleri, DNS resolution, DNS spoofing riskleri, DHCP DORA süreci, DHCP Snooping, HTTP/HTTPS ve TLS handshake.</em></sub>

✅ Load Balancing & Proxy Concepts — <sub><em>Forward proxy ve reverse proxy, Layer 4 ve Layer 7 load balancing, health checks, session persistence, TLS termination ve TLS passthrough.</em></sub>

🐧 Module 02: Enterprise Linux System Administration — 🚧 In Progress

✅ Linux Architecture, Boot Process & FHS — <sub><em>Kernel, initramfs, GRUB, systemd, boot süreci ve Linux dizin hiyerarşisi; /etc, /var, /home, /opt, /tmp, /proc, /sys ve /dev.</em></sub>

✅ Shell, CLI & Stream Processing — <sub><em>Standard input/output, pipes, redirection, environment variables ve grep, awk, sed, cut, sort, uniq, find, xargs gibi araçlar.</em></sub>

✅ User & Permission Management — <sub><em>User ve group yönetimi, UID/GID, chmod, chown, umask, POSIX ACLs, sudoers ve least-privilege yaklaşımı.</em></sub>

✅ Process & Resource Management — <sub><em>Process lifecycle, signals, foreground/background jobs, ps, top, htop, kill, file descriptors, memory ve CPU resource takibi.</em></sub>

✅ systemd, Services & Scheduled Tasks — <sub><em>Unit files, service dependencies, systemctl, journalctl, restart policies, systemd timers ve cron.</em></sub>

✅ Storage, Filesystems & LVM — <sub><em>Partitioning, block devices, ext4, XFS, mount işlemleri, /etc/fstab, inode yapısı, swap, LVM, RAID ve disk kapasite yönetimi.</em></sub>

✅ Linux Networking & Troubleshooting — <sub><em>Interface yönetimi, routing, DNS çözümleme, /etc/hosts, systemd-resolved, ip, ss, tcpdump, dig, curl, nc ve network namespaces.</em></sub>

✅ Package & Repository Management — <sub><em>apt, dpkg, dnf, rpm, repository yapıları, package signatures, dependency management ve güvenli güncelleme süreçleri.</em></sub>

✅ Logging, Time & Operational Maintenance — <sub><em>/var/log, systemd journal, rsyslog, log rotation, NTP, chrony, backup, restore validation ve bakım planları.</em></sub>

🛡️ Module 03: Security Engineering & Incident Management — ⏳ Planned

⏳ Security Foundations & Risk Management — <sub><em>CIA triad, authentication, authorization, accountability, assets, threats, vulnerabilities, risk, attack surface ve defense in depth.</em></sub>

⏳ Threat Modeling & Attack Lifecycle — <sub><em>Trust boundaries, data-flow diagrams, STRIDE, attack trees, Cyber Kill Chain, MITRE ATT&CK ve saldırı yaşam döngüsü.</em></sub>

⏳ Network and Availability Threats — <sub><em>MITM, spoofing, session hijacking, DNS ve ARP saldırıları, DoS/DDoS, SYN flood ve hizmet kesintisi senaryoları.</em></sub>

⏳ Malware & Endpoint Defense — <sub><em>Worm, Trojan, ransomware, rootkit, botnet, fileless malware, sandboxing, hashes, EDR ve endpoint hardening.</em></sub>

⏳ Firewalling & Network Defense — <sub><em>Stateful ve stateless filtering, Netfilter, iptables, nftables, connection tracking, NAT, rate limiting ve segmentation.</em></sub>

⏳ Cryptography, PKI & SSH Security — <sub><em>Symmetric ve asymmetric cryptography, hashing, digital signatures, certificates, SSH key pairs, host keys ve sshd_config hardening.</em></sub>

⏳ Linux Hardening & Vulnerability Management — <sub><em>Attack-surface reduction, CIS Benchmarks, CVE/CVSS, patching, service inventory, permissions, PAM, SELinux ve AppArmor temelleri.</em></sub>

⏳ System Auditing, Logging & SIEM — <sub><em>/var/log, systemd journal, auditd, log bütünlüğü, merkezi loglama, SIEM, detection rules ve security-event analysis.</em></sub>

⏳ Log-Based Abuse Mitigation — <sub><em>Fail2Ban çalışma mantığı, authentication log analizi, geçici engelleme, rate limiting ve aracın güvenlik sınırlamaları.</em></sub>

⏳ Incident Response Fundamentals — <sub><em>Preparation, detection, triage, containment, eradication, recovery, evidence preservation, communication ve post-incident review.</em></sub>

<div align="right">

⬆️ Back to Top

</div>

</details>

<a id="level-2"></a>

<details>
<summary><strong>📦 Level 2: Application Infrastructure, Containers & Infrastructure as Code — ⏳ Planned</strong></summary>

Uygulama, Veri, Otomasyon ve Konteyner Altyapıları

Web uygulamalarının, API'lerin ve veri katmanlarının çalışma prensiplerinin öğrenildiği; konteyner tabanlı platformların ve kodla tanımlanan altyapıların oluşturulduğu seviyedir.

🌍 Module 04: Web, API & Database Foundations — ⏳ Planned

⏳ Web Architecture Fundamentals — <sub><em>Client-server modeli, request-response akışı, stateless ve stateful application yaklaşımları.</em></sub>

⏳ HTTP & HTTPS — <sub><em>HTTP methods, status codes, headers, content types, caching, HTTP/1.1, HTTP/2, HTTP/3 ve HTTPS.</em></sub>

⏳ TLS & Certificate Validation — <sub><em>TLS handshake, X.509 certificates, Certificate Authorities, SNI, certificate chains ve renewal süreçleri.</em></sub>

⏳ Cookies, Sessions & Authentication — <sub><em>Cookies, session management, authentication state, secure cookie attributes ve session security.</em></sub>

⏳ REST API Fundamentals — <sub><em>Resources, endpoints, HTTP methods, idempotency, pagination, versioning, rate limiting ve API error handling.</em></sub>

⏳ Tokens, JWT & OAuth 2.0 — <sub><em>Access tokens, refresh tokens, JWT yapısı, OAuth 2.0 akışları ve OpenID Connect temelleri.</em></sub>

⏳ Browser & API Security — <sub><em>CORS, CSRF, XSS, Host header risks, input validation, authentication ve authorization ayrımı.</em></sub>

⏳ Relational Database Fundamentals — <sub><em>PostgreSQL ve MySQL genel mimarisi, schemas, tables, primary/foreign keys ve normalization.</em></sub>

⏳ Transactions & Concurrency — <sub><em>ACID, transaction isolation levels, locks, deadlocks ve consistency kavramları.</em></sub>

⏳ Indexes & Query Performance — <sub><em>Index yapıları, query plans, full-table scans, latency ve temel query optimization.</em></sub>

⏳ Database Availability & Protection — <sub><em>Replication, backup, restore testing, encryption, access control ve credential management.</em></sub>

⏳ Caching & In-Memory Data Stores — <sub><em>Redis temelleri, cache-aside yaklaşımı, expiration, persistence ve cache invalidation problemleri.</em></sub>

📦 Module 05: Container Technologies — Docker Deep Dive — ⏳ Planned

⏳ Containerization vs Virtualization — <sub><em>Hypervisor tabanlı virtual machines ile operating-system-level containers arasındaki mimari farklar.</em></sub>

⏳ Linux Kernel Namespaces & Cgroups — <sub><em>Container isolation ve resource-control mekanizmalarının Linux kernel içindeki temelleri.</em></sub>

⏳ Container Runtime Internals — <sub><em>Docker Engine, containerd, runc, OCI specifications ve Kubernetes Container Runtime Interface ilişkisi.</em></sub>

⏳ Docker Core — <sub><em>Docker CLI, image ve container lifecycle, Dockerfile yazım kuralları, layers, build cache ve multi-stage builds.</em></sub>

⏳ Docker Storage — <sub><em>Writable layers, volumes, bind mounts, tmpfs, UID/GID sorunları ve veri kalıcılığı.</em></sub>

⏳ Docker Networking — <sub><em>Network namespaces, veth pairs, bridge networks, port publishing, NAT, container DNS ve service discovery.</em></sub>

⏳ Docker Compose — <sub><em>Çok konteynerli yerel geliştirme, test ve doğrulama ortamlarının tanımlanması.</em></sub>

⏳ Registries & Image Lifecycle — <sub><em>Docker Hub, GHCR, Amazon ECR, tags, digests, retention, signing ve image provenance.</em></sub>

⏳ Container Security & Image Hardening — <sub><em>Non-root containers, reduced capabilities, read-only filesystems, seccomp, minimal images ve Trivy/Grype taramaları.</em></sub>

⏳ Docker Operations & Troubleshooting — <sub><em>Logs, inspect, events, health checks, networking, storage, OOM ve safe-cleanup yaklaşımları.</em></sub>

☸️ Module 06: Kubernetes Orchestration & Platform Fundamentals — ⏳ Planned

⏳ Kubernetes Architecture — <sub><em>API Server, etcd, Scheduler, kube-controller-manager, cloud-controller-manager, kubelet, kube-proxy ve container runtime görevleri.</em></sub>

⏳ Kubernetes Core Objects — <sub><em>Pods, ReplicaSets, Deployments, StatefulSets, DaemonSets, Jobs ve CronJobs.</em></sub>

⏳ Scheduling & Resources — <sub><em>Requests, limits, QoS classes, affinity, anti-affinity, taints, tolerations ve pod placement.</em></sub>

⏳ Services & Service Discovery — <sub><em>ClusterIP, NodePort, LoadBalancer Services, EndpointSlices, kube-proxy ve CoreDNS.</em></sub>

⏳ Configuration & Secrets — <sub><em>ConfigMaps, Secrets, environment variables, mounted volumes ve Kubernetes Secrets nesnelerinin güvenlik sınırlamaları.</em></sub>

⏳ Kubernetes Storage — <sub><em>PersistentVolumes, PersistentVolumeClaims, StorageClasses, dynamic provisioning, access modes ve backup considerations.</em></sub>

⏳ Kubernetes Security & RBAC — <sub><em>ServiceAccounts, Roles, ClusterRoles, RoleBindings, Security Contexts, Pod Security Standards ve least privilege.</em></sub>

⏳ Traffic Management — <sub><em>Ingress Controllers, Gateway API, NetworkPolicy, service mesh, mTLS, retries ve circuit breaking.</em></sub>

⏳ Workload Health & Recovery — <sub><em>Startup, readiness ve liveness probes; restart behavior ve controller'ların desired state'i koruma yaklaşımı.</em></sub>

⏳ Scaling & Reliability — <sub><em>HPA, VPA kavramları, Cluster Autoscaler, PodDisruptionBudget, graceful shutdown ve controlled rollout stratejileri.</em></sub>

⏳ Cluster Lifecycle & Packaging — <sub><em>kubeadm, managed Kubernetes, upgrades, Helm, Kustomize, certificates ve etcd backup temelleri.</em></sub>

⏳ Kubernetes Observability & Troubleshooting — <sub><em>Events, logs, Metrics Server, Prometheus, CrashLoopBackOff, ImagePullBackOff, DNS ve volume failures.</em></sub>

⏳ GitOps & Declarative Delivery — <sub><em>Kubernetes kaynaklarının Argo CD veya Flux ile deklaratif biçimde yönetilmesi.</em></sub>

⚙️ Module 07: Infrastructure as Code & Configuration Management — ⏳ Planned

⏳ Infrastructure as Code Fundamentals — <sub><em>Declarative ve imperative yaklaşımlar, desired state, idempotency, drift, convergence ve repeatability.</em></sub>

⏳ Terraform Language & Workflow — <sub><em>Providers, resources, data sources, HCL, dependencies, init, plan, apply ve destroy.</em></sub>

⏳ Terraform Variables & Data Modeling — <sub><em>Variables, outputs, locals, lists, maps, objects, conditionals, count ve for_each.</em></sub>

⏳ Terraform Modules — <sub><em>Root ve child modules, reusable architecture, module boundaries, version pinning ve environment separation.</em></sub>

⏳ Terraform State Security — <sub><em>terraform.tfstate güvenliği, Amazon S3 remote state, native S3 state locking, encryption, versioning ve restricted IAM access.</em></sub>

⏳ Legacy State Locking Considerations — <sub><em>DynamoDB tabanlı locking yönteminin mevcut eski yapıların geçişi veya uyumluluk ihtiyaçları kapsamında değerlendirilmesi.</em></sub>

⏳ Terraform Lifecycle & Testing — <sub><em>Lifecycle rules, import, moved blocks, static analysis, Policy as Code, security scanning ve destructive-change protection.</em></sub>

⏳ Ansible Fundamentals — <sub><em>Agentless architecture, SSH connections, inventories, modules, facts, ad-hoc commands ve privilege escalation.</em></sub>

⏳ Ansible Playbooks & Roles — <sub><em>Playbooks, handlers, templates, variables, loops, conditionals, roles ve idempotent service management.</em></sub>

⏳ Ansible Vault & Operational Safety — <sub><em>Hassas değerlerin şifrelenmesi, secret injection, least privilege, serial changes ve rollback limitations.</em></sub>

⏳ Git & Version Control Integration — <sub><em>Commit disiplini, pull requests, protected branches, code review, GitFlow ve trunk-based development.</em></sub>

⏳ Infrastructure Validation — <sub><em>Formatting, validation, linting, policy checks, controlled plan/apply ve CI/CD entegrasyonu.</em></sub>

⏳ Terraform & Ansible Integration — <sub><em>Provisioning ve configuration-management sorumluluklarının ayrılması, dynamic inventory ve doğrulama süreçleri.</em></sub>

<div align="right">

⬆️ Back to Top

</div>

</details>

<a id="level-3"></a>

<details>
<summary><strong>🚀 Level 3: Software Engineering, Automation, CI/CD & Observability — ⏳ Planned</strong></summary>

Yazılım Teslimi, Otomasyon ve Gözlemlenebilirlik

Altyapı ve platform yazılımlarının sürdürülebilir biçimde geliştirildiği; teslim süreçlerinin otomatikleştirildiği ve sistemlerin metrics, logs ve traces kullanılarak gözlemlendiği seviyedir.

🔄 Module 08: CI/CD Pipelines & GitOps — ⏳ Planned

⏳ CI/CD Core Concepts — <sub><em>Continuous Integration, Continuous Delivery ve Continuous Deployment arasındaki farklar.</em></sub>

⏳ Git Workflows & Pipeline Triggers — <sub><em>Branching strategies, pull requests, protected branches, tags, releases ve event-based triggers.</em></sub>

⏳ Pipeline Platforms — <sub><em>GitHub Actions veya GitLab CI ile workflows, jobs, runners, stages, artifacts ve environment yönetimi.</em></sub>

⏳ Build, Test & Artifact Pipelines — <sub><em>Unit, integration ve end-to-end tests, caching, matrices, reports ve reproducible builds.</em></sub>

⏳ Container Build & Delivery — <sub><em>Docker/BuildKit, image tagging, commit SHA kullanımı, registry authentication, scanning, signing ve provenance.</em></sub>

⏳ DevSecOps & Security Gates — <sub><em>SonarQube, Gitleaks, TruffleHog, Trivy, SAST, SCA, IaC scanning ve controlled policy enforcement.</em></sub>

⏳ Artifact Management — <sub><em>Docker Hub, GitHub Container Registry, Amazon ECR ve JFrog Artifactory gibi registry ve artifact-repository çözümleri.</em></sub>

⏳ Secure Pipeline Authentication — <sub><em>Uzun ömürlü cloud credentials yerine OpenID Connect ve kısa ömürlü kimlik bilgilerinin kullanılması.</em></sub>

⏳ Software Supply-Chain Security — <sub><em>SBOM, dependency trust, signing, Sigstore, SLSA ve artifact-integrity yaklaşımı.</em></sub>

⏳ Deployment Strategies — <sub><em>Rolling update, blue/green, canary, feature flags, progressive delivery ve rollback yaklaşımları.</em></sub>

⏳ GitOps Delivery — <sub><em>Application code ile deployment state'inin ayrılması, declarative manifests, reconciliation ve drift correction.</em></sub>

⏳ Pipeline Observability & Troubleshooting — <sub><em>Pipeline metrics, runner utilization, flaky tests, failures, retries, concurrency ve runbooks.</em></sub>

🐍 Module 09: Systems Automation & Scripting — Bash & Python — ⏳ Planned

⏳ Bash Scripting Fundamentals — <sub><em>Variables, conditionals, loops, functions, arguments, arrays, exit codes ve debugging.</em></sub>

⏳ Shell Safety Practices — <sub><em>Quoting, input validation, temporary files, cleanup traps ve set -Eeuo pipefail kullanımının bağlama bağlı değerlendirilmesi.</em></sub>

⏳ Regular Expressions & Text Processing — <sub><em>Regex, grep, sed, awk, cut, sort, uniq, tr, find, xargs, jq ve yq.</em></sub>

⏳ System Administration Automation — <sub><em>Backup, restore validation, disk monitoring, log rotation, service-health checks ve raporlama.</em></sub>

⏳ Python Fundamentals for Operations — <sub><em>Data types, functions, exceptions, virtual environments, type hints, logging ve configuration.</em></sub>

⏳ Python for Filesystems & Processes — <sub><em>os, pathlib, sys, shutil, subprocess, signals, permissions ve safe command execution.</em></sub>

⏳ API Automation — <sub><em>Authentication, pagination, timeout, retry, exponential backoff, rate limiting ve schema validation.</em></sub>

⏳ Network & Security Automation — <sub><em>DNS checks, TCP/UDP connectivity, certificate inspection, log parsing, firewall validation ve alert enrichment.</em></sub>

⏳ Secure Automation — <sub><em>Secrets yönetimi, least privilege, auditability, idempotency ve destructive-action protections.</em></sub>

⏳ Automation Quality & Packaging — <sub><em>Unit tests, mocking, linting, type checking, dependency pinning, CLI design ve CI integration.</em></sub>

💻 Module 10: Software Engineering for Infrastructure & Platform Teams — ⏳ Planned

⏳ Software Engineering Fundamentals — <sub><em>Requirements, maintainability, readability, testability ve technical-debt yönetimi.</em></sub>

⏳ Clean Code Principles — <sub><em>Naming, small functions, separation of concerns, error handling ve understandable abstractions.</em></sub>

⏳ SOLID Principles — <sub><em>Single Responsibility, Open/Closed, Liskov Substitution, Interface Segregation ve Dependency Inversion.</em></sub>

⏳ Design Patterns — <sub><em>Factory, Strategy, Adapter, Observer, Dependency Injection ve infrastructure automation kullanım örnekleri.</em></sub>

⏳ Testing Strategies — <sub><em>Unit, integration, end-to-end, contract ve smoke tests; mocks, stubs ve test doubles.</em></sub>

⏳ Refactoring & Code Review — <sub><em>Safe refactoring, code smells, pull-request reviews ve incremental improvement.</em></sub>

⏳ Application Architecture — <sub><em>Monolith, modular monolith, microservices, event-driven architecture ve architecture trade-offs.</em></sub>

⏳ Distributed-System Fundamentals — <sub><em>Partial failures, timeouts, retries, idempotency, consistency ve eventual consistency.</em></sub>

⏳ API & Service Design — <sub><em>Versioning, backward compatibility, error contracts, rate limits ve graceful degradation.</em></sub>

⏳ Configuration & Dependency Management — <sub><em>Configuration separation, feature flags, dependency pinning ve environment consistency.</em></sub>

⏳ Secure Software Development — <sub><em>Input validation, secrets, dependency riskleri, threat modeling ve secure defaults.</em></sub>

⏳ Engineering Documentation — <sub><em>Architecture Decision Records, diagrams, runbooks, API documentation ve operational handover.</em></sub>

📊 Module 11: Observability, Logging & Monitoring — ⏳ Planned

⏳ Observability Fundamentals — <sub><em>Metrics, logs, traces, events ve profiles arasındaki farklar ve birlikte kullanım biçimleri.</em></sub>

⏳ Monitoring Approaches — <sub><em>White-box, black-box, synthetic monitoring, health checks ve baselines.</em></sub>

⏳ The Four Golden Signals — <sub><em>Latency, traffic, errors ve saturation.</em></sub>

⏳ RED & USE Methods — <sub><em>Request-driven services ve infrastructure resources için gözlem yöntemleri.</em></sub>

⏳ Metrics & Prometheus — <sub><em>Prometheus architecture, service discovery, scraping, exporters, PromQL, recording ve alerting rules.</em></sub>

⏳ Grafana & Dashboard Engineering — <sub><em>Dashboards, panels, variables, annotations, thresholds, provisioning ve dashboard anti-patterns.</em></sub>

⏳ Centralized Logging — <sub><em>Elasticsearch tabanlı çözümler veya Grafana Loki ile log aggregation, parsing, retention ve analysis.</em></sub>

⏳ Distributed Tracing — <sub><em>OpenTelemetry instrumentation ve Collector; Jaeger veya Grafana Tempo gibi tracing backend'leri.</em></sub>

⏳ Alerting & Notification Engineering — <sub><em>Prometheus Alertmanager ile routing, grouping, deduplication, inhibition, silences ve escalation.</em></sub>

⏳ SLI, SLO & Reliability Monitoring — <sub><em>Availability, latency, error budgets, burn rate ve SLO-based alerting.</em></sub>

⏳ Platform & Kubernetes Observability — <sub><em>Host, container, node, pod, workload ve control-plane telemetry.</em></sub>

⏳ Security Observability — <sub><em>Authentication, authorization, audit, network, DNS, firewall ve cloud-security telemetry.</em></sub>

⏳ Operational Reliability — <sub><em>Actionable alerts, alert fatigue, missing telemetry, runbooks ve incident-response bağlantısı.</em></sub>

⏳ Observability Troubleshooting — <sub><em>Scrape failures, missing logs, trace gaps, cardinality explosions, storage ve query-performance sorunları.</em></sub>

<div align="right">

⬆️ Back to Top

</div>

</details>

<a id="level-4"></a>

<details>
<summary><strong>☁️ Level 4: Cloud Computing, Reliability & Intelligent Operations — ⏳ Planned</strong></summary>

Bulut, Dayanıklı Sistemler ve Akıllı Operasyon

Yüksek erişilebilirlik, dayanıklılık, ölçeklenebilirlik, güvenli erişim, performans, felaket kurtarma ve yapay zekâ destekli mühendislik ilkelerine dayalı sistemlerin ele alındığı seviyedir.

☁️ Module 12: Cloud Computing — AWS Focus — ⏳ Planned

⏳ Cloud Computing Foundations — <sub><em>On-premises ve cloud farkları, IaaS, PaaS, SaaS, elasticity, scalability ve Shared Responsibility Model.</em></sub>

⏳ AWS Global Infrastructure — <sub><em>Regions, Availability Zones, edge locations, regional ve global services.</em></sub>

⏳ Identity & Access Management — <sub><em>IAM users, groups, roles, policies, trust policies, STS ve Principle of Least Privilege.</em></sub>

⏳ AWS Networking & VPC Architecture — <sub><em>VPC, CIDR planning, public/private subnets, route tables, Internet Gateway, NAT Gateway ve VPC endpoints.</em></sub>

⏳ Network Security — <sub><em>Security Groups, Network ACLs, VPC Flow Logs, segmentation ve egress-control yaklaşımları.</em></sub>

⏳ Compute Services — <sub><em>Amazon EC2, AMIs, launch templates, user data, metadata service, Auto Scaling Groups ve instance lifecycle.</em></sub>

⏳ Load Balancing — <sub><em>Application Load Balancer, Network Load Balancer, listeners, target groups, health checks ve TLS.</em></sub>

⏳ Storage Services — <sub><em>Amazon S3, EBS ve EFS arasındaki kullanım, availability, durability, performance ve cost trade-off'ları.</em></sub>

⏳ Managed Databases — <sub><em>Amazon RDS, Aurora, DynamoDB ve ElastiCache genel mimarileri.</em></sub>

⏳ Serverless & Event-Driven Architecture — <sub><em>AWS Lambda, Amazon SQS, SNS, EventBridge, retries, DLQs ve idempotency.</em></sub>

⏳ Secrets & Key Management — <sub><em>AWS Secrets Manager, Systems Manager Parameter Store, AWS KMS ve envelope encryption.</em></sub>

⏳ Cloud Security Services — <sub><em>CloudTrail, Config, GuardDuty, Security Hub, Inspector, WAF, Shield ve Macie temelleri.</em></sub>

⏳ Monitoring & Auditing — <sub><em>CloudWatch metrics, logs, alarms, dashboards, CloudTrail events ve centralized logging.</em></sub>

⏳ High Availability & Disaster Recovery on AWS — <sub><em>Multi-AZ, Multi-Region, backup, snapshots, replicas, Route 53 failover, RTO ve RPO.</em></sub>

⏳ Governance & Cost Management — <sub><em>AWS Organizations, OUs, SCPs, tagging, budgets, Cost Explorer, quotas ve guardrails.</em></sub>

⏳ AWS & Infrastructure as Code Integration — <sub><em>Terraform, remote state, OIDC identities, environment promotion ve drift detection.</em></sub>

🏛️ Module 13: Site Reliability Engineering & High Availability — ⏳ Planned

⏳ SRE Foundations & Service Ownership — <sub><em>SRE ve DevOps ilişkisi, service boundaries, ownership, operational readiness ve reliability as a product feature.</em></sub>

⏳ SLI, SLO, SLA & Error Budgets — <sub><em>Service indicators, objectives, external commitments, measurement windows ve burn rate.</em></sub>

⏳ Toil Reduction & Reliability Automation — <sub><em>Repetitive operational work, safe automation, self-service, guardrails ve human-in-the-loop.</em></sub>

⏳ Monitoring, Alerting & On-Call — <sub><em>Golden Signals, actionable alerts, severity, routing, escalation, handoffs ve on-call health.</em></sub>

⏳ Incident Management — <sub><em>Incident detection, declaration, triage, Incident Commander, stabilization, communication ve recovery.</em></sub>

⏳ Blameless Postmortems — <sub><em>Timeline, impact, contributing factors, root-cause analysis, corrective actions ve learning culture.</em></sub>

⏳ Resilience Patterns — <sub><em>Timeouts, retries, exponential backoff, jitter, circuit breakers, bulkheads, rate limiting ve load shedding.</em></sub>

⏳ High Availability Architectures — <sub><em>Redundancy, single points of failure, active-passive, active-active, quorum, replication ve DNS failover.</em></sub>

⏳ Disaster Recovery & Business Continuity — <sub><em>RTO, RPO, backup/restore, Pilot Light, Warm Standby, Active-Active ve recovery testing.</em></sub>

⏳ Chaos Engineering — <sub><em>Yetkili ortamlarda kontrollü, hipotez tabanlı failure experiments, abort conditions ve blast-radius management.</em></sub>

⏳ Release Engineering & Change Reliability — <sub><em>Rolling, blue/green, canary, feature flags, migration safety ve rollback.</em></sub>

⏳ Operational Readiness — <sub><em>Runbooks, dashboards, alerts, deployment validation, capacity ownership ve recovery plans.</em></sub>

⚡ Module 14: Performance Engineering & Capacity Analysis — ⏳ Planned

⏳ Performance Engineering Fundamentals — <sub><em>Latency, throughput, concurrency, utilization, saturation ve bottleneck kavramları.</em></sub>

⏳ CPU Performance Analysis — <sub><em>CPU utilization, load average, run queues, context switches, user/system time ve CPU profiling.</em></sub>

⏳ Memory Performance Analysis — <sub><em>Memory allocation, cache, swap, page faults, memory pressure, leaks ve OOM behavior.</em></sub>

⏳ Storage & I/O Performance — <sub><em>IOPS, throughput, latency, queue depth, filesystem overhead ve disk saturation.</em></sub>

⏳ Network Performance Analysis — <sub><em>Bandwidth, packet loss, jitter, retransmissions, connection limits ve socket queues.</em></sub>

⏳ Application Profiling — <sub><em>Hot paths, function-level profiling, memory allocation, lock contention ve flame graphs.</em></sub>

⏳ Database Performance — <sub><em>Query latency, indexes, execution plans, connection pools, locks ve slow-query analysis.</em></sub>

⏳ Load, Stress & Soak Testing — <sub><em>Normal load, peak load, breaking points, long-duration tests ve test-environment limitations.</em></sub>

⏳ Benchmarking Methodology — <sub><em>Baselines, controlled experiments, repeatability, warm-up effects ve misleading benchmark risks.</em></sub>

⏳ Capacity Planning — <sub><em>Growth forecasting, resource headroom, seasonality, quotas ve scaling decisions.</em></sub>

⏳ Backpressure & Queue Management — <sub><em>Queue depth, concurrency limits, rate limiting ve overload behavior.</em></sub>

⏳ Performance Troubleshooting — <sub><em>Metrics, logs, traces ve profiles kullanılarak katmanlı bottleneck analizi.</em></sub>

⏳ Performance vs Cost Trade-offs — <sub><em>Vertical ve horizontal scaling, overprovisioning, efficiency ve cloud-cost etkileri.</em></sub>

🤖 Module 15: AI-Assisted Engineering & Operational Intelligence — ⏳ Planned

⏳ AI-Assisted Engineering Fundamentals — <sub><em>Yapay zekâ araçlarının yazılım, sistem, cloud ve güvenlik mühendisliğinde doğru kullanım alanları.</em></sub>

⏳ Prompt Design for Engineers — <sub><em>Gereksinim, bağlam, kısıt, doğrulama kriteri ve beklenen çıktı formatının açık tanımlanması.</em></sub>

⏳ AI-Assisted Code Generation — <sub><em>Bash, Python, Terraform, Ansible ve Kubernetes manifest taslaklarının kontrollü üretilmesi.</em></sub>

⏳ AI-Assisted Code Review — <sub><em>Logic errors, security risks, maintainability issues, race conditions ve missing error handling kontrolleri.</em></sub>

⏳ Infrastructure Review — <sub><em>Terraform plans, IAM policies, Kubernetes manifests ve network rules için AI destekli inceleme.</em></sub>

⏳ Log & Incident Analysis — <sub><em>Log özetleme, timeline oluşturma, correlation, hypothesis generation ve incident triage desteği.</em></sub>

⏳ AI-Assisted Troubleshooting — <sub><em>Hata mesajları, metrics, traces ve system outputs üzerinden olası nedenlerin önceliklendirilmesi.</em></sub>

⏳ Security Analysis & Threat Hunting — <sub><em>Detection-rule taslakları, IOC enrichment, suspicious-pattern analysis ve false-positive değerlendirmesi.</em></sub>

⏳ Documentation & Knowledge Management — <sub><em>README, runbook, postmortem, architecture documentation ve technical-summary üretimi.</em></sub>

⏳ Output Verification — <sub><em>AI tarafından üretilen komut, kod, policy ve teknik iddiaların bağımsız olarak doğrulanması.</em></sub>

⏳ Hallucination & Context Risks — <sub><em>Uydurulmuş komutlar, eski bilgiler, yanlış varsayımlar ve eksik bağlam nedeniyle oluşabilecek riskler.</em></sub>

⏳ Sensitive Data Protection — <sub><em>Credentials, logs, source code, personal data ve şirket bilgilerinin AI araçlarına aktarılma riskleri.</em></sub>

⏳ Human Approval & Safety Boundaries — <sub><em>Production değişiklikleri, destructive commands, security decisions ve automated actions için insan onayı.</em></sub>

⏳ AI-Augmented Operations — <sub><em>Alert enrichment, anomaly summarization, runbook suggestions ve controlled remediation yaklaşımları.</em></sub>

⏳ Responsible AI Use — <sub><em>Privacy, auditability, intellectual property, bias, accountability ve organizational-policy gereksinimleri.</em></sub>

<div align="right">

⬆️ Back to Top

</div>

</details>

<a id="level-5"></a>

<details>
<summary><strong>🤖 Level 5: AI Systems Security, MLSecOps & Governance — ⏳ Planned</strong></summary>

Yapay Zekâ Sistemleri Güvenliği, Güvenli Model Yaşam Döngüsü ve AI Yönetişimi

Yapay zekâ ve makine öğrenmesi sistemlerinin mimarisinin, tehdit yüzeylerinin ve yaşam döngüsünün incelendiği; LLM, RAG ve agent tabanlı uygulamaların güvenli biçimde geliştirildiği, dağıtıldığı, izlendiği ve yönetildiği ileri seviye uzmanlık alanıdır.

🧠 Module 16: AI, Machine Learning & LLM Foundations for Security Engineers — ⏳ Planned

⏳ AI & Machine Learning Foundations — <sub><em>Artificial Intelligence, Machine Learning, Deep Learning ve Generative AI arasındaki farklar; supervised, unsupervised, reinforcement ve self-supervised learning yaklaşımları.</em></sub>

⏳ AI System Lifecycle — <sub><em>Veri toplama, veri hazırlama, training, validation, testing, fine-tuning, deployment, inference, monitoring, retraining ve retirement aşamaları.</em></sub>

⏳ Model Architecture Fundamentals — <sub><em>Neural networks, parameters, weights, layers, loss functions, optimization, checkpoints ve model formats kavramları.</em></sub>

⏳ Large Language Model Fundamentals — <sub><em>Transformer mimarisi, attention, tokens, tokenization, context window, temperature, sampling ve hallucination kavramları.</em></sub>

⏳ Embeddings & Semantic Search — <sub><em>Embedding modelleri, similarity search, vector representations, indexing ve semantic-retrieval çalışma mantığı.</em></sub>

⏳ Retrieval-Augmented Generation — <sub><em>RAG ingestion, chunking, embedding, retrieval, reranking, context construction ve generation akışı.</em></sub>

⏳ Vector Databases — <sub><em>Vector indexes, metadata filtering, namespaces, document permissions, multi-tenancy ve vector-store erişim kontrolü.</em></sub>

⏳ AI Agents & Tool Use — <sub><em>Planning, reasoning loops, memory, tool calling, function calling, external APIs ve autonomous-action riskleri.</em></sub>

⏳ Model Context Protocol Fundamentals — <sub><em>MCP servers, clients, tools, resources, prompts, trust boundaries ve third-party integration riskleri.</em></sub>

⏳ Training, Fine-Tuning & Inference — <sub><em>Pre-training, supervised fine-tuning, instruction tuning, adapters, LoRA, quantization ve inference-serving farkları.</em></sub>

⏳ AI Security Terminology — <sub><em>Model, dataset, prompt, system prompt, guardrail, agent, inference endpoint, model registry ve evaluation kavramları.</em></sub>

🧭 Module 17: AI Threat Modeling & Security Architecture — ⏳ Planned

⏳ AI System Asset Identification — <sub><em>Models, datasets, prompts, embeddings, vector stores, APIs, agents, tools, credentials, logs ve model artifacts gibi korunması gereken varlıkların belirlenmesi.</em></sub>

⏳ AI Data-Flow Diagrams — <sub><em>Kullanıcı, uygulama, model provider, RAG pipeline, agent tools, databases ve external services arasındaki veri akışlarının çıkarılması.</em></sub>

⏳ Trust Boundaries — <sub><em>User input, retrieved content, model output, agent memory, tool responses ve third-party models arasındaki güven sınırlarının belirlenmesi.</em></sub>

⏳ AI Attack-Surface Analysis — <sub><em>Training, supply chain, model storage, inference API, application, RAG, agent ve monitoring katmanlarının saldırı yüzeyleri.</em></sub>

⏳ Threat Actors & Capabilities — <sub><em>External attackers, malicious users, insiders, compromised suppliers ve automated-abuse actors.</em></sub>

⏳ AI Misuse & Abuse Cases — <sub><em>Modelin yetkisiz, zararlı, maliyet artırıcı veya kurum politikasına aykırı amaçlarla kullanılması.</em></sub>

⏳ Threat Modeling Methods — <sub><em>STRIDE, attack trees, abuse cases ve AI sistemlerine uyarlanmış threat-modeling yaklaşımları.</em></sub>

⏳ OWASP LLM & Generative AI Risks — <sub><em>LLM uygulamalarına özgü güvenlik risklerinin mimari ve uygulama seviyesinde değerlendirilmesi.</em></sub>

⏳ MITRE ATLAS — <sub><em>Predictive AI, Generative AI ve Agentic AI sistemlerine yönelik saldırı taktikleri, teknikleri ve mitigations.</em></sub>

⏳ NIST AI Risk Management Framework — <sub><em>Govern, Map, Measure ve Manage fonksiyonlarının AI güvenlik risklerine uygulanması.</em></sub>

⏳ Security Requirements Engineering — <sub><em>Authentication, authorization, confidentiality, integrity, availability, privacy, auditability ve human-oversight gereksinimleri.</em></sub>

⏳ AI Security Architecture Patterns — <sub><em>Isolation, least privilege, defense in depth, zero trust, secure gateways ve controlled tool execution.</em></sub>

⏳ AI Risk Register — <sub><em>Risk owner, likelihood, impact, control, residual risk, acceptance ve remediation tracking.</em></sub>

🔐 Module 18: LLM, RAG & Agentic Application Security — ⏳ Planned

⏳ Direct Prompt Injection — <sub><em>Kullanıcının model davranışını veya sistem talimatlarını doğrudan değiştirmeye çalıştığı saldırılar.</em></sub>

⏳ Indirect Prompt Injection — <sub><em>Web pages, documents, emails, retrieved content veya external tool outputs içine yerleştirilmiş kötü amaçlı talimatlar.</em></sub>

⏳ System-Prompt Extraction — <sub><em>System prompt, hidden instructions, internal policies ve application logic bilgilerinin model üzerinden elde edilmesi.</em></sub>

⏳ Jailbreak Attacks — <sub><em>Modelin güvenlik ve kullanım sınırlamalarının crafted prompts, encoding, role-play veya multi-step interactions ile aşılması.</em></sub>

⏳ Sensitive Information Disclosure — <sub><em>Credentials, personal data, proprietary data, internal documents ve confidential context bilgilerinin model çıktılarında açığa çıkması.</em></sub>

⏳ Insecure Output Handling — <sub><em>Model çıktılarının doğrulanmadan shell, database, browser, HTML renderer veya downstream application tarafından çalıştırılması.</em></sub>

⏳ Excessive Agency — <sub><em>Agentlara gereğinden fazla sistem, dosya, network, database veya cloud yetkisi verilmesi.</em></sub>

⏳ Agent Identity & Authorization — <sub><em>Kullanıcı kimliği, agent identity, delegated permissions, service accounts ve end-to-end authorization kontrolleri.</em></sub>

⏳ Tool-Calling Security — <sub><em>Tool allowlists, parameter validation, schema enforcement, confirmation gates ve destructive-action restrictions.</em></sub>

⏳ MCP Server & Tool Security — <sub><em>Malicious MCP servers, tool impersonation, poisoned tool descriptions, overprivileged tools ve untrusted resources.</em></sub>

⏳ Agent Memory Security — <sub><em>Persistent memory, conversation history, long-term state ve stored context bilgilerinin zehirlenmesi veya sızdırılması.</em></sub>

⏳ Context Poisoning — <sub><em>Model context'inin yanlış, kötü amaçlı veya yanıltıcı bilgilerle manipüle edilmesi.</em></sub>

⏳ RAG Poisoning — <sub><em>Knowledge base, document store veya retrieved content içine manipüle edilmiş içerik yerleştirilmesi.</em></sub>

⏳ Retrieval Authorization — <sub><em>Kullanıcının yalnızca erişim yetkisi bulunan dokümanların RAG pipeline tarafından getirilmesi.</em></sub>

⏳ Vector Database Security — <sub><em>Authentication, encryption, tenant isolation, metadata authorization, backup ve audit logging.</em></sub>

⏳ Cross-Tenant Data Leakage — <sub><em>Çok kiracılı sistemlerde kullanıcı veya kurum verilerinin başka tenant'lara sızması.</em></sub>

⏳ SSRF & External-Resource Abuse — <sub><em>Agent veya tool mekanizmalarının internal services, metadata endpoints veya yetkisiz URL'lere erişmek için kullanılması.</em></sub>

⏳ Code-Execution & Sandbox Risks — <sub><em>Model tarafından üretilen kodun kontrollü olmayan ortamlarda çalıştırılması ve sandbox escape riskleri.</em></sub>

⏳ Output Validation & Policy Enforcement — <sub><em>Structured output, schema validation, content filtering, allowlists ve deterministic security controls.</em></sub>

⏳ Human-in-the-Loop Controls — <sub><em>Para transferi, veri silme, production değişikliği, email gönderme ve credential işlemlerinde insan onayı.</em></sub>

⏳ Rate Limiting & Cost Controls — <sub><em>Token, request, concurrency, tool-call ve spending limitleriyle abuse ve denial-of-wallet risklerinin azaltılması.</em></sub>

⏳ Secure Conversation Design — <sub><em>Session isolation, context boundaries, history management ve conversation-state güvenliği.</em></sub>

🧬 Module 19: Adversarial Machine Learning & Model Security — ⏳ Planned

⏳ Adversarial ML Foundations — <sub><em>Model davranışının kötü amaçlı girdiler, training data veya model artifacts üzerinden manipüle edilmesi.</em></sub>

⏳ Evasion Attacks — <sub><em>Model inference sırasında girdilerin sınıflandırma veya karar mekanizmasını yanıltacak biçimde değiştirilmesi.</em></sub>

⏳ Adversarial Examples — <sub><em>İnsan için küçük görünen ancak model sonucunu önemli ölçüde değiştiren crafted inputs.</em></sub>

⏳ Training-Data Poisoning — <sub><em>Training veya fine-tuning verilerinin model davranışını bozmak amacıyla manipüle edilmesi.</em></sub>

⏳ Backdoor & Trojan Attacks — <sub><em>Belirli trigger girdilerinde saldırganın istediği sonucu üreten gizli model davranışları.</em></sub>

⏳ Fine-Tuning Attacks — <sub><em>Zararlı dataset, adapter veya fine-tuning süreçleri üzerinden model davranışının değiştirilmesi.</em></sub>

⏳ Model Extraction — <sub><em>Query-response interactions üzerinden hedef modelin davranışının veya işlevinin kopyalanması.</em></sub>

⏳ Model Inversion — <sub><em>Model outputs veya gradients üzerinden training data özelliklerinin yeniden oluşturulmaya çalışılması.</em></sub>

⏳ Membership Inference — <sub><em>Belirli bir verinin modelin training dataset'i içinde bulunup bulunmadığının tahmin edilmesi.</em></sub>

⏳ Model Fingerprinting — <sub><em>Model family, version, architecture veya provider bilgilerinin davranışsal yöntemlerle belirlenmesi.</em></sub>

⏳ Model Theft — <sub><em>Model weights, checkpoints, adapters, architecture bilgileri veya private endpoints'in yetkisiz biçimde ele geçirilmesi.</em></sub>

⏳ Privacy Attacks — <sub><em>Training data memorization, personal-data leakage ve reconstruction saldırıları.</em></sub>

⏳ Model Integrity Verification — <sub><em>Hashes, digital signatures, artifact validation ve trusted model-source kontrolleri.</em></sub>

⏳ Robustness Evaluation — <sub><em>Modelin adversarial, noisy, out-of-distribution ve manipulated inputs karşısındaki dayanıklılığının ölçülmesi.</em></sub>

⏳ Mitigation Limitations — <sub><em>Adversarial training, filtering, detection ve guardrail yöntemlerinin sınırlamalarının değerlendirilmesi.</em></sub>

🗃️ Module 20: AI Data Security, Privacy & Knowledge Protection — ⏳ Planned

⏳ AI Data Classification — <sub><em>Public, internal, confidential, restricted, personal ve regulated data türlerinin sınıflandırılması.</em></sub>

⏳ Dataset Inventory & Ownership — <sub><em>Dataset owner, source, purpose, access, retention ve approved-use bilgilerinin kayıt altına alınması.</em></sub>

⏳ Data Lineage & Provenance — <sub><em>Verinin kaynağı, dönüşümleri, training süreçleri ve model üzerindeki kullanımının izlenebilir olması.</em></sub>

⏳ Data Quality & Integrity — <sub><em>Eksik, yanlış, manipüle edilmiş, outdated veya malicious data'nın tespit edilmesi.</em></sub>

⏳ Training-Data Access Control — <sub><em>Dataset storage, notebooks, pipelines ve processing systems için least-privilege erişim.</em></sub>

⏳ Secrets & Credential Detection — <sub><em>API keys, passwords, certificates, tokens ve connection strings'in dataset, prompt veya notebook içinde bulunmasının engellenmesi.</em></sub>

⏳ Personal-Data Protection — <sub><em>PII detection, masking, anonymization, pseudonymization ve data minimization.</em></sub>

⏳ Encryption — <sub><em>Data at rest, data in transit, model artifacts, embeddings, backups ve logs için şifreleme.</em></sub>

⏳ RAG Knowledge-Base Protection — <sub><em>Document permissions, ingestion approval, integrity validation ve source trust değerlendirmesi.</em></sub>

⏳ Prompt & Response Privacy — <sub><em>Kullanıcı promptları, model outputs, feedback verileri ve conversation history'nin güvenli saklanması.</em></sub>

⏳ Logging & Retention Controls — <sub><em>Prompt, response ve tool-call loglarının içerik, erişim, saklama süresi ve silme politikaları.</em></sub>

⏳ Tenant Isolation — <sub><em>Dataset, vector namespace, memory ve retrieval context'in tenant bazında ayrılması.</em></sub>

⏳ Data-Poisoning Detection — <sub><em>Anomalous records, duplicate content, malicious instructions ve unexpected distribution değişimlerinin tespiti.</em></sub>

⏳ Data Usage Rights — <sub><em>Dataset licensing, consent, intellectual property ve permitted-use gereksinimleri.</em></sub>

⏳ Privacy-Enhancing Technologies — <sub><em>Differential privacy, federated learning, secure aggregation ve confidential-computing kavramları.</em></sub>

⏳ Secure Data Deletion — <sub><em>Dataset, embeddings, caches, model artifacts ve backups içindeki verilerin lifecycle'a uygun silinmesi.</em></sub>

🖥️ Module 21: Secure AI Infrastructure & Model Serving — ⏳ Planned

⏳ AI Infrastructure Architecture — <sub><em>Model gateway, inference server, vector database, object storage, agent runtime, API ve monitoring bileşenleri.</em></sub>

⏳ GPU & Accelerator Fundamentals — <sub><em>GPU resources, drivers, CUDA ecosystem, device access ve accelerator-sharing kavramları.</em></sub>

⏳ GPU Workload Security — <sub><em>GPU device permissions, workload isolation, driver security, resource quotas ve multi-tenant accelerator riskleri.</em></sub>

⏳ Secure Model Serving — <sub><em>Model loading, inference endpoints, batching, caching, streaming ve request-isolation güvenliği.</em></sub>

⏳ Containerized AI Workloads — <sub><em>Non-root containers, minimal images, reduced capabilities, read-only filesystems ve image scanning.</em></sub>

⏳ Kubernetes for AI Security — <sub><em>GPU scheduling, ServiceAccounts, RBAC, Pod Security Standards, NetworkPolicy ve workload isolation.</em></sub>

⏳ Model-Serving Platforms — <sub><em>vLLM, Triton, KServe, Ray Serve veya benzeri serving çözümlerinin güvenlik mimarisi.</em></sub>

⏳ Identity & Workload Authentication — <sub><em>Users, services, agents, workloads ve external APIs için güçlü kimlik doğrulama.</em></sub>

⏳ Secrets & Key Management — <sub><em>Model API keys, database credentials, encryption keys ve signing keys'in güvenli yönetimi.</em></sub>

⏳ Network Segmentation — <sub><em>Training, inference, management, storage ve monitoring ağlarının ayrılması.</em></sub>

⏳ API Gateway Security — <sub><em>Authentication, authorization, request limits, schema validation, TLS termination ve audit logging.</em></sub>

⏳ Service-to-Service Security — <sub><em>Mutual TLS, workload identities, certificate rotation ve service-mesh kontrolleri.</em></sub>

⏳ Multi-Tenancy Isolation — <sub><em>Compute, memory, network, model, cache ve data katmanlarında tenant ayrımı.</em></sub>

⏳ Model Registry Security — <sub><em>Model versioning, access control, signing, approval workflows ve artifact immutability.</em></sub>

⏳ Artifact Storage Security — <sub><em>Model weights, checkpoints, adapters, tokenizer files ve configuration dosyalarının korunması.</em></sub>

⏳ Secure Sandboxing — <sub><em>Untrusted code execution, interpreters, browser tools ve file-processing workloads için izolasyon.</em></sub>

⏳ Availability & Resource Exhaustion — <sub><em>GPU, CPU, memory, token, queue ve concurrency tüketimine karşı koruma.</em></sub>

⏳ Denial-of-Service & Denial-of-Wallet — <sub><em>Yüksek maliyetli inference requests, long-context attacks ve automated API abuse.</em></sub>

⏳ AI Infrastructure Observability — <sub><em>Inference latency, token usage, errors, resource consumption, model loading ve tool-call telemetry.</em></sub>

⏳ Backup, Recovery & Resilience — <sub><em>Model registry, vector database, configuration, prompts ve AI-service dependencies için recovery planları.</em></sub>

⏳ Cloud AI Shared Responsibility — <sub><em>Managed model services, customer data, IAM, network, logging ve application-security sorumluluklarının ayrılması.</em></sub>

🔗 Module 22: MLSecOps, AI Supply Chain & Secure AI Development Lifecycle — ⏳ Planned

⏳ MLSecOps Foundations — <sub><em>Machine Learning, MLOps, DevSecOps ve AI Security kontrollerinin tek yaşam döngüsünde birleştirilmesi.</em></sub>

⏳ Secure AI SDLC — <sub><em>Requirements, design, development, training, testing, release, deployment, monitoring ve retirement süreçlerine güvenlik eklenmesi.</em></sub>

⏳ AI Pipeline Security — <sub><em>Data ingestion, feature engineering, training, evaluation, packaging ve deployment pipeline'larının korunması.</em></sub>

⏳ Secure Development Environments — <sub><em>Notebooks, IDEs, experiment platforms, build runners ve development credentials güvenliği.</em></sub>

⏳ Dataset Supply-Chain Security — <sub><em>Third-party datasets, provenance, licenses, integrity checks ve poisoning riskleri.</em></sub>

⏳ Model Supply-Chain Security — <sub><em>Public models, adapters, checkpoints ve pre-trained artifacts için source-trust değerlendirmesi.</em></sub>

⏳ Third-Party Model Risk — <sub><em>Model provider, hosting service, API, license, data usage ve security-assurance kontrolleri.</em></sub>

⏳ Dependency Security — <sub><em>Python packages, container images, ML libraries, CUDA dependencies ve transitive-dependency riskleri.</em></sub>

⏳ Unsafe Model Serialization — <sub><em>Pickle ve benzeri formats üzerinden arbitrary-code-execution riskleri ve güvenli format değerlendirmesi.</em></sub>

⏳ Artifact Signing & Verification — <sub><em>Model, dataset, container ve configuration artifacts için digital signatures, hashes ve provenance.</em></sub>

⏳ AI Bill of Materials — <sub><em>Model, dataset, library, container, provider ve service dependencies'in envanterinin oluşturulması.</em></sub>

⏳ Reproducibility — <sub><em>Dataset version, code commit, configuration, dependency, model checkpoint ve environment kayıtlarının tutulması.</em></sub>

⏳ Secure CI/CD for AI — <sub><em>Automated tests, security scanning, policy checks, approvals ve protected deployment environments.</em></sub>

⏳ Security Evaluation Gates — <sub><em>Model veya AI uygulaması production'a alınmadan önce zorunlu güvenlik ve risk testleri.</em></sub>

⏳ Model Registry Promotion — <sub><em>Development, testing, staging ve production modelleri arasında kontrollü promotion.</em></sub>

⏳ Canary & Shadow Deployment — <sub><em>Yeni model sürümlerinin sınırlı trafik veya shadow mode ile güvenli doğrulanması.</em></sub>

⏳ Rollback & Model Revocation — <sub><em>Güvensiz veya bozuk model sürümlerinin hızla geri alınması ve kullanımının engellenmesi.</em></sub>

⏳ Drift & Unauthorized Change Detection — <sub><em>Model, dataset, configuration ve infrastructure üzerinde beklenmeyen değişikliklerin tespiti.</em></sub>

⏳ Vulnerability & Patch Management — <sub><em>AI frameworks, serving platforms, drivers, dependencies ve containers için güncelleme süreçleri.</em></sub>

⏳ Vendor & Acquisition Security — <sub><em>Satın alınan veya dışarıdan kullanılan AI sistemlerinin security requirements ve assurance evidence ile değerlendirilmesi.</em></sub>

🧪 Module 23: AI Security Testing, Evaluation & Red Teaming — ⏳ Planned

⏳ AI Security Test Planning — <sub><em>System scope, assets, threat model, test objectives, rules of engagement ve success criteria.</em></sub>

⏳ Prompt Fuzzing — <sub><em>Farklı prompt structures, encodings, languages, obfuscation ve edge cases ile otomatik güvenlik testi.</em></sub>

⏳ Jailbreak Testing — <sub><em>Model policy ve guardrail sınırlarının yetkili test ortamlarında değerlendirilmesi.</em></sub>

⏳ Prompt-Injection Testing — <sub><em>Direct ve indirect injection senaryolarının application, RAG ve agent katmanlarında denenmesi.</em></sub>

⏳ RAG Security Testing — <sub><em>Malicious documents, poisoned context, unauthorized retrieval ve source-manipulation senaryoları.</em></sub>

⏳ Agentic Security Testing — <sub><em>Tool abuse, privilege escalation, unsafe planning, memory poisoning ve autonomous-action testleri.</em></sub>

⏳ MCP Security Testing — <sub><em>Malicious server, poisoned tool metadata, unauthorized resource ve tool substitution senaryoları.</em></sub>

⏳ Model Extraction Testing — <sub><em>Query limits, output precision, rate controls ve model-copying risklerinin değerlendirilmesi.</em></sub>

⏳ Privacy Leakage Testing — <sub><em>Memorized data, personal information, training-data exposure ve membership-inference senaryoları.</em></sub>

⏳ Adversarial Input Testing — <sub><em>Evasion, adversarial examples, malformed data ve multimodal-input manipülasyonları.</em></sub>

⏳ Multimodal Security Testing — <sub><em>Image, audio, video, document ve hidden-instruction içeriklerinin modele etkisi.</em></sub>

⏳ Tool & Output Exploitation — <sub><em>Model outputs üzerinden SQL injection, command injection, XSS, SSRF veya unsafe code execution riskleri.</em></sub>

⏳ Abuse & Cost Testing — <sub><em>High-token prompts, recursive agents, repeated tool calls ve resource-exhaustion senaryoları.</em></sub>

⏳ Automated AI Evaluations — <sub><em>Repeatable test suites, benchmark datasets, attack templates ve regression testing.</em></sub>

⏳ Human Red Teaming — <sub><em>Context-dependent, creative ve chained attack paths'in deneyimli test uzmanları tarafından incelenmesi.</em></sub>

⏳ Security vs Safety Evaluation — <sub><em>Siber güvenlik açıkları, kullanım politikası ihlalleri ve harmful-output risklerinin ayrı fakat ilişkili değerlendirilmesi.</em></sub>

⏳ Findings & Severity Assessment — <sub><em>Exploitability, business impact, affected assets, reproducibility ve remediation priority.</em></sub>

⏳ Remediation Verification — <sub><em>Uygulanan kontrollerin tekrar test edilmesi ve yeni bypass yöntemlerinin araştırılması.</em></sub>

⏳ Responsible Testing Boundaries — <sub><em>Yalnızca yetkili sistemlerde, kontrollü test verileri ve sınırlı blast radius ile çalışma.</em></sub>

📡 Module 24: AI Security Monitoring, Threat Detection & Incident Response — ⏳ Planned

⏳ AI Security Telemetry — <sub><em>Prompt, response, retrieval, model, tool-call, identity, authorization, network ve infrastructure olaylarının kaydı.</em></sub>

⏳ Secure AI Logging — <sub><em>Hassas veriyi gereksiz toplamadan olay araştırmasına yetecek seviyede audit trail oluşturulması.</em></sub>

⏳ Prompt & Response Monitoring — <sub><em>Injection patterns, data leakage, policy bypass ve suspicious-output davranışlarının tespiti.</em></sub>

⏳ Agent Activity Monitoring — <sub><em>Tool selection, parameter values, file access, network requests, external actions ve approval decisions.</em></sub>

⏳ RAG Monitoring — <sub><em>Retrieved documents, source trust, unusual retrieval patterns ve authorization failures.</em></sub>

⏳ Model-Behavior Monitoring — <sub><em>Unexpected outputs, integrity issues, safety-control degradation ve model-version farklılıkları.</em></sub>

⏳ Abuse Detection — <sub><em>Automated scraping, model extraction, credential testing, spam, fraud ve denial-of-wallet davranışları.</em></sub>

⏳ Drift Monitoring — <sub><em>Data drift, concept drift, performance drift ve security-control drift arasındaki farklar.</em></sub>

⏳ AI Security Metrics — <sub><em>Injection success rate, blocked actions, sensitive-data exposure, anomalous tool calls ve security-test regression results.</em></sub>

⏳ SIEM Integration — <sub><em>AI application, model gateway, cloud, Kubernetes ve identity logs'un merkezi güvenlik izleme sistemlerine aktarılması.</em></sub>

⏳ Detection Engineering — <sub><em>AI-specific detection rules, behavioral baselines, correlation rules ve alert tuning.</em></sub>

⏳ Threat Hunting with MITRE ATLAS — <sub><em>AI sistemlerine yönelik tactics ve techniques üzerinden hipotez tabanlı araştırma.</em></sub>

⏳ AI Incident Classification — <sub><em>Prompt injection, data leakage, compromised agent, poisoned RAG source, stolen model ve supply-chain compromise olayları.</em></sub>

⏳ AI Incident Triage — <sub><em>Etkilenen model, dataset, users, tools, credentials, infrastructure ve business processes'in belirlenmesi.</em></sub>

⏳ Containment Strategies — <sub><em>Agent tools'un kapatılması, model endpoint'in izole edilmesi, retrieval source'un kaldırılması ve access tokens'ın iptali.</em></sub>

⏳ Eradication & Recovery — <sub><em>Poisoned content'in temizlenmesi, model veya dataset'in güvenli sürümden yeniden yüklenmesi ve controls'un doğrulanması.</em></sub>

⏳ Model Rollback & Revocation — <sub><em>Güvensiz model, adapter veya configuration sürümünün kullanım dışı bırakılması.</em></sub>

⏳ Credential & Secret Rotation — <sub><em>Agent, service, model provider ve tool credentials bilgilerinin olay sonrasında yenilenmesi.</em></sub>

⏳ AI Forensics & Evidence Preservation — <sub><em>Prompts, outputs, retrieved content, tool executions, model versions ve configuration bilgilerinin korunması.</em></sub>

⏳ Communication & Reporting — <sub><em>Teknik ekipler, yönetim, hukuk, privacy ve business stakeholders için olay iletişimi.</em></sub>

⏳ Post-Incident Review — <sub><em>Timeline, root causes, control failures, lessons learned ve corrective actions.</em></sub>

⚖️ Module 25: AI Governance, Assurance, Standards & Compliance — ⏳ Planned

⏳ AI Governance Foundations — <sub><em>AI ownership, roles, responsibilities, decision rights ve organizational oversight.</em></sub>

⏳ NIST AI RMF — <sub><em>Govern, Map, Measure ve Manage fonksiyonlarıyla AI risk-management programı oluşturulması.</em></sub>

⏳ NIST Generative AI Profile — <sub><em>Generative AI sistemlerine özgü risklerin ve risk-treatment yöntemlerinin değerlendirilmesi.</em></sub>

⏳ NIST Secure AI Development Practices — <sub><em>AI model ve sistem geliştirme süreçlerinin Secure Software Development Framework ile uyumlu hâle getirilmesi.</em></sub>

⏳ ISO/IEC 42001 — <sub><em>Artificial Intelligence Management System kurulumu, policy, objectives, risk management ve continuous improvement.</em></sub>

⏳ ISO/IEC 23894 — <sub><em>AI risk-management principles, lifecycle risk identification ve risk-treatment yaklaşımı.</em></sub>

⏳ EU AI Act — <sub><em>Prohibited practices, risk classifications, high-risk systems, general-purpose AI ve provider/deployer obligations.</em></sub>

⏳ CSA AI Controls Matrix — <sub><em>Cloud tabanlı AI sistemleri için governance, security, privacy, supply chain ve operational controls.</em></sub>

⏳ OWASP & MITRE Mapping — <sub><em>OWASP AI risks ve MITRE ATLAS techniques'in security controls ve test cases ile eşleştirilmesi.</em></sub>

⏳ AI System Inventory — <sub><em>Kurum içinde geliştirilen, kullanılan veya third party üzerinden alınan AI sistemlerinin kayıt altına alınması.</em></sub>

⏳ AI Risk Classification — <sub><em>Use case, autonomy, data sensitivity, user impact, business criticality ve regulatory exposure değerlendirmesi.</em></sub>

⏳ AI Impact Assessments — <sub><em>Güvenlik, privacy, safety, fairness, human rights ve operational etkilerin sistem devreye alınmadan incelenmesi.</em></sub>

⏳ Model Cards & System Cards — <sub><em>Model purpose, limitations, training information, evaluation results, risks ve intended-use bilgilerinin dokümantasyonu.</em></sub>

⏳ Transparency & Explainability — <sub><em>Kullanıcı bilgilendirmesi, AI-generated content disclosure, model limitations ve decision explanations.</em></sub>

⏳ Human Oversight — <sub><em>Kritik karar ve eylemlerde insan kontrolü, intervention, override ve escalation mekanizmaları.</em></sub>

⏳ Accountability & Auditability — <sub><em>AI kararlarının, model versions'ın, approvals'ın ve configuration değişikliklerinin izlenebilir olması.</em></sub>

⏳ Third-Party Assurance — <sub><em>Model providers, datasets, AI APIs ve SaaS services için due diligence ve contractual security requirements.</em></sub>

⏳ Acceptable AI Use Policies — <sub><em>Çalışanların AI araçlarıyla hangi veri ve sistemleri kullanabileceğini belirleyen kurallar.</em></sub>

⏳ AI Exception & Risk Acceptance — <sub><em>Güvenlik kontrollerinden sapmaların business owner ve risk owner tarafından onaylanması.</em></sub>

⏳ Continuous Compliance — <sub><em>AI system, model, dataset, regulation ve control değişikliklerinin sürekli takip edilmesi.</em></sub>

⏳ Responsible AI Principles — <sub><em>Privacy, fairness, transparency, reliability, safety, security ve accountability gereksinimlerinin birlikte yönetilmesi.</em></sub>

🏗️ Level 5 — Capstone Projects

⏳ Project 1: Secure RAG Application — <sub><em>Authentication, document-level authorization, vector database isolation, prompt-injection protection, output validation, logging ve security testing içeren güvenli RAG uygulaması.</em></sub>

⏳ Project 2: Agentic AI Security Laboratory — <sub><em>Kontrollü sandbox içinde tool kullanan bir AI agent'ın kurulması; least privilege, approval gates, MCP security, memory isolation, monitoring ve red-team testleri.</em></sub>

⏳ Project 3: Secure AI Model-Serving Platform — <sub><em>Docker veya Kubernetes üzerinde model serving; API gateway, TLS, workload identity, secrets management, rate limiting, model registry, monitoring ve rollback.</em></sub>

⏳ Project 4: AI Supply-Chain Security Pipeline — <sub><em>Model, dataset, container ve dependency artifacts için provenance, signing, vulnerability scanning, approval gates ve controlled deployment pipeline'ı.</em></sub>

⏳ Project 5: AI Security Monitoring & Incident Response Lab — <sub><em>Prompt, RAG, agent ve infrastructure telemetry'sinin SIEM'e aktarılması; detection rules, simulated incidents, containment ve post-incident documentation.</em></sub>

⏳ Project 6: Production-Grade Cloud AI Security Architecture — <sub><em>AWS, Terraform, Kubernetes, managed AI services, private networking, IAM, encryption, centralized logging, high availability ve AI governance kontrolleriyle uçtan uca güvenli platform.</em></sub>

<div align="right">

⬆️ Back to Top

</div>

</details>

<a id="milestone-projects"></a>

🏆 Milestone Portfolio Projects

Project 1 — Hardened & Audited Linux Server Infrastructure

Zamanlama: Level 1 tamamlandıktan sonra.

Networking, Linux administration, users and permissions, systemd, Linux networking, nftables, SSH hardening, Fail2Ban, auditd, logging, backup, Bash automation, incident validation ve teknik dokümantasyon alanlarını birleştiren kapsamlı bir sunucu güvenliği projesi.

Project 2 — Automated Provisioning of a Production-Oriented Kubernetes Environment

Zamanlama: Level 2 tamamlandıktan sonra.

Docker, secure images, Kubernetes workloads, Services, RBAC, NetworkPolicy, storage, probes, Terraform, remote state, Ansible, AWS VPC/EC2, automated validation ve operasyonel dokümantasyon alanlarını birleştiren otomatik altyapı projesi.

Project 3 — Resilient Cloud-Native GitOps & Observability Platform

Zamanlama: Level 3 ve Level 4 tamamlandıktan sonra.

CI/CD, DevSecOps security gates, software supply-chain security, GitOps, Kubernetes, Prometheus, Grafana, Loki, OpenTelemetry, alerting, SLOs, AWS Multi-AZ, IAM, backup, disaster recovery, resilience testing, runbooks, postmortems, performance analysis ve AI-assisted operational workflows alanlarını birleştiren kapsamlı platform mühendisliği projesi.

🔎 Detailed Milestone Project Specifications

The following three progressively advanced portfolio projects are planned to validate the knowledge acquired throughout the roadmap.

Each project combines concepts from multiple modules and is designed around production-oriented security, automation, reliability, validation, and documentation practices.

🛡️ Project 1: Hardened & Audited Private Linux Server Infrastructure — ⏳ Planned

Focus: Host Hardening, Stateful Firewalling, Security Auditing and Log-Based Abuse MitigationTimeline: At the end of Level 1 — Module 03

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

📦 Project 2: Declarative Provisioning of a Production-Oriented Kubernetes Environment — ⏳ Planned

Focus: Cloud Provisioning, Infrastructure as Code, Configuration Management and Kubernetes OrchestrationTimeline: At the end of Level 2 — Module 06

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

🚀 Project 3: Resilient Cloud-Native GitOps & Observability Platform — ⏳ Planned

Focus: DevSecOps, Declarative Delivery, SRE Practices, Observability and Multi-AZ ResilienceTimeline: At the end of Level 4 — Module 11

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

<a id="documentation-approach"></a>

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

<a id="project-status"></a>

⚠️ Project Status Notice

This repository represents an ongoing learning process.

Completed entries describe topics that have been studied and reviewed. Planned modules and project descriptions define the intended scope of future work.

Technologies, architectures, implementation details and project boundaries may be revised as the systems are built, tested, measured and documented.

A planned feature should not be interpreted as an already implemented or validated capability.

<div align="center">

<sub>Built as a living learning journal focused on secure systems, automation, reliability and measurable engineering practice.</sub>

⬆️ Back to Top

</div>
