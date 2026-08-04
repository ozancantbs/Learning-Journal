# 07 — Load Balancing & Proxy Concepts

Bu doküman, modern dağıtık sistem mimarilerinde trafik yönetimi, ölçeklenebilirlik, yüksek erişilebilirlik ve ağ güvenliğinin temelini oluşturan **Load Balancing (Yük Dengeleme)** ve **Proxy (Vekil Sunucu)** kavramlarını derinlemesine incelemektedir. Doküman; Katman 4 (L4) ve Katman 7 (L7) arasındaki mimari farklardan oturum yönetimine, TLS/SSL şifreleme stratejilerinden başlık sahtekarlığı (header spoofing) risklerine, Nginx/HAProxy/Envoy gibi vektör araçlardan Kubernetes Ingress ve Gateway API ekosistemine kadar geniş bir yelpazeyi üretim (production) standartlarında ele almaktadır.

---

## 1. Genel Bakış

İnternet üzerindeki uygulamalar genişledikçe tek bir sunucunun hesaplama, bellek ve ağ bant genişliği kapasitesi yetersiz kalır. Monolitik yapılardan çok sunuculu ve mikroservis tabanlı mimarilere geçildiğinde iki temel ihtiyaç ortaya çıkar:

1. **İstemci ile Sunucu Arasındaki İlişkiyi Soyutlamak ve Yönetmek (Proxying):** İstemcilerin doğrudan iç ağdaki sunuculara erişmesini engellemek, trafiği denetlemek, önbelleklemek ve şifrelemek.
2. **Trafiği Birden Fazla İşlem Birimine Dağıtmak (Load Balancing):** Gelen istek kümesini sistemdeki mevcut sunucular arasında yükü dengeleyecek şekilde paylaştırmak ve çöken sunucuları trafik akışından çıkarmak.

Proxy ve Load Balancer bileşenleri, istemci (client) ile uygulama sunucusu (upstream/backend) arasında **Katman 4 (Transport)** veya **Katman 7 (Application)** seviyesinde konumlanan akıllı ağ geçitleridir (gateways).

---

## 2. Neden Önemlidir?

Trafik yönetimi mimarileri aşağıdaki mühendislik disiplinleri için kritik birleşme noktasıdır:

* **Sistem Yönetimi ve SRE (Site Reliability Engineering):** Sunucu çöküşlerinde kesintisiz hizmet (Zero Downtime) sunulmasını, felaket kurtarma (Disaster Recovery) senaryolarının işletilmesini ve sistem performansının ölçülmesini (observability) sağlar.
* **Ağ Mühendisliği (Networking):** BGP Anycast, DNS tabanlı yönlendirme, TCP bağlantı havuzlama (connection pooling) ve WAN optimizasyonu ile hat verimliliğini artırır.
* **DevSecOps ve Siber Güvenlik (Defensive Security):** Uygulama sunucularını doğrudan dış dünyaya maruz bırakmayarak saldırı yüzeyini (attack surface) küçültür. Web Application Firewall (WAF) entegrasyonu, Rate Limiting ve SSL/TLS sonlandırma ile L7 seviyesinde koruma sağlar.
* **Cloud & Platform Engineering:** Kubernetes mimarisinde Pod'ların dinamik IP adreslerine kesintisiz erişim (Service/Ingress/Gateway API) ve otomatik ölçeklenme (Autoscaling) süreçlerinin sorunsuz çalışmasını mümkün kılar.

---

## 3. Temel Kavramlar ve Terminoloji

| Kavram | Teknik Açıklama | Sistemdeki Rolü | İlişkili Kavramlar |
| :--- | :--- | :--- | :--- |
| **Proxy (Vekil Sunucu)** | İki ağ bileşeni arasındaki trafiğe aracılık eden, paketleri okuyup/değiştirip ileten sunucu. | İstemci veya sunucu adına iletişim kurarak soyutlama sağlar. | Forward Proxy, Reverse Proxy |
| **Forward Proxy** | İstemci (Client) tarafında konumlanan ve iç ağdaki kullanıcıların dış internete erişimini yöneten proxy. | İstemci IP'sini gizler, içerik filtreleme ve önbellekleme yapar. | Explicit Proxy, Transparent Proxy |
| **Reverse Proxy** | Sunucu (Server) tarafında konumlanan, dış dünyadan gelen istekleri karşılayıp iç ağdaki sunuculara dağıtan proxy. | Sunucu altyapısını gizler, TLS sonlandırma ve WAF görevi üstlenir. | Upstream, Backend Pool, Gateway |
| **Load Balancer** | Trafiği önceden tanımlanmış algoritmalar ve sağlık kontrolleri doğrultusunda sunucu havuzuna dağıtan cihaz veya yazılım. | Yüksek erişilebilirlik (HA) ve dikey/yatay ölçeklenebilirlik sağlar. | Round Robin, Health Check, Failover |
| **Upstream / Backend** | Proxy veya Load Balancer'ın arkasında çalışan, asıl iş mantığını (business logic) yürüten uygulama sunucusu. | Müşteri isteğine yanıt üreten kaynak birimdir. | Pod, Target Group, Real Server |
| **Virtual IP (VIP)** | Bir Load Balancer kümesine veya yedekli yapıya atanan, fiziksel donanımdan bağımsız mantıksal IP adresi. | Tekil erişim noktası sunar, failover anında yedek cihaza kayar. | Keepalived, VRRP, CARP |

---

## 4. Çalışma Mantığı ve Katman Farkları (L4 vs L7)

Load Balancer ve Proxy yapıları OSI modelinin çalıştıkları katmanına göre iki ana gruba ayrılır.

```text
  [ OSI Katmanı ]            [ İşlenen Veri ]                   [ Yetenek / Karar ]
┌─────────────────┐    ┌──────────────────────────┐    ┌──────────────────────────────────┐
│ L7 Application  │ ──>│ HTTP Headers, URL, Body, │ ──>│ Akıllı Yönlendirme, Cookie Hash, │
│                 │    │ Cookies, TLS Handshake   │    │ WAF, SSL Termination             │
└─────────────────┘    └──────────────────────────┘    └──────────────────────────────────┘
┌─────────────────┐    ┌──────────────────────────┐    ┌──────────────────────────────────┐
│ L4 Transport    │ ──>│ Source/Dest IP,          │ ──>│ Yüksek Başarım (Throughput),     │
│                 │    │ Source/Dest Port, TCP/UDP│    │ Düşük Gecikme, Paket Yönlendirme │
└─────────────────┘    └──────────────────────────┘    └──────────────────────────────────┘
```

### A. Layer 4 (L4) Load Balancing — Taşıma Katmanı
L4 yük dengeleme, yalnızca TCP ve UDP paket başlıklarına (IP adresi ve Port) bakarak karar verir. Paket içeriği (HTTP verisi, payload) analiz edilmez ve değiştirilmez.

