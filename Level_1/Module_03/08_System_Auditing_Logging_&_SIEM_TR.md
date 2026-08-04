# 08. Sistem Denetimi, Loglama ve SIEM (System Auditing, Logging & SIEM)

## 🇹🇷 Türkçe Dokümantasyon

### Genel Bakış
Sistem denetimi ve loglama; güvenlik operasyonlarının, tehdit tespitinin ve olay sonrası dijital adli incelemenin (digital forensics) belkemiğini oluşturur. Merkezi, tahrif edilmeye karşı korumalı loglama altyapısı ve gerçek zamanlı olay korelasyonu (correlation) olmadan güvenlik ekiplerinin sunucu seviyesindeki anormallikleri, yanal hareketleri (lateral movement) veya yetkisiz yetki yükseltme faaliyetlerini tespit etmesi imkansızdır. Bu modül; Linux sistem loglamasını (`/var/log`, `journalctl`), `auditd` ile çekirdek denetimini, log bütünlüğü mekanizmalarını, merkezi log iletimini ve SIEM entegrasyonunu kapsar.

---

### 1. Linux Loglama Mimarisi: `/var/log` & Systemd Journal

Linux işletim sistemleri; geleneksel düz metin (flat-file) loglar ve ikili (binary) systemd journal yapısından oluşan çift katmanlı bir loglama mimarisi kullanır.

#### Geleneksel Düz Metin Log Dosyaları (`/var/log/`)
* `/var/log/auth.log` (Debian/Ubuntu) veya `/var/log/secure` (RHEL/CentOS): Kimlik doğrulama denemelerini, SSH girişlerini, `sudo` kullanımını ve PAM aktivitelerini takip eder.
* `/var/log/syslog` veya `/var/log/messages`: Genel sistem olaylarını, servis aktivitelerini ve kimlik doğrulama dışı çekirdek mesajlarını saklar.
* `/var/log/audit/audit.log`: Doğrudan Linux çekirdek denetim altyapısı tarafından üretilen yapılandırılmış olay kayıtlarını tutar.

#### Systemd Journal (`journalctl`)
`systemd-journald`; çekirdekten, initrd yapısından, servislerin standart çıktı/hata (stdout/stderr) kanallarından ve syslog olaylarından gelen geçici veya kalıcı ikili (binary) logları toplar.
* **İndekslenmiş Üst Veriler:** Log mesajları süreç ID'si (PID), kullanıcı ID'si (UID), systemd birim adı ve mikrosaniye hassasiyetinde zaman damgalarıyla zenginleştirilir.
* **Tahrifat Hassasiyeti:** İkili format basit metin düzenlemelerini engellese de, olası bir sistem ihlalinde logların silinmesini önlemek adına journal kayıtları kalıcı diske yazılmalı (`/etc/systemd/journald.conf` içinde `Storage=persistent`) ve anında uzak sunuculara iletilmelidir.

---

### 2. `auditd` ile Çekirdek Seviyesinde Denetim

Linux Denetim Alt Sistemi (`auditd`), kullanıcı alanı (user-space) uygulamalarına bağımlı kalmaksızın güvenliği ilgilendiren olayları doğrudan çekirdek (kernel) seviyesinde kaydetmek için çalışır.

* **Denetim Kancaları (Audit Hooks):** Sistem çağrılarını (syscalls), dosya erişimlerini, süreç çalıştırma işlemlerini ve ağ soket hareketlerini anlık olarak yakalar.
* **Kural Tanımları (`/etc/audit/rules.d/audit.rules`):**
  * **Dosya Sistemi Kuralları (Watches):** Belirli dosya veya dizinleri okuma (`r`), yazma (`w`), çalıştırma (`x`) veya öznitelik değiştirme (`a`) erişimlerine karşı izler.
  * **Sistem Çağrısı Kuralları:** UID, GID veya süreç bağlamına göre filtrelenmiş belirli çekirdek sistem çağrılarını (örneğin `execve`, `chmod`, `ptrace`) izler.
