# Web, API ve Veri Tabanı Temelleri (Güvenlik ve Mimari)

Bu rehber; DevSecOps ve yazılım mühendisliği perspektifinden temel web mimarilerini, API tasarım standartlarını, kimlik doğrulama protokollerini, tarayıcı güvenlik mekanizmalarını ve veri tabanı iç yapılarını kapsayan detaylı bir başvuru kaynağıdır.

---

## 🔬 Temel Kavramlar ve Sistem Mimarisi

### 1. Web Mimarisi ve HTTP/HTTPS Protokolleri
Modern web uygulamaları, OSI Modelinin 7. Katmanında (Uygulama Katmanı) haberleşen İstemci-Sunucu (Client-Server) mimarisine dayanır.

*   **Stateless (Durumsuzluk) ve Durum Yönetimi:** HTTP doğası gereği durumsuzdur (stateless). Her istek diğerinden bağımsız olarak işlenir. Oturum durumu (state); oturum çerezleri (session cookies), sunucu tarafı oturum depoları (ör. Redis) veya kriptografik olarak imzalanmış token'lar (ör. JWT) kullanılarak uygulama katmanında yönetilir.
*   **HTTP Metodları ve Idempotency (Eşgüçlülük):**
    *   `GET`: Kaynakları okumak için kullanılır. Güvenli (Safe) ve Eşgüçlüdür (Idempotency).
    *   `POST`: Yeni kaynak oluşturmak için kullanılır. Güvenli veya eşgüçlü değildir.
    *   `PUT`: Hedef kaynağı istek gövdesiyle tamamen değiştirir. Eşgüçlüdür.
    *   `PATCH`: Kaynak üzerinde kısmi değişiklik yapar. Doğası gereği eşgüçlü olmak zorunda değildir.
    *   `DELETE`: Belirtilen kaynağı siler. Eşgüçlüdür.
*   **HTTPS ve TLS El Sıkışması (TLS 1.3):**
    1. **Client Hello:** İstemci, desteklenen Cipher Suite'leri ve TLS sürümünü gönderir.
    2. **Server Hello & Certificate:** Sunucu şifreleme algoritmasını seçer ve X.509 Dijital Sertifikasını sunar.
    3. **Certificate Verification:** İstemci, yerel güvenilir Kök Sertifika Otoritelerini (CA) kullanarak zinciri (Chain of Trust) doğrular.
    4. **Key Exchange (ECDHE):** Diffie-Hellman anahtar değişimi ile simetrik oturum anahtarları (Session Keys) türetilir.
    5. **Encrypted Communication:** Tüm HTTP trafiği bu simetrik anahtarla şifrelenerek akar.

---

### 2. Kimlik Doğrulama, Oturum Yönetimi ve Token Mimarisi

#### Oturum Bazlı (Session-Based) vs. Token Bazlı Kimlik Doğrulama
*   **Oturum Bazlı:** Sunucu oturum durumunu saklar (ör. Bellek, Redis) ve istemciye bir çerez aracılığıyla `session_id` iletir.
*   **Token Bazlı (Stateless):** Sunucu, kullanıcı bilgilerini (claims) içeren imzalı bir token (JWT) üretir. Sunucu, her istekte veri tabanına gitmeden sadece imzayı doğrulayarak işlemi gerçekleştirir.

#### Çerez (Cookie) Güvenlik Bayrakları
Hassas oturum kimliklerini çerezlerde saklarken şu güvenlik parametrelerinin kullanımı zorunludur:
*   `HttpOnly`: İstemci tarafı kodların (JavaScript - `document.cookie`) çereze erişmesini engelleyerek **XSS Tabanlı Oturum Çalma (Session Hijacking)** saldırılarını önler.
*   **`Secure`**: Çerezin sadece şifrelenmiş HTTPS bağlantıları üzerinden iletilmesini sağlar.
*   **`SameSite`**: Çapraz site isteklerinde çerez gönderimini kontrol ederek **CSRF** saldırılarını engeller:
    *   `Strict`: Çerez, çapraz site isteklerinde asla gönderilmez.
    *   `Lax`: Çerez, üst düzey gezintilerde (ör. standart bağlantı tıklamaları) gönderilir.
    *   `None`: Çerez her koşulda gönderilir (`Secure` bayrağı zorunludur).

