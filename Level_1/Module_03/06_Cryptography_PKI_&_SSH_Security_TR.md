# 06. Şifreleme, PKI ve SSH Güvenliği (Cryptography, PKI & SSH Security)

## 🇹🇷 Türkçe Dokümantasyon

### Genel Bakış
Kriptografi; modern bilgisayar sistemlerinde gizlilik, bütünlük, kimlik doğrulama ve inkar edilemezlik ilkelerini sağlamak için temel matematiksel yapı taşlarını sunar. Bu modül; simetrik ve asimetrik şifreleme, kriptografik hash fonksiyonları, dijital imzalar, Açık Anahtar Altyapısı (PKI), dijital sertifikalar ve Secure Shell (SSH) iletişim kanallarının kapsamlı bir şekilde sıkılaştırılmasını kapsar.

---

### 1. Kriptografik Yapı Taşları ve Mekanizmalar

* **Simetrik Kriptografi:** Şifreleme ve deşifreleme işlemleri için tek bir gizli ortak anahtar kullanır. Yüksek hacimli veri akışları için aşırı yüksek hesaplama hızı sunar. Yaygın algoritmalar arasında AES-256 (GCM/CBC modları) ve ChaCha20-Poly1305 yer alır.
* **Asimetrik Kriptografi:** Matematiksel olarak birbiriyle ilişkili anahtar çiftlerini kullanır—şifreleme veya imza doğrulaması için bir açık anahtar (public key) ve deşifreleme veya imzalama için bir gizli anahtar (private key). Anahtar dağıtım sorununu çözer. Yaygın algoritmalar arasında RSA (2048/4096-bit), ECC (Elliptic Curve Cryptography) ve Ed25519 bulunur.
* **Kriptografik Hash'leme:** Rastgele boyuttaki girdi verilerini sabit boyutlu bir özete (digest) dönüştüren tek yönlü matematiksel fonksiyonlardır. Geri döndürülemezler ve çakışmaya dirençlidirler (collision-resistant). Güvenli parola saklama (Argon2, bcrypt) ve dosya bütünlüğü doğrulama (SHA-256, SHA-3) için kritik önem taşırlar.
* **Dijital İmzalar:** Mesaj bütünlüğünü, gönderenin kimliğini ve inkar edilemezliği garanti etmek için asimetrik şifreleme ile kriptografik hash'lemeyi birleştirir.

---

### 2. Açık Anahtar Altyapısı (PKI) ve Dijital Sertifikalar

PKI, güvenilmeyen ağlar üzerinde açık anahtarların dağıtılması, doğrulanması ve iptal edilmesi için güvenilir bir çerçeve oluşturur:

* **Sertifika Otoriteleri (CA):** Bir açık anahtarı doğrulanmış bir kimliğe (alan adı, kuruluş veya kullanıcı) bağlamak için sertifikaları düzenleyen ve dijital olarak imzalayan güvenilir kurumlar.
* **X.509 Sertifika Yapısı:** Konu kimliği, açık anahtar, yayınlayan imzası, geçerlilik süresi ve uzantıları (Subject Alternative Names / SAN gibi) içeren standart format.
* **Sertifika Yaşam Döngüsü Yönetimi:**
  1. **CSR Üretimi:** Özel anahtar ve kimlik üst verilerini içeren bir Sertifika İmzalama Talebi (CSR) üretilmesi.
  2. **Sertifika Düzenleme:** CA'nın alan adı veya kuruluş sahipliğini doğrulayarak imzalı X.509 sertifikasını teslim etmesi.
  3. **İptal Doğrulaması:** Sertifika geçerlilik durumunun CRL (Sertifika İptal Listeleri) veya gerçek zamanlı OCSP (Çevrimiçi Sertifika Durum Protokolü) stapling ile kontrol edilmesi.

---

### 3. SSH Mimarisi ve Servis Sıkılaştırma

Secure Shell (SSH), şifreli interaktif kabuk erişimi ve dosya transferi yetenekleri sağlar. SSH servisini (`sshd`) sıkılaştırmak; otomatik kaba kuvvet saldırılarını, kimlik bilgisi hırsızlığını ve protokol sürüm düşürme (downgrade) risklerini ortadan kaldırır.

#### Kriptografik El Sıkışma ve Sunucu (Host) Anahtarları
Bağlantı başlangıcında SSH, kullanıcı kimlik doğrulaması gerçekleşmeden önce şifreli bir tünel oluşturmak için Diffie-Hellman veya ECDH anahtar değişimi gerçekleştirir. Sunucu anahtarları (`/etc/ssh/ssh_host_*_key`), Aradaki Adam (MITM) saldırılarını önlemek için sunucu kimliğini doğrular.

#### Önerilen Güvenlik Yapılandırmaları (`/etc/ssh/sshd_config`):
* `Port 2222`: Otomatik tarayıcı gürültüsünü engellemek için varsayılan 22 portunu değiştirir.
* `PermitRootLogin no`: Doğrudan root kabuk erişimini kapatır, `sudo` ile izlenebilir yetki yükseltmeyi zorunlu kılar.
* `PasswordAuthentication no`: Düz metin parola girişlerini devre dışı bırakır, SSH Anahtar Çiftlerini zorunlu kılar.
* `KbdInteractiveAuthentication no`: PAM interaktif parola istemlerini engeller.
* `PubkeyAuthentication yes`: Asimetrik anahtar çifti doğrulamasını zorunlu kılar.
* `HostKeyAlgorithms`: Kabul edilebilir sunucu anahtarlarını güçlü algoritmalarla kısıtlar (Ed25519, RSA-SHA2-512).
* `KexAlgorithms`: Anahtar değişimini güvenli eğrilerle kısıtlar (örneğin `curve25519-sha256`).

---

### 🛠️ Pratik Uygulama ve Sıkılaştırma Komutları

* **Yüksek Güvenlikli Ed25519 SSH Anahtar Çifti Üretme:**  
  `ssh-keygen -t ed25519 -a 100 -C "admin@security.internal"`

* **OpenSSL Özel Anahtar ve CSR (Sertifika İmzalama Talebi) Oluşturma:**  
  `openssl req -new -newkey rsa:4096 -nodes -keyout server.key -out server.csr`

* **X.509 Sertifika Üst Verilerini İnceleme ve Doğrulama:**  
  `openssl x509 -in server.crt -text -noout`

* **SSH Servis Yapılandırma Sözdizimini Doğrulama:**  
  `sudo sshd -t`

---

### 📊 Kriptografik Uygulama Matrisi

| Güvenlik Yapı Taşı | Birincil Algoritma | Hedef Kullanım Senaryosu | Savunma Kazancı |
| :--- | :--- | :--- | :--- |
| **Simetrik Şifreleme** | AES-256-GCM / ChaCha20 | Durağan Veri & TLS Akışları | Doğrulama etiketiyle yüksek hızlı gizlilik |
| **Asimetrik Anahtar Değişimi** | Ed25519 / Curve25519 | SSH Erişim & TLS El Sıkışması | Kaba kuvvet saldırılarına karşı matematiksel direnç |
| **Kriptografik Hash** | SHA-256 / Argon2id | Parola Saklama & Bütünlük | Geri döndürülemez, tuzlanmış (salted) hash özetleri |
| **PKI Doğrulaması** | X.509 + OCSP Stapling | Web & Servis Kimliği | MITM ve yetkisiz taklit (spoofing) engelleme |
