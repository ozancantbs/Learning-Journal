# 06 — Uygulama Katmanı Servisleri (Application Layer Services)

Bu doküman, bilgisayar ağlarında son kullanıcı uygulamaları ile ağ altyapısı arasındaki etkileşimi sağlayan **Uygulama Katmanı (Application Layer)** servislerini ve protokollerini teknik olarak incelemektedir. Doküman; DNS mimarisi, DHCP IP tahsis mekanizmaları, HTTP/HTTPS protokol evrimi, TLS/PKI güvenliği, siber güvenlik riskleri, troubleshooting yöntemleri ve modern Kubernetes/Cloud entegrasyonlarını kapsar.

---

## 1. Genel Bakış

Uygulama katmanı, OSI modelinin 7. katmanı ve TCP/IP mimarisinin en üst katmanıdır. Temel görevi, yazılımların ve son kullanıcı süreçlerinin ağ altyapısına erişimini sağlamak, verinin biçimlendirilmesini düzenlemek ve istemci-sunucu arasındaki iletişimi yönetmektir.

Uygulama katmanında süreçler doğrudan ağ kabloları veya yönlendiricilerle etkileşime girmez; bunun yerine alt katmanların (Transport, Network vb.) sunduğu soket (socket) arayüzlerini ve protokollerini kullanır. Ağ üzerindeki iletişimin büyük bir kısmı iki temel iletişim modeli ve iki mimari durum (state) yapısı üzerinde gerçekleşir:

*   **İstemci-Sunucu (Client-Server) İletişim Modeli:** İstemci (Client) bir kaynağa veya servise erişmek için isteği başlatan taraftır. Sunucu (Server) ise belirli bir ağ portunu dinleyen (listening), gelen istekleri karşılayan ve yanıt dönen yetkili taraftır.
*   **İstek-Yanıt (Request-Response) Modeli:** İletişim, istemcinin oluşturduğu yapılandırılmış bir istek paketi ile başlar. Sunucu bu isteği işler ve durum bilgisini (Status) de içeren bir yanıt paketi üretir. Senkron veya asenkron olarak çalışabilir.
*   **Durumsuz (Stateless) ve Durumlu (Stateful) Uygulama Kavramları:**
    *   *Stateless (Durumsuz):* Sunucu, istemciden gelen her bir isteği tamamen bağımsız bir işlem olarak değerlendirir. Geçmiş isteklere ait hiçbir durum verisi (state) sunucu hafızasında tutulmaz (Örn: Yalın HTTP, DNS). Ölçeklenebilirliği yüksektir.
    *   *Stateful (Durumlu):* Sunucu veya uygulama katmanı, istemciyle yapılan önceki etkileşimlerin bilgisini oturum (session), veri tabanı veya bellek içi veri depolarında (Örn: Redis) saklar (Örn: FTP, SSH, Veri tabanı bağlantıları).

---

## 2. Neden Önemlidir?

Uygulama katmanı servisleri, modern BT altyapılarının ve dağıtık sistemlerin çalışabilmesi için zorunlu ön koşuldur.

*   **Sistem ve Ağ Mühendisliği:** IP adreslerinin dinamik dağıtımı (DHCP) ve insan tarafından okunabilir alan adlarının IP adreslerine dönüştürülmesi (DNS) olmadan ölçeklenebilir bir ağ altyapısı yönetilemez.
*   **Siber Güvenlik ve Savunma (Defensive Security):** Ağa yönelik saldırıların (DDoS, MitM, Data Exfiltration) önemli bir kısmı DNS, DHCP ve HTTP/TLS katmanındaki zafiyetleri veya yapılandırma hatalarını hedefler.
*   **DevOps ve Platform Engineering:** Mikroservis mimarilerinde servislerin birbiriyle haberleşmesi (Service Discovery), Kubernetes CoreDNS altyapısı ve otomatik TLS sertifika yönetimi (ACME/Cert-Manager) tamamen bu katmandaki servislerle yürütülür.
*   **SRE (Site Reliability Engineering):** Web uygulamalarındaki gecikme (latency), hata oranları (5xx hataları) ve kullanılabilirlik (availability) problemleri doğrudan HTTP durum kodları, önbellek başlıkları ve TLS el sıkışma süreleri izlenerek teşhis edilir.

---

## 3. Temel Kavramlar ve Terminoloji

### A. DNS (Domain Name System) Kavramları

*   **FQDN (Fully Qualified Domain Name — Tam Nitelikli Alan Adı):** İnternet hiyerarşisinde bir kaynağın mutlak konumunu belirten tam adrestir. En sonda gizli bir kök nokta (`.`) içerir (Örn: `app.example.com.`).
*   **Domain Hierarchy (Alan Adı Hiyerarşisi):**
    *   *Root Zone (`.`):* İsim hiyerarşisinin en üst noktasıdır.
    *   *TLD (Top-Level Domain — Üst Seviye Alan Adı):* `.com`, `.org`, `.net` (gTLD) veya `.tr`, `.de` (ccTLD) gibi uzantılardır.
    *   *SLD (Second-Level Domain):* `example.com` içindeki `example` gibi tescil edilen ana isimdir.
    *   *Subdomain (Alt Alan Adı):* `app.example.com` yapısındaki `app` gibi ana domaine bağlı alt birimdir.
*   **Sorgu Türleri:**
    *   *Recursive Query (Özyinelemeli Sorgu):* İstemcinin çözücüye (Resolver) gönderdiği sorgudur. İstemci "Bana IP adresini bul ya da hata dön" der; aradaki tüm dolaşım işini çözücü üstlenir.
    *   *Iterative Query (Yinelemeli Sorgu):* Çözücünün isim sunucularına sırayla sorduğu sorgudur. Sunucu "Tam cevabı bilmiyorum ama bir sonraki sorman gereken sunucu şudur" yanıtı döner.