* **Çalışma Mantığı:** L4 yük dengeleme bir NAT (Network Address Translation) veya TCP SYN/ACK paket yönlendirmesi gibi çalışır. İstemci ile backend sunucusu arasında doğrudan bir TCP soket akışı mantıksal olarak korunur.
* **Avantajları:** 
  * Aşırı düşük CPU/bellek kullanımı ve ultra düşük gecikme (latency).
  * Paket gövdesi okunmadığı için yüksek başarım (high throughput - Gbps/Tbps seviyeleri).
* **Dezavantajları:**
  * HTTP path (`/api` vs `/static`), URL, Host header veya Cookie bazlı yönlendirme yapamaz.
  * SSL/TLS içeriğini çözemediği için L7 güvenlik denetimi (WAF) uygulayamaz.

### B. Layer 7 (L7) Load Balancing — Uygulama Katmanı
L7 yük dengeleme, HTTP/HTTPS, gRPC, WebSocket gibi uygulama katmanı protokollerini tam olarak anlar ve ayrıştırır (parsing).

* **Çalışma Mantığı:** L7 araçları bir **Dual-Soket / Reverse Proxy** olarak çalışır. İstemci ile proxy arasında ayrı bir TCP/TLS bağlantısı; proxy ile backend sunucusu arasında **ikinci bir ayrı TCP/TLS bağlantısı** kurulur.
* **Avantajları:**
  * İçerik bazlı yönlendirme (URL path, HTTP method, Host header, Request Headers).
  * Cookie bazlı oturum kalıcılığı (Sticky Sessions).
  * SSL Termination, gzip/brotli sıkıştırma, WAF entegrasyonu.
* **Dezavantajları:**
  * Paketlerin şifresinin çözülmesi ve HTTP parser'dan geçirilmesi nedeniyle yüksek CPU tüketimi.
  * L4'e göre daha yüksek işlem gecikmesi (latency).

---

## 5. Mimari ve Bileşenler

### A. Proxy Türleri ve Mimari Konumları

```text
[ Explicit / Transparent Forward Proxy ]
Client A ──┐
Client B ──┼──> [ Forward Proxy ] ──> (Firewall) ──> [ Public Internet ]
Client C ──┘    (Erişim Kontrolü, CA Önbellek)

[ Reverse Proxy & Load Balancer ]
                                                     ┌──> [ Application Server 1 ]
[ Public Internet ] ──> [ Edge Load Balancer ] ──────┼──> [ Application Server 2 ]
                        (L7 Proxy / TLS / WAF)       └──> [ Application Server 3 ]
```

1. **Explicit Forward Proxy:** İstemci cihazın işletim sistemi veya tarayıcı ayarlarında IP ve Port adresi açıkça (explicitly) tanımlanan proxy'dir. İstemci isteği doğrudan proxy adresine paketler.
2. **Transparent Forward Proxy:** İstemcinin konfigürasyon değişikliği yapmadığı, ağ katmanında (Gateway/Router üzerindeki iptables/nftables kuralları ile) trafiğin çaktırmadan proxy üzerine sapmasıyla (interception) çalışan modeldir.
3. **Reverse Proxy:** Dış istemcilerin iç ağdaki sunuculara ulaşırken uğradığı tek giriş noktasıdır (Ingress). İç ağın topolojisini saklar.

### B. Control Plane ve Data Plane Ayrımı
Modern yük dengeleyici mimarilerinde (örneğin Envoy Proxy):
* **Data Plane (Veri Düzlemi):** Paketlerin okunduğu, şifrelerinin çözüldüğü, yönlendirildiği ve gerçek trafiğin aktığı yüksek performanslı katmandır (C/C++ ile yazılır).
* **Control Plane (Kontrol Düzlemi):** Hangi backend sunucularının aktif olduğunu, yönlendirme kurallarını ve TLS sertifikalarını Data Plane'e gRPC/API (xDS) üzerinden canlı olarak bildiren yönetim katmanıdır.

---

## 6. Adım Adım Gerçek Teknik Akış

Bir istemcinin HTTPS üzerinden bir web sitesine L7 Reverse Proxy aracılığıyla erişiminin paket seviyesindeki adımları:

```text
[ Client ]               [ L7 Reverse Proxy ]               [ Backend Server ]
    │                             │                                  │
    │─── 1. TCP 3-Way Handshake ─>│                                  │
    │─── 2. TLS Handshake ───────>│ (TLS Terminated & Decrypted)     │
    │─── 3. HTTP GET /checkout ──>│                                  │
    │                             │─── 4. Health Check & Algoritma ──│ (Server Seçildi)
    │                             │─── 5. TCP Handshake (Pool) ─────>│
    │                             │─── 6. HTTP GET /checkout ───────>│ (Header Injection)
    │                             │    (X-Forwarded-For Eklendi)     │
    │                             │                                  │── 7. İşlem & 200 OK
    │                             │<── 8. HTTP 200 OK (Payload) ─────│
    │<── 9. HTTP 200 OK (TLS) ────│                                  │
```

1. **TCP Bağlantısı:** İstemci, L7 Proxy'nin Virtual IP (VIP) adresine TCP SYN atarak 3 yollu el sıkışmayı (3-Way Handshake) tamamlar.
2. **TLS El Sıkışması:** İstemci ile Proxy arasında TLS oturumu kurulur. Proxy, kendi üzerindeki SSL sertifikasını sunar.
3. **İstek Gönderimi:** İstemci şifreli tünel içinden `GET /checkout HTTP/1.1` isteğini iletir.
4. **L7 Analizi ve Algoritma Çalışması:** Proxy, TLS tünelini çözer. HTTP istek başlıklarını ve URL yolunu (`/checkout`) okur. Yük dengeleme algoritmasını (örneğin Least Connections) ve sağlık kontrolü durumunu çalıştırarak isteği "Backend Server 2"ye göndermeye karar verir.
5. **Upstream Bağlantısı:** Proxy, kendi iç ağ IP'sinden Backend Server 2'nin IP:Port adresine (eğer önceden açık bir bağlantı havuzu - Connection Pool yoksa) yeni bir TCP bağlantısı açar.
6. **Başlık Değişikliği (Header Injection):** Proxy, istemcinin orijinal IP adresini `X-Forwarded-For` başlığına ekler ve isteği arkadaki sunucuya iletir.
7. **İşlem:** Backend Server yanıtı hazırlar (`HTTP 200 OK`) ve Proxy'ye geri döner.
8. **İletim:** Proxy yanıtı alır, gerekirse sıkıştırır (gzip), istemci ile arasındaki TLS tüneliyle şifreleyerek istemciye iletir.

---

## 7. Gerçek Dünya Kullanım Senaryoları

