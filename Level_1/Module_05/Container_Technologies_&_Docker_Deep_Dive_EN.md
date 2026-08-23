# 🐳 Module 05: Container Technologies & Docker Deep Dive
> **Engineering Learning Journal & DevSecOps Architecture Log**  
> **Focus:** Containerization Mechanics, Linux Kernel Isolation, Storage Strategies, Deterministic Networking, and Supply Chain Security.

---

## 📌 Architectural Overview & Core Mission
This module covers OS-level virtualization and container runtime mechanics, prioritizing a **DevSecOps-first approach**. Rather than treating containers as opaque black boxes, this module breaks down lower-level host kernel primitives (Namespaces, Cgroups v2), Open Container Initiative (OCI) engine abstractions, layer-based storage drivers, deterministic networking models, and supply chain hardening patterns.

---

## 🏗️ Deep-Dive Architectural Breakdown

### 1. Containerization vs. Hypervisor Virtualization
* **Hypervisor Architecture (VMs):** Hardware-level virtualization. Requires a dedicated Guest OS per instance, introducing heavy resource overhead, slow boot times, and duplicated kernel footprints.
* **OS-Level Virtualization (Containers):** Shared-kernel architecture. Containers abstract user-space binaries and dependencies while sharing the host’s underlying Linux Kernel, providing millisecond-level startup times and minimal memory footprint.

### 2. Linux Kernel Isolation Primitives
* **Namespaces (Visibility Boundaries):** 
  * `PID`: Process isolation (container process sees itself as PID 1).
  * `NET`: Virtualized network stacks (`veth` pairs, routing tables).
  * `MNT`: Isolated file system mount points.
  * `IPC`: Inter-process communication boundary.
  * `UTS`: Independent hostname configuration.
  * `USER`: Unprivileged user ID mapping (container root mapped to non-root host UID).
* **Cgroups v2 (Resource Capping & Control):**
  * Enforces hard upper limits on CPU, Memory, and I/O utilization.
  * **OOMKilled Behavior (Exit Code 137):** Triggers when a process breaches its allocated Cgroup memory boundary, preventing host-wide kernel starvation.

### 3. Container Runtime Internals & OCI Standards
* **High-Level vs. Low-Level Runtimes:** Decoupled execution model conforming to Open Container Initiative (OCI) specifications.
* **Execution Stack:** `Docker CLI` ➔ `dockerd` (Daemon) ➔ `containerd` (Image lifecycle & management) ➔ `runc` (OCI low-level worker) ➔ `Linux Kernel`.
* **Kubernetes Alignment:** `containerd` communicates directly via the Container Runtime Interface (CRI), removing the legacy need for full Docker Engine dependencies in modern orchestrators.

### 4. Docker Core: Layers, Caching & Multi-Stage Builds
* **Layer Mechanics (OverlayFS):** Stacked read-only layers topped with a single ephemeral read-write container layer.
* **Build Cache Optimization:** Ordering Dockerfile instructions from least-frequently changed (`package.json`, system packages) to most-frequently changed (source code) to maximize build-cache hits.
* **Multi-Stage Build Pipeline:** Separation of the build SDK environment from the final execution runtime, resulting in minimal production footprints (`node:20-alpine`, Distroless) and stripped attack surfaces.

### 5. Storage Architecture & Persistence Strategies
* **Writable Layer:** High I/O penalty (Copy-on-Write) and strictly ephemeral; unfit for database persistence.
* **Named Volumes:** Docker-managed host paths (`/var/lib/docker/volumes/`). Production standard for stateful services and database durability.
* **Bind Mounts:** Direct host directory mapping. Reserved exclusively for local development hot-reloading.
* **tmpfs Mounts:** Host RAM storage for transient, high-performance, or sensitive in-memory data (never written to disk).
* **UID/GID Ownership:** Explicit host-to-container permission mapping to prevent `Permission Denied` exceptions when running non-root containers over mounted volumes.

### 6. Container Networking & Service Discovery
* **Network Drivers:** `Bridge` (Default/Custom), `Host` (Zero-NAT overhead), `Overlay` (Multi-node VXLAN bridging).
* **User-Defined Bridge Networks:** Embedded DNS resolver (`127.0.0.11`) enabling dynamic **Service Discovery** via container names instead of unstable ephemeral IPs.
* **NAT & Port Publishing (`-p`):** `iptables` rules mapping host ports to isolated container network namespaces.
* **Least-Privilege Exposure:** Internal backend services (e.g., PostgreSQL on 5432) remain unpublished on custom bridges, accessible solely via inter-container DNS.

### 7. Multi-Container Orchestration (Docker Compose)
* **Infrastructure as Code (IaC):** Declarative configuration (`docker-compose.yml`) ensuring 100% environment parity across local, staging, and production tiers.
* **Directed Acyclic Graph (DAG):** Deterministic execution order resolution based on dependency definitions.
* **Healthchecks & Real Readiness:** Using `condition: service_healthy` via health-probe scripts (`pg_isready`) to prevent downstream services (APIs) from starting until upstream dependencies (DBs) are fully ready to accept sockets.

### 8. Supply Chain Security & Image Lifecycle
* **Tags vs. Digests:** Mutability of image tags (`v1.0.0`, `latest`) poses severe supply-chain risks. Immutable cryptographic digests (`sha256:...`) guarantee exact code identity.
* **Registry Hardening:** Enforcing Tag Immutability, Vulnerability Scanning-on-Push, and strict Role-Based Access Control (RBAC) on registries (AWS ECR, GHCR).
* **Image Provenance & Signing:** Cryptographic signing via **Cosign / Sigstore** to verify binary origin and integrity before execution.

### 9. Container Hardening & DevSecOps Defense-in-Depth
* **Non-Root Execution:** Explicit runtime user declaration (`USER 10001`) preventing container escape scenarios from yielding host-level root privilege.
* **Capabilities Dropping:** Invoking `--cap-drop=ALL` to strip extraneous Linux Kernel privileges.
* **Read-Only Root Filesystem:** Executing containers with `--read-only` root filesystems (using `tmpfs` for `/tmp`), blocking runtime malware drops.
* **Automated Vulnerability Scanning:** Integrating **Trivy / Grype** into build pipelines to block images containing critical CVEs.

### 10. Operations, Troubleshooting & Forensic Analysis
* **Adli Analysis & Diagnostics:** Utilizing `docker logs`, `docker inspect`, and real-time kernel event streams (`docker events`) for root-cause failure analysis.
* **Memory Starvation Forensics:** Identifying OOMKilled events via exit code 137 checks.
* **Self-Healing Infrastructure:** Automated restarts driven by container health status monitors.
* **Safe Garbage Collection:** Scheduled prunes (`docker system prune`) to eliminate orphaned layers, networks, and untagged dangling images.