*   **Çözücü ve Sunucu Rolleri:**
    *   *Stub Resolver:* İşletim sisteminde çalışan, karmaşık sorgu yapma yeteneği olmayan, isteği doğrudan bir Recursive Resolver'a ileten istemci bileşenidir.
    *   *Recursive Resolver (Recursive Name Server):* İstemciden aldığı özyinelemeli sorguyu çözmek için kök sunuculardan başlayarak adım adım dolaşan ve sonucu istemciye sunan sunucudur (Örn: 8.8.8.8, 1.1.1.1).
    *   *Authoritative Resolver (Yetkili İsim Sunucusu):* Belirli bir DNS bölgesi (Zone) için nihai ve kesin yanıtı barındıran sunucudur.
    *   *Root Name Servers:* Dünya genelinde Anycast IP altyapısıyla dağıtılmış 13 yetkili kök sunucu kümesidir (`a.root-servers.net` - `m.root-servers.net`).
    *   *TLD Name Servers:* İlgili TLD'ye ait yetkili sunucuların adreslerini barındıran isim sunucularıdır.
*   **DNS Zones and Delegation (Bölge ve Devir):** Bir alan adının idari yönetim alanına DNS Zone adı verilir. Bir üst bölgenin alt bölgenin sorumluluğunu başka isim sunucularına devretmesine Delegation (Yetki Devri) denir.
*   **DNS Kayıt Türleri (Resource Records):**

| Kayıt Tipi | Açıklama | Örnek Kullanım Amacı |
| :--- | :--- | :--- |
| **A** | Domain ismini IPv4 adresine eşler | `example.com` $\rightarrow$ `192.0.2.1` |
| **AAAA** | Domain ismini IPv6 adresine eşler | `example.com` $\rightarrow$ `2001:db8::1` |
| **CNAME** | Bir domain ismini başka bir domain ismine yönlendirir (Alias) | `www.example.com` $\rightarrow$ `example.com` |
| **MX** | Domain adının e-posta sunucularını ve öncelik sıralarını belirtir | `example.com` $\rightarrow$ `mail.example.com` (Priority: 10) |
| **TXT** | Metin verisi saklar; doğrulama ve güvenlik protokollerinde kullanılır | SPF, DKIM, DMARC, Domain doğrulamaları |
| **NS** | Bölgenin yetkili isim sunucularını (Authoritative DNS) belirtir | `example.com` $\rightarrow$ `ns1.example.com` |
| **PTR** | Reverse DNS için IP adresini domain ismine eşler | `1.2.0.192.in-addr.arpa` $\rightarrow$ `example.com` |
| **SOA** | Bölge hakkındaki temel yetkili bilgileri, TTL ve seri numarasını saklar | Zone Transfer ve yetki senkronizasyonu |
| **SRV** | Belirli servislerin port, protokol ve öncelik bilgilerini tanımlar | Active Directory, VoIP (SIP), Kubernetes servis keşfi |

*   **DNS Önbellekleme (Caching) ve TTL:**
    *   *Positive Caching:* Başarılı çözümlenen kayıtların belirli bir süre hafızada saklanması.
    *   *Negative Caching:* Var olmayan domain (`NXDOMAIN`) cevabının saklanarak gereksiz trafiğin önlenmesi.
    *   *TTL (Time To Live):* Kaydın önbellekte kaç saniye tutulacağını belirten SOA/Kayıt değeridir.
*   **Forward ve Reverse DNS:**
    *   *Forward DNS:* İsım $\rightarrow$ IP dönüşümüdür.
    *   *Reverse DNS (rDNS):* IP $\rightarrow$ İsim dönüşümüdür. Özel `in-addr.arpa` (IPv4) veya `ip6.arpa` (IPv6) alanları üzerinden `PTR` kayıtları ile yapılır.
*   **İletim ve Genişletme Protokolleri:**
    *   *DNS over UDP/TCP:* DNS varsayılan olarak UDP Port 53 kullanır. Yanıt boyutu **512 Byte** sınırını aştığında veya Bölge Aktarımı (Zone Transfer - AXFR/IXFR) yapıldığında **TCP Port 53** protokolüne geçilir.
    *   *EDNS (Extension Mechanisms for DNS / EDNS0 - RFC 6891):* UDP paket sınırını 4096 Byte'a kadar çıkarabilen, DNSSEC imzalarına izin veren ve istemci konum bilgisini ileten (`EDNS Client Subnet - ECS`) genişletme standardıdır.
*   **Gizlilik, Güvenlik ve Mimari Yaklaşımlar:**
    *   *DNSSEC (DNS Security Extensions):* Dijital imzalar (RRSIG, DNSKEY, DS) kullanarak DNS yanıtlarının yolda değiştirilmediğini (bütünlük) kanıtlar. Trafiği şifrelemez.
    *   *DNS over TLS (DoT):* DNS sorgularını **TCP Port 853** üzerinden varsayılan TLS katmanı ile şifreler.
    *   *DNS over HTTPS (DoH):* DNS sorgularını **TCP Port 443** üzerinden HTTP/2 veya HTTP/3 trafiği içine gizleyerek şifreler.
    *   *Split-Horizon DNS (Split-Brain):* İç ağdaki (Internal) ve dış ağdaki (External) istemcilere aynı alan adı için farklı IP adresleri yanıtı dönme tekniğidir.

### B. DHCP (Dynamic Host Configuration Protocol) Kavramları

*   **DHCP:** Ağa katılan cihazlara otomatik IP adresi, Alt Ağ Maskesi (Subnet Mask), Varsayılan Geçit (Default Gateway) ve DNS sunucularını atayan uygulama katmanı protokolüdür. UDP Port 67 (Sunucu) ve UDP Port 68 (İstemci) kullanır.
*   **DORA Süreci:** İstemci ile sunucu arasındaki 4 adımlı IP kiralama el sıkışmasıdır (Discover, Offer, Request, Acknowledge).
*   **Lease Lifecycle (Kiralama Yaşam Döngüsü):** IP adresinin geçici kiralama süresidir.
    *   *T1 Zamanlayıcısı (%50):* Sürenin yarısında istemci sunucuya doğrudan (Unicast) `DHCPREQUEST` atarak süreyi uzatmak ister.
    *   *T2 Zamanlayıcısı (%87.5):* Yanıt alınamazsa istemci tüm ağa (Broadcast) `DHCPREQUEST` atarak herhangi bir sunucudan uzatma ister.