### Senaryo 1: Yüksek Trafikli E-Ticaret Platformu (L7 Routing & Auto-scaling)
* **Problem:** Flaş indirim dönemlerinde `/cart` ve `/pay` uç noktalarına gelen aşırı yük, ürün katalog sayfalarının (`/products`) çökmesine neden olmaktadır.
* **Çözüm:** En ön kata L7 Reverse Proxy/Load Balancer koyularak URL bazlı yönlendirme yapılır. `/products` trafiği statik önbellek sunucularına, `/pay` trafiği ise izole edilmiş, otomatik ölçeklenen (autoscaled) ödeme mikroservis kümesine yönlendirilir.

### Senaryo 2: Kurumsal Şirket Ağında İnternet Çıkış Güvenliği (Transparent Proxy)
* **Problem:** Şirket çalışanlarının zararlı yazılım içeren sitelere girmesini önlemek ve veri sızıntısını (DLP) engellemek.
* **Çözüm:** Ağın ana router'ı üzerine eklenen iptables kuralı ile 80/443 portlu tüm çıkış trafiği şeffaf olarak (Transparent Forward Proxy) bir Next-Generation Proxy/WAF cihazına aktarılır. Kullanıcı cihazında konfigürasyon yapmaya gerek kalmadan içerik taranır ve kısıtlanır.

---

## 8. Yük Dağıtım Algoritmaları

Yük dengeleyicinin gelen her yeni bağlantı veya isteği hangi backend sunucusuna ileteceğini belirleyen matematiksel yaklaşımlardır.

```text
[ Round Robin ]          [ Weighted RR ]           [ Least Connections ]
Req 1 ──> Sunucu A       Req 1 ──> Sunucu A (%60)  Req 1 ──> Sunucu A (2 Conn)
Req 2 ──> Sunucu B       Req 2 ──> Sunucu A        Req 2 ──> Sunucu B (0 Conn) ◄── Seçildi
Req 3 ──> Sunucu C       Req 3 ──> Sunucu B (%40)
```

### A. Round Robin (Sıralı Dağıtım)
* **Çalışma Mantığı:** Gelen istekleri listedeki sunuculara dairesel sırayla dağıtır (1-2-3-1-2-3).
* **Kullanım Zamanı:** Sunucuların donanım kapasiteleri (CPU/RAM) ve üzerlerine alan iş yükleri eşit olduğunda.

### B. Weighted Round Robin (Ağırlıklı Sıralı Dağıtım)
* **Çalışma Mantığı:** Sunuculara kapasitelerine göre bir ağırlık (weight) değeri verilir (Örn: Sunucu A: 3, Sunucu B: 1). İstekler bu oranlara göre dağıtılır.
* **Kullanım Zamanı:** Sunucu havuzunda hem güçlü hem de eski/zayıf makineler bir arada çalıştığında.

### C. Least Connections (En Az Bağlantılı)
* **Çalışma Mantığı:** O an üzerinde **en az aktif TCP/HTTP bağlantısı** bulunan sunucuyu seçer.
* **Kullanım Zamanı:** İsteklerin işlenme sürelerinin homojen olmadığı (örneğin dosya indirme veya karmaşık rapor sorguları gibi uzun süren işlemlerin bulunduğu) durumlar.

### D. Source IP Hash
* **Çalışma Mantığı:** İstemcinin IP adresi bir hash fonksiyonundan (`hash(Client_IP) % Sunucu_Sayisi`) geçirilir. Çıkan sonuca göre istek belirli bir sunucuya eşlenir.
* **Kullanım Zamanı:** Basit seviyede istemci-sunucu bağımlılığı oluşturmak istendiğinde.

### E. Consistent Hashing (Tutarlı Hashing)
Geleneksel modüler hash sistemlerinde (`hash % N`), sunucu sayısı ($N$) değiştiğinde (yeni sunucu eklendiğinde veya var olan çöktüğünde) neredeyse tüm IP eşleşmeleri değişir ve tüm önbellekler (cache miss) patlar.

```text
                Sunucu A (Hash: 100)
                     /        \
                    /          \
   İstemci 1 (Hash: 50)        Sunucu B (Hash: 200)
                    \          /
                     \        /
                Sunucu C (Hash: 300)
```

* **Çalışma Mantığı:** Hash uzayı $0$ ile $2^{32}-1$ arasında dairesel bir halka (Hash Ring) olarak düşünülür. Sunucular ve istemciler bu halka üzerine yerleştirilir. Bir istemci, halka üzerinde saat yönünde ilerleyerek rastladığı ilk sunucuya yönlendirilir.
* **Avantajı:** Sisteme yeni bir sunucu katıldığında veya çıkarıldığında tüm anahtarlar değil, yalnızca halkanın o dilimine denk gelen anahtarlar ($1/N$ kadarı) yeniden eşlenir. Dağıtık önbellekleme (distributed caching) sistemlerinin temelidir.

---

## 9. Sağlık Kontrolleri (Health Checks) ve Bağımlılık Yönetimi

Sağlıksız bir sunucuya trafik göndermek uygulamanın kullanılamaz hale gelmesine yol açar. Yük dengeleyiciler sunucu durumlarını sürekli izler.

```text
[ Health Check Tipleri ]
 ├── Active Health Check  ──> Proxy belirli aralıklarla arka plana istek atar (Probe).
 └── Passive Health Check ──> Gerçek kullanıcı isteklerinin hata oranlarını izler (Circuit Breaker).
```

### A. Active vs Passive Health Checks

* **Active Health Checks (Aktif Kontrol):** Yük dengeleyici, belirlenen periyotlarda (`interval`, örn: 5sn) backend sunucusunun özel bir endpoint'ine (`GET /healthz`) probe istekleri atar.
  * **Thresholds (Eşik Değerler):** Bir sunucunun "çöktü" kabul edilmesi için üst üste $N$ kez başarısız yanıt vermesi (`fall=3`), tekrar "sağlıklı" kabul edilmesi için $M$ kez başarılı yanıt vermesi (`rise=2`) gerekir.
* **Passive Health Checks (Pasif Kontrol):** Yük dengeleyici ekstra paket atmaz. Gerçek kullanıcı isteklerini izler. Eğer bir sunucu son $X$ saniye içinde $Y$ adet `5xx HTTP` hatası dönerse veya TCP timeout verirse, o sunucuyu geçici olarak havuzdan çıkarır.

### B. Dependency Health (Bağımlılık Sağlığı) ve Riskleri
Uygulama sunucusunun `/healthz` ucu tasarlanırken yapılan en büyük hata, sunucunun bağlı olduğu veritabanı (DB), Redis veya 3. parti API'lerin durumunu körü körüne bu kontrolün içine eklemektir.

