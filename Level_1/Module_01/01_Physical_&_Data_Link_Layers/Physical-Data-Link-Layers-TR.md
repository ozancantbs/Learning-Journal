# Fiziksel ve Veri Bağı Katmanları: Platformlar İçin Gelişmiş Ağ Altyapısı

Bu doküman, "LEVEL 1 — FOUNDATIONS OF SYSTEMS & SECURITY" seviyesi altındaki "MODULE 01 — ADVANCED NETWORKING FOR PLATFORMS" modülünün ilk bölümünü oluşturmaktadır. Dokümanın amacı; ağ altyapısının temelini oluşturan Fiziksel (Layer 1) ve Veri Bağı (Layer 2) katmanlarının teorik çalışma mantığını, işletim sistemi düzeyindeki sanallaştırma bileşenlerini, siber güvenlik risklerini ve modern bulut/konteyner platformlarındaki yansımalarını derinlemesine incelemektir. Bu çalışmayı tamamlayan bir sistem, SRE veya platform mühendisi; veri paketlerinin fiziksel ortamdan başlayarak yerel ağda nasıl iletildiğini, donanımsal ve mantıksal arızaların nasıl tespit edileceğini ve bu katmanlardaki saldırılara karşı nasıl savunma mekanizmaları kurulacağını uygulamalı olarak öğrenecektir.

---

## 1. Genel Bakış

Bilgisayar ağları, heterojen sistemlerin birbirleriyle güvenilir, ölçeklenebilir ve standartlaştırılmış protokoller çerçevesinde veri alışverişi yapmasını sağlayan donanımsal ve yazılımsal altyapıların bütünüdür. Bir verinin bir uçtan diğer uca iletilmesi sürecinde, karmaşıklığı azaltmak ve sistemleri modüler hale getirmek için katmanlı mimariler tasarlanmıştır. 

Ağ iletişiminin en altında yer alan iki katman, verinin somut dünyaya açılan kapısıdır:
* **Fiziksel Katman (Physical Layer - Layer 1):** Sayısal verilerin (bitlerin) elektrik, ışık veya radyo dalgaları gibi fiziksel sinyallere dönüştürülerek iletim ortamı üzerinden aktarılmasından sorumludur.
* **Veri Bağı Katman (Data Link Layer - Layer 2):** Doğrudan bağlı iki cihaz arasında güvenilir veri aktarımını sağlar. Fiziksel katmandaki ham bit akışını anlamlı bloklara (çerçevelere/framelere) dönüştürür, hata denetimi yapar ve yerel ağ (LAN) içindeki fiziksel adreslemeyi yönetir.

Bu iki katmanın koordinasyonu, üst katmanlardaki (IP yönlendirmeleri, TCP bağlantıları ve HTTP gibi uygulama protokolleri) soyutlama katmanlarının kesintisiz çalışması için zorunlu temel taşıdır.

---

## 2. Neden Önemlidir?

Platform mühendisliği, DevOps, sistem yönetimi ve siber güvenlik disiplinlerinde en karmaşık sorunların önemli bir kısmı ağ altyapısının en alt katmanlarında gizlidir. Üst katman yazılımları (örneğin Kubernetes podları veya mikroservisler) her ne kadar sanal ağlar kullansalar da, nihayetinde fiziksel donanımın sınırlılıklarına tabidirler.

* **Sistem ve Platform Mühendisliği (SRE/DevOps):** Yüksek trafikli Kubernetes cluster'larında veri aktarım hızının düşmesi, donanımsal arayüz kuyruklarının (overruns/drops) dolması veya yanlış MTU yapılandırmaları nedeniyle servislerin beklenmedik şekilde donması (Black Hole) doğrudan L1/L2 katmanlarında teşhis edilir.
* **Siber Güvenlik (Defensive Security / SOC):** Yerel ağa sızmış bir saldırganın trafiği dinlemesi (sniffing), kimlik doğrulama mekanizmalarını atlatması ve ağ anahtarlarını (switch) devre dışı bırakması gibi en kritik ilk aşama saldırıları (MAC Spoofing, CAM Exhaustion) tamamen Layer 2 seviyesinde gerçekleşir.
* **Bulut Altyapısı (Cloud Engineering):** AWS VPC gibi modern yazılım tanımlı ağlarda (SDN), klasik Layer 2 protokollerinin (örneğin ARP veya Broadcast) nasıl simüle edildiğini veya kısıtlandığını bilmek, yüksek performanslı mimariler tasarlamak için zorunludur.

---

## 3. Temel Kavramlar ve Terminoloji

Ağ iletişiminin temellerini kavramak için standartları belirleyen kurumları, katmanlı referans modellerini ve bu katmanlardaki veri birimlerini iyi analiz etmek gerekir.

### Standartlar ve Protokoller
Ağ dünyasında kaosun önlenmesi için protokoller ve standartlar kullanılır. **Protokol**, iki veya daha fazla cihazın iletişim kurmak için kabul ettiği kurallar kümesidir. **Standartlar** ise bu protokollerin donanım ve yazılım üreticilerinden bağımsız olarak çalışabilmesini garanti eden resmî belgelerdir. En kritik standart belirleyici kurumlar şunlardır:
* **IEEE (Institute of Electrical and Electronics Engineers):** Özellikle Layer 1 ve Layer 2 standartlarını (802.3 Ethernet, 802.11 Wi-Fi, 802.1X Ağ Erişimi) belirler.
* **IETF (Internet Engineering Task Force):** TCP/IP protokol ailesini ve internet omurgasını düzenleyen RFC (Request for Comments) standartlarını yayınlar.

### Referans Modelleri: OSI ve TCP/IP

Ağ mimarileri iki temel model ile açıklanır: **OSI (Open Systems Interconnection)** referans modeli ve günümüz internetinin fiili standardı olan **TCP/IP** modeli.



[Image of the OSI Model vs TCP/IP Model]


| OSI Katmanı | TCP/IP Katmanı | Sorumluluk | Örnek Protokol/Teknoloji |
| :--- | :--- | :--- | :--- |
| **7. Application (Uygulama)** | <br>yüzeysel olarak **Application** | Kullanıcı/Uygulama arayüzü | HTTP, DNS, SSH, gRPC |
| **6. Presentation (Sunum)** | Veri biçimlendirme, şifreleme | TLS, JSON, XML |
| **5. Session (Oturum)** | Bağlantı oturumu yönetimi | NetBIOS, Sockets |
| **4. Transport (Taşıma)** | **Transport** | Uçtan uca güvenilir/hızlı veri akışı | TCP, UDP, SCTP |
| **3. Network (Ağ)** | **Internet** | Mantıksal adresleme ve yönlendirme (routing) | IP, ICMP, OSPF, BGP |
| **2. Data Link (Veri Bağı)** | <br>yüzeysel olarak **Network Access** | Fiziksel adresleme ve doğrudan komşu iletişimi | Ethernet, ARP, VLAN (802.1Q) |
| **1. Physical (Fiziksel)** | Ham bitlerin fiziksel ortamdan aktarılması | Kablolar (CAT6, Fiber), SFP+, RJ45 |