*   **DHCP Options (DHCP Seçenekleri):** IP paketine eklenen ek konfigürasyon parametreleridir.
    *   *Option 3:* Default Gateway
    *   *Option 6:* DNS Sunucuları
    *   *Option 15:* Domain İsmi
    *   *Option 66/67:* Ağ üzerinden ön yükleme (PXE Boot / TFTP Server)
*   **DHCP Relay Agent:** Farklı bir alt ağdaki (Subnet) istemcilerin Layer 2 Broadcast tipindeki `DHCPDISCOVER` paketlerini yakalayıp, Unicast paket olarak merkezdeki DHCP sunucusuna ileten yönlendirici (Router) veya L3 Switch bileşenidir.

### C. HTTP, HTTPS ve TLS Kavramları

*   **HTTP (Hypertext Transfer Protocol):** Web üzerindeki istemci ve sunucuların haberleşmesini sağlayan durumsuz (stateless), metin tabanlı uygulama katmanı protokolüdür (TCP Port 80).
*   **HTTP Metotları:**
    *   `GET`: Kaynak okuma (Safe ve Idempotent).
    *   `POST`: Yeni kaynak oluşturma (Non-idempotent).
    *   `PUT`: Kaynağı tamamen güncelleme veya yoksa oluşturma (Idempotent).
    *   `PATCH`: Kaynağı kısmi güncelleme.
    *   `DELETE`: Kaynağı silme (Idempotent).
    *   `HEAD`: `GET` ile aynı, ancak yanıt gövdesi (Body) dönmez.
*   **HTTP Durum Kodları (Status Codes):**
    *   `1xx`: Bilgilendirme (`100 Continue`).
    *   `2xx`: Başarı (`200 OK`, `201 Created`).
    *   `3xx`: Yönlendirme (`301 Moved Permanently`, `304 Not Modified`).
    *   `4xx`: İstemci Hatası (`400 Bad Request`, `401 Unauthorized`, `403 Forbidden`, `404 Not Found`).
    *   `5xx`: Sunucu Hatası (`500 Internal Server Error`, `502 Bad Gateway`, `503 Service Unavailable`).
*   **Oturum Yönetimi ve Önbellek Başlıkları:**
    *   *Cookie:* Sunucu tarafından belirlenen (`Set-Cookie`), istemcide saklanan ve sonraki isteklerde sunucuya iletilen küçük veri parçalarıdır.
    *   *Session:* Durum bilgisinin sunucu tarafında saklanıp istemciye rastgele bir `Session ID` verilmesi yöntemidir.
    *   *Caching Headers:* `Cache-Control` (önbellek politikası), `ETag` (içerik özet doğrulama).
*   **HTTP Sürümlerinin Evrimi:**
    *   *HTTP/1.1:* Tek TCP bağlantısından sırayla istek atma. HTTP Head-of-Line (HoL) Blocking sorununa sahiptir.
    *   *HTTP/2:* Tek TCP bağlantısı üzerinde ikili (binary) formatta çoklamalı (**Multiplexing**) iletim sağlar. HPACK header sıkıştırması kullanır. TCP seviyesindeki paket kayıplarında TCP HoL Blocking yaşanabilir.
    *   *HTTP/3:* TCP yerine UDP tabanlı **QUIC** protokolünü kullanır. Paket kaybı tıkanıklıklarını çözer, varsayılan olarak entegre TLS 1.3 içerir.
*   **HTTPS ve TLS (Transport Layer Security):** HTTP trafiğinin TLS katmanı ile şifrelenmiş halidir (TCP Port 443).
    *   *Kriptografi:* El sıkışmada Asimetrik (Public/Private Key), veri aktarımında ise hızlı Simetrik (Session Key) şifreleme bir arada kullanılır.
    *   *X.509 Sertifikaları & PKI:* Sunucunun kimliğini doğrulayan dijital belgedir. Public Key Infrastructure (PKI) ve güvenilir Certificate Authority (CA) hiyerarşisine (Chain of Trust) dayanır.
    *   *SNI (Server Name Indication):* İstemcinin TLS el sıkışmasının başında (`ClientHello`) bağlanmak istediği domain adını açık olarak bildirmesidir. Tek bir IP üzerinde birden fazla HTTPS sitesi barındırılmasını sağlar.

---

## 4. Çalışma Mantığı

Uygulama katmanı servislerinin arka plandaki çalışma mantığı, katmanlar arası veri transferine ve durum makinelerine dayanır.

### A. DNS İsim Çözümleme Mantığı (Iterative / Recursive)

```text
[ İstemci (Stub Resolver) ]
            | (1) Recursive Query: app.example.com
            v
[ Recursive Resolver ] ----------------------------------------+
    |                                                          |
    |--- (2) Iterative Query: app.example.com ---> [ Root DNS ]|
    |<-- (3) Referal: Go to .com TLD DNS ---------+            |
    |                                                          |
    |--- (4) Iterative Query: app.example.com ---> [ TLD DNS ] |
    |<-- (5) Referal: Go to example.com NS -------+            |
    |                                                          |
    |--- (6) Iterative Query: app.example.com ---> [ Authoritative DNS ]
    |<-- (7) Answer: 192.0.2.10 (TTL: 3600) -------+
            |
            v (8) Final Answer: 192.0.2.10
[ İstemci ]
```