> **Risk (Cascading Failures - Müteselsil Çöküş):** Veritabanında geçici bir yavaşlama olduğunda tüm web sunucularının `/healthz` uçları `500 Internal Server Error` dönmeye başlayabilir. Yük dengeleyici tüm sunucuları "sağlıksız" ilan ederek havuzdan çıkarır ve sağlam olan web altyapısı da tamamen erişilemez hale gelir.
>
> **Çözüm:** Sağlık kontrolleri **Liveness** (Süreç ayakta mı?) ve **Readiness** (İstek kabul etmeye hazır mı?) olarak ayrılmalıdır. Liveness sorguları bağımlı servisleri tetiklememelidir.

---

## 10. Oturum Yönetimi, Yüksek Erişilebilirlik ve Bakım

### A. Session Persistence (Sticky Sessions) vs Stateless Design

#### Sticky Sessions (Oturum Kalıcılığı)
Yük dengeleyicinin, bir kullanıcının ilk isteğini karşılayan sunucuya sonraki tüm isteklerini de bağlamasıdır.
* **Çerez Bazlı (Cookie-based - L7):** Proxy, istemciye `Set-Cookie: SERVERID=node01` basar ve sonraki isteklerde bu değer okunarak yönlendirme yapılır.
* **IP Bazlı (Source IP - L4):** Kullanıcı IP'sine göre yönlendirilir. (CGNAT arkasındaki binlerce kullanıcının aynı sunucuya yüklenmesi riski taşır).
* **Dezavantajı:** Sunucu çöktüğünde oturum verisi kaybolur. Düzensiz yük dağılımı oluşur.

#### Stateless Application Design (Durumsuz Tasarım) — Üretim Standardı
Uygulama sunucularının kendi üzerinde durum (state) tutmadığı mimaridir. Oturum verileri merkezi ve hızlı bir veri deposunda (**Redis / Memcached**) tutulur veya istemci tarafında şifreli olarak (**JWT - JSON Web Token**) taşınır.

```text
                                 ┌──> [ App Server A (Stateless) ] ──┐
[ Client ] ──> [ Load Balancer ] ┼──> [ App Server B (Stateless) ] ──┼──> [ Central Redis / DB ]
                                 └──> [ App Server C (Stateless) ] ──┘
```

### B. Connection Draining & Graceful Shutdown
Bir sunucu bakıma alınırken veya otomasyon ile kapatılırken kullanıcıların aniden kesintiye uğramasını önleyen mekanizmadır.

1. Yük dengeleyiciye ilgili sunucu için `DRAIN` komutu verilir.
2. Yük dengeleyici bu sunucuya **YENİ istek/bağlantı göndermeyi durdurur**.
3. Sunucu üzerinde aktif olarak devam eden mevcut bağlantıların bitmesi için bir süre (**Grace Period**, örn: 30-60 saniye) beklenir.
4. Mevcut işlemler bittikten sonra sunucu güvenle kapatılır.

### C. High Availability (HA) ve Failover Mimarileri
Load Balancer'ın kendisinin bir "Tekil Başarısızlık Noktası" (Single Point of Failure - SPOF) olmaması için yedekli kurulması gerekir.

```text
[ Active-Passive HA Mimari ]
[ Master LB (Active) ]  ── (Heartbeat / VRRP) ──  [ Backup LB (Passive) ]
        │ (Virtual IP: 192.168.1.100 sahibi)                 │ (Hazırda bekler)
        ▼                                                     │ (VIP'yi devralır)
[ Application Pool ] <────────────────────────────────────────┘
```

* **Active-Passive Mimari:** Bir aktif, bir pasif sunucu bulunur. İkisi arasında **VRRP (Virtual Router Redundancy Protocol)** veya **Keepalived** ile sanal bir IP (VIP) paylaşılır. Aktif sunucudan kalp atışı (heartbeat) kesilirse VIP otomatik olarak pasif sunucuya kayar (Failover).
* **Active-Active Mimari:** Birden fazla yük dengeleyici aynı anda aktiftir. Trafik, en ön katmanda **DNS Round Robin** veya **BGP Anycast** kullanılarak yük dengeleyiciler arasında paylaştırılır.

---

## 11. TLS/SSL Stratejileri ve Şifreleme Mimarileri

```text
1. SSL Termination : [ İstemci ] ── (HTTPS/TLS) ──> [ LB ] ── (HTTP/Şifresiz) ──> [ Backend ]
2. SSL Passthrough   : [ İstemci ] ── (HTTPS/TLS) ──> [ LB ] ── (HTTPS/TLS) ───> [ Backend ]
3. TLS Re-encryption : [ İstemci ] ── (HTTPS/TLS) ──> [ LB ] ── (Yeni TLS) ────> [ Backend ]
```

### A. SSL Termination (SSL Sonlandırma)
* **Mantık:** TLS şifrelemesi Load Balancer üzerinde çözülür. Proxy ile backend sunucuları arasındaki iç ağda (LAN/VPC) trafik şifresiz (HTTP) olarak akar.
* **Avantajı:** Backend sunucularını ağır TLS handshake ve kriptografik işlemci yükünden kurtarır. Sertifika yönetimi tek noktadan yapılır.
* **Risk:** İç ağ dinleniyorsa (internal eavesdropping) veri açık metin olarak görülebilir.

### B. SSL Passthrough (Doğrudan Geçiş)
* **Mantık:** Proxy L4 seviyesinde çalışır. Şifreli paket içeriğine dokunmadan doğrudan backend sunucusuna iletir. TLS sonlandırma backend üzerinde yapılır.
* **Avantajı:** Uçtan uca şifreleme (End-to-End Encryption) sağlar. Proxy dahi veriyi okuyamaz (Finans/Sağlık düzenlemeleri için zorunlu olabilir).
* **Dezavantajı:** L7 yönlendirme, Cookie okuma, Header müdahalesi ve WAF denetimi yapılamaz.

### C. TLS Re-encryption (Yeniden Şifreleme)
* **Mantık:** Proxy, istemci ile olan TLS bağlantısını sonlandırır, L7 denetimlerini (WAF, Routing) yapar. Ardından backend ile arasında **ikinci bir TLS tüneli** başlatarak veriyi tekrar şifreleyip gönderir.
* **Avantajı:** Hem L7 akıllı yetenekleri kullanılır hem de "Zero Trust" prensibine uygun olarak iç ağda %100 şifreleme sağlanır.

---

## 12. HTTP Başlıkları, Protokol Düzeyi İzlenebilirlik ve Sınır Güvenliği

L7 Reverse Proxy kullanıldığında, backend sunucuları tüm isteklerin istemci IP'si olarak Proxy'nin IP adresini görür. Gerçek istemci kimliğini taşımak için HTTP başlıkları kullanılır.

### A. Kritik HTTP Başlıkları

* **`X-Forwarded-For` (XFF):** İstek zincirindeki istemci ve proxy IP'lerini taşır.
  * *Format:* `X-Forwarded-For: <Müşteri_IP>, <Proxy1_IP>, <Proxy2_IP>`
