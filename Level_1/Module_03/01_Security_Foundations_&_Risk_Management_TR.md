## 🇹🇷 Türkçe Dökümantasyon

### Genel Bakış
Güvenlik Mühendisliği, sistemlerin nasıl tasarlandığını, korunduğunu ve denetlendiğini belirleyen temel ilkelerle başlar. Her güvenlik denetimi, güvenlik duvarı kuralı veya erişim izni bu temel kavramları korumak için tasarlanır.

---

### 1. CIA Üçgeni
Bilgi Güvenliğinin temelini oluşturan üç ana hedef:
* **Gizlilik (Confidentiality):** Verinin yalnızca yetkili kişilerce erişilebilir olması (Örn: SSH anahtar doğrulaması, disk şifreleme).
* **Bütünlük (Integrity):** Verinin iletim esnasında veya saklanırken yetkisizce değiştirilmediğinin garantisi (Örn: SHA-256 özeti doğrulama, dijital imzalar).
* **Erişilebilirlik (Availability):** Sistem ve verilerin ihtiyaç duyulduğu an kesintisiz ulaşılabilir olması (Örn: Yedeklilik, DDoS koruması, web sunucu sürekliliği).

---

### 2. AAA Modeli
Sisteme erişen her kullanıcı veya süreç şu 3 aşamadan geçer:
1. **Authentication - Kimlik Doğrulama:** Kimlik bilgileri veya SSH anahtarlarıyla kişinin kimliğini kanıtlama.
2. **Authorization - Yetkilendirme:** Kullanıcının sistemdeki işlem yetkilerini belirleme (sudo, 644 dosya izinleri).
3. **Accountability - İzlenebilirlik:** Yapılan her eylemin sistem kayıtlarına (/var/log/auth.log) işlenerek takip edilmesi.

---

### 3. Temel Risk Denklemi
Risk yönetimi, olası kayıpları şu denklem üzerinden değerlendirir:

Risk = Tehdit * Zafiyet * Varlık Değeri

* **Varlık (Asset):** Korumaya çalıştığımız öğe (Örn: Müşteri Veritabanı, Web Sunucusu).
* **Zafiyet (Vulnerability):** Sistemdeki zayıf nokta veya açık (Örn: Güncellenmemiş işletim sistemi, geniş dosya izinleri).
* **Tehdit (Threat):** Zafiyeti kullanabilecek potansiyel tehlike (Örn: Saldırganlar, Zararlı Yazılımlar).
* **Saldırı Yüzeyi (Attack Surface):** Saldırganın sisteme sızmak için kullanabileceği tüm açık kapıların toplamı.

---

### 4. Derinlemesine Savunma (Defense in Depth)
Güvenlik asla tek bir savunma hattına bırakılamaz. Katmanlı savunma yaklaşımı ("Soğan Modeli"), bir katman aşıldığında diğerinin saldırganı durdurmasını sağlar:
1. **Çevre / Bulut Katmanı:** AWS Security Group ve Network ACL yapılandırması.
2. **Ağ Katmanı:** Sunucu içi güvenlik duvarları (iptables / ufw).
3. **Sistem Katmanı:** İşletim sistemi sıkılaştırma, SSH yapılandırması (sshd_config).
4. **Uygulama Katmanı:** Web sunucu ayarları, en düşük yetkili servis hesapları.
5. **Veri Katmanı:** Dosya izinleri, verinin şifrelenmesi.