1. İstemci, işletim sistemi seviyesindeki Stub Resolver aracılığıyla konfigüre edilmiş Recursive Resolver'a (`8.8.8.8`) özyinelemeli sorgu gönderir.
2. Recursive Resolver önbelleğini kontrol eder. Kayıt yoksa Root DNS sunucularına yinelemeli sorgu fırlatır.
3. Root DNS, `.com` TLD sunucularının adresini döner.
4. Resolver, TLD sunucusuna başvurur; TLD sunucusu `example.com` domaininin yetkili isim sunucularını (NS) gösterir.
5. Resolver, Yetkili İsim Sunucusuna (Authoritative DNS) başvurur.
6. Yetkili sunucu, ilgili kaydın IP adresini ve TTL değerini döner.
7. Resolver yanıtı önbelleğine kaydeder ve istemciye iletir.

### B. DHCP DORA Süreci Mantığı

```text
İstemci (Client)                                            DHCP Sunucusu
   |                                                             |
   |--- (1) DHCPDISCOVER (Broadcast: 255.255.255.255, UDP 67) -->|
   |<-- (2) DHCPOFFER    (Unicast/Broadcast, IP + Options) ----|
   |--- (3) DHCPREQUEST  (Broadcast: "Teklifi Kabul Ettim") ---->|
   |<-- (4) DHCPACK      (Unicast/Broadcast, Kira Onayı) --------|
```

1.  **Discover:** Ağa henüz katılan cihaz IP'si olmadığı için IP paketi kaynak adresi `0.0.0.0`, hedef adresi `255.255.255.255` olacak şekilde Layer 2/3 Broadcast yayını yapar.
2.  **Offer:** İsteği duyan DHCP sunucusu, havuzdan uygun bir IP'yi rezerve eder ve `DHCPOFFER` paketi ile istemciye önerir.
3.  **Request:** İstemci, birden fazla DHCP sunucusu arasından gelen tekliflerden birini seçtiğini ağa Broadcast yayınla bildirir (`DHCPREQUEST`).
4.  **Acknowledge:** İlgili DHCP sunucusu kiralama parametrelerini onaylar (`DHCPACK`) ve kiralama sayacı başlar.

---

## 5. Mimari ve Bileşenler

Uygulama katmanı servislerinin istemci, sunucu, yerel ağ ve genel internet bileşenleri arasındaki mimari ilişkisi aşağıda gösterilmiştir:

```text
+-----------------------------------------------------------------------+
| İSTEMCİ AĞI (Local Subnet: 192.168.1.0/24)                             |
|                                                                       |
|  [ İstemci Cihaz ]                                                     |
|   (Stub Resolver)                                                     |
|         |                                                             |
|         |-- (UDP 67/68) --> [ DHCP Relay Agent / Router ]             |
|         |                         |                                   |
+---------|-------------------------|-----------------------------------+
          |                         | (Unicast UDP 67)
          | (DNS UDP 53)            v
          v                 +-------------------------------+
+-----------------------+   | KURUMSAL MERKEZ SERVISLERI    |
| Recursive DNS Server  |   |                               |
| (Local Caching DNS)   |   |  [ Central DHCP Server ]      |
+-----------------------+   +-------------------------------+
          |
          | (EDNS0 / TCP 53 / DoT / DoH)
          v
+-----------------------------------------------------------------------+
| DIŞ AĞ / İNTERNET (WAN)                                               |
|                                                                       |
|  [ Root / TLD DNS ] ---> [ Authoritative DNS ]                        |
|                                 | (A / AAAA Kaydı)                    |
|                                 v                                     |
|  [ İstemci Browser ] === (TLS 1.3 Handshake - Port 443) ==> [ Web App ]|
|                                                                       |
+-----------------------------------------------------------------------+
```

### Bileşen Görev ve Sorumluluk Tablosu

| Bileşen | Control / Data Plane | Görevi |
| :--- | :--- | :--- |
| **Stub Resolver** | Control Plane | İşletim sisteminde istemci adına DNS sorgusu başlatır. |
| **Recursive Resolver** | Data / Control Plane | Önbellekleme yapar, internet üzerinde yinelemeli DNS dolaşımını yönetir. |
| **Authoritative DNS** | Control Plane | Bir domain bölgesinin orijinal kaynak kayıtlarını barındırır. |
| **DHCP Server** | Control Plane | İstemcilere IP havuzundan dinamik adres ve konfigürasyon seçeneği dağıtır. |
| **DHCP Relay** | Data Plane | L2 Broadcast DHCP paketlerini L3 Unicast paketlere dönüştürerek yönlendirir. |
| **Web Server / Reverse Proxy**| Data Plane | HTTP/HTTPS isteklerini işler, TLS el sıkışmasını sonlandırır (TLS Termination). |

---

## 6. Adım Adım Gerçek Teknik Akış

Bir son kullanıcının web tarayıcısına `https://app.example.com` yazdığı senaryoda gerçekleşen teknik akış sırasıyla şu şekildedir:

1.  **Ağa Katılım ve IP Alma (DHCP):**
    *   İstemci ağ arayüzünü (Interface) kaldırır.
    *   `DHCPDISCOVER` paketi fırlatılır.
    *   Ağdaki DHCP sunucusu `DHCPOFFER` ile `192.168.1.100` IP'sini, Gateway (`192.168.1.1`) ve DNS sunucu (`192.168.1.2`) adreslerini sunar.
    *   İstemci `DHCPREQUEST` atar ve `DHCPACK` alarak IP konfigürasyonunu tamamlar.
2.  **Domain İsim Çözümleme (DNS):**
    *   Tarayıcı `app.example.com` için yerel işletim sistemi önbelleğine bakar. Bulamazsa Stub Resolver üzerinden `192.168.1.2` DNS sunucusuna UDP 53 portundan sorgu gönderir.
    *   DNS sunucusu özyinelemeli sorguları tamamlar ve yetkili sunucudan `198.51.100.50` A kaydı cevabını alır. İstemciye iletir.
3.  **Taşıma Katmanı Bağlantısı (TCP):**
    *   İstemci, `198.51.100.50` IP adresinin 443 numaralı portuna `SYN` paketi atar. `SYN-ACK` ve `ACK` paketleri ile TCP 3-Way Handshake tamamlanır.
