# 🐳 Modül 05: Konteyner Teknolojileri & Docker Deep Dive
> **Mühendislik Öğrenme Günlüğü & DevSecOps Mimari Kaydı**  
> **Odak:** Konteynerizasyon Mekanizmaları, Linux Çekirdek İzolasyonu, Depolama Stratejileri, Deterministik Ağ Yapıları ve Tedarik Zinciri Güvenliği.

---

## 📌 Mimari Genel Bakış & Ana Misyon
Bu modül, **DevSecOps öncelikli bir yaklaşımı** merkeze alarak işletim sistemi seviyesindeki sanallaştırma ve konteyner runtime mekanizmalarını kapsar. Konteynerları kapalı birer kara kutu olarak ele almak yerine; alt seviye çekirdek bileşenlerini (Namespaces, Cgroups v2), Open Container Initiative (OCI) motor soyutlamalarını, katman tabanlı depolama sürücülerini, deterministik ağ modellerini ve tedarik zinciri sertleştirme (hardening) kalıplarını derinlemesine inceler.

---

## 🏗️ Detaylı Mimari Analiz

### 1. Konteynerizasyon vs. Hypervisor Sanallaştırma
* **Hypervisor Mimarisi (VM'ler):** Donanım seviyesinde sanallaştırmadır. Her örnek için ayrı bir Konuk İşletim Sistemi (Guest OS) gerektirir; bu da yüksek kaynak tüketimine, yavaş başlatma sürelerine ve tekrarlanan çekirdek yüküne yol açar.
* **İşletim Sistemi Seviyesinde Sanallaştırma (Konteynerlar):** Paylaşımlı çekirdek mimarisidir. Konteynerlar, ana sunucunun Linux Çekirdeğini (Host Kernel) paylaşırken kullanıcı alanı (user-space) ikiliklerini ve bağımlılıklarını soyutlar. Milisaniyeler seviyesinde başlatma süresi ve minimum RAM kullanımı sağlar.

### 2. Linux Çekirdeği İzolasyon Bileşenleri
* **Namespaces (Görünürlük Sınırları):** 
  * `PID`: Süreç izolasyonu (konteyner süreci kendisini PID 1 olarak görür).
  * `NET`: Sanallaştırılmış ağ katmanları (`veth` çiftleri, yönlendirme tabloları).
  * `MNT`: İzole edilmiş dosya sistemi bağlama noktaları.
  * `IPC`: Süreçler arası iletişim sınırı.
  * `UTS`: Bağımsız sunucu adı (hostname) yapılandırması.
  * `USER`: Yetkisiz kullanıcı ID eşlemesi (konteyner içindeki root, ana sunucuda yetkisiz UID'ye eşlenir).
* **Cgroups v2 (Kaynak Sınırlama & Kontrol):**
  * CPU, RAM ve I/O kullanımı üzerinde katı üst limitler uygular.
  * **OOMKilled Davranışı (Exit Code 137):** Bir süreç kendisine tahsis edilen Cgroup bellek sınırını aştığında tetiklenir ve ana sunucu çekirdeğinin kilitlenmesini engeller.

### 3. Container Runtime Internals & OCI Standartları
* **Üst Seviye vs. Alt Seviye Runtime'lar:** Open Container Initiative (OCI) standartlarına uygun olarak ayrıştırılmış yürütme modelidir.
* **Çalışma Mimarisi:** `Docker CLI` ➔ `dockerd` (Daemon) ➔ `containerd` (İmaj yaşam döngüsü & yönetimi) ➔ `runc` (OCI alt seviye işçi) ➔ `Linux Kernel`.
* **Kubernetes Hizalaması:** `containerd`, Container Runtime Interface (CRI) üzerinden doğrudan haberleşir; böylece modern orkestratörlerde eski Docker Engine bağımlılığı ortadan kalkar.

### 4. Docker Core: Katmanlar, Önbellekleme & Multi-Stage Builds
* **Katman Mimarisi (OverlayFS):** Üst üste binmiş salt okunur katmanların en üstüne eklenen tek bir geçici yazılabilir (Read-Write) konteyner katmanıdır.
* **Build Cache Optimizasyonu:** Dockerfile talimatlarını en az değişenden (`package.json`, sistem paketleri) en çok değişene (kaynak kod) doğru sıralayarak önbellek verimliliğini maksimuma çıkarma yaklaşımıdır.
* **Multi-Stage Build Boru Hattı:** Derleme aşamasındaki SDK ortamını nihai çalışma ortamından ayırarak minimal üretim imajları (`node:20-alpine`, Distroless) elde etme ve saldırı yüzeyini daraltma yöntemidir.

### 5. Depolama Mimarisi & Kalıcılık Stratejileri
* **Writable Layer (Yazılabilir Katman):** Yüksek I/O maliyetine (Copy-on-Write) sahiptir ve tamamen geçicidir; veri tabanlarının kalıcılığı için uygun değildir.
* **Named Volumes (İsimlendirilmiş Hacimler):** Docker tarafından yönetilen sunucu dizinleridir (`/var/lib/docker/volumes/`). Durum bilgisi içeren (stateful) servisler ve veri tabanları için üretim ortamı standardıdır.
* **Bind Mounts:** Sunucudaki bir dizinin doğrudan konteyner içine bağlanmasıdır. Sadece yerel geliştirmedeki canlı kod güncellemeleri (hot-reloading) için kullanılır.
* **tmpfs Mounts:** Performans kritik veya hassas verilerin diske yazılmadan doğrudan sunucu RAM'inde tutulmasını sağlar.
* **UID/GID Sahipliği:** Root olmayan konteynerların bağlı hacimler üzerinde `Permission Denied` hatası almasını önlemek için yapılan kullanıcı izin eşlemesidir.

### 6. Konteyner Ağ Mimarisi & Servis Keşfi
* **Ağ Sürücüleri:** `Bridge` (Varsayılan/Özel), `Host` (Sıfır NAT yükü), `Overlay` (Çok sunuculu VXLAN tünelleme).
* **User-Defined Bridge Ağları:** Gömülü DNS sunucusu (`127.0.0.11`) sayesinde konteynerların değişen IP'ler yerine servis isimleriyle dinamik olarak haberleşmesini (**Service Discovery**) sağlar.
* **NAT & Port Yönlendirme (`-p`):** Sunucu portlarını izole konteyner ağlarına bağlayan `iptables` kurallarıdır.
* **En Az Yetki İlkesi ile Port Yönetimi:** Arka plan servisleri (örneğin 5432 portundaki PostgreSQL) dış dünyaya kapatılır, sadece iç ağdaki konteynerların erişimine açılır.

### 7. Çoklu Konteyner Orkestrasyonu (Docker Compose)
* **Infrastructure as Code (IaC):** Bildirimsel yapılandırma (`docker-compose.yml`) kullanarak yerel, test ve üretim ortamları arasında %100 eşitlik (environment parity) sağlar.
* **Yönlendirilmiş Devirsel Olmayan Graf (DAG):** Servislerin başlatılma sırasını bağımlılıklara göre çözen mimari yapıdır.
* **Healthchecks & Gerçek Hazır Olma Durumu:** Bağımlı servislerin (API) veritabanı tam olarak istek kabul edebilir duruma gelmeden başlatılmasını önlemek için `condition: service_healthy` ve `pg_isready` denetimleri kullanılır.

### 8. Tedarik Zinciri Güvenliği & İmaj Yaşam Döngüsü
* **Tags vs. Digests:** İmaj etiketlerinin (`v1.0.0`, `latest`) değiştirilebilir (mutable) olması ciddi tedarik zinciri riskleri yaratır. Değiştirilemez kriptografik `Digest` (`sha256:...`) kullanımı kod bütünlüğünü garanti eder.
* **Depo (Registry) Sertleştirme:** Depolarda (AWS ECR, GHCR) etiket değiştirilemezliği (Tag Immutability), otomatik zafiyet taraması ve katı RBAC yetkilendirmeleri uygulanır.
* **İmaj Kaynak Doğrulama & İmzalama:** Çalıştırma öncesinde imajın orijinalliğini doğrulamak için **Cosign / Sigstore** ile dijital imzalama yapılır.

### 9. Konteyner Sertleştirme & DevSecOps Savunma Derinliği
* **Non-Root Çalıştırma:** Konteyner içindeki root yetkisinin ana sunucuya sıçramasını (Container Escape) engellemek için yetkisiz kullanıcı (`USER 10001`) tanımlanır.
* **Capabilities Kısıtlama:** `--cap-drop=ALL` kullanılarak Linux Çekirdeğinin gereksiz tüm ekstra yetkileri elinden alınır.
* **Read-Only Dosya Sistemi:** Konteynerın kök dosya sistemi `--read-only` yapılarak içeriye zararlı yazılım (malware) indirilmesi engellenir; geçici veriler için `tmpfs` kullanılır.
* **Otomatik Zafiyet Taraması:** Derleme süreçlerinde **Trivy / Grype** kullanılarak bilinen kritik CVE zafiyetine sahip imajların canlıya çıkması engellenir.

### 10. Operasyon, Sorun Giderme & Adli Analiz
* **Adli Analiz & Teşhis:** Hata ve sızma izlerini sürmek için `docker logs`, `docker inspect` ve gerçek zamanlı `docker events` akışları kullanılır.
* **Bellek Aşımı Analizi:** `Exit Code 137` kontrolü ile OOMKilled durumları tespit edilir.
* **Kendi Kendini İyileştirme (Self-Healing):** Sağlık denetimi (Healthcheck) başarısız olan konteynerların otomatik olarak yeniden başlatılması sağlanır.
* **Güvenli Temizlik:** Sunucudaki eski imaj ve atıkları temizlemek, saldırı yüzeyini daraltmak için `docker system prune` operasyonları yürütülür.


