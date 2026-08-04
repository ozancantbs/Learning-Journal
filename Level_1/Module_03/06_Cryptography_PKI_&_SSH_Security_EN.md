# 06. Cryptography, PKI & SSH Security

## 🇬🇧 English Documentation

### Overview
Cryptography provides the core mathematical primitives for achieving confidentiality, integrity, authenticity, and non-repudiation in modern computing systems. This module explores symmetric and asymmetric encryption, cryptographic hashing, digital signatures, Public Key Infrastructure (PKI), digital certificates, and the comprehensive hardening of Secure Shell (SSH) communication channels.

---

### 1. Cryptographic Primitives & Mechanisms

* **Symmetric Cryptography:** Uses a single shared secret key for both encryption and decryption. Offers extreme computational speed for high-volume data streams. Common algorithms include AES-256 (GCM/CBC modes) and ChaCha20-Poly1305.
* **Asymmetric Cryptography:** Employs mathematically linked key pairs—a public key for encryption or signature verification, and a private key for decryption or signing. Solves the key distribution challenge. Common algorithms include RSA (2048/4096-bit), ECC (Elliptic Curve Cryptography), and Ed25519.
* **Cryptographic Hashing:** One-way mathematical functions that convert arbitrary input data into a fixed-size digest. They are irreversible and collision-resistant. Essential for secure password storage (Argon2, bcrypt) and file integrity verification (SHA-256, SHA-3).
* **Digital Signatures:** Combines asymmetric encryption with cryptographic hashing to guarantee message integrity, sender authenticity, and non-repudiation.

---

### 2. Public Key Infrastructure (PKI) & Digital Certificates

PKI establishes a trustworthy framework for distributing, validating, and revoking public keys across untrusted networks:

* **Certificate Authorities (CAs):** Trusted entities that issue and digitally sign certificates to bind a public key to an verified identity (domain name, organization, or user).
* **X.509 Certificate Structure:** Standard format containing the subject identity, public key, issuer signature, validity period, and extensions (such as Subject Alternative Names / SAN).
* **Certificate Lifecycle Management:**
  1. **CSR Generation:** Generating a private key and a Certificate Signing Request (CSR) containing identity metadata.
  2. **Issuance:** The CA validates domain or organization ownership and returns a signed X.509 certificate.
  3. **Revocation Validation:** Checking certificate validity status via CRLs (Certificate Revocation Lists) or real-time OCSP (Online Certificate Status Protocol) stapling.

---

### 3. SSH Architecture & Daemon Hardening

Secure Shell (SSH) provides encrypted interactive shell access and file transfer capabilities. Hardening the SSH daemon (`sshd`) eliminates automated brute-force attacks, credential theft, and protocol downgrade risks.

#### Cryptographic Handshake & Host Keys
Upon connection initiation, SSH performs Diffie-Hellman or ECDH key exchange to establish an encrypted tunnel before user authentication occurs. Host keys (`/etc/ssh/ssh_host_*_key`) authenticate the server identity to prevent Man-in-the-Middle (MITM) attacks.

#### Recommended Security Enforcement (`/etc/ssh/sshd_config`):
* `Port 2222`: Obfuscates default port 22 to eliminate automated scanner noise.
* `PermitRootLogin no`: Disables direct root shell access, forcing accountable user elevation via `sudo`.
* `PasswordAuthentication no`: Disables plain password logins in favor of SSH Key Pairs.
* `KbdInteractiveAuthentication no`: Blocks PAM interactive password prompts.
* `PubkeyAuthentication yes`: Mandates asymmetric key pair authentication.
* `HostKeyAlgorithms`: Restricts acceptable host keys to strong algorithms (Ed25519, RSA-SHA2-512).
* `KexAlgorithms`: Restricts key exchange to secure curves (e.g., `curve25519-sha256`).

---

### 🛠️ Hands-On Execution & Hardening Commands

* **Generate High-Security Ed25519 SSH Key Pair:**  
  `ssh-keygen -t ed25519 -a 100 -C "admin@security.internal"`

* **Generate OpenSSL Private Key & CSR (Certificate Signing Request):**  
  `openssl req -new -newkey rsa:4096 -nodes -keyout server.key -out server.csr`

* **Inspect & Verify X.509 Certificate Metadata:**  
  `openssl x509 -in server.crt -text -noout`

* **Validate SSH Daemon Configuration Syntax:**  
  `sudo sshd -t`

---

### 📊 Cryptographic Implementation Matrix

| Security Primitive | Primary Algorithm | Target Use Case | Defensive Benefit |
| :--- | :--- | :--- | :--- |
| **Symmetric Encryption** | AES-256-GCM / ChaCha20 | Data-at-Rest & TLS Streams | High-speed confidentiality with auth tag |
| **Asymmetric Key Exchange** | Ed25519 / Curve25519 | SSH Access & TLS Handshakes | Mathematical resistance to brute-force |
| **Cryptographic Hash** | SHA-256 / Argon2id | Password Storage & Integrity | Irreversible, salt-protected hash digests |
| **PKI Validation** | X.509 + OCSP Stapling | Web & Service Identity | Prevents MITM & unauthorized spoofing |
