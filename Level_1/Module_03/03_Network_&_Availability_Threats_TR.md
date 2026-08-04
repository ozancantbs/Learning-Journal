## 🇹🇷 Türkçe Dökümantasyon

### Genel Bakış
Ağ seviyesindeki tehditler öncelikle servis erişilebilirliğini bozmayı (Denial of Service) veya şifrelenmemiş ağ trafiğini dinlemeyi (Man-in-the-Middle) hedefler. TCP el sıkışması gibi protokol mekanizmalarını anlamak sunucu savunması için kritik önem taşır.

---

### 1. Erişilebilirlik Tehditleri (DoS, DDoS, SYN Flood)

* **DoS (Denial of Service):** Tek bir IP kaynağından gelen saldırıdır. Yerel güvenlik duvarı kuralları (iptables) ile kolayca engellenebilir.
* **DDoS (Distributed DoS):** Ele geçirilmiş binlerce botnet cihazından aynı anda gelen saldırıdır. Çevre koruması (AWS Shield, Cloudflare) gerektirir.

#### 🚨 Teknik İnceleme: SYN Flood Saldırısı
Standart TCP iletişimi 3'lü El Sıkışma (3-Way Handshake) prensibiyle çalışır:

[İstemci]  --- (1) SYN --->  [Sunucu]  (Bağlantı İsteği)
[İstemci]  <-- (2) SYN-ACK - [Sunucu]  (Onay & Kaynak Ayırma)
[İstemci]  --- (3) ACK --->  [Sunucu]  (Bağlantı Kuruldu)

* **Saldırı Mekanizması:** Saldırgan sunucuya binlerce sahte SYN paketi atar. Sunucu SYN-ACK ile yanıt verip bağlantıyı HALF-OPEN (Yarı-Açık) durumunda bekletir. Saldırgan son ACK paketini asla göndermez, böylece sunucunun bağlantı tablosu tamamen dolar ve gerçek kullanıcılar engellenir.
* **Linux Çekirdek Çözümü:** Çekirdek parametrelerinde TCP SYN Cookies özelliğini aktif etmek:
  sysctl -w net.ipv4.tcp_syncookies=1

---

### 2. Ortadaki Adam (MITM) ve Sahtecilik (Spoofing) Saldırıları

* **ARP Zehirlemesi (ARP Poisoning):** Yerel ağda (LAN) gerçekleşir. Saldırgan, kendi MAC adresini varsayılan ağ geçidinin (Router) IP adresiyle eşleştiren sahte ARP paketleri atarak tüm trafiği kendi üzerinden geçirir.
* **DNS Zehirlemesi:** DNS sunucusuna sahte kayıtlar enjekte ederek kullanıcıları gerçek site yerine saldırganın IP adresine yönlendirme işlemidir.
* **Session Hijacking (Oturum Çalma):** Kullanıcının aktif oturum anahtarını/çerezini çalarak parola gerekmeden oturuma sızma saldırısıdır.

---

### 3. Savunma Matrisi

| Saldırı Türü | Hedeflenen İlke | Savunma Yöntemi |
| :--- | :--- | :--- |
| **SYN Flood** | Erişilebilirlik | tcp_syncookies = 1, iptables ile oran sınırlama. |
| **ARP Spoofing** | Bütünlük / Gizlilik | Dinamik ARP Denetimi (DAI), Statik ARP tabloları. |
| **DNS Spoofing** | Bütünlük | DNSSEC kullanımı, HTTPS/TLS zorunluluğu. |
| **Session Hijacking**| Gizlilik | Çerezleri Secure ve HttpOnly olarak işaretleme. |