#### JWT (JSON Web Token) Yapısı
Bir JWT, noktalarla (`.`) ayrılmış Base64URL ile kodlanmış üç ana parçadan oluşur:
1.  **Header (Başlık):** Kullanılan algoritmayı (`alg`) ve token türünü (`typ`) belirtir.
2.  **Payload (İçerik):** Kullanıcı verilerini ve haklarını içerir (`sub`, `iat`, `exp`, roller). *Not: Payload kodlanmıştır, şifrelenmemiştir. Hassas veriler burada tutulmamalıdır.*
3.  **Signature (İmza):** `Header + Payload` bilgisinin sunucu tarafındaki gizli bir anahtarla (HMAC) veya özel anahtarla (RS256) imzalanmasıyla oluşur. **Veri bütünlüğünü (Integrity)** sağlar, verinin yolda değiştirilmesini önler.

#### OAuth 2.0 ve OpenID Connect (OIDC)
*   **OAuth 2.0:** Bir uygulamanın, kullanıcının şifresini görmeden başka bir servisteki kaynaklarına erişmesine izin veren **Yetkilendirme (Authorization)** standardıdır. Çıktı olarak `Access Token` üretir.
*   **OpenID Connect (OIDC):** OAuth 2.0 üzerine inşa edilmiş bir **Kimlik Doğrulama (Authentication)** katmanıdır. Kullanıcının kimliğini doğrulamak için `ID Token` (JWT) yapısını kullanır.

---

### 3. API Tasarımı ve Rate Limiting Stratejileri

#### RESTful API Prensipleri
*   Kaynak odaklı URL yapıları (ör. `/api/v1/users/{id}`).
*   HTTP durum kodlarının doğru kullanımı (`200 OK`, `201 Created`, `400 Bad Request`, `401 Unauthorized`, `403 Forbidden`, `404 Not Found`, `429 Too Many Requests`, `500 Internal Server Error`).