* **Adli İnceleme Değeri:** `auditd`, yetki yükseltme gerçekleşmiş olsa dahi saldırganın gerçek kullanıcı kimliğini, çalıştırılan tam komut satırı argümanlarını ve üst süreç ID'sini (PPID) eksiksiz yakalar.

---

### 3. Log Bütünlüğü, Merkezi Loglama ve SIEM Entegrasyonu

Ele geçirilen bir sunucu üzerindeki yerel log dosyalarına güvenilemez. Saldırganlar operasyonel izlerini gizlemek için genellikle yerel logları (`/var/log/*`) siler veya tahrif ederler.

#### Log Bütünlüğü ve Uzak Sunucuya İletim
* **Güvenli Taşıma:** Yerel log servisleri (`rsyslog`, `syslog-ng`, `vector`, `fluentbit`), logları şifrelenmiş kanallar (TLS) üzerinden gerçek zamanlı olarak uzak bir log toplayıcıya aktarmalıdır.
* **Salt-Eklenebilir Öznitelikler:** Kritik yerel log dosyaları, dosya sistemi öznitelikleriyle (`chattr +a /var/log/custom.log`) korunarak silme veya değiştirme işlemleri engellenebilir, sadece yeni satır eklenmesine izin verilebilir.

#### SIEM ve Tespit Mühendisliği (Detection Engineering)
* **SIEM Platformları:** Wazuh, Elastic Security (ELK), Splunk ve Microsoft Sentinel gibi çözümler; tüm ortamdan gelen log telemetrisini toplar, ayrıştırır ve indeksler.
* **Tespit Kuralları:** Log olaylarının standart şemalara (Elastic Common Schema / ECS) dönüştürülmesi; kaba kuvvet saldırılarını, kalıcılık (persistence) oluşturma faaliyetlerini ve yetki yükseltme kalıplarını tespit eden otomatik kuralların (örneğin Sigma kuralları) çalıştırılmasını sağlar.

---

### 🛠️ Pratik Uygulama ve Denetim Komutları

* **SSH Kimlik Doğrulama Hatalarını `journalctl` ile Canlı İzleme:**  
  `sudo journalctl -u ssh -f -p err`

* **`/etc/shadow` Parola Dosyası Üzerine Çekirdek Denetim Kuralı (Watch) Ekleme:**  
  `sudo auditctl -w /etc/shadow -p wa -k shadow_tampering`

* **Etiketlenmiş Audit Loglarını Sorgulama:**  
  `sudo ausearch -k shadow_tampering --format raw`

* **Başarısız Sistem Çağrılarına Dair Adli Rapor Oluşturma:**  
  `sudo aureport --syscall --failed`

* **Rsyslog Uzak Log İletimini TLS Üzerinden Yapılandırma (`/etc/rsyslog.d/50-default.conf`):**  
  `*.* @@siem.company.internal:6514`

---

### 📊 Telemetry & Auditing Summary Matrix / Telemetri ve Denetim Özet Matrisi

| Log Alt Sistemi | Log Depolama Yolu | Birincil Kullanım Senaryosu | Güvenlik Değeri |
| :--- | :--- | :--- | :--- |
| **Auth Log** | `/var/log/auth.log` | SSH & Sudo Takibi | Kaba kuvvet ve yetkisiz yetki yükseltmeyi tespit eder |
| **Systemd Journal** | `/run/log/journal/` | Servis ve Daemon Sağlığı | Süreçlerin stdout/stderr çıktılarını anlık toplar |
| **Auditd** | `/var/log/audit/audit.log` | Sistem Çağrısı ve Dosya İzleme | Çekirdek seviyesinde adli düzeyde dosya/komut takibi sağlar |
| **SIEM / Uzak Log** | Uzak TCP/6514 (TLS) | Korelasyon ve Önceliklendirme | Tahrif edilemez, değiştirilemez log depolama ve tespit sağlar |