### Kapsülleme (Encapsulation) ve Kapsül Açma (Decapsulation)
Veri, gönderici tarafında üst katmanlardan alt katmanlara doğru indikçe her katman kendi kontrol bilgilerini (başlık/header ve bazen kuyruk/trailer) verinin önüne ekler. Bu işleme **Kapsülleme (Encapsulation)** denir. Alıcı tarafta ise veri alt katmanlardan üst katmanlara çıkarken her katman ilgili kontrol başlığını söker; bu işleme de **Kapsül Açma (Decapsulation)** adı verilir.

```text
[Uygulama Verisi]                                       (Katman 7 - Veri)
     |
     v
[TCP Başlığı][Uygulama Verisi]                         (Katman 4 - Segment)
     |
     v
[IP Başlığı][TCP Başlığı][Uygulama Verisi]             (Katman 3 - Paket)
     |
     v
[Ethernet Başlığı][IP Başlığı][TCP Başlığı][Veri][Trailer] (Katman 2 - Çerçeve/Frame)
     |
     v
10101101000110101110010110110001...                    (Katman 1 - Bit)
```

### Protokol Veri Birimleri (Protocol Data Units - PDU)
Verinin her bir katmandaki özelleşmiş haline farklı isimler verilir:
* **Data (Veri):** Üst katmanlarda (Uygulama, Sunum, Oturum) işlenen ham veridir.
* **Segment:** Transport katmanında (TCP) verinin parçalara ayrılmış ve sıra numaraları eklenmiş halidir. UDP kullanıldığında buna **Datagram** denir.
* **Packet (Paket):** Network katmanında (IP) kaynak ve hedef IP adreslerinin eklendiği birimdir.
* **Frame (Çerçeve):** Data Link katmanında kaynak ve hedef fiziksel adreslerinin (MAC) ve hata kontrol kodunun (FCS) eklendiği birimdir.
* **Bit:** Physical katmanda kablodan akan 1 ve 0 değerleridir.

---

## 4. Çalışma Mantığı

Katman 2'nin temel çalışma mantığı, yerel ağ içerisindeki cihazların birbirlerini benzersiz fiziksel adreslerle tanımlaması ve verinin bu adresler üzerinden hatasız olarak yönlendirilmesidir.

### Ethernet Çerçeve Yapısı (Ethernet Frame Structure)
Fiziksel kablodan gelen bitlerin bir Ethernet çerçevesi (frame) oluşturabilmesi için IEEE 802.3 standardında belirtilen yapıya sahip olması gerekir:

```text
+----------+--------+---------+---------+---------+-----------------+----------+
| Preamble |  SFD   | Dest MAC| Src MAC |Type/Len |     Payload     |   FCS    |
| (7 Byte) |(1 Byte)| (6 Byte)| (6 Byte)| (2 Byte)| (46-1500 Byte)  | (4 Byte) |
+----------+--------+---------+---------+---------+-----------------+----------+
```

* **Preamble & SFD (Start Frame Delimiter):** Alıcı ağ kartının gelen sinyalle senkronize olmasını sağlar ve çerçevenin başladığını bildirir.
* **Destination MAC (Hedef Fiziksel Adres):** Çerçevenin hangi ağ kartına gittiğini belirtir.
* **Source MAC (Kaynak Fiziksel Adres):** Çerçeveyi gönderen ağ kartının adresidir.
* **Type / Length:** Üst katmandaki protokolün türünü (Örn: `0x0800` için IPv4, `0x0806` için ARP) belirtir.
* **Payload (Veri Alanı):** Üst katmandan gelen paketi (Örn: IP Paketi) içerir. Boyutu standart olarak en az 46, en fazla 1500 byte olmalıdır.
* **FCS (Frame Check Sequence):** CRC (Cyclic Redundancy Check) algoritması kullanılarak çerçevenin yolda bozulup bozulmadığını denetleyen hata kontrol alanıdır. Alıcı cihaz hesabı tekrar yapar; eğer değer eşleşmiyorsa çerçeveyi sessizce düşürür (drop).

### Maksimum İletim Birimi (MTU - Maximum Transmission Unit)
Bir fiziksel arayüzden tek seferde gönderilebilecek en büyük IP paketi boyutuna **MTU** denir. 
* **Standart MTU:** Ethernet ağları için varsayılan limit **1500 byte**'tır.
* **Jumbo Frames:** Modern veri merkezlerinde ağ kartları ve switch'ler arasındaki paket işleme yükünü (CPU interrupt sayılarını azaltmak amacıyla) düşürmek için MTU değeri **9000** veya **9001 byte** gibi yüksek değerlere çıkarılabilir. Buna Jumbo Frame denir.
* **MTU Mismatch (MTU Uyuşmazlığı):** İletişim yolundaki cihazların veya arayüzlerin MTU değerlerinin farklı olması durumunda yaşanır. Örneğin, 9000 MTU ile gönderilen bir paket, yol üzerindeki 1500 MTU'luk bir arayüze geldiğinde eğer IP başlığındaki **DF (Don't Fragment - Parçalama)** bayrağı set edilmişse paket düşürülür. Bu durum, bağlantı kurulmasına rağmen büyük veri transferi sırasında bağlantının donmasına sebep olan meşru bir karadelik bağlantısı (Black Hole Connection) yaratır.

---

## 5. Mimari ve Bileşenler

### Fiziksel Katman Sorumlulukları ve Donanımları
Fiziksel katman veriyi değil, veriyi taşıyan sinyali yönetir.
* **Sinyal Tipleri:** Sayısal (Digital - kare dalgalar) ve Analog (Sürekli sinüs dalgaları) sinyaller kullanılır.
* **Ağ Başarımı Metrikleri:**
    * **Bandwidth (Bant Genişliği):** Bir iletim ortamının teorik olarak saniyede taşıyabileceği maksimum veri kapasitesidir.
    * **Throughput (Verim):** Belirli bir zaman diliminde fiziksel ortamdan başarıyla aktarılan gerçek veri miktarıdır.
    * **Latency (Gecikme):** Bir verinin kaynaktan hedefe ulaşması için geçen süredir.
    * **Jitter (Gecikme Dalgalanması):** Gecikme sürelerindeki tutarsızlıktır (Özellikle gerçek zamanlı ses ve video akışlarını bozar).
    * **Packet Loss (Paket Kaybı):** Gönderilen paketlerin hedefe ulaşmadan yolda kaybolması veya bozulmasıdır.
