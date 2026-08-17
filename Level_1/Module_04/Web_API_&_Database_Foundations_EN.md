# Web, API & Database Foundations (Security & Architecture)

A comprehensive reference guide covering core web architectures, API design standards, authentication protocols, browser security mechanisms, and database internals from an engineering and DevSecOps perspective.

---

## 🔬 Core Concepts & System Architecture

### 1. Web Architecture & HTTP/HTTPS Protocols
Modern web applications rely on a client-server model communicating over the Application Layer (Layer 7 of the OSI Model).

*   **Statelessness vs. State Management:** HTTP is fundamentally stateless. Every request is processed independently. State is maintained at the application level using mechanisms like session cookies, server-side session stores (e.g., Redis), or cryptographically signed tokens (e.g., JWT).
*   **HTTP Methods & Idempotency:**
    *   `GET`: Used to retrieve resources. Safe and Idempotent.
    *   `POST`: Used to create new resources. Neither safe nor idempotent.
    *   `PUT`: Replaces the target resource with the request payload. Idempotent.
    *   `PATCH`: Applies partial modifications to a resource. Not inherently idempotent.
    *   `DELETE`: Removes the specified resource. Idempotent.
*   **HTTPS & TLS Handshake (TLS 1.3):**
    1. **Client Hello:** İstemci, desteklenen Cipher Suite'leri ve TLS sürümünü gönderir.
    2. **Server Hello & Certificate:** Sunucu şifreleme algoritmasını seçer ve X.509 Dijital Sertifikasını sunar.
    3. **Certificate Verification:** İstemci, yerel güvenilir Kök Sertifika Otoritelerini (CA) kullanarak zinciri (Chain of Trust) doğrular.
    4. **Key Exchange (ECDHE):** Diffie-Hellman anahtar değişimi ile simetrik oturum anahtarları (Session Keys) türetilir.
    5. **Encrypted Communication:** Tüm HTTP trafiği bu simetrik anahtarla şifrelenerek akar.

---

### 2. Authentication, Session Management & Token Architecture

#### Session-Based vs. Token-Based Authentication
*   **Session-Based:** Server stores session state (e.g., memory, Redis) and passes a `session_id` to the client via a cookie.
*   **Token-Based (Stateless):** Server issues a signed token (JWT) containing claims. The server validates the signature on subsequent requests without querying a session database.

#### Cookie Security Flags
When storing sensitive session identifiers in cookies, the following security attributes are imperative:
*   `HttpOnly`: Prevents client-side scripts (JavaScript) from accessing `document.cookie`, mitigating **XSS-based Session Hijacking**.
*   `Secure`: Ensures cookies are only transmitted over encrypted HTTPS connections.
*   `SameSite`: Controls cross-site cookie transmission to mitigate **CSRF**:
    *   `Strict`: Cookie is never sent in cross-site requests.
    *   `Lax`: Cookie is sent with top-level navigations (e.g., standard links).
    *   `None`: Cookie is sent in all contexts (requires `Secure` attribute).

#### JWT (JSON Web Token) Structure
A JWT consists of three Base64URL-encoded parts separated by dots (`.`):
1.  **Header:** Specifies algorithm (`alg`) and token type (`typ`).
2.  **Payload:** Contains claims (`sub`, `iat`, `exp`, roles, custom data). *Note: Payload is encoded, not encrypted. Do not store secrets here.*
3.  **Signature:** Created by hashing `Header + Payload` with a server-side secret key (HAC) or a private key (RS256). Ensures **integrity** and prevents tampering.

#### OAuth 2.0 & OpenID Connect (OIDC)
*   **OAuth 2.0:** An **Authorization** framework enabling third-party applications to obtain limited access to an HTTP service on behalf of a resource owner. Issues an `Access Token`.
*   **OpenID Connect (OIDC):** An **Authentication** layer built on top of OAuth 2.0. Introduces the `ID Token` (JWT) to verify the identity of the end-user.

---

### 3. API Design & Rate Limiting Strategies

#### RESTful API Principles
*   Resource-oriented URL structures (e.g., `/api/v1/users/{id}`).
*   Proper utilization of HTTP status codes (`200 OK`, `201 Created`, `400 Bad Request`, `401 Unauthorized`, `403 Forbidden`, `404 Not Found`, `429 Too Many Requests`, `500 Internal Server Error`).