4.  **Güvenlik Katmanı Kurulumu (TLS 1.3 Handshake):**
    *   İstemci `ClientHello` paketi gönderir (Desteklenen Cipher'lar, Key Share ve **SNI: app.example.com**).
    *   Sunucu `ServerHello` atar, X.509 Sertifikasını ve kendi Key Share parametrelerini sunar.
    *   İstemci sertifika zincirini (Chain of Trust) ve OCSP/CRL ile iptal durumunu doğrular. Simetrik anahtar (Session Key) iki tarafta da üretilir.
5.  **Uygulama Katmanı Veri Transferi (HTTP/2):**
    *   İstemci, şifrelenmiş TLS kanalı üzerinden HTTP/2 `GET /` isteği atar.
    *   Sunucu `200 OK` yanıt kodunu ve HTML/JSON içeriğini döndürür.

---

## 7. Gerçek Dünya Kullanım Senaryoları

### Senaryo 1: Çok Lokasyonlu Şirket Ağında Split-Horizon DNS ve DHCP Relay
*   **Problem:** Şirket çalışanlarının iç ağdayken `erp.sirket.com` sunucusuna yerel bant genişliğinden (`10.0.5.50`), dış ağdayken (evden/internet) ise güvenlik duvarı kamu IP'sinden (`198.51.100.10`) ulaşması gerekmektedir. Ayrıca farklı şubelerdeki istemcilerin merkez DHCP'den IP alması istenmektedir.
*   **Yaklaşım:**
    *   Şube router'larına `DHCP Relay Agent` kurulur.
    *   İç DNS sunucusunda `sirket.com` için yerel kayıtlar (`10.0.5.50`), dış DNS (Cloudflare/Route53) üzerinde kamu adresi tanımlanarak **Split-Horizon DNS** mimarisi oluşturulur.
*   **Fayda & Karmaşıklık:** İç trafik internete çıkmadan doğrudan yerel ağda çözülür, bant genişliği korunur. DNS kayıtlarının iki farklı bölgede senkron tutulması operasyonel yük getirir.

### Senaryo 2: Yüksek Trafikli Web Uygulamasında TLS Termination ve HTTP/2
*   **Problem:** Saniyede 50.000 istek alan bir e-ticaret platformunda TLS şifre çözme işlemleri uygulama sunucularında yüksek CPU tüketimine neden olmaktadır.
*   **Yaklaşım:** Uygulama sunucularının önüne Nginx / HAProxy tabanlı yük dengeleyici (Load Balancer) yerleştirilir. HTTPS trafiği yük dengeleyicide sonlandırılır (TLS Termination). Yük dengeleyici ile arka plandaki mikroservisler arasında hafif HTTP/1.1 veya gRPC (HTTP/2) trafiği kullanılır.
*   **Fayda & Karmaşıklık:** Sunucu CPU yükü azalır, sertifika yönetimi tek noktaya indirilir. Yük dengeleyici ile uygulama sunucuları arasındaki iç ağ güvenliği (mTLS) ekstra konfigürasyon gerektirebilir.

---

## 8. Güvenlik Perspektifi

### A. DNS Saldırıları ve Savunma Kontrolleri

#### 1. DNS Spoofing ve Cache Poisoning
*   **Teknik Sorun:** Recursive Resolver önbelleğine sahte A/AAAA kayıtlarının enjekte edilmesi.
*   **Çalışma Mantığı:** Saldırgan, resolver bir yetkili sunucuya sorgu attığında, yanıt gelmeden önce sahte IP adresi içeren UDP yanıt paketlerini fırlatır. DNS Transaction ID ve UDP kaynak portu eşleşirse resolver sahte yanıtı doğrular ve önbelleğine alır.
*   **Ön Koşul:** Tahmin edilebilir UDP portları ve Transaction ID dizilimi.
*   **Olası Etki:** Confidentiality ve Integrity ihlali. Kullanıcılar kimlik avı (Phishing) sitelerine yönlendirilir.
*   **Tespit:** DNS loglarında ani IP değişiklikleri, yüksek DNS sorgu hataları ve `RRSIG` doğrulama başarısızlıkları.
*   **Savunma:** Source Port Randomization, **DNSSEC** kullanımı.
*   **Sınırlama:** DNSSEC alan adı sahibi tarafından imzalanmamışsa koruma sağlamaz.

#### 2. DNS Amplification (DDoS)
*   **Teknik Sorun:** Açık DNS çözücülerin (Open Resolvers) yansıma ve büyütme saldırılarında kullanılması.
*   **Çalışma Mantığı:** Saldırgan, kaynak IP adresini kurbanın IP'si olarak değiştirip (IP Spoofing) açık DNS sunucularına büyük metin içeren `EDNS0` veya `ANY` sorguları gönderir. 60 Baytlık sorgu kurbana 4000 Baytlık yanıt olarak döner.
*   **Olası Etki:** Availability ihlali. Kurbanın ağ bant genişliği doygunluğa ulaşır.
*   **Savunma:** **Response Rate Limiting (RRL)**, Open Resolver özelliğini kapatmak, BCP 38 (Source IP Filtering).

#### 3. DNS Tunneling
*   **Teknik Sorun:** Veri sızdırma (Exfiltration) veya Komuta Kontrol (C2) trafiğinin DNS sorguları içine gizlenmesi.
*   **Çalışma Mantığı:** Zararlı yazılım veriyi Base64 ile şifreleyerek `v3r1-s1zm4.attacker.com` şeklinde subdomain sorgusu olarak atar. Saldırganın yetkili DNS sunucusu veriyi toplar.
*   **Tespit:** DNS loglarında anormal uzunlukta subdomain sorguları, yüksek Entropy ve aşırı sayıda `TXT` sorgusu.
*   **Savunma:** DNS Inspection, DNS Sinkholing ve Anomali Tabanlı NIDS/SIEM kuralları.

---

### B. DHCP Saldırıları ve Savunma Kontrolleri

#### 1. DHCP Starvation
*   **Teknik Sorun:** DHCP sunucusundaki kullanılabilir IP havuzunun tüketilmesi.
*   **Çalışma Mantığı:** Saldırgan sahte MAC adresleri üreterek (MAC Spoofing) binlerce `DHCPDISCOVER` isteği atar. Sunucu tüm havuzu kiralar.
*   **Olası Etki:** Availability ihlali. Meşru cihazlar IP alamaz.

#### 2. Rogue DHCP Server
*   **Teknik Sorun:** Ağa yetkisiz bir DHCP sunucusu bağlanarak istemcilere hatalı ağ ayarları dağıtması.
*   **Çalışma Mantığı:** Saldırgan kendi cihazını DHCP sunucusu yapar. Meşru istekçilere daha hızlı yanıt vererek Default Gateway olarak kendi IP'sini atar ve MitM konumu elde eder.
*   **Savunma:** Layer 2 Switch üzerinde **DHCP Snooping** aktif edilir. Switch portları `Trusted` (Güvenli) ve `Untrusted` (Güvenmeyen) olarak ayrılır. Untrusted portlardan gelen sunucu tipi DHCP yanıtları (`DHCPOFFER`, `DHCPACK`) switch tarafından engellenir.

---

### C. Web ve Uygulama Katmanı Güvenliği

*   **Cookie Güvenlik Bayrakları:**
    *   `Secure`: Çerezin yalnızca HTTPS kanalı üzerinden iletilmesini zorunlu kılar.
    *   `HttpOnly`: Client-side JavaScript (`document.cookie`) erişimini engelleyerek XSS ile çerez çalınmasını önler.
    *   `SameSite`: `Strict` veya `Lax` ayarlanarak siteler arası isteklerde çerez iletimi kısıtlanır ve CSRF engellenir.
*   **Host-Header Attacks:** İstemcinin gönderdiği `Host` başlığının doğrulanmaması durumunda sunucunun sahte şifre sıfırlama bağlantıları üretmesi veya Web Cache Poisoning oluşmasıdır. Nginx/Apache konfigürasyonunda tanımlanmayan domain istekleri için `default_server` bloğu ile `444` veya `400` yanıtı dönülmelidir.
*   **TLS Configuration Hardening:** TLS 1.0 ve TLS 1.1 ile zayıf şifreleme paketleri (3DES, RC4, CBC modu) kapatılmalıdır. Yalnızca **TLS 1.2 ve TLS 1.3** aktif edilmeli, `Strict-Transport-Security` (HSTS) başlığı zorunlu kılınmalıdır.

---

## 9. Hardening ve Best Practices

| Kontrol / Yapılandırma | Neden Önemli? | Uygulama Alanı | Sınırlama / Trade-off |
| :--- | :--- | :--- | :--- |
| **DNSSEC Aktivasyonu** | DNS yanıtlarının sahteliğini ve değiştirilmediğini doğrular. | Authoritative ve Recursive DNS | CPU yükünü hafif artırır; düzgün yapılandırılmazsa Zone erişilemez olur. |
| **DHCP Snooping** | Sahte DHCP sunucularını ve IP havuzu tüketimini engeller. | Layer 2 Access Switch'ler | Switch üzerinde donanımsal destek ve doğru port konfigürasyonu gerektirir. |
| **HSTS (HTTP Strict Transport Security)** | Tarayıcıları sadece HTTPS kullanmaya zorlar, SSL Stripping'i önler. | Web Server / Reverse Proxy | Yanlış yapılandırmada HTTP servislerine erişim kalıcı olarak kesilebilir. |
| **DoT / DoH Kullanımı** | DNS sorgularının yoldaki dinleyiciler tarafından görülmesini engeller. | İstemci ve Recursive Resolver | Ağ seviyesinde içerik filtrelemeyi ve DNS tabanlı güvenlik duvarlarını zorlaştırır. |
| **Otomatik Sertifika Yenileme** | Sertifika süresi dolması kaynaklı hizmet kesintilerini önler. | Cloud / K8s Cert-Manager | ACME challenge için yetkilendirme ve dış ağ erişim bağımlılığı oluşturur. |

---

## 10. Avantajlar, Dezavantajlar ve Trade-off’lar

### A. HTTP Protocol Evrimi Karşılaştırması

| Özellik | HTTP/1.1 | HTTP/2 | HTTP/3 |
| :--- | :--- | :--- | :--- |
| **Alt Taşıma Protokolü** | TCP | TCP | UDP (QUIC) |
| **Veri Biçimi** | Metin (Text) | İkili (Binary) | İkili (Binary) |
| **Multiplexing** | Yok (Pipelining kısıtlı) | Var (Tek TCP bağlantısında) | Var (QUIC Akışlarında) |
| **Head-of-Line Blocking** | Hem HTTP hem TCP seviyesinde | Sadece TCP seviyesinde | **Tamamen Çözüldü** |
| **TLS Entegrasyonu** | Opsiyonel (HTTPS) | Pratikte Zorunlu (h2) | **Protokole Doğal Entegre** |
| **Bağlantı Kurulum Gecikmesi**| High (TCP + TLS ayrı) | Medium (TCP + TLS) | **Low (0-RTT / 1-RTT Handshake)**|

### B. DNS TTL Seçim Trade-off'u
*   **Yüksek TTL (Örn: 86400 sn / 24 Saat):** Önbellek başarım oranını artırır, DNS sunucu yükünü ve gecikmeyi düşürür. Ancak sunucu IP adresi değiştirildiğinde kullanıcıların yeni adresi öğrenmesi 24 saat sürer.
*   **Düşük TTL (Örn: 60 sn):** Dinamik IP değişikliklerinin ve Failover mekanizmalarının anında yayılmasını sağlar. Ancak DNS sunucularına sürekli sorgu gitmesine neden olur ve gecikmeyi artırır.

---

## 11. Yaygın Yanlış Anlamalar

### Yanlış Anlama 1
*   **Yanlış düşünce:** DNSSEC, kullanıcı ile DNS sunucusu arasındaki DNS trafiğini şifreleyerek gizlilik sağlar.
*   **Neden yanlış?:** DNSSEC trafiği şifrelemez. Yalnızca kriptografik imzalar kullanarak verinin yetkili sunucudan geldiğini (Otantiklik) ve yolda değiştirilmediğini (Bütünlük) doğrular.
*   **Doğru teknik yaklaşım:** Gizlilik (Encryption) için **DoT (DNS over TLS)** veya **DoH (DNS over HTTPS)** kullanılmalıdır.

### Yanlış Anlama 2
*   **Yanlış düşünce:** HTTP/2 kullanıldığında Head-of-Line (HoL) Blocking problemi tamamen ortadan kalkar.
*   **Neden yanlış?:** HTTP/2 uygulama katmanındaki HTTP HoL blocking sorununu çözer; ancak alt katmanda tek bir TCP bağlantısı kullandığı için, TCP seviyesinde bir paket kaybolduğunda tüm akışlar o paket tekrar iletilene kadar bekler (TCP HoL Blocking).
*   **Doğru teknik yaklaşım:** Bu sorunun tam çözümü UDP tabanlı QUIC kullanan **HTTP/3** protokolüdür.

### Yanlış Anlama 3
*   **Yanlış düşünce:** DHCP sunucusu bir cihaza IP adresi atadığında o IP adresi artık kalıcı olarak o cihaza aittir.
*   **Neden yanlış?:** DHCP bir adres satışı değil, **kiralama (Lease)** mekanizmasıdır. Süre dolduğunda ve yenilenmediğinde IP adresi havuza geri döner.
*   **Doğru teknik yaklaşım:** Sabit IP gerektiren cihazlar için ya cihazda Static IP konfigüre edilmeli ya da DHCP üzerinde **MAC Reservation** yapılmalıdır.

---

## 12. Troubleshooting ve Validation

### Sistematik Sorun Giderme Metodolojisi
1. **Belirti:** "Uygulamaya erişilemiyor" şikayeti.
2. **Katman İzolasyonu:**
   * L3/L4 Erişilebilirliği (`ping`, `nc`, `traceroute`)
   * İsim Çözümleme (`dig`, `resolvectl`)
   * TLS El Sıkışması (`openssl s_client`)
   * Uygulama Yanıtı (`curl`)

---

### Doğrulanmış Komut Satırı Araçları

#### 1. `dig` (Domain Information Groper)
DNS sorgularını doğrulamak için kullanılır.

```bash
# Temel A kaydı sorgusu
dig example.com A

# Sadece IP yanıtını almak (Scripting için)
dig example.com +short

# Sorguyu belirli bir DNS sunucusuna fırlatmak (Örn: 8.8.8.8)
dig @8.8.8.8 example.com A

# Root sunuculardan başlayarak çözümleme adımlarını izlemek
dig example.com +trace
```
*   *Çıktı Analizi:* `NOERROR` durumu sorgunun başarılı olduğunu, `NXDOMAIN` domainin bulunmadığını, `ANSWER SECTION` gelen veriyi gösterir.

#### 2. `resolvectl`
Modern systemd tabanlı Linux dağıtımlarında yerel çözücü durumunu kontrol eder.

```bash
# Aktif arayüzlerdeki DNS sunucularını ve modları listeleme
resolvectl status

# Yerel sistem üzerinden bir ismi çözme
resolvectl query app.example.com
```

#### 3. `curl`
HTTP/HTTPS isteklerini ve yanıt başlıklarını test eder.

```bash
# Sadece HTTP yanıt başlıklarını almak
curl -I [https://example.com](https://example.com)

# Detaylı HTTP/TLS el sıkışma adımlarını incelemek (Verbose)
curl -v [https://example.com](https://example.com)

# DNS çözünürlüğünü bypass ederek doğrudan belirli bir IP'ye HTTPS isteği atmak
curl -v --resolve example.com:443:198.51.100.50 [https://example.com](https://example.com)
```

#### 4. `openssl`
TLS bağlantısını ve sertifika geçerliliğini doğrular.

```bash
# Sunucu ile TLS el sıkışması başlatmak ve sertifika zincirini görmek
openssl s_client -connect example.com:443 -servername example.com

# Yerel sertifika dosyasının detaylarını ve son kullanma tarihini okumak
openssl x509 -in cert.pem -text -noout
```

---

## 13. Uygulamalı Doğrulama

Bu konu için ayrı bir laboratuvar ortamı kurulumu zorunlu değildir. İlgili kavramlar, uygulama katmanı servislerinin konfigürasyonunu ve sorun giderme adımlarını içeren milestone projesi kapsamında doğrulanacaktır.

---

## 14. Production Ortamı Perspektifi

### A. Container ve Kubernetes Ortamında CoreDNS
Kubernetes mimarisinde her Pod'un içinde `/etc/resolv.conf` dosyası bulunur ve cluster içerisindeki **CoreDNS** servisini (`ClusterIP`) gösterir.

```text
[ Pod ] ---> DNS Sorgusu: "auth-service"
                 |
                 v
     [ CoreDNS Pod (10.96.0.10) ]
                 |
                 v (Çözümler)
   "auth-service.default.svc.cluster.local -> 10.100.200.15"
```

*   **Service Discovery:** Kubernetes'te bir Service oluşturulduğunda CoreDNS otomatik olarak `<service-adi>.<namespace>.svc.cluster.local` formatında bir FQDN kaydı oluşturur.
*   **ndots Performans Riski:** `/etc/resolv.conf` içindeki `options ndots:5` ayarı nedeniyle, noktası 5'ten az olan harici sorgularda (Örn: `api.stripe.com`) tarayıcı/kod önce cluster içi arama alanlarını (`default.svc.cluster.local`) dener. Bu durum gereksiz CoreDNS yüküne ve milisaniyelik gecikmelere (latency) yol açar. Production'da dış adres sorgularının sonuna nokta koymak (`api.stripe.com.`) veya `NodeLocal DNSCache` kullanmak best practice'dir.

### B. Cloud DNS (AWS Route 53, GCP Cloud DNS)
*   **Global Anycast Ağı:** Cloud DNS servisleri dünya genelinde tek bir Anycast IP adresi üzerinden duyurulur. Sorgular fiziksel olarak istemciye en yakın veri merkezinde yanıtlanır.
*   **Gelişmiş Yönlendirme:** Latency-Based, Geo-DNS ve Sağlık Kontrolü (Health Check) tabanlı otomasyonel Failover yönlendirmeleri desteklenir.

---

## 15. Diğer Teknolojilerle İlişkisi

*   **Linux System Administration:** `/etc/resolv.conf`, `/etc/hosts`, `systemd-resolved` ve `/etc/dhcp/dhcpd.conf` dosyalarının yönetimi.
*   **Network Engineering:** L2/L3 Switch'lerde DHCP Snooping, Router'larda DHCP Relay ve Firewall üzerinde Port 53/80/443 erişim kuralları.
*   **Kubernetes & Cloud Native:** CoreDNS yapılandırması, Ingress Controller (Nginx/Envoy) üzerinde TLS termination, Cert-Manager ile ACME (Let's Encrypt) sertifika otomasyonu.
*   **DevSecOps & SRE:** TLS sertifika ömürlerinin izlenmesi (Prometheus exporter'lar), HTTP 5xx hata oranları takibi ve DNS tabanlı C2 engelleme.

---

## 16. Junior Seviyede Bilinmesi Gerekenler

Junior bir sistem, ağ veya DevOps mühendisinin bu konuda sahip olması gereken temel yetkinlikler:

*   [ ] Bir domain adının `A`, `AAAA`, `CNAME`, `MX`, `TXT` ve `NS` kayıtlarının ne anlama geldiğini açıklayabilmek.
*   [ ] DHCP DORA (Discover, Offer, Request, Acknowledge) adımlarını sırasıyla sayabilmek ve ne işe yaradığını bilmek.
*   [ ] HTTP ve HTTPS arasındaki farkı, TLS el sıkışmasının temel amacını tarif edebilmek.
*   [ ] `dig` komutu ile A kaydı sorgulayıp dönen TTL ve yanıt durumunu (`NOERROR`, `NXDOMAIN`) yorumlayabilmek.
*   [ ] `curl -I` veya `curl -v` kullanarak sunucudan dönen HTTP durum kodunu (200, 301, 404, 500 vb.) doğrulayabilmek.
*   [ ] Host-Header ve Cookie güvenlik parametrelerinin (`Secure`, `HttpOnly`) temel amacını kavramış olmak.

---

## 17. Kısa Özet

1. Uygulama Katmanı, OSI'nin 7. katmanı olup istemci-sunucu mimarisinde istek-yanıt modeliyle çalışır.
2. DNS, alan adlarını IP adreslerine dönüştüren hiyerarşik ve dağıtık bir veritabanıdır (UDP/TCP Port 53).
3. DNS sorguları istemciden özyinelemeli (Recursive), sunucular arasında yinelemeli (Iterative) olarak ilerler.
4. DHCP, cihazlara otomatik IP ve ağ ayarlarını DORA süreciyle kiralar (UDP Port 67/68).
5. Layer 2 switch'lerde `DHCP Snooping` kullanılarak sahte DHCP sunucuları (Rogue DHCP) engellenir.
6. HTTP durumsuz (stateless) bir protokoldür; durum bilgisi Cookies ve Sessions ile yönetilir.
7. HTTP/2 Multiplexing sunarken, HTTP/3 QUIC (UDP) kullanarak TCP Head-of-Line blocking sorununu çözer.
8. HTTPS, HTTP trafiğinin TLS katmanı ile şifrelenmiş halidir (TCP Port 443).
9. TLS El Sıkışması kimlik doğrulamada Asimetrik, veri iletiminde Simetrik şifreleme kullanır.
10. SNI, tek bir IP adresi üzerinde birden fazla HTTPS sitesinin barındırılmasına olanak tanır.
11. DNSSEC bütünlük sağlar; trafiği şifrelemek için DoT (Port 853) veya DoH (Port 443) gereklidir.
12. Troubleshooting işlemlerinde `dig` (DNS), `curl` (HTTP) ve `openssl` (TLS) en temel doğrulama araçlarıdır.

---

## 18. Kaynaklar

*   **RFC 1034 & 1035** — *Domain Names - Concepts and Facilities / Implementation and Specification* (IETF Standardı)  
    *Doğrulanan Konu:* DNS mimarisi, mesaj formatı ve kaynak kayıt tipleri.
*   **RFC 2131** — *Dynamic Host Configuration Protocol* (IETF Standardı)  
    *Doğrulanan Konu:* DHCP paket yapısı, DORA adımları ve durum geçişleri.
*   **RFC 2616 & 7540 & 9000** — *HTTP/1.1, HTTP/2 (Multiplexing) ve HTTP/3 (QUIC)* (IETF Standartları)  
    *Doğrulanan Konu:* HTTP evrimi, durum kodları ve QUIC protokol yapısı.
*   **RFC 8446** — *The Transport Layer Security (TLS) Protocol Version 1.3* (IETF Standardı)  
    *Doğrulanan Konu:* TLS 1.3 El sıkışma adımları ve simetrik/asimetrik anahtar değişimi.
*   **Kubernetes Official Documentation** — *DNS for Services and Pods / CoreDNS Integration*  
    *Doğrulanan Konu:* CoreDNS servis keşfi ve `<service>.<namespace>.svc.cluster.local` adlandırma standardı.
*   **Linux Manual Pages (`dig(1)`, `curl(1)`, `openssl(1)`)**  
    *Doğrulanan Konu:* Komut satırı parametreleri ve kullanım bayrakları.