* **İletim Ortamları (Transmission Media):**
    * *Bakır (Copper):* CAT5e, CAT6 kablolar. Elektriksel sinyaller kullanır. Elektromanyetik parazitlerden (EMI) etkilenir. Mesafe sınırı genellikle 100 metredir.
    * *Fiber Optik:* Işık sinyalleri kullanır. EMI'dan etkilenmez, çok yüksek hızlarda ve kilometrelerce mesafeye veri taşıyabilir (Single-mode ve Multi-mode tipleri bulunur).
    * *Kablosuz (Wireless):* Radyo frekanslarını (RF) kullanır. Çevresel engellerden yüksek oranda etkilenir.

### Ağ Kartları (NIC) ve Hız/Dubleks El Sıkışması
Ağ kartı (Network Interface Card - NIC), bilgisayarı ağ ortamına bağlayan fiziksel donanımdır. Her ağ kartı benzersiz bir donanımsal adrese sahiptir; buna **MAC Adresi (Media Access Control)** denir. MAC adresleri 48 bit (6 Byte) uzunluğundadır ve hexadecimal (onaltılık) formatta yazılır (Örn: `06:62:d3:8c:29:27`). İlk 3 byte üreticiyi (OUI - Organizationally Unique Identifier), son 3 byte ise benzersiz cihaz kimliğini belirtir.

* **Duplex Mode (Çift Yönlülük):**
    * *Half-Duplex:* Cihaz aynı anda sadece veri gönderebilir veya alabilir. (Örn: Hub cihazları, eski kablosuz ağlar). Aynı anda gönderim olursa çarpışma (collision) yaşanır.
    * *Full-Duplex:* Cihaz aynı anda hem veri gönderebilir hem de alabilir. Modern Ethernet ağları full-duplex çalışır.
* **Hız ve Dubleks Anlaşması (Speed and Duplex Negotiation):** Karşılıklı bağlı iki port (örneğin sunucu ağ kartı ile switch portu) destekledikleri en yüksek hız ve en iyi dubleks modunda otomatik olarak el sıkışır (Auto-Negotiation). Bir tarafın manuel, diğer tarafın otomatik yapılandırılması durumunda **Duplex Mismatch (Dubleks Uyuşmazlığı)** hatası oluşur. Bu hata, ağda ciddi paket kayıplarına ve performans düşüşlerine yol açar.

### Yayılım Türleri (Transmission Types)
* **Unicast (Tekli Gönderim):** Bire bir iletişimdir. Trafik yalnızca ilgili kaynak ve hedef MAC adresleri arasında akar.
* **Broadcast (Yoruma Açık Gönderim):** Bire herkes iletişimidir. Hedef MAC adresi her zaman `FF:FF:FF:FF:FF:FF` değerindedir. Aynı yerel ağdaki (Broadcast Domain) tüm cihazlar bu paketi almak ve işlemek zorundadır (Örn: ARP istekleri).
* **Multicast (Grup Gönderimi):** Bire çok iletişimidir. Belirli bir gruba üye olan cihazlar bu trafiği alır (Örn: Dinamik yönlendirme protokolleri, video yayınları).

### Çarpışma ve Yayın Alanları (Collision & Broadcast Domains)
* **Collision Domain (Çarpışma Alanı):** Aynı fiziksel ortamı paylaşan ve aynı anda veri gönderdiklerinde paketleri çarpışabilecek cihazların oluşturduğu alandır. Klasik **Hub** cihazları tek bir büyük collision domain oluşturur. **Switch** cihazlarında ise her bir port bağımsız bir collision domain'dir; full-duplex iletişimde çarpışma tamamen engellenir.
* **Broadcast Domain (Yayın Alanı):** Gönderilen bir broadcast paketinin ulaştığı tüm cihazların sınırıdır. Switch'ler broadcast paketlerini tüm portlarından dışarı ilettiği için tüm switch portları aynı broadcast domain içindedir. Broadcast domain sınırını yalnızca Layer 3 cihazlar (Yönlendirici - Router) veya Layer 2'de oluşturulan sanal ağlar (**VLAN - Virtual LAN**) çizebilir.

### Switch Çalışma Mantığı ve MAC Adres Tablosu (CAM)
Bir ağ anahtarı (Switch), Layer 2 düzeyinde çalışan akıllı bir cihazdır. Gelen çerçeveleri tüm portlara göndermek yerine, yalnızca hedef cihazın bağlı olduğu porta iletir.

```text
       +------------------------------------+
       |         Switch (Katman 2)          |
       |  CAM Tablosu:                      |
       |  Port 1 -> AA:BB:CC:DD:EE:11       |
       |  Port 2 -> AA:BB:CC:DD:EE:22       |
       +------------------------------------+
         /                                \
   [Port 1]                             [Port 2]
     /                                      \
[Sunucu A]                             [Sunucu B]
MAC: AA:BB:CC:DD:EE:11                 MAC: AA:BB:CC:DD:EE:22
```

1.  **MAC Öğrenme (MAC Learning):** Switch, bir portundan içeri giren her çerçevenin "Kaynak MAC" adresini okur. Bu adresi ve girdiği fiziksel port numarasını **CAM (Content Addressable Memory)** tablosuna kaydeder.
2.  **İletme ve Filtreleme (Forwarding & Filtering):** Gelen çerçevenin "Hedef MAC" adresini CAM tablosunda arar. Eğer adresi bulursa, çerçeveyi sadece o porttan dışarı gönderir (forwarding). Diğer portlara göndermez (filtering).
3.  **Bilinmeyen Tekli Gönderim Yayılımı (Unknown Unicast Flooding):** Eğer hedef MAC adresi CAM tablosunda kayıtlı değilse, switch bu cihazın nerede olduğunu bulmak için çerçeveyi gelen port hariç tüm portlara kopyalayarak gönderir (Flooding). Hedef cihaz cevap verdiğinde, onun da MAC adresini öğrenip tabloya kaydeder ve sonraki iletişimleri unicast olarak sürdürür.

### Linux Ağ Sanallaştırması (Linux Network Virtualization)
Modern bulut ve konteyner altyapılarında Linux çekirdeği (kernel) fiziksel switch ve kabloları yazılımsal olarak simüle eder:



* **Linux Bridge (Yazılımsal Switch):** Çekirdek içinde çalışan tam özellikli bir Layer 2 switch'tir. Kendisine bağlanan sanal veya fiziksel ağ kartları arasında MAC tablosuna bakarak veri iletimi yapar. Docker'ın varsayılan `docker0` köprüsü bir Linux bridge örneğidir.
* **Virtual Ethernet (veth) Pairs (Sanal Ethernet Çifti):** İki ucu olan sanal bir ağ kablosudur. Bir ucuna (`vethA`) yazılan her bit, anında diğer ucundan (`vethB`) çıkar. Konteynerleri (Network Namespace) ana sisteme (Host) veya bir Linux Bridge'e bağlamak için kullanılır.
* **TUN ve TAP Arayüzleri:** * *TAP (Network Tap):* Sanal makine (KVM/QEMU) sanallaştırmasında kullanılır. Kullanıcı alanındaki (user space) sanal makinenin ürettiği ham Layer 2 Ethernet çerçevelerini doğrudan kernel seviyesindeki köprüye aktarır.
    * *TUN (Network Tunnel):* Layer 3 düzeyinde çalışır. Genellikle VPN yazılımlarında şifrelenmiş IP paketlerini tünellemek için kullanılır.

---

## 6. Adım Adım Gerçek Teknik Akış

Bir Linux sunucusu üzerindeki bir konteynerden, fiziksel ağda bulunan bir veritabanı sunucusuna veri paketi gönderildiğinde gerçekleşen uçtan uca Layer 2 akışı şu şekildedir:

### Başlangıç Durumu
* **Kaynak Konteyner (Namespace):** IP: `172.17.0.2`, MAC: `02:42:ac:11:00:02`
* **Host Linux Bridge (`docker0`):** IP: `172.17.0.1`, MAC: `02:42:f8:8a:12:34`
* **Hedef Fiziksel Veritabanı:** IP: `172.17.0.100` (Fiziksel MAC adresi henüz bilinmiyor)

### Teknik Adımlar

```text
Konteyner (Namespace)               Host (Bridge)                Fiziksel Switch
+-------------------+             +---------------+             +---------------+
|  IP: 172.17.0.2   |             |               |             |               |
|  [veth_container] |======veth===|   [docker0]   |===fiziksel==|   [L2 Switch] |
+-------------------+             +---------------+             +---------------+
```

1.  **ARP Çözümlemesi Başlangıcı:** Konteyner işletim sistemi, `172.17.0.100` IP adresine paket göndermek ister. Önce kendi yerel ARP (Address Resolution Protocol) tablosuna bakar. Hedef IP'ye ait MAC adresi tabloda yoksa, bir **ARP İsteği (ARP Request)** çerçevesi oluşturur.
2.  **ARP İstek Kapsüllemesi:** ARP istek paketi oluşturulur: *"Who has IP 172.17.0.100? Tell 172.17.0.2"*. Bu paket, hedef MAC adresi `FF:FF:FF:FF:FF:FF` (Broadcast) olacak şekilde kapsüllenir.
3.  **Veth Geçişi:** Çerçeve, konteyner içindeki sanal ağ arayüzünden gönderilir. Sanal kablonun (veth pair) doğası gereği, paket anında host tarafındaki `docker0` Linux köprüsüne (bridge) ulaşır.
4.  **Sanal Switch İşlemi (Linux Bridge):** `docker0` köprüsü, gelen paketin kaynak MAC adresini (`02:42:ac:11:00:02`) kendi CAM tablosuna kaydeder. Hedef adres broadcast (`FF:FF:FF:FF:FF:FF`) olduğu için, bu çerçeveyi kendisine bağlı olan tüm diğer sanal arayüzlere ve fiziksel ağ kartına kopyalayarak gönderir (Flooding).
5.  **Fiziksel Ağa Çıkış:** Çerçeve, host sunucusunun fiziksel ağ kartı (Örn: `ens5`) üzerinden elektrik veya ışık sinyaline dönüştürülerek fiziksel switch'e aktarılır.
6.  **Fiziksel Switch Yayılımı:** Fiziksel switch de benzer şekilde broadcast çerçevesini gelen port hariç tüm portlarına iletir.
7.  **Hedefin Cevap Vermesi (ARP Reply):** Yerel ağdaki `172.17.0.100` IP adresine sahip fiziksel veritabanı sunucusu broadcast isteğini alır. Kendi IP'siyle eşleştiği için bir **ARP Yanıtı (ARP Reply)** hazırlar: *"I am 172.17.0.100, my MAC is 00:50:56:bf:01:aa"*. Bu yanıtı, yalnızca istekte bulunan kaynak konteynerin MAC adresine (`02:42:ac:11:00:02`) hedefleyecek şekilde **Unicast** olarak kapsüller.
8.  **Yolun Tamamlanması:** Unicast çerçeve fiziksel switch ve ardından host üzerindeki `docker0` köprüsü tarafından CAM tablolarına bakılarak doğrudan ilgili konteynerin bağlı olduğu porta yönlendirilir.
9.  **İletişimin Başlaması:** Konteyner, veritabanı sunucusunun MAC adresini kendi ARP tablosuna kaydeder. Artık asıl veri paketleri (TCP/IP) doğrudan unicast çerçeveler halinde bu MAC adresleri üzerinden iletilmeye başlar.

---

## 7. Gerçek Dünya Kullanım Senaryoları