#### Rate Limiting & Bot Protection
To prevent Denial of Service (DoS), brute-force attacks, and scraping, rate-limiting algorithms are implemented:
*   **Token Bucket / Leaky Bucket Algorithms:** Regulate traffic flow by maintaining a bucket of tokens consumed per request.
*   **Fixed/Sliding Window Log:** Tracks request timestamps within defined time windows.
*   **Mitigation Parameters:**
    *   **IP-Based:** Limits requests per source IP. (Can be bypassed using proxy networks/botnets).
    *   **User/Key-Based:** Limits requests per authenticated user or API key.
    *   **Behavioral WAF / CAPTCHA:** Challenges suspicious automated traffic.

---

### 4. Browser & Frontend Security Foundations

#### SOP (Same-Origin Policy) & CORS
*   **SOP:** A critical browser security mechanism that restricts how a document or script loaded from one origin can interact with a resource from another origin (Origin = Scheme + Host + Port).
*   **CORS (Cross-Origin Resource Sharing):** An HTTP-header-based mechanism that allows a server to indicate any origins other than its own from which a browser should permit loading resources.
    *   Headers: `Access-Control-Allow-Origin`, `Access-Control-Allow-Methods`, `Access-Control-Allow-Headers`.

#### Common Vulnerabilities
*   **XSS (Cross-Site Scripting):** Injection of malicious JavaScript into a trusted web application.
    *   *Stored XSS:* Malicious script is permanently stored in the database.
    *   *Reflected XSS:* Malicious script is reflected off the web server in an error or search result.
    *   *DOM XSS:* Vulnerability exists in client-side code rather than server-side code.
*   **CSRF (Cross-Site Request Forgery):** Forces an end user to execute unwanted actions on a web application in which they're currently authenticated.
    *   *Prevention:* Anti-CSRF Tokens (Synchronizer Token Pattern), `SameSite` cookies, Re-authentication for sensitive actions.

---

### 5. Database Architecture, Concurrency & Caching

#### Relational Databases (RDBMS) & Normalization
*   Organizes data into tables with Primary Keys (PK) and Foreign Keys (FK).
*   **Normalization (1NF, 2NF, 3NF):** Eliminates data redundancy and updates anomalies.
*   **Anomalies:**
    *   *Insertion Anomaly:* Inability to insert data without adding unrelated attributes.
    *   *Update Anomaly:* Inconsistent data updates across duplicate rows.
    *   *Deletion Anomaly:* Unintended loss of data when deleting a record.

#### ACID Properties in Transactions
1.  **Atomicity:** All operations within a transaction complete successfully, or the entire transaction is rolled back.
2.  **Consistency:** Data meets all schema constraints and validation rules before and after the transaction.
3.  **Isolation:** Concurrent transactions execute without interfering with one another.
4.  **Durability:** Committed transactions are permanently saved to non-volatile storage.

#### Concurrency Control & Race Conditions
When multiple transactions access the same data simultaneously:
*   **Pessimistic Locking:** Locks the target row (`SELECT ... FOR UPDATE`) until the transaction completes, blocking other readers/writers.
*   **Optimistic Locking:** Uses a version column (`version=1`). Checks if the version changed prior to commit; fails or retries if updated by another transaction.

#### Caching Strategies (Redis / In-Memory)
*   **Cache-Aside Pattern:**
    1. Application checks Redis for requested data (**Cache Hit**).
    2. If missing (**Cache Miss**), queries the primary RDBMS.
    3. Writes the fetched data to Redis for subsequent requests.
*   **Persistence Mechanisms:**
    *   **RDB (Redis Database Snapshots):** Point-in-time snapshots saved to disk at specified intervals.
    *   **AOF (Append-Only File):** Logs every write operation received by the server, replayed on startup to reconstruct the dataset.

---

## 🛠️ Security & Architecture Checklist for Developers

- [ ] HTTPS enforced across all endpoints with TLS 1.3/1.2.
- [ ] Cookies configured with `HttpOnly`, `Secure`, and `SameSite=Lax/Strict`.
- [ ] JWT signatures validated using a strong secret/private key on every request.
- [ ] Rate limiting implemented at the API Gateway or Application Level (`429 Too Many Requests`).
- [ ] Inputs sanitized and parameterized queries (Prepared Statements) used to prevent SQL Injection.
- [ ] Proper CORS policies defined; avoiding wildcard `Access-Control-Allow-Origin: *` on authenticated APIs.
- [ ] Database transactions configured with appropriate isolation levels and locking mechanisms to prevent race conditions.
- [ ] Caching layers configured with eviction policies (TTL) and disk persistence (RDB/AOF) where applicable.
