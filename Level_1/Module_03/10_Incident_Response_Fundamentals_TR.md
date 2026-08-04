# 10. Olay Müdahalesi Temelleri (Incident Response Fundamentals)

## 🇹🇷 Türkçe Dokümantasyon

### Genel Bakış
Olay Müdahalesi (Incident Response - IR); siber güvenlik ihlallerini ve sistem ele geçirme vakalarını tespit etmek, kontrol altına almak (karantinaya almak), etkilerini ortadan kaldırmak ve sistemleri güvenli bir şekilde normal çalışır duruma getirmek için uygulanan yapılandırılmış ve sistematik bir metodolojidir. Olay müdahalesinin birincil amacı; iş sürekliliğindeki aksamaları en aza indirmek, adli adli bilişim (forensics) incelemeleri için dijital delilleri korumak, finansal ve itibar kayıplarını kısıtlamak ve sistemleri tekrarlayan saldırı vektörlerine karşı sıkılaştırmaktır.

---

### 1. Olay Müdahale Yaşam Döngüsü (PICERL Çerçevesi)

Modern siber güvenlik operasyonları, olay yönetim süreçlerini uluslararası standartlarla (NIST SP 800-61 ve SANS IR gibi) uyumlu şekilde yürütür:

1. **Hazırlık (Preparation):** Olay gerçekleşmeden önce politikaların oluşturulması, müdahale rehberlerinin (playbook) hazırlanması, Olay Müdahale Takımının (CIRT/CSIRT) eğitilmesi, adli bilişim araçlarının tedarik edilmesi ve bant dışı (out-of-band) güvenli iletişim kanallarının kurulması.
2. **Tespit, Teşhis ve Önceliklendirme (Identification, Detection & Triage):** İhlali doğrulamak için güvenlik uyarılarının, SIEM telemetrilerinin ve kullanıcı bildirimlerinin analiz edilmesi. Tehdidin kritiklik seviyesinin sınıflandırılması, ihlalin etki alanının (blast radius) belirlenmesi ve etkilenen varlıkların önceliklendirilmesi.
3. **Sınırlandırma / Karantinaya Alma (Containment):** Kritik delilleri korurken aktif bir ihlalin yayılmasını önlemek için anlık tedbirlerin uygulanması.
   * *Kısa Vadeli Sınırlandırma:* Ele geçirilen sunucuları kapatmadan (güç kesmeden) ağdan izole etmek (VLAN izolasyonu, dinamik güvenlik duvarı engellemeleri).
   * *Uzun Vadeli Sınırlandırma:* Geçici yamaların uygulanması, ele geçirilen kullanıcı hesaplarının devre dışı bırakılması veya Komuta Kontrol (C2) alan adı/IP adreslerinin engellenmesi.