#### Rate Limiting (Oran Sınırlama) ve Bot Koruması
Hizmet Engelleme (DoS), kaba kuvvet (brute-force) ve veri kazıma (scraping) saldırılarını önlemek için kullanılan algoritmalar:
*   **Token Bucket / Leaky Bucket Algoritmaları:** İstek başına tüketilen token havuzlarını yöneterek trafik akışını düzenler.
*   **Fixed/Sliding Window Log:** Belirli zaman aralıklarındaki istek zaman damgalarını takip eder.
*   **Sınırlama Parametreleri:**
    *   **IP Bazlı:** Kaynak IP başına sınır koyar (Proxy ağları veya botnet'ler ile aşılabilir).
    *   **Kullanıcı/Anahtar Bazlı:** Doğrulanmış kullanıcı veya API anahtarı başına sınır koyar.
    *   **Davranışsal WAF / CAPTCHA:** Otomatize edilmiş şüpheli trafiği tespit eder ve doğrulamaya tabi tutar.

---

### 4. Tarayıcı ve Frontend Güvenlik Temelleri

#### SOP (Same-Origin Policy) ve CORS
*   **SOP (Aynı Köken Politikası):** Bir kökenden (Origin = Şema + Host + Port) yüklenen belge veya betiğin, başka bir kökenden gelen kaynakla nasıl etkileşime gireceğini kısıtlayan temel tarayıcı güvenlik kuralıdır.
*   **CORS (Cross-Origin Resource Sharing):** Sunucunun, kendi kökeni dışındaki tarayıcıların kaynakları okumasına izin vermesini sağlayan HTTP başlık tabanlı mekanizmadır.
    *   Başlıklar: `Access-Control-Allow-Origin`, `Access-Control-Allow-Methods`, `Access-Control-Allow-Headers`.

#### Yaygın Zafiyetler
*   **XSS (Cross-Site Scripting):** Güvenilir bir web uygulamasına zararlı JavaScript kodlarının enjekte edilmesidir.
    *   *Stored XSS:* Zararlı betik kalıcı olarak veri tabanına kaydedilir.
    *   *Reflected XSS:* Zararlı betik bir arama veya hata sonucunda sunucudan geri yansır.
    *   *DOM XSS:* Zafiyet sunucu tarafında değil, istemci tarafındaki kod yapısında yer alır.
*   **CSRF (Cross-Site Request Forgery):** Oturum açmış bir kullanıcının tarayıcısını kandırarak istem dışı eylemler (istekler) gerçekleştirmesini sağlama saldırısıdır.
    *   *Korumalar:* Anti-CSRF Token'ları (Synchronizer Token Pattern), `SameSite` çerezleri, kritik işlemlerde yeniden kimlik doğrulama.

---

### 5. Veri Tabanı Mimarisi, Eşzamanlılık ve Önbellekleme

#### İlişkisel Veri Tabanları (RDBMS) ve Normalizasyon
*   Verileri Birincil Anahtar (Primary Key - PK) ve Yabancı Anahtar (Foreign Key - FK) kullanarak tablolar halinde düzenler.
*   **Normalizasyon (1NF, 2NF, 3NF):** Veri tekrarını önler ve güncelleme karmaşalarını ortadan kaldırır.
*   **Anomaliler:**
    *   *Ekleme Anomalisi:* İlişkisiz verileri eklemeden yeni veri ekleyememe durumu.
    *   *Güncelleme Anomalisi:* Tekrarlanan verilerin bir kısmının güncellenip bir kısmının unutulmasıyla oluşan tutarsızlık.
    *   *Silme Anomalisi:* Bir kaydı silerken istemeden ilişkili başka bir verinin de kaybolması.

#### İşlemlerde ACID Prensipleri
1.  **Atomicity (Bölünemezlik):** İşlem içindeki tüm adımlar ya tamamen başarılı olur ya da tüm işlem geri alınır (Rollback).
2.  **Consistency (Tutarlılık):** Veri, işlem öncesinde ve sonrasında tanımlanmış tüm kurallara ve kısıtlamalara uygun kalır.
3.  **Isolation (Yalıtım):** Eşzamanlı yürütülen işlemler birbirini olumsuz etkilemez.
4.  **Durability (Kalıcılık):** Onaylanan (Commit) işlemler kalıcı hafızaya yazılır ve sistem çökse bile kaybolmaz.

#### Eşzamanlılık Kontrolü ve Race Condition (Yarış Durumu)
Birden fazla işlem aynı veriye aynı anda eriştiğinde:
*   **Pessimistic Locking (Kötümser Kilitleme):** İşlem bitene kadar ilgili satırı kilitler (`SELECT ... FOR UPDATE`), diğer okuma/yazma isteklerini bekletir.
*   **Optimistic Locking (İyimser Kilitleme):** Versiyon sütunu (`version=1`) kullanır. Güncelleme öncesi versiyonu kontrol eder; başka bir işlem veriyi değiştirdiyse işlemi reddeder veya yeniden dener.

#### Önbellekleme Stratejileri (Redis / In-Memory)
*   **Cache-Aside Pattern:**
    1. Uygulama istenen veri için önce Redis'i kontrol eder (**Cache Hit**).
    2. Veri yoksa (**Cache Miss**), ana RDBMS'e sorgu atar.
    3. Veriyi alıp sonraki istekler için Redis'e yazar.
*   **Kalıcılık Mekanizmaları:**
    *   **RDB (Redis Database Snapshots):** Belirli zaman aralıklarında bellek görüntüsünü diske kaydeder.
    *   **AOF (Append-Only File):** Sunucuya gelen her yazma komutunu bir günlüğe kaydeder, yeniden başlangıçta bu komutları çalıştırarak veriyi inşa eder.

---

## 🛠️ Geliştiriciler İçin Güvenlik ve Mimari Kontrol Listesi

- [ ] Tüm uç noktalarda TLS 1.3/1.2 ile HTTPS zorunlu kılındı.
- [ ] Çerezler `HttpOnly`, `Secure` ve `SameSite=Lax/Strict` bayrakları ile yapılandırıldı.
- [ ] JWT imzaları her istekte güçlü bir gizli/özel anahtar kullanılarak doğrulandı.
- [ ] API Gateway veya Uygulama katmanında Rate Limiting (`429 Too Many Requests`) uygulandı.
- [ ] SQL Injection önlemek için girdi temizliği yapıldı ve parametreli sorgular (Prepared Statements) kullanıldı.
- [ ] Doğru CORS politikaları tanımlandı; kimlik doğrulaması yapılan API'lerde joker karakter `Access-Control-Allow-Origin: *` kullanımından kaçınıldı.
- [ ] Race Condition durumlarını önlemek için veri tabanı işlemleri uygun yalıtım seviyeleri ve kilitleme mekanizmaları ile yapılandırıldı.
- [ ] Önbellekleme katmanları uygun son kullanma süreleri (TTL) ve disk kalıcılığı (RDB/AOF) ile yapılandırıldı.
