# 09. Log Tabanlı Suistimal Engelleme (Log-Based Abuse Mitigation)

## 🇹🇷 Türkçe Dokümantasyon

### Genel Bakış
Log tabanlı suistimal engelleme; sistem ve servis loglarını sürekli analiz ederek kötü amaçlı otomatik trafiği (SSH kaba kuvvet saldırıları, kimlik bilgisi doldurma / *credential stuffing* ve dizin tarama gibi) tespit etmek ve engellemek için tasarlanmış reaktif bir güvenlik yaklaşımıdır. Log izleme servisleri, tekrarlanan başarısız kimlik doğrulama denemelerini veya anormal istek kalıplarını tespit ederek, saldırgan IP adreslerini geçici veya kalıcı olarak karantinaya almak için güvenlik duvarı kurallarını dinamik olarak günceller.

---

### 1. Fail2Ban Mimarisi ve Davranışsal Mekanizmaları

Fail2Ban, Linux sistemlerde log tabanlı izinsiz giriş önleme için kullanılan standart açık kaynaklı çerçevedir. Üç temel yapı taşı üzerinden çalışır:

* **Filtreler (Filters):** `/etc/fail2ban/filter.d/` dizininde saklanan düzenli ifade (regex) tanımlarıdır. Filtreler, başarısız kimlik doğrulama denemelerini yakalamak için gelen log akışlarını ayrıştırır (örneğin "Failed password for invalid user").
* **Eylemler (Actions):** `/etc/fail2ban/action.d/` dizininde saklanan betik tetikleyicilerdir. Bir filtre eşik değere ulaştığında eylemler, dinamik engelleme kuralları uygulamak veya uyarı bildirimleri göndermek için yerel güvenlik araçlarını (`iptables`, `nftables`, `ufw` veya `tcpwrappers`) çağırır.
* **Hapishaneler (Jails):** `/etc/fail2ban/jail.local` dosyasında tanımlanan ve belirli bir log dosyasını (örneğin `/var/log/auth.log`), ilgili filtreyi ve bir veya daha fazla eylemi belirli eşik parametreleri altında birbirine bağlayan operasyonel birimlerdir.

#### Kritik Operasyonel Parametreler:
* `findtime`: Başarısız denemelerin sayıldığı geriye dönük zaman penceresi (örneğin 600 saniye).
* `maxretry`: Bir eylemi tetiklemeden önce `findtime` penceresi içinde izin verilen maksimum başarısız deneme sayısı.
* `bantime`: Saldırgan IP adresinin engellendiği süre (örneğin 3600 saniye veya kalıcı engelleme için `-1`).

---

### 2. Kimlik Doğrulama Log Analizi ve Dinamik Engelleme Akışı

Log tabanlı engelleme, aktif ağ durumunu değiştirmek için gerçek zamanlı log akışı analizine dayanır:

1. **Log Üretimi:** Hedef servis (`sshd`, Nginx vb.), başarısız kimlik doğrulama kayıtlarını `/var/log/auth.log` veya systemd journal içerisine yazar.
2. **Kalıp Eşleşmesi (Pattern Matching):** Fail2Ban'ın çalışan iş parçacığı (worker thread) yeni log satırlarını okur ve derlenmiş regex kuralları ile karşılaştırır.
3. **Eşik Değer İhlali:** Saldırgan bir IP adresi, `findtime` süresi içinde `maxretry` sınırını aşar.
4. **Güvenlik Duvarı Kuralı Ekleme:** Fail2Ban bir `action.d` komutunu tetikleyerek aktif güvenlik duvarı zincirine (`iptables` içindeki `f2b-sshd` zinciri veya `nftables` kümeleri) dinamik bir engelleme (DROP) kuralı ekler.
5. **Otomatik Süre Sonu (Expiration):** `bantime` süresi dolduğunda, erişimi tekrar sağlamak için otomatik bir temizlik görevi güvenlik duvarı kuralını kaldırır.

---

### 3. Log Tabanlı Kontrollerin Operasyonel Sınırları ve Riskleri

Log tabanlı suistimal engelleme, düşük yoğunluklu otomatik saldırılara karşı etkili bir savunma katmanı sağlasa da mimari açıdan bazı dezavantajlara sahiptir:

* **Reaktif Gecikme (Açık Kalma Penceresi):** Engelleme kuralları, kötü niyetli denemeler gerçekleştikten *sonra* oluşturulur. Eşzamanlı olarak binlerce istek gönderen bir saldırgan (örneğin çok iş parçacıklı dağıtık saldırılar), log ayrıştırma bir engelleme tetiklemeden önce başarılı olabilir.
* **Log Tahrifatı ve Atlatma (Evasion):** Saldırganlar, regex kalıplarını atlatmak için girdi verilerini şekillendirebilir veya Log Enjeksiyonu (*Log Injection*) saldırıları yaparak Fail2Ban'ı meşru yönetici IP adreslerini engellemeye zorlayabilir.
* **Kaynak Tükenmesi ve Disk I/O Yükü:** Yüksek hacimli saldırılarda log izleme servisi ağır log akışlarını sürekli işlediği için disk I/O ve CPU kullanımı tavan yapabilir.
* **Dağıtık Saldırılar (DDoS / Botnet'ler):** Fail2Ban tekil IP adreslerini takip eder. Her düğümün yalnızca 1 veya 2 deneme yaptığı dağıtık botnet saldırıları, tekil IP eşik tespitini tamamen atlatır.
* **Durum Tablosu Şişmesi (State Table Bloat):** Geleneksel güvenlik duvarı tablolarına (`iptables`) on binlerce bireysel IP engelleme kuralı eklemek paket işleme performansını düşürür.

---

### 🛠️ Pratik Uygulama ve Yapılandırma Komutları

* **Genel Fail2Ban Servis Durumunu ve Aktif Hapishaneleri İnceleme:**  
  `sudo fail2ban-client status`

* **SSH Hapishanesi İçin Detaylı Durumu ve Engellenen IP'leri Kontrol Etme:**  
  `sudo fail2ban-client status sshd`

* **Belirli Bir Hapishaneden Bir IP Adresinin Engelini Manuel Kaldırma:**  
  `sudo fail2ban-client set sshd unbanip 192.168.1.50`

* **Zararlı Bir IP Adresini Manuel Olarak Engelleme:**  
  `sudo fail2ban-client set sshd banip 203.0.113.10`

* **Özel Bir Regex Filtresini Log Dosyasına Karşı Test Etme:**  
  `fail2ban-regex /var/log/auth.log /etc/fail2ban/filter.d/sshd.conf`

---

### 📊 Suistimal Engelleme ve Kontrol Karşılaştırma Matrisi

| Kontrol Yönü | Reaktif Log Ayrıştırma (Fail2Ban) | Proaktif Oran Sınırlama (Netfilter/iptables) |
| :--- | :--- | :--- |
| **İnceleme Katmanı** | Uygulama / Kullanıcı Alanı (User-Space) Logları | Çekirdek Alanı (Kernel-Space) Ağ Paketleri |
| **Gecikme Süresi** | Yüksek (Log yazımı ve analizine kadar gecikmeli) | Aşırı Düşük (Anında paket seviyesinde müdahale) |
| **Atlatma Hassasiyeti**| Regex atlatma, Log Enjeksiyonu, Botnet'ler | Bağlantı durumu sahtekarlığı (State spoofing) |
| **Performans Etkisi** | Yüksek Disk I/O ve CPU yükü | İhmal edilebilir CPU etkisi, hızlı paket düşürme |