4. **Ortadan Kaldırma (Eradication):** Saldırının kök nedenlerinin, zararlı kalıntıların, kalıcılık (persistence) mekanizmalarının (zamanlanmış görevler/crontab, systemd servisleri, web shell'ler) ve yetkisiz hesapların sistemden tamamen temizlenmesi.
5. **Kurtarma / İyileştirme (Recovery):** Etkilenen sistemlerin doğrulanmış temiz yedeklerden üretim ortamına geri döndürülmesi, güvenlik yamalarının uygulanması, sistem bütünlüğünün doğrulanması ve artırılmış izleme kontrollerinin devreye alınması.
6. **Olay Sonrası İnceleme / Alınan Dersler (Post-Incident Review / Lessons Learned):** Paydaşlarla geriye dönük bir değerlendirme toplantısı yapılması, resmi Olay Müdahale Raporu'nun oluşturulması, olay zaman çizelgesinin belgelenmesi ve tekrarı önlemek adına müdahale rehberlerinin güncellenmesi.

---

### 2. Delil Koruma ve Adli Bilişim Bütünlüğü

Bir güvenlik olayı sırasında, kök neden analizi ve olası yasal süreçler için dijital delillerin hukuki ve operasyonel bütünlüğünü korumak kritik önem taşır.

* **Uçuculuk Sıralaması (Order of Volatility - RFC 3227):** Deliller, en uçucu bellek bileşeninden en kalıcı depolama ortamına doğru sırayla toplanmalıdır:
  1. CPU yazmaçları (registers) ve önbellek
  2. Ana bellek (RAM)
  3. Ağ durumu ve aktif soket tabloları
  4. Çalışan süreç (process) durumları
  5. Disk depolama alanları (takas alanı/swap, dosya sistemleri)
  6. Uzak log kayıtları ve arşivlenmiş yedekler
* **Uçucu Bellek Edinimi (RAM Dump):** Bir sunucuyu kapatmadan veya yeniden başlatmadan önce canlı RAM'in çekirdek modülleri (örneğin LiME) kullanılarak kopyalanması. Sunucunun kapatılması; çalışan zararlı yazılım kodlarını, şifreleme anahtarlarını ve aktif ağ bağlantılarını tamamen yok eder.
* **Disk İmajı ve Hash Doğrulaması:** Fiziksel depolama medyasının birebir bit düzeyinde kopyasının (`dd` veya `dc3dd` ile) çıkarılması ve delil bütünlüğünü doğrulamak için edinimden hemen sonra kriptografik hash değerlerinin (SHA-256) hesaplanması.
* **Gözetim Zinciri (Chain of Custody):** Delili elinde bulunduran, aktaran veya analiz eden her bir personelin zaman damgaları, depolama konumları ve kriptografik özet değerleriyle birlikte eksiksiz belgelenmesi.

---

### 3. İletişim Kanalları ve Tırmandırma Stratejileri

Yapılandırılmış iletişim; bilgi sızıntılarını önler, karmaşayı azaltır ve kriz anında yasal/düzenleyici uyumluluğu sağlar:

* **Bant Dışı (Out-of-Band - OOB) İletişim:** Ele geçirilmiş olma ihtimali bulunan kurumsal altyapıya (eposta, iç mesajlaşma) dayanmayan, izole edilmiş ve şifrelenmiş iletişim kanallarının (örneğin özel Signal grupları, bağımsız VoIP sistemleri) kullanılması.
* **Paydaş Tırmandırma (Escalation):** Üst düzey yöneticiler (C-suite), hukuk danışmanları, halkla ilişkiler (PR), düzenleyici kurumlar (örneğin KVKK/GDPR bildirimleri) ve kolluk kuvvetleri için önceden tanımlanmış bildirim hatlarının işletilmesi.
* **Operasyonel Bilgi Paylaşımını Sınırlandırma (Need-to-Know):** Detaylı olay bilgilerinin yalnızca müdahalede doğrudan görev alan yetkili personelle sınırlandırılması.

---

### 🛠️ Adli Bilişim ve Olay Müdahalesi Komutları

* **Ele Geçirilen Sunucuyu Ağdan İzole Etme (Sadece Loopback İletişimine İzin Verme):**  
  `sudo iptables -P INPUT DROP`  
  `sudo iptables -P OUTPUT DROP`  
  `sudo iptables -A INPUT -i lo -j ACCEPT`  
  `sudo iptables -A OUTPUT -o lo -j ACCEPT`

* **LiME Çekirdek Modülü ile Uçucu Bellek (RAM) Dökümü Alma:**  
  `sudo insmod lime-$(uname -r).ko "path=/tmp/memory_dump.lime format=raw"`

* **SHA-256 Hash Doğrulamalı Adli Birebir Disk İmajı Oluşturma:**  
  `sudo dd if=/dev/sdb of=/mnt/forensics/disk_image.raw bs=64K status=progress`  
  `sha256sum /dev/sdb /mnt/forensics/disk_image.raw`

* **Dosya Zaman Damgalarını İnceleme (MACB - Modified, Accessed, Changed, Birth):**  
  `stat /dosya/yolu/supheli_yazilim`

* **Autopsy / TSK (Sleuth Kit) Kullanarak Dosya Sistemi Zaman Çizelgesi Oluşturma:**  
  `fls -r -m / /mnt/forensics/disk_image.raw > bodyfile`  
  `mactime -b bodyfile 2026-08-01..2026-08-04`

---

### 📊 Olay Müdahalesi Yaşam Döngüsü Özet Matrisi

| Olay Müdahale Evresi | Birincil Amaç | Kritik Çıktılar / Varlıklar | Operasyonel Risk |
| :--- | :--- | :--- | :--- |
| **Tespit & Önceliklendirme** | İhlal kapsamını & etki alanını belirlemek | Tehdit göstergeleri (IoC), Teşhis raporu | Hatalı kapsam tespiti eksik izolasyona yol açar |
| **Sınırlandırma** | Saldırgan hareketini durdurmak & RAM'i korumak | Ağ izolasyon kuralları, RAM imajı (`.lime`) | Sunucuyu kapatmak uçucu RAM delillerini yok eder |
| **Ortadan Kaldırma** | Kök nedeni ve kalıcılığı temizlemek | Temizlenen arka kapılar, yamalanan açıklar | Eksik temizlik saldırganın hızlıca geri girmesine neden olur |
| **Kurtarma** | Güvenilir operasyonel duruma dönmek | Doğrulanmış sistem yedekleri, izlenen sunucular | Zararlı bulaşmış veya yamalanmamış yedekten dönülmesi |
| **Alınan Dersler** | Rehberleri güncellemek ve tekrarı önlemek | Resmi Olay Raporu, kök neden analizi | Gelecekteki ihlallerde prosedürel hataların tekrarlanması |