* **`X-Forwarded-Proto`:** İstemcinin proxy'ye bağlanırken kullandığı protokolü belirtir (`http` veya `https`).
* **`Forwarded`:** RFC 7239 ile standartlaştırılmış, yukarıdaki tüm `X-Forwarded-*` başlıklarını tek bir yapıda toplayan resmi standarttır.
  * *Format:* `Forwarded: for=198.51.100.7;proto=https;by=203.0.113.43`
* **`Host` Header:** İstemcinin tarayıcıya yazdığı asıl domain bilgisidir (`example.com`). Sanal konaklama (Virtual Hosting) için gereklidir.
* **`Proxy Protocol` (v1/v2):** L4 (TCP) seviyesinde çalışan, HTTP dışı protokollerde de (örn: SMTP, MySQL, TLS Passthrough yapılmış HTTPS) gerçek istemci IP ve Port bilgisini TCP paketinin başına ekleyen HAProxy tarafından geliştirilmiş standarttır.

### B. Trusted Proxy Boundaries (Güvenilir Proxy Sınırları) ve Header Spoofing

Saldırganlar HTTP isteklerine elle sahte bir `X-Forwarded-For: 127.0.0.1` başlığı ekleyerek ip-tabanlı yetkilendirme sistemlerini atlatmaya çalışabilir.

```text
[ Saldırgan IP: 5.5.5.5 ] ── (X-Forwarded-For: 127.0.0.1) ──> [ Edge Proxy ] ──> [ Backend ]
```

> **Güvenlik Mimarisi (Trusted Boundaries):** 
> Edge Proxy, dış dünyadan (internet) gelen tüm `X-Forwarded-For` başlıklarını **silmeli veya ezmelidir (override)**. Backend sunucuları ise yalnızca kendi güvenli iç ağındaki IP adreslerinden (Trusted Proxies) gelen XFF başlıklarındaki IP bilgilerini kabul etmelidir.

---

## 13. Trafik Sınırlama ve WAF Entegrasyonu

### A. Connection Limiting vs Rate Limiting

* **Connection Limiting (Bağlantı Sınırlama):** Tek bir IP adresinden açılabilecek **maksimum eşzamanlı TCP bağlantı sayısını** sınırlar (L4).
  * *Amaç:* SYN Flood ve Slowloris gibi kaynak tüketim (DoS) saldırılarını önlemek.
* **Rate Limiting (İstek Hızı Sınırlama):** Belirli bir zaman penceresinde kabul edilecek **HTTP istek sayısını** sınırlar (L7 - Örn: 1 dakikada max 100 istek).
  * *Algoritmalar:* **Leaky Bucket** (Sızdıran Kova) veya **Token Bucket** (Jeton Kovası).
  * *Amaç:* Brute-force, API istismarı ve L7 DDoS koruması.

### B. Web Application Firewall (WAF) İlişkisi
Reverse Proxy, L7 paketlerini açabildiği için bir WAF motoru (örn: ModSecurity, Coraza) ile entegre çalışır. İstek gövdesini (body), query parametrelerini ve başlıkları inceleyerek zararlı imzaları süzebilir:
* **SQL Injection (SQLi):** `' OR '1'='1` kalıplarını engeller.
* **Cross-Site Scripting (XSS):** `<script>` zararlı kod yüklerini (payload) bloklar.
* **Command Injection:** İşletim sistemi seviyesinde komut çalıştırma denemelerini düşürür.

---

## 14. Sektörel Proxy Araçları (Nginx, HAProxy, Envoy)