### Senaryo 1: Kubernetes Düğümleri Arasında Yüksek Başarımlı Veri Depolama Senaryosu
* **Ortam:** Bir şirketin kendi veri merkezinde (on-premise) çalışan 10 düğümlü (node) Kubernetes cluster'ı. Node'lar arasında yoğun miktarda veri replikasyonu yapan bir distributed depolama sistemi (Ceph/Rook) bulunuyor.
* **Sorun:** Varsayılan 1500 MTU ayarıyla, büyük veri transferlerinde CPU kullanımı tavan yapıyor ve saniyedeki paket (PPS - Packets Per Second) miktarı çok yüksek olduğu için switch portlarında gecikmeler yaşanıyor.
* **Çözüm:** Fiziksel ağ anahtarları (switch'ler) ve tüm Kubernetes node'larının fiziksel ağ kartları (NIC) üzerinde **Jumbo Frames (9000 MTU)** aktif hale getirilir.
* **Elde Edilen Fayda:** Tek bir çerçevede taşınan veri miktarı 6 katına çıktığı için, işletim sistemlerinin paketleri işlemek için harcadığı CPU interrupt oranları düşer. Ağ throughput performansı %20 ile %30 arasında artış gösterir.
* **Oluşan Yeni Karmaşıklık:** Ağdaki tüm switch'lerin, firewall'ların ve arayüzlerin istisnasız olarak 9000 MTU destekleyecek şekilde yapılandırılması zorunludur. Unutulan tek bir 1500 MTU'luk geçiş noktası, ağda paketlerin parçalanamadan düşmesine (Silent Drop / Black Hole) yol açar.

### Senaryo 2: Kurumsal Ağlarda Yetkisiz Cihaz Erişiminin Engellenmesi
* **Ortam:** Bir belediyenin fiziksel ofisleri ve açık alanlardaki Ethernet prizleri.
* **Sorun:** Binaya giren herhangi bir ziyaretçi veya saldırgan, duvardaki bir Ethernet kablosunu kendi laptopuna takarak doğrudan yerel ağa (iç sunuculara) erişim sağlayabiliyor.
* **Çözüm:** Tüm kenar switch'lerde **IEEE 802.1X** protokolü devreye alınır. Cihazlar ağa bağlandığında bir merkezi RADIUS (Örn: FreeRADIUS) veya NAC sunucusu üzerinden sertifika (EAP-TLS) veya kullanıcı adı şifre doğrulaması yapmak zorunda bırakılır.
* **Elde Edilen Fayda:** Kimlik doğrulaması başarılı olmayan hiçbir cihazın switch portu aktif edilmez; bu sayede fiziksel sızma riskleri tamamen ortadan kalkar.
* **Güvenlik Gereksinimi:** Şirket envanterindeki tüm cihazlara önceden dijital sertifikaların (Grup Politikaları - GPO veya MDM aracılığıyla) dağıtılmış olması gerekir.

---

## 8. Güvenlik Perspektifi

Layer 2 protokolleri tasarlanırken güvenlik ve kimlik doğrulama göz önünde bulundurulmamıştır. Yerel ağa erişimi olan bir saldırgan için Layer 2, tüm trafiği manipüle edebileceği en zayıf halkadır.

### A. MAC Spoofing (MAC Adresi Sahtekarlığı)
* **Teknik Sorun:** Ağ kartlarının fiziksel MAC adresleri, işletim sistemi seviyesinde yazılımsal olarak saniyeler içinde değiştirilebilir.
* **Çalışma Mantığı:** Saldırgan, hedef ağdaki yetkilendirilmiş bir sunucunun veya ağ geçidinin (Gateway) MAC adresini kopyalayarak kendi ağ kartına tanımlar.
* **Ön Koşullar:** Saldırganın hedef ağ ile aynı fiziksel veya sanal Layer 2 yayın alanında (VLAN) bulunması.
* **Olası Etki:** * *Confidentiality (Gizlilik):* Yetkili cihaza gitmesi gereken unicast paketler saldırganın bilgisayarına yönlenebilir.
    * *Integrity (Bütünlük):* Saldırgan araya girerek paketleri değiştirebilir.
* **Tespit:** Aynı MAC adresinin switch üzerinde birden fazla fiziksel portta hızlıca yer değiştirdiğine dair logların (MAC Flapping) takibi.
* **Savunma:** Switch portlarında **Port Security** kullanılarak her porta belirli MAC adreslerinin kilitlenmesi veya **802.1X/NAC** ile sıkı kimlik doğrulaması yapılması.

### B. MAC Flooding & CAM Table Exhaustion (CAM Tablosu Taşırma)
* **Teknik Sorun:** Ağ anahtarlarındaki (Switch) CAM tablolarının donanımsal kapasitesi (giriş sınırı) limitlidir.
* **Çalışma Mantığı:** Saldırgan, uydurma kaynak MAC adreslerine sahip on binlerce sahte Ethernet çerçevesini saniyeler içinde switch'e gönderir. Switch'in CAM tablosu tamamen bu sahte kayıtlarla dolar ve meşru cihazlara yer kalmaz.
* **Fail-Open Davranışı:** CAM tablosunda yer kalmayan bir switch, ağın tamamen çökmesini önlemek için **"Fail-Open"** moduna geçer. Bu modda switch, gelen her yeni tekli gönderim (unicast) çerçevesini hedef portunu bilmediği için **"Unknown Unicast Flooding"** yaparak gelen port hariç tüm portlara kopyalar. Switch, işlevsel olarak güvensiz bir **Hub** cihazına dönüşür.
* **Saldırganın Amacı:** Switch tüm trafiği her porta kopyaladığı için, saldırgan kendi portuna gelen diğer meşru sunucuların özel trafiklerini dinleyebilir (Sniffing/Eavesdropping).



* **Olası Etki:** Yerel ağdaki tüm sunucuların trafiğinin ifşa olması (Confidentiality kaybı).
* **Tespit:** Switch üzerindeki CAM tablosu doluluk oranının izlenmesi, anormal derecede yüksek MAC adresi sayısı ve paket analizinde aşırı unknown unicast trafiğinin tespiti.
* **Savunma:** Switch portlarında Port Security limitlerinin (`maximum MAC`) tanımlanması.
* **Sınırlamalar:** Port Security sadece kenar (access) portlarda uygulanabilir; switchler arası geçiş (trunk) portlarında uygulanamaz.

### Layer 2 Güvenlik Kontrolleri ve Farkları

#### Port Security Modları
Port Security ihlali durumunda (Violation) switch portunun alacağı aksiyonlar:
1.  **Protect:** Sınır aşıldığında, yetkisiz yeni MAC adresinden gelen çerçeveleri sessizce düşürür (drop). Log üretmez, portu kapatmaz.
2.  **Restrict:** Yetkisiz çerçeveleri düşürür, bir SNMP uyarısı (trap) gönderir, Syslog üretir ve hata sayacını artırır. Port açık kalmaya devam eder.
3.  **Shutdown:** En güvenli moddur. Limit aşıldığı an portu tamamen kapatır ve durumu **`err-disable`** moduna çeker. Sistem yöneticisi müdahale edene kadar porttan hiçbir trafik geçemez.

#### IEEE 802.1X ve NAC Bileşenleri
802.1X mimarisinde üç temel rol bulunur:
* **Supplicant (İstemci):** Ağa bağlanmak isteyen kullanıcı bilgisayarı veya sunucu üzerindeki yazılım bileşeni.
* **Authenticator (Doğrulayıcı):** İstemci ile kimlik doğrulama sunucusu arasında köprü olan switch veya kablosuz erişim noktası (AP).
* **Authentication Server (Doğrulama Sunucusu):** Genellikle bir RADIUS sunucusudur. Kimlik bilgilerini (sertifika vb.) doğrular ve switch'e portu açma talimatı gönderir.

---

## 9. Hardening ve Best Practices

Fiziksel ve Veri Bağı katmanlarında güvenliği ve kararlılığı artırmak için uygulanması gereken temel sıkılaştırma adımları şunlardır:

| Kontrol / Önlem | Neden Önemli? | Uygulama Alanı | Sınırlama / Trade-off |
| :--- | :--- | :--- | :--- |
| **Kullanılmayan Portların Kapatılması** | Saldırganların boş prizleri ağa sızmak için kullanmasını engeller. | Tüm Switchler (Kenar Portlar) | Operasyonel yük; yeni bir cihaz bağlanırken portun manuel açılması gerekir. |
| **Port Security Yapılandırması** | MAC Flooding ve MAC Spoofing saldırılarını donanımsal olarak engeller. | Kenar (Access) Portlar | Dinamik IP alan ve sık değişen kullanıcı ortamlarında yönetim zorluğu. |
| **DHCP Snooping** | Saldırganların ağa sahte DHCP sunucusu kurup IP dağıtmasını (Man-in-the-Middle) engeller. | Layer 2 Switchler | Yanlış yapılandırılırsa meşru DHCP paketleri de düşebilir. |
| **Dynamic ARP Inspection (DAI)** | ARP Spoofing/Poisoning saldırılarını engellemek için ARP paketlerini doğrular. | Layer 2 Switchler | Switch üzerinde ek CPU yükü oluşturur. |
| **BPDU Guard** | Switch portlarına yetkisiz switch takılarak ağ topolojisinin (Spanning Tree) bozulmasını engeller. | Kenar (Access) Portlar | Portların yanlışlıkla `err-disable` moduna girmesine yol açabilir. |

---

## 10. Avantajlar, Dezavantajlar ve Trade-off'lar

Ağ altyapı kararlarında tek bir doğru yoktur; her teknik seçim beraberinde bir maliyet ve karmaşıklık getirir.

### Jumbo Frames vs Standart MTU (1500)
* *Jumbo Frames (9000+ MTU) Avantajı:* Yüksek throughput, düşük CPU interrupt kullanımı, büyük veri yedeklemelerinde hız artışı.
* *Dezavantajı/Trade-off:* Ağdaki tüm cihazların (switch, router, NIC) bunu desteklemesi zorunluluğu. Tek bir uyumsuz cihazda paketlerin sessizce kaybolması. Gecikmeye duyarlı (real-time) trafik için paketlerin kuyrukta bekleme süresinin (transmission delay) artması.

### Fiziksel İzolasyon vs Sanal İzolasyon (VLAN)
* *Fiziksel İzolasyon Avantajı:* Maksimum güvenlik. Kablolar ve switch'ler tamamen ayrıdır; bir ağdaki sızıntı diğerini asla etkilemez.
* *Dezavantajı/Trade-off:* Çok yüksek donanım ve kablolama maliyeti, esneklik kaybı.
* *VLAN İzolasyonu:* Aynı fiziksel donanım üzerinde mantıksal ağlar oluşturarak maliyeti düşürür ancak yapılandırma hataları (VLAN Hopping vb.) güvenlik riskleri barındırır.

---

## 11. Yaygın Yanlış Anlamalar

### Yanlış Anlama 1
* **Yanlış Düşünce:** "Jumbo Frames (9000 MTU) aktif edildiğinde genel internet hızım ve web sitelerine erişimim hızlanır."
* **Neden Yanlış?** Genel internet omurgasındaki router'lar ve internet servis sağlayıcıları (ISP) varsayılan olarak **1500 MTU** sınırıyla çalışır. Sen yerelde ne kadar büyük paket üretirsen üret, internet çıkışında bu paketler ya parçalanacaktır (fragmentation) ya da düşürülecektir.
* **Doğru Teknik Yaklaşım:** Jumbo Frames yalnızca uçtan uca kontrol edebildiğin, yüksek hızlı yerel ağlarda (LAN/VPC) ve veri merkezi içi depolama ağlarında kullanılmalıdır.

### Yanlış Anlama 2
* **Yanlış Düşünce:** "AWS, Azure gibi genel bulut ortamlarındaki sanal makinelerimde (EC2 vb.) klasik ARP Spoofing saldırıları yaparak diğer sunucuların trafiğini dinleyebilirim."
* **Neden Yanlış?** Genel bulut sağlayıcılarının altyapısı fiziksel switch'lerle değil, gelişmiş **Yazılım Tanımlı Ağ (SDN)** mimarileriyle yönetilir. Bulut ağlarında gerçek anlamda Layer 2 broadcast trafiği bulunmaz. Sanal makinelerden çıkan ARP istekleri tüm ağa yayılmaz; hypervisor seviyesinde yakalanarak SDN kontrol düzlemi tarafından doğrudan unicast olarak yanıtlanır.
* **Doğru Teknik Yaklaşım:** Bulut ortamlarında Layer 2 tabanlı sızma ve sniffing yöntemleri çalışmaz. Güvenlik sınırları tamamen Layer 3/4 seviyesindeki Security Group ve IAM politikaları ile çizilir.

---

## 12. Troubleshooting ve Validation

Bir sistem veya platform mühendisinin ağ sorunlarını çözerken izlemesi gereken metodoloji aşağıda özetlenmiştir.

### Sistematik Ağ Sorun Giderme Metodolojisi
1.  **Belirtiyi Netleştir:** Bağlantı tamamen mi kesik, yoksa sadece büyük veri aktarımlarında mı donuyor?
2.  **Etkilenen Kapsamı Belirle:** Tek bir sunucu mu etkileniyor, yoksa aynı switch'e bağlı tüm sunucular mı?
3.  **İlgili Katmanı Tespit Et:** Önce fiziksel bağlantı (L1) ve komşuluk durumunu (L2), ardından IP/Routing (L3) katmanını kontrol et.
4.  **Sayaçları ve Logları Topla:** İşletim sistemindeki ağ kartı istatistiklerini (`ip -s link`, `ethtool`) incele.

### Kritik Linux Ağ Teşhis Komutları

#### 1. Donanımsal Durum ve Hız Bilgisi (`ethtool`)
Ağ kartının fiziksel durumunu, bağlantı hızını ve çift yönlülük (duplex) modunu doğrulamak için kullanılır.

```bash
sudo ethtool ens5
```
* **Önemli Çıktı Alanları:**
    * `Speed: 10000Mb/s`: Ağ kartının anlık el sıkıştığı hız. Beklenenden düşükse kablo veya port arızası olabilir.
    * `Duplex: Full`: Tam çift yönlü çalıştığını gösterir. `Half` olması durumunda performans kaybı yaşanır.
    * `Link detected: yes`: Fiziksel olarak sinyal (elektrik/ışık) alındığını doğrular. `no` ise kablo takılı değildir veya karşı port kapalıdır.

#### 2. Ağ Arayüz Sayaçları ve Hata Analizi (`ip`)
Arayüz seviyesindeki paket kayıplarını ve donanımsal arızaları tespit etmek için kullanılır.

```bash
ip -s link show ens5
```
* **Önemli Parametreler:** `-s` (statistics) detaylı istatistikleri basar.
* **Çıktı Yorumlama:**
    * `RX errors / TX errors`: Fiziksel kablo arızalarına veya bozuk SFP modüllerine işaret eder. Normal şartlarda sıfır olmalıdır.
    * `dropped`: İşletim sisteminin paket işleme kuyruklarının (ring buffer) yetersiz kalması sonucu paketlerin silindiğini gösterir.
    * `overruns`: Ağ kartının donanımsal FIFO tampon belleğinin dolduğunu ve CPU'nun kesmeleri (interrupt) yetiştiremediğini gösterir. CPU darboğazına işaret eder.

#### 3. Komşu ARP Tablosunun İzlenmesi (`ip neigh`)
Layer 2 düzeyinde komşu cihazlarla sağlıklı el sıkışılıp sıkışılmadığını doğrular.

```bash
ip neigh show
```
* **Çıktı Durumları:**
    * `172.31.32.1 dev ens5 lladdr 06:11:22:33:44:55 REACHABLE`: İletişimin sağlıklı olduğunu ve MAC adresinin doğru çözümlendiğini gösterir.
    * `172.31.32.1 dev ens5 FAILED` veya `INCOMPLETE`: Ağ geçidinden ARP isteğimize cevap gelmediğini, yani Layer 2 seviyesinde bir iletişim kopukluğu olduğunu gösterir.

---

## 13. Uygulamalı Doğrulama

Bu laboratuvar çalışmasında, bir Linux sunucusunda (bu örnekte AWS EC2) MTU sınırlarını, "Don't Fragment" (Parçalama) bayrağını ve sistemin limit aşımında ürettiği yerel hataları canlı olarak test edeceğiz.

### Amaç
Fiziksel ağ arayüzünün MTU sınırını yerel olarak doğrulamak, parçalanamayan paket sınırlarını gözlemlemek ve ağ geçidi ile olan Jumbo Frame iletişimini test etmek.

### Ortam ve Gereksinimler
* İşletim Sistemi: Ubuntu (veya herhangi bir Linux dağıtımı)
* Ağ Kartı: `ens5` (9001 MTU - Jumbo Frame aktif edilmiş AWS EC2 ağ kartı)
* Ağ Geçidi IP: `172.31.32.1`

### Adımlar ve Analizler

#### Adım 1: Ağ Geçidi (Default Gateway) IP Adresini Keşfetme
İlk olarak sistemimizin dış dünyaya çıkarken kullandığı ilk durak olan sanal yönlendiricinin IP adresini öğreniyoruz:

```bash
ip route show default
```
* *Beklenen Çıktı:* `default via 172.31.32.1 dev ens5 proto dhcp src 172.31.43.154 metric 100`
* *Teknik Neden:* Bu komut yönlendirme tablosunu okuyarak hedef olarak kullanacağımız geçerli IP'yi (`172.31.32.1`) bize söyler.

#### Adım 2: Limit Aşımında Yerel Hata Üretme (Local Error Test)
Arayüzümüzün MTU değeri **9001 byte**'tır. Bu arayüzden gönderilebilecek maksimum veri boyutu (payload) IP ve ICMP başlıkları düşüldükten sonra **8973 byte**'tır (`9001 - 28 = 8973`). 

Sisteme bu sınırın tam **1 byte fazlasını** parçalamadan göndermesini söyleyeceğiz:

```bash
ping -M do -s 8974 192.168.1.100
```
* **Parametreler:**
    * `-M do`: "Don't Fragment" (Parçalama yapma) bayrağını zorunlu kılar.
    * `-s 8974`: Gönderilecek ICMP payload boyutunu (byte) belirtir.
* **Beklenen Sonuç:**
    ```text
    PING 192.168.1.100 (192.168.1.100) 8974(9002) bytes of data.
    ping: local error: message too long, mtu=9001
    ```
* **Doğrulama:** Karşı hedef IP adresi aktif olmasa bile, paket daha ağ kartına inmeden Linux çekirdeği (kernel) boyutu kontrol etmiş ve `9002 byte > 9001 MTU` olduğu için yerel olarak (`local error`) işlemi engellemiştir.

#### Adım 3: Standart Paket ile Ağ Geçidine Ulaşma (1500 MTU)
Standart internet paket boyutu olan 1500 MTU (`1472 byte payload + 28 byte header`) ile ağ geçidine ping gönderiyoruz:

```bash
ping -M do -s 1472 172.31.32.1
```
* **Beklenen Sonuç:**
    ```text
    PING 172.31.32.1 (172.31.32.1) 1472(1500) bytes of data.
    1480 bytes from 172.31.32.1: icmp_seq=1 ttl=64 time=0.090 ms
    ```
* **Doğrulama:** Paket sorunsuzca gitmiş ve ağ geçidinden yanıt dönmüştür. Çıktıda yazan `1480 bytes`, IP başlığı (20 byte) soyulduktan sonra geriye kalan ICMP paketi boyutudur (`1472 veri + 8 ICMP başlığı`).

#### Adım 4: Jumbo Frame Sınırında İletişim (9001 MTU)
Şimdi AWS VPC altyapısının Jumbo Frame desteğini test etmek için tam sınırdaki dev paketi (`8973 byte payload`) gönderiyoruz:

```bash
ping -M do -s 8973 172.31.32.1
```
* **Beklenen Sonuç:**
    ```text
    PING 172.31.32.1 (172.31.32.1) 8973(9001) bytes of data.
    8981 bytes from 172.31.32.1: icmp_seq=1 ttl=64 time=0.095 ms
    ```
* **Doğrulama:** Devasa 9-kilobytelik paket, hiçbir parçalama işlemine maruz kalmadan tek seferde başarıyla ağ geçidine ulaşmış ve aynı hızda (0.095 ms) geri dönmüştür. Bu, AWS VPC ağımızın iç altyapısının kesintisiz Jumbo Frame desteğine sahip olduğunu kanıtlar.

### Temizleme ve Geri Alma
Bu bir aktif ping komutu olduğu için terminali durdurmak için `Ctrl + C` tuş kombinasyonunu kullanmak yeterlidir. Sisteme herhangi bir kalıcı konfigürasyon yazılmadığı için ek bir temizlik adımı gerekmez.

### Güvenlik Notu
Bu testler tamamen yerel ve güvenli ICMP paketleriyle gerçekleştirilmiştir. Herhangi bir dış ağa saldırı veya tarama niteliği taşımaz.

---

## 14. Production Ortamı Perspektifi

Laboratuvarda başarıyla çalışan sistemlerin, production (canlı hizmet) ortamlarında ölçeklenebilirlik, kararlılık ve izlenebilirlik açılarından farklı dinamikleri vardır.

* **Sanal Ağ Sürücü Seçimleri:** Sanallaştırma ortamlarında (KVM/AWS) sunucuların ağ performansını artırmak için standart emüle edilmiş ağ kartları yerine, hypervisor ile doğrudan konuşabilen **virtio** veya AWS'in **ENA (Elastic Network Adapter)** gibi yarı-sanallaştırılmış (para-virtualized) sürücüleri kullanılmalıdır. Bu sürücüler donanım seviyesinde paket işlemeyi optimize eder.
* **Kapasite Planlaması (Capacity Planning):** Production ortamlarında saniyedeki paket miktarı (PPS) ve bant genişliği kullanımı sürekli olarak izlenmelidir. Arayüzlerdeki `dropped` ve `overruns` sayaçları sıfırın üzerine çıktığı an, sistemde darboğaz olduğu anlaşılmalı ve NIC ring buffer boyutları (`ethtool -G`) artırılmalıdır.
* **Hata Toleransı (High Availability - L2):** Kritik sunucularda tek bir fiziksel kablo veya switch arızasında bağlantının kesilmesini önlemek için birden fazla fiziksel ağ kartı mantıksal olarak birleştirilir. Buna Linux dünyasında **NIC Bonding** veya **Teaming** (LACP - Link Aggregation Control Protocol) denir. Bu sayede kablolardan biri kopsa bile iletişim kesintisiz devam eder.

---

## 15. Diğer Teknolojilerle İlişkisi

Fiziksel ve Veri Bağı katmanları, üst seviyedeki modern platform araçlarının doğrudan temelini oluşturur.

* **Docker:** Docker host üzerinde bir konteyner başlattığınızda arka planda bir `veth pair` çifti oluşturulur. Bir ucu konteynere `eth0` olarak verilir, diğer ucu host işletim sistemindeki `docker0` adlı Linux Bridge'e bağlanır. Konteynerler arası iletişim tamamen bu yazılımsal Layer 2 switch üzerinden yürütülür.
* **Kubernetes CNI (Calico, Flannel vb.):** Farklı fiziksel sunuculardaki (node) pod'ların birbiriyle konuşabilmesi için CNI'lar **Overlay Network** teknolojileri kullanır. Örneğin **VXLAN** modu, Layer 2 Ethernet çerçevesini alır ve bunu standart bir Layer 3 UDP paketinin içine gömer (encapsulation). Paket karşı sunucuya ulaştığında UDP paketi soyularak orijinal Layer 2 çerçevesi pod'a teslim edilir.
* **Terraform & AWS Cloud:** Terraform ile bir AWS VPC oluşturduğunuzda, arka plandaki SDN mimarisini API'ler üzerinden yapılandırmış olursunuz. Terraform ile yazdığınız Security Group kuralları, sanal makinenizin ağ kartı (vNIC) seviyesinde çalışan durum korumalı (stateful) firewall kurallarına dönüştürülür.

---

## 16. Junior Seviyede Bilinmesi Gerekenler

Junior bir sistem, DevOps veya siber güvenlik mühendisinin mülakatlarda ve günlük operasyonlarda bu konuda bilmesi gereken en temel yetkinlikler şunlardır:

* **OSI Modelini Ezbere Bilmek Değil, Yaşatmak:** Katmanların isimlerini sırayla saymaktan ziyade; bir web sitesi açılmadığında sorunun kabloda mı (L1), ARP çözümlenmesinde mi (L2), IP yönlendirmesinde mi (L3) yoksa DNS'te mi (L7) olduğunu teşhis edebilmek.
* **MAC ve IP Adresi Farkı:** MAC adresinin yerel ağ içindeki fiziksel kapı numarası olduğunu, IP adresinin ise dünya genelindeki mantıksal adres olduğunu açıklayabilmek.
* **Temel Araçları Kullanabilmek:** Bir sunucunun ağ kartının çalışıp çalışmadığını anlamak için `ip link`, `ip neigh` ve `ethtool` komutlarını çalıştırıp çıktılarını yorumlayabilmek.
* **MTU Hatalarını Tanımak:** Ping atılmasına rağmen büyük veri transferlerinin yarıda kesilmesinin bir MTU uyuşmazlığı hatası (Black Hole) olabileceğini akla getirebilmek.

---

## 17. Kısa Özet

* **Fiziksel Katman (L1),** verinin elektrik, ışık veya radyo sinyalleriyle kablodan bit olarak akmasını yönetir.
* **Veri Bağı Katmanı (L2),** yerel ağda doğrudan komşu olan cihazlar arasında çerçeveler (frame) halinde veri iletimini sağlar.
* **MAC Adresi,** ağ kartına üretim aşamasında basılan ve yerel ağda benzersiz olan 48-bitlik fiziksel kimlik bilgisidir.
* **Switch'ler,** gelen çerçevelerin kaynak MAC adreslerini öğrenerek donanımsal bir **CAM (MAC) tablosu** oluşturur ve trafiği yalnızca ilgili hedef porta iletir.
* **Unknown Unicast Flooding,** switch'in hedef MAC adresini tablosunda bulamadığında paketi tüm portlara kopyalayarak göndermesi işlemidir.
* **MTU,** bir arayüzden gönderilebilecek en büyük IP paketi sınırıdır (Varsayılan Ethernet sınırı 1500 byte'tır).
* **Jumbo Frames,** veri merkezlerinde performansı artırmak için kullanılan 9000/9001 byte boyutundaki büyük çerçevelerdir.
* **Linux Bridge,** Linux çekirdeğinde çalışan yazılımsal bir switch'tir ve konteyner ağlarının temelini oluşturur.
* **MAC Flooding,** sahte MAC adresleriyle switch'in CAM tablosunu doldurarak cihazı tüm trafiği dışarı saçan (Fail-Open) bir Hub'a dönüştürme saldırısıdır.
* **Port Security,** switch portlarına bağlanabilecek maksimum MAC adresi sınırını çizerek siber saldırıları ve yetkisiz erişimleri donanımsal olarak engeller.

---

## 18. Kaynaklar

* **IEEE 802.3 Ethernet Standardı — IEEE**
    * *Doğrulanan Konu:* Ethernet çerçeve yapısı, fiziksel katman standartları ve hız/dubleks el sıkışma mekanizmaları.
* **RFC 894: A Standard for the Transmission of IP Datagrams over Ethernet Networks — IETF**
    * *Doğrulanan Konu:* IP paketlerinin Ethernet çerçeveleri içine nasıl kapsüllendiği ve MTU standart sınırları.
* **Linux Kernel Networking Documentation — Linux Kernel Org**
    * *Doğrulanan Konu:* Linux bridge, virtual ethernet (veth) çiftleri ve TAP/TUN sanal arayüzlerinin kernel seviyesindeki çalışma mimarisi.
* **AWS VPC User Guide: Network Maximum Transmission Unit (MTU) — Amazon Web Services**
    * *Doğrulanan Konu:* AWS altyapısındaki sanal ağ geçitlerinin, ENA sürücülerinin ve Jumbo Frame (9001 MTU) sınırlarının davranış kuralları.