| Özellik | Nginx | HAProxy | Envoy Proxy |
| :--- | :--- | :--- | :--- |
| **Ana Odak Noktası** | Web Server, Reverse Proxy, Static Caching | Yüksek Performanslı L4/L7 Load Balancer | Cloud-Native Service Mesh & Edge Proxy |
| **Mimari** | Event-driven, Asynchronous (Master/Worker) | Event-driven Single Thread (Multi-thread destegiyle) | Asynchronous C++11, Thread-per-Core |
| **Dinamik Konfigürasyon** | Sınırlı (Reload gerektirir veya Lua/Nginx Plus) | Runtime API var, kısmen reload gerektirir | **Tam Dinamik (xDS gRPC API'leri ile)** |
| **L4 / L7 Desteği** | İkisi de var (Stream / HTTP) | İkisi de çok gelişmiş | İkisi de çok gelişmiş |
| **Observability** | Standart Access Log, Prometheus modülü | Stats page, detaylı log matrisi | **Doğuştan gRPC Tracing, Prometheus metrics** |
| **Sık Kullanım Alanı** | Web siteleri, basit Ingress, Caching | Yüksek trafikli DB/Web yük dengeleme | Kubernetes Service Mesh (Istio), API Gateways |

---

## 15. Bulut ve Kubernetes Ekosisteminde Yük Dengeleme

### A. Kubernetes Services (L4 Yük Dengeleme)
Kubernetes içinde Pod'lar dinamiktir ve IP adresleri sürekli değişir. `Service` kaynağı, Pod kümesine sabit bir IP/DNS sağlar.

```text
[ External Traffic ] ──> [ NodePort / Cloud LB ]
                               │
                               ▼
                    [ Kubernetes Service (ClusterIP) ]
                    (kube-proxy / eBPF iptables kuralları)
                               │
                      ┌────────┴────────┐
                      ▼                 ▼
                 [ Pod A ]          [ Pod B ]
```

* **ClusterIP:** Sadece küme (cluster) içinden erişilebilen varsayılan L4 sanal IP.
* **NodePort:** Tüm worker node'ların IP'leri üzerinde sabit bir port (30000-32767) açarak dış trafiği içeri alır.
* **LoadBalancer:** Bulut sağlayıcının (AWS, GCP, Azure) API'sini çağırarak otomatik olarak harici bir Cloud Load Balancer (Örn: AWS NLB) oluşturur.

### B. Ingress ve Ingress Controllers (L7)
`Service (LoadBalancer)` L4 çalıştığı için her servis için ayrı bir cloud LB açmak maliyetlidir. **Ingress**, tek bir IP adresi üzerinden domain ve path bazlı L7 yönlendirmesi sağlar.
* **Ingress Resource:** Yönlendirme kurallarını belirten YAML dosyası.
* **Ingress Controller:** Kuralları okuyup trafiği yöneten Proxy yazılımı (Örn: Nginx Ingress Controller).

### C. Kubernetes Gateway API
Ingress kaynağının esneklik eksikliklerini (Advanced Routing, Header Matching, Trafik Bölme - Canary) çözmek için geliştirilen yeni nesil Kubernetes standardıdır.

* **Rol Tabanlı Yönetim:**
  * `GatewayClass`: Altyapı sağlayıcısı tanımlar (Örn: Envoy, Cilium).
  * `Gateway`: Cluster Admin IP, Port ve TLS sertifikasını bağlar.
  * `HTTPRoute` / `TLSRoute`: Uygulama geliştirici kendi yönlendirme mantığını yazar.

### D. Bulut ve Küresel Yük Dengeleme (Global & Regional LB)
* **Regional LB:** Trafiği tek bir bulut bölgesindeki (Region) Kullanılabilirlik Bölgeleri (Availability Zones - AZ) arasında dağıtır.
* **Global LB & Anycast:** **BGP Anycast** teknolojisi ile dünyanın farklı yerindeki POP (Point of Presence) noktalarına aynı IP verilir. Kullanıcı ağ seviyesinde kendine en yakın veri merkezine yönlendirilir.
* **DNS-Based Distribution (GSLB):** Geo-DNS ile kullanıcının bulunduğu ülkeye/kıtaya göre farklı IP yanıtı dönülür.
  * *Sınırlama:* DNS önbellekleme (TTL) nedeniyle çöküş anında trafiği anında başka yere kaydırmak gecikebilir.

---

## 16. Güvenlik Perspektifi ve Risk Analizi

### Risk 1: Open-Proxy (Açık Vekil Sunucu) Zafiyeti
* **Teknik Sorun:** Yanlış yapılandırılmış bir Forward Proxy'nin dış internetten gelen yetkisiz her türlü isteğe açık olması.
* **Olası Etki:** Saldırganlar bu proxy'yi kullanarak kendi IP'lerini gizler, yasadışı faaliyetleri sizin altyapınız üzerinden yürütür. Sunucularınız karalisteye (black-list) girer.
* **Savunma:** Proxy erişimleri yalnızca şirket içi IP bloklarına (CIDR) kısıtlanmalı ve kimlik doğrulama (Authentication) zorunlu tutulmalıdır.

### Risk 2: HTTP Request Smuggling (İstek Kaçakçılığı)
* **Teknik Sorun:** Front-End Proxy ile Back-End sunucunun, gelen HTTP paket sınırlarını belirleyen `Content-Length` (CL) ve `Transfer-Encoding` (TE) başlıklarını farklı yorumlaması (`CL.TE` veya `TE.CL` desenkronizasyonu).
* **Olası Etki:** Saldırgan tek bir isteğin içine gizlenmiş ikinci bir istek gömer. Bu gizli istek, arkadaki başka bir kullanıcının isteğinin önüne geçer; oturum çalma (session hijacking) ve yetki yükseltme (privilege escalation) gerçekleşir.
* **Savunma:** HTTP/1.1 pipelining devre dışı bırakılmalı, proxy ile backend arasında HTTP/2 kullanımı teşvik edilmeli ve RFC standartlarına tam uyan proxy yazılımları (güncel Nginx/HAProxy) kullanılmalıdır.

### Risk 3: Sağlık Kontrolü uç noktalarının Kötüye Kullanımı (Unauthenticated DoS)
* **Teknik Sorun:** Sağlık kontrolü uç noktasının (`/healthz`) veritabanı, disk ve external servisleri derinden sorgulayan (heavy check) yapıda olması ve dış dünyaya açık bırakılması.
* **Olası Etki:** Saldırgan `/healthz` adresine binlerce istek atarak veritabanı bağlantı havuzunu tüketir ve sistemi servis dışı bırakır (DoS).
* **Savunma:** Healthcheck uç noktaları dış internete kapatılmalı, yalnızca Load Balancer iç IP adreslerine yetkilendirilmelidir.

---

## 17. Hardening ve Best Practices

| Kontrol | Neden Önemli? | Uygulama Alanı | Sınırlama / Trade-off |
| :--- | :--- | :--- | :--- |
| **Drop Unhandled Hosts** | Sahte Host header ile yapılan saldırıları ve taramaları engeller. | Reverse Proxy | Geçerli tüm domainlerin explicit tanımlanmasını gerektirir. |
| **Disable Weak TLS Ciphers** | Traffic eavesdropping (dinleme) ve MitM saldırılarını önler. | TLS Termination | Eski cihaz/tarayıcı desteği kesilir (TLS 1.0/1.1 kaldırılır). |
| **Timeouts Optimization** | Slowloris ve yetersiz soket kapatma kaynaklı DoS'u engeller. | L4/L7 Proxy | Çok yavaş istemcilerin (2G/3G) bağlantıları kopabilir. |
| **Header Sanitization** | `X-Forwarded-For` sahteciliğini önler. | Edge Proxy | Doğru konfigüre edilmezse gerçek kullanıcı IP'leri kaybolur. |
| **Enforce Rate Limiting** | Brute force ve L7 DDoS etkisini azaltır. | API Gateway / L7 Proxy | Ani meşru trafik patlamalarında (burst) gerçek kullanıcılar bloklanabilir. |

---

## 18. Avantajlar, Dezavantajlar ve Trade-off'lar

### L4 vs L7 Load Balancing Değerlendirmesi
* **L4 Avantajı:** Aşırı yüksek performans, düşük bellek kullanımı, protokol bağımsızlığı.
* **L4 Dezavantajı:** Sıfır uygulama farkındalığı, WAF uygulanamaması, SSL içeriğini görememe.
* **L7 Avantajı:** Gelişmiş akıllı yönlendirme, SSL termination, mikroservis ve WAF uyumu.
* **L7 Dezavantajı:** Yüksek CPU yükü, artan gecikme (latency), daha karmaşık sorun giderme (troubleshooting).

### SSL Termination vs TLS Re-encryption
* **SSL Termination:** Yüksek performans ve kolay sertifika yönetimi sunar; ancak proxy ile sunucu arasındaki iç ağ şifresiz kalır.
* **TLS Re-encryption:** Uçtan uca Zero-Trust güvenlik sağlar; ancak çift şifreleme/şifre çözme nedeniyle yüksek CPU tüketir ve gecikmeyi artırır.

---

## 19. Yaygın Yanlış Anlamalar

### Yanlış Anlama 1
* **Yanlış Düşünce:** *"Load Balancer eklemek sistemin performansını ve hızını her zaman artırır."*
* **Neden Yanlış?** Load Balancer sisteme ek bir "hop" (ağ sıçraması) getirir. Ağ gecikmesini (latency) birkaç milisaniye artırır.
* **Doğru Teknik Yaklaşım:** Load Balancer tekil bir isteği hızlandırmaz; sistemin **toplam kapasitesini (throughput/concurrency)** ve **erişilebilirliğini (availability)** artırır.

### Yanlış Anlama 2
* **Yanlış Düşünce:** *"X-Forwarded-For başlığındaki ilk IP adresi her zaman güvenilirdir ve kullanıcının gerçek IP'sidir."*
* **Neden Yanlış?** Dış dünyadaki herhangi bir istemci HTTP isteğine elle `X-Forwarded-For: 1.1.1.1` ekleyebilir.
* **Doğru Teknik Yaklaşım:** Sadece ilk güvenilir edge proxy tarafından ezilen veya doğru işlenen IP zinciri güvenilirdir. Sınır proxy'si dışarıdan gelen XFF başlığını doğrulamadan kabul etmemelidir.

---

## 20. Observability, Access Logs ve Troubleshooting

### A. L7 Access Log Yapısı ve Kritik Metrikler
Sağlıklı bir L7 proxy logunda yalnızca yanıt kodu değil, zamanlama metrikleri de yer almalıdır (Nginx log formatı örneği):

```nginx
log_format custom_obs '$remote_addr - $remote_user [$time_local] "$request" '
                      '$status $body_bytes_sent "$http_referer" '
                      'rt=$request_time uct="$upstream_connect_time" '
                      'uht="$upstream_header_time" urt="$upstream_response_time"';
```

* `request_time` ($rt$): İsteğin proxy'ye girmesi ile yanıtın istemciye tamamen gönderilmesi arasında geçen toplam süre.
* `upstream_connect_time` ($uct$): Proxy'nin backend sunucusu ile TCP el sıkışması kurarken harcadığı süre.
* `upstream_response_time` ($urt$): Backend sunucusunun isteği alıp yanıtın ilk byte'ını dönmesine kadar geçen süre (Uygulama hızı).

### B. 502 Bad Gateway vs 504 Gateway Timeout Kök Neden Analizi

* **`502 Bad Gateway` (Kötü Ağ Geçidi):**
  * *Anlamı:* Proxy, arkadaki sunucunun kapısını çaldı ancak sunucu bağlantıyı reddetti (Connection Refused) veya sunucu tamamen kapalı.
  * *Olası Nedenler:* Backend servis çökmüş (crash), yanlış port dinleniyor, firewall proxy IP'sini engelliyor.
* **`504 Gateway Timeout` (Ağ Geçidi Zaman Aşımı):**
  * *Anlamı:* Proxy, arkadaki sunucuya başarıyla bağlandı fakat sunucu belirlenmiş bir süre (`proxy_read_timeout`) boyunca yanıt vermedi.
  * *Olası Nedenler:* Veritabanı sorgusu kilitlendi (deadlock), arka plandaki uygulama sonsuz döngüde veya kilitli (CPU saturation), sunucu aşırı yük altında yanıt üretemiyor.

### C. Doğrulanmış Troubleshooting Komutları

* **1. Proxy'den Backend Portuna TCP Erişilebilirliğini Test Etme:**
  ```bash
  nc -zv -w 3 10.0.0.5 8080
  ```
  *Açıklama:* `10.0.0.5` IP'li sunucunun `8080` portuna 3 saniye timeout ile TCP SYN atar. Portun açık ve dinlemede olup olmadığını doğrular.

* **2. Proxy'yi Bypass Edip Doğrudan Backend Uç Noktasını Sorgulama:**
  ```bash
  curl -ivH "Host: example.com" [http://10.0.0.5:8080/healthz](http://10.0.0.5:8080/healthz)
  ```
  *Açıklama:* Proxy katmanını aradan çıkararak doğrudan backend sunucusuna `Host` başlığıyla istek atar. Uygulamanın L7 seviyesinde canlılığı test edilir.

* **3. Canlı Trafik Dinleme ve Paket Analizi:**
  ```bash
  sudo tcpdump -i eth0 port 8080 -nn -A
  ```
  *Açıklama:* `eth0` arayüzündeki 8080 port trafiğini yakalar. IP adreslerini çözmeden (`-nn`) ASCII formatında (`-A`) ekrana basar. Proxy ile backend arasındaki HTTP paketleşmesini incelemeye yarar.

* **4. Soket Durumlarını ve Bağlantı Havuzunu İnceleme:**
  ```bash
  ss -tulpn | grep :80
  ```
  *Açıklama:* Yerel makinede 80 portunu dinleyen süreçleri (process PID ve ismiyle) listeler.

---

## 21. Uygulamalı Doğrulama (Validation)

Bu bölümde, Nginx kullanarak bir L7 Reverse Proxy, SSL Termination ve 2 adet Backend sunucusu arasındaki yük dengeleme davranışını simüle eden güvenli bir doğrulama senaryosu sunulmuştur.

### Amaç
Nginx üzerinde Round Robin yük dengeleme, `X-Forwarded-For` başlık aktarımı ve basit bir sağlık kontrolü mekanizmasını doğrulamak.

### Ortam ve Gereksinimler
* Docker ve Docker Compose kurulu bir Linux/Unix ortamı.

### 1. Konfigürasyon Dosyalarının Hazırlanması

Aşağıdaki `nginx.conf` dosyasını oluşturun:

```nginx
events { worker_connections 1024; }

http {
    upstream backend_cluster {
        # Round Robin Dağıtımı
        server app1:8080 max_fails=3 fail_timeout=10s;
        server app2:8080 max_fails=3 fail_timeout=10s;
    }

    server {
        listen 80;
        server_name localhost;

        location / {
            proxy_pass http://backend_cluster;
            proxy_set_header Host $host;
            proxy_set_header X-Real-IP $remote_addr;
            proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
            proxy_set_header X-Forwarded-Proto $scheme;
            
            # Timeout Ayarları
            proxy_connect_timeout 2s;
            proxy_read_timeout 5s;
        }
    }
}
```

### 2. Ortamın `docker-compose.yml` ile Başlatılması

```yaml
version: '3.8'

services:
  proxy:
    image: nginx:alpine
    ports:
      - "8080:80"
    volumes:
      - ./nginx.conf:/etc/nginx/nginx.conf:ro
    depends_on:
      - app1
      - app2

  app1:
    image: hashicorp/http-echo
    command: ["-text=Yanit Sunucu 1"]

  app2:
    image: hashicorp/http-echo
    command: ["-text=Yanit Sunucu 2"]
```

### 3. Doğrulama Adımları

1. Mimarileri ayağa kaldırın:
   ```bash
   docker-compose up -d
   ```
2. Yük dengeleme çalışmasını sırayla istek atarak doğrulayın:
   ```bash
   curl http://localhost:8080
   curl http://localhost:8080
   ```
   *Beklenen Sonuç:* İlk istek "Yanit Sunucu 1", ikinci istek "Yanit Sunucu 2" dönecektir (Round Robin doğrulaması).

3. Sunucu Çöküş Simülasyonu (Failover Testi):
   ```bash
   docker-compose stop app1
   curl http://localhost:8080
   ```
   *Beklenen Sonuç:* Nginx `app1` sunucusunun kapalı olduğunu anlar ve istemciye hata dönmeden tüm trafiği otomatik olarak `app2` sunucusuna yönlendirir.

4. Ortamı Temizleme:
   ```bash
   docker-compose down -v
   ```

---

## 22. Production Ortamı Perspektifi

Laboratuvar ortamında çalışan bir yük dengeleyici mimarisi, canlı (production) ortama taşınırken şu kritik faktörler dikkate alınmalıdır:

* **Kapasite Planlaması (Capacity Planning):** SSL/TLS el sıkışmaları ciddi CPU tüketir. Üretim ortamlarında L7 Proxy'ler için CPU limitleri yüksek tutulmalı veya donanımsal TLS Offloading/Cryptographic Accelerator kartları kullanılmalıdır.
* **Yüksek Erişilebilirlik ve Fault Domains:** Yük dengeleyiciler farklı fiziksel kabinlere (Racks) veya bulut ortamında farklı Kullanılabilirlik Bölgelerine (Multi-AZ) dağıtılmalıdır.
* **Sertifika Yaşam Döngüsü (Certificate Lifecycle):** TLS sertifikalarının sürelerinin dolması üretimi durduran en yaygın hatalardandır. `Cert-Manager` veya ACME protokolleri ile sertifika yenileme süreçleri otomatikleştirilmelidir.
* **Gözlemlenebilirlik Entegrasyonu:** Proxy erişim logları canlı olarak merkezi log sistemine (Loki, Elasticsearch) akıtılmalı; Prometheus metrikleri (`nginx_ingress_controller_requests`, HAProxy `http_responses_total`) üzerinden $p99$ gecikme alarmları kurulmalıdır.

---

## 23. Diğer Teknolojilerle İlişkisi

* **Linux Networking:** IPTables, IPVS, eBPF ve `sysctl` ağ parametreleri (`net.ipv4.ip_local_port_range`, `net.core.somaxconn`) yük dengeleyicilerin maksimum performansla çalışmasında alt katmanı oluşturur.
* **Kubernetes & CNI:** Calico, Cilium gibi CNI'lar eBPF kullanarak L4 yük dengelemeyi Linux çekirdeği (kernel) seviyesinde `kube-proxy` yükü olmadan gerçekleştirir.
* **Observability (OpenTelemetry / Jaeger):** L7 Proxy'ler, gelen isteklere `traceparent` (W3C Trace Context) başlıkları ekleyerek dağıtık izleme (distributed tracing) süreçlerinin başlatılmasını sağlar.

---

## 24. Junior Seviyede Bilinmesi Gerekenler

Junior seviyedeki bir sistem, DevOps veya güvenlik mühendisinin bu konuda sahip olması gereken temel yetkinlikler:

* **Farkı Anlayabilmek:** Forward Proxy (istemciyi korur/gizler) ile Reverse Proxy (sunucuyu korur/gizler) arasındaki temel farkı net olarak açıklayabilmek.
* **L4 vs L7 Ayrımı:** L4'ün sadece IP/Port bildiğini, L7'nin ise HTTP başlığı, URL ve Cookie okuyabildiğini bilmek.
* **Başlık Mantığı:** `X-Forwarded-For` başlığının ne işe yaradığını ve neden proxy arkasındaki uygulamanın istemci IP'sini doğrudan göremediğini anlamak.
* **Temel Troubleshooting:** `502 Bad Gateway` (sunucu kapalı/ulaşılamıyor) ile `504 Gateway Timeout` (sunucu yavaş/yanıt vermiyor) farkını bilmek ve `curl`/`nc` komutlarıyla ilk teşhisi koyabilmek.
* **Sağlık Kontrolleri:** Çöken bir sunucunun yük dengeleyici tarafından havuzdan nasıl çıkarıldığını mantıksal olarak kavramak.

---

## 25. Kısa Özet

1. **Proxy**, iki sistem arasında vekillik yapar; Forward Proxy istemci tarafında, Reverse Proxy sunucu tarafında konumlanır.
2. **L4 Load Balancing**, IP/Port seviyesinde çalışır, ultra hızlıdır ancak paket içeriğini okuyamaz.
3. **L7 Load Balancing**, HTTP/HTTPS içeriğini okur; URL routing, Cookie bazlı sticky session ve WAF yetenekleri sunar.
4. **Consistent Hashing**, sunucu eklenip çıkarıldığında önbellek patlamalarını minimize eden halka bazlı algoritmadır.
5. **SSL Termination**, şifreyi Proxy üzerinde çözer; sunucuları CPU yükünden kurtarır.
6. **TLS Re-encryption**, hem L7 incelemesi yapmayı sağlar hem de iç ağda Zero-Trust şifrelemeyi korur.
7. **`X-Forwarded-For`**, L7 proxy arkasındaki backend sunucularına istemcinin gerçek IP adresini taşır.
8. **Trusted Boundaries**, dışarıdan gelen sahte IP başlıklarının (Header Spoofing) ezilmesini sağlayarak güvenliği korur.
9. **Stateless Mimari**, oturum verilerini sunucu üzerinde değil Redis/JWT gibi yapılarda tutarak sorunsuz ölçeklenme sağlar.
10. **Connection Draining**, bakım anında mevcut kullanıcı işlemlerinin yarım kalmasını önler.
11. **`502 Bad Gateway`**, backend sunucusunun kapalı olduğunu; **`504 Gateway Timeout`**, backend sunucusunun yanıt vermekte geciktiğini gösterir.
12. **Kubernetes Gateway API**, eski Ingress standardının yerini alan rol tabanlı ve esnek yeni nesil L7 trafik yönetim standardıdır.

---

## 26. Kaynaklar

* **RFC 7239 (Forwarded HTTP Extension) — IETF:**
  Doğrulanan konu: Resmi `Forwarded` başlığı standart formatı ve kullanımı.
* **RFC 9110 (HTTP Semantics) — IETF:**
  Doğrulanan konu: HTTP/1.1 ve HTTP/2 yönlendirme, başlık ve durum kodları standartları.
* **HAProxy Documentation — HAProxy Technologies:**
  Doğrulanan konu: Proxy Protocol v1/v2 çalışma mantığı, L4/L7 altyapısı ve runtime API.
* **Nginx Reference Documentation — F5 / Nginx:**
  Doğrulanan konu: Upstream modülleri, `proxy_pass` parametreleri ve time/logging metrikleri.
* **Envoy Proxy xDS API Specification — CNCF / Envoy:**
  Doğrulanan konu: Dynamic control plane, Service Mesh sidecar architecture ve L7 routing.
* **Kubernetes Gateway API Documentation — SIG-Network:**
  Doğrulanan konu: `GatewayClass`, `Gateway` ve `HTTPRoute` kaynaklarının rol tabanlı mimarisi.
* **Linux Manual Pages (`tcpdump(8)`, `ss(8)`, `nc(1)`):**
  Doğrulanan konu: Troubleshooting komut parametreleri ve soket analizi adımları.
