# 05 — Transport Layer & Traffic Control

Bu doküman, OSI Modelinin 4. katmanı olan Taşıma Katmanı (Transport Layer) mekanizmalarını, IP ağları üzerindeki uçtan uca iletişim ilkelerini, Linux çekirdeğindeki (kernel) ağ yığını yönetimini ve trafik denetimini ele almaktadır. Dokümanın amacı; ağ mühendisliği, sistem yönetimi, DevSecOps ve SRE perspektifinden Taşıma Katmanı'nın çalışma mantığını, güvenlik boyutunu ve operasyonel sorun giderme (troubleshooting) yaklaşımlarını derinlemesine incelemektir.

---

## 1. Genel Bakış

Taşıma Katmanı (Transport Layer), Ağ Katmanının (Network Layer / IP) sunduğu cihazdan cihaza (Host-to-Host) paket taşıma hizmetini, süreçten sürece (Process-to-Process) mantıksal iletişim seviyesine yükselten katmandır. Ağ katmanı verinin doğru IP adresine ulaşmasını sağlarken; taşıma katmanı gelen verinin işletim sistemi üzerindeki hangi uygulamaya veya sürece ait olduğunu belirler ve bu verinin aktarım kalitesini yönetir.

Taşıma Katmanı'nın temel sorumlulukları şunlardır:

* **Süreçten Sürece İletişim (Process-to-Process Communication):** Port numaraları aracılığıyla ağ trafiğini ilgili süreçlerle (processes) eşleştirir.
* **Çoklama ve Ayırma (Multiplexing / Demultiplexing):** Farklı uygulamalardan gelen verileri tek bir ağ akışına toplar (multiplexing) ve gelen paketleri doğru uygulamalara dağıtır (demultiplexing).
* **Hata Denetimi ve Güvenilirlik (Reliability and Ordering):** Veri bütünlüğünü doğrulamak için checksum (kontrol toplamı) hesaplamaları yapar, paketlerin sırasını korur ve kayıp paketleri yeniden iletir.
* **Akış ve Tıkanıklık Denetimi (Flow & Congestion Control):** Alıcının ve ağın kapasitesine göre veri iletim hızını dinamik olarak ayarlar.

Taşıma katmanında iki temel iletişim modeli bulunur:

1. **Bağlantı Yönelimli İletişim (Connection-Oriented Communication):** Veri aktarımı başlamadan önce gönderici ve alıcı arasında mantıksal bir oturum (session) kurulur. Verinin eksiksiz, hatasız ve doğru sırada ulaşması garanti edilir. Ana protokolü **TCP**'dir.
2. **Bağlantısız İletişim (Connectionless Communication):** Herhangi bir ön el sıkışma veya durum takibi yapılmaksızın veriler bağımsız bloklar halinde ağa gönderilir. Sıralama veya teslimat garantisi verilmez. Ana protokolü **UDP**'dir.

---

## 2. Neden Önemlidir?

Taşıma Katmanı, modern bilişim altyapılarında sistem performansı ve güvenliğinin kesişim noktasında yer alır:

* **Sistem Yönetimi ve SRE:** Sunucu üzerindeki bellek kullanımı, açık dosya tanımlayıcıları (File Descriptors) ve kernel kuyruk yönetimi (backlog) doğrudan TCP soketlerinin durumuna bağlıdır. `TIME_WAIT` birikmeleri veya tampon bellek (buffer) yetersizlikleri doğrudan servis kesintilerine yol açar.
* **Ağ Mühendisliği:** Ağ bant genişliğinin verimli kullanılması, RTT (Round-Trip Time) sürelerinin optimizasyonu ve tıkanıklık yönetimi bu katmandaki parametrelerle sağlanır.
* **DevOps ve Platform Engineering:** Mikroservis mimarilerinde servislerin birbiriyle iletişimi (Service-to-Service), Load Balancer yönlendirmeleri ve Service Mesh yapılardaki gecikme (latency) metrikleri TCP akış denetimi parametreleriyle şekillenir.
* **Siber Güvenlik ve Defensive Security:** Güvenlik duvarları (Stateful Firewalls) ve IPS/IDS sistemleri durum takibini (Connection Tracking - `conntrack`) 4. katman seviyesinde yapar. SYN Flood gibi DoS/DDoS saldırıları ve Session Hijacking girişimleri doğrudan taşıma katmanı zafiyetlerini hedef alır.

---

## 3. Temel Kavramlar ve Terminoloji

### 3.1. Portlar ve Türleri

Portlar, işletim sisteminde ağ hizmetlerini sunan veya ağa bağlanan süreçleri tanımlayan 16-bitlik tamsayılardır (0 - 65535). IANA standartlarına göre üç gruba ayrılırlar:

* **Well-Known Ports (Tanınmış Portlar - 0 – 1023):** Sistem düzeyindeki standart servisler için rezerve edilmiştir. POSIX standartlarında bu portları dinlemek (bind etmek) için ayrıcalıklı yetki (root/`CAP_NET_BIND_SERVICE`) gerekir. Örn: SSH (22), DNS (53), HTTP (80), HTTPS (443).
* **Registered Ports (Kayıtlı Portlar - 1024 – 49151):** Belirli uygulamalara veya veritabanı servislerine tahsis edilmiş portlardır. Örn: MySQL (3306), PostgreSQL (5432), Redis (6379).
* **Ephemeral Ports (Geçici / Dinamik Portlar - 49152 – 65535):** İstemci uygulamaların dış sunuculara bağlanırken anlık olarak kullandığı geçici portlardır. Linux sistemlerde varsayılan aralık `/proc/sys/net/ipv4/ip_local_port_range` altında genellikle 32768 – 60999 olarak yapılandırılmıştır.

### 3.2. Soket (Socket) ve Soket Çifti (Socket Pair)

Ağ programlamasında ve kernel seviyesinde iletişim uç noktaları **Soket** mimarisiyle temsil edilir:

    Socket = IP Adresi + Port Numarası

Ağ üzerindeki tekil bir TCP/UDP bağlantısı ise **4-Tuple (Soket Çifti)** ile tanımlanır:

    Socket Pair = (Kaynak IP, Kaynak Port, Hedef IP, Hedef Port)

*Protokol bilgisi de eklendiğinde bu yapı **5-Tuple** olarak adlandırılır.*

### 3.3. Kavram Karşılaştırma Tablosu

| Kavram | Teknik Açıklama | Sistemdeki Rolü | İlişkili Kavramlar |
| :--- | :--- | :--- | :--- |
| **TCP (Transmission Control Protocol)** | Bağlantı yönelimli, güvenilir, sıralı bayt akışı sunan protokol (RFC 793 / RFC 9293). | Hata toleransı olmayan kritik veri transferleri. | Handshake, Sliding Window, Congestion Control |
| **UDP (User Datagram Protocol)** | Bağlantısız, minimalist, düşük gecikmeli veri bloğu (datagram) protokolü (RFC 768). | Hız odaklı, paket kaybı toleransı olan iletişim. | Datagram, Checksum, Stateless |
| **MSS (Maximum Segment Size)** | Bir TCP segmentinin taşıyabileceği maksimum saf uygulama verisi (payload) miktarı (Bayt). | Ağdaki IP/TCP başlık yükü düşülerek paket bölünmesini (fragmentation) engeller. | MTU, IP Header, TCP Header |
| **Byte-Stream Model** | TCP'nin veriyi mesaj sınırları olmadan, kesintisiz bir bayt nehir akışı olarak işlemesi. | Uygulamanın veriyi parça parça veya blok halinde okuyabilmesini sağlar. | Sequence Number, Buffer, Stream |
| **TCP Reset (RST)** | Geçersiz bir pakete karşılık veya acil kapatma durumlarında gönderilen bayrak. | Geçersiz veya yetkisiz bağlantıları anında iptal eder. | TCP Flags, Abort, Firewall |

### 3.4. Paket Başlık Yapıları (TCP ve UDP Headers)

#### UDP Header Yapısı
UDP başlığı sabittir ve sadece **8 Byte (64 bit)** uzunluğundadır:

    0                   16                  31 bit
       +-------------------+-------------------+
       |    Source Port    |  Destination Port |  (4 Byte)
       +-------------------+-------------------+
       |      Length       |     Checksum      |  (4 Byte)
       +-------------------+-------------------+
       |              DATA (Payload)           |
       +---------------------------------------+

#### TCP Header Yapısı
TCP başlığı, opsiyonlar hariç **20 Byte** sabittir. Opsiyon alanlarıyla birlikte **60 Byte**'a kadar çıkabilir:

    0                   16                  31 bit
       +-------------------+-------------------+
       |    Source Port    |  Destination Port |  (4 Byte)
       +---------------------------------------+
       |            Sequence Number            |  (4 Byte)
       +---------------------------------------+
       |         Acknowledgment Number         |  (4 Byte)
       +-------+-------+---+-------------------+
       | Data  |Reser- |F  |    Window Size    |  (4 Byte)
       |Offset |ved    |lags|                  |
       +-------+-------+---+-------------------+
       |     Checksum      |   Urgent Pointer  |  (4 Byte)
       +-------------------+-------------------+
       |        Options (0 - 40 Bytes)         |
       +---------------------------------------+
       |              DATA (Payload)           |
       +---------------------------------------+

#### TCP Flags (Bayraklar)
* **SYN (Synchronize):** Bağlantı başlatma ve başlangıç sıra numaralarını (ISN) senkronize etme bayrağı.
* **ACK (Acknowledgment):** Alınan verilerin veya kontrol paketlerinin doğrulandığını belirten onay bayrağı.
* **FIN (Finish):** Göndericinin iletecek verisi kalmadığını ve bağlantıyı kapatmak istediğini bildiren bayrak.
* **RST (Reset):** Anormal durumlarda bağlantıyı anında ve zorla sonlandıran bayrak.
* **PSH (Push):** Alıcı kernel'a, tamponda beklemeden veriyi doğrudan üst uygulamaya iletmesi talimatını veren bayrak.
* **URG (Urgent):** Acil işlenmesi gereken veri işaretçisinin (Urgent Pointer) aktif olduğunu gösteren bayrak.

### 3.5. TCP Durum Makinesi (TCP States)

TCP bağlantısı yaşam döngüsü boyunca belirli durumlar (states) arasında geçiş yapar:

* **LISTEN:** Sunucunun belirtilen portta gelen bağlantı isteklerini kabul etmeye hazır olduğunu gösterir.
* **SYN_SENT:** İstemcinin `SYN` paketini gönderip sunucudan yanıt beklediği durumdur.
* **SYN_RCVD:** Sunucunun `SYN` paketini alıp `SYN-ACK` gönderdiği ve istemciden `ACK` beklediği durumdur.
* **ESTABLISHED:** 3-Way Handshake tamamlanmış, çift yönlü veri aktarımına hazır durumdur.
* **FIN_WAIT_1:** Aktif kapatmayı başlatan tarafın `FIN` gönderip yanıt beklediği durumdur.
* **FIN_WAIT_2:** Aktif kapatan tarafın `ACK` alıp karşı tarafın `FIN` paketini beklediği durumdur.
* **CLOSE_WAIT:** Pasif kapatan tarafın `FIN` alıp uygulamadan `close()` çağrısı beklediği durumdur.
* **CLOSING:** Her iki tarafın aynı anda bağlantıyı kapatmaya çalıştığı (Simultaneous Close) durumdur.
* **LAST_ACK:** Pasif kapatan tarafın kendi `FIN` paketini gönderip son `ACK`'yi beklediği durumdur.
* **TIME_WAIT:** Aktif kapatan tarafın ağdaki gecikmiş paketlerin temizlenmesi için 2MSL süreyle beklediği durumdur.
* **CLOSED:** Bağlantının tamamen kapandığı, soket kaynağının serbest kaldığı durumdur.

---

## 4. Çalışma Mantığı

### 4.1. Bağlantı Kurulumu: TCP Three-Way Handshake

TCP oturumu, istemci (client) ve sunucu (server) arasında üç aşamalı el sıkışma ile başlatılır:

    İstemci (Client)                                   Sunucu (Server)
       |                                                    |
       | --- 1. SYN (Seq = ISN_c) ------------------------> | (SYN_RCVD)
       |                                                    |
       | <-- 2. SYN-ACK (Seq = ISN_s, Ack = ISN_c + 1) ---- |
       |                                                    |
       | --- 3. ACK (Ack = ISN_s + 1) --------------------> | (ESTABLISHED)
       v                                                    v

1. **SYN:** İstemci, rastgele bir Başlangıç Sıra Numarası (**ISN — Initial Sequence Number**) üreterek `SYN` paketini sunucuya gönderir ve `SYN_SENT` durumuna geçer.
2. **SYN-ACK:** Sunucu isteği kabul ederse kendi `ISN` değerini belirler. İstemcinin `SYN` paketini onaylamak için `ACK = ISN_c + 1` ve bağlantı kurmak için `SYN` bayrağını içeren `SYN-ACK` paketini döner. Sunucu `SYN_RCVD` durumuna geçer.
3. **ACK:** İstemci, sunucunun `SYN` paketini onaylamak üzere `ACK = ISN_s + 1` yanıtını gönderir. Her iki taraf da `ESTABLISHED` durumuna geçer.

> **Initial Sequence Number (ISN) Güvenliği:** ISN değerlerinin her bağlantıda 0'dan başlamak yerine kriptografik olarak rastgele üretilmesinin temel nedeni; ağda gecikmiş eski paketlerin yeni bağlantıyı kirletmesini önlemek ve **TCP Sequence Prediction (Oturum Gaspı)** saldırılarını engellemektir.

### 4.2. Veri Doğrulama ve Bayt Sıralama (Sequence & ACK Math)

TCP paket bazlı değil, **bayt bazlı** takip yapar:
* **Sequence Number (Seq):** İlgili segment içinde gönderilen ilk baytın sırasını gösterir.
* **Acknowledgment Number (Ack):** Alıcının beklediği bir sonraki baytın numarasıdır. Kümülatif onay (Cumulative ACK) prensibi çalışır.

    MSS = MTU - (IP_Header + TCP_Header)

Standart bir Ethernet ağında (MTU = 1500 Byte), varsayılan başlıklar (20 Byte IP + 20 Byte TCP) çıkarıldığında standart MSS = 1460 Byte olarak hesaplanır.

### 4.3. Bağlantı Sonlandırma: Four-Way Termination

TCP bağlantısı tam çift yönlü (full-duplex) olduğu için her yönün ayrı kapatılması gerekir:

    İstemci (Aktif Kapatan)                           Sunucu (Pasif Kapatan)
       |                                                    |
       | --- 1. FIN --------------------------------------> | (CLOSE_WAIT)
       | <-- 2. ACK --------------------------------------- |
       |                                                    | (Uygulama close() çağırır)
       | <-- 3. FIN --------------------------------------- | (LAST_ACK)
       | --- 4. ACK (TIME_WAIT) --------------------------> | (CLOSED)

#### Kritik Durumlar: `TIME_WAIT` ve `CLOSE_WAIT`
* **`TIME_WAIT`:** Aktif kapatmayı başlatan tarafın, son `ACK` paketinin kaybolma ihtimaline karşı ve ağdaki gecikmiş paketlerin temizlenmesi amacıyla **2MSL (Maximum Segment Lifetime — Linux'ta genelde 60 saniye)** boyunca beklediği durumdur.
* **`CLOSE_WAIT`:** Pasif kapatan tarafın kernel seviyesinde `FIN` aldığı, ancak uygulama katmanının soketi kapatmak için `close()` sistem çağrısını henüz çalıştırmadığı durumdur. Yüksek sayıda `CLOSE_WAIT` görülmesi **yazılım seviyesinde bir soket sızıntısı (socket leak)** göstergesidir.

### 4.4. Akış Denetimi (Flow Control) ve Pencere Yönetimi

Akış denetimi, hızlı bir göndericinin yavaş bir alıcıyı veri yağmuruna tutarak tampon belleğini (Receive Buffer) taşırmasını engeller.

* **Receive Window (rwnd):** Alıcının TCP başlığında ilan ettiği, anlık olarak kabul edebileceği boş tampon alanıdır.
* **Sliding Window (Kayar Pencere):** Göndericinin onay (`ACK`) beklemeden ağa verebileceği maksimum veri miktarını yöneten dinamik mekanizmadır.
* **Window Scaling (RFC 1323):** Standart TCP başlığındaki 16-bitlik pencere boyutu alanı maks. 65,535 Byte (64 KB) destekler. Yüksek hızlı ağlarda bu sınırı aşmak için `TCP Options` alanındaki ölçekleme faktörüyle pencere boyutu 1 GB'a kadar yükseltilir.

### 4.5. Tıkanıklık Kontrolü (Congestion Control)

Tıkanıklık kontrolü, göndericinin ağ altyapısındaki (router/switch) darboğazları algılayarak ağın çökmesini engelleme mekanizmasıdır. Gönderici kendi bünyesinde bir **Congestion Window (`cwnd`)** değeri hesaplar. Ağa verilecek maksimum veri miktarı şu şekilde kısıtlanır:

    Gönderim Sınırı = min(rwnd, cwnd)

#### Algoritma Aşamaları ve Zamanlayıcılar
1. **Slow Start (Yavaş Başlatma):** Bağlantı başında `cwnd` küçük bir değerle (örn: 10 MSS) başlar. Her başarılı RTT döngüsünde `cwnd` **üstel olarak (exponential)** katlanır (1 -> 2 -> 4 -> 8).
2. **Congestion Avoidance (Tıkanıklık Önleme):** `cwnd` değeri belirlenen eşik değere (**`ssthresh` — Slow Start Threshold**) ulaştığında büyüme hızı **doğrusal (linear)** moda geçer (her RTT'de +1 MSS).
3. **Fast Retransmit & Fast Recovery:** Alıcı kayıp bir segment tespit ettiğinde arka arkaya aynı `ACK` değerini fırlatır. Gönderici **3 adet üst üste Duplicate ACK** aldığında, zamanlayıcının (**RTO — Retransmission Timeout**) dolmasını beklemeden kayıp paketi anında yeniden gönderir (Fast Retransmit). Ardından `ssthresh` değerini yarıya indirerek doğrusal büyümeye devam eder (Fast Recovery).
4. **Round-Trip Time (RTT) ve RTO Hesaplaması:** Gönderici, paketlerin gidiş-dönüş süresini (RTT) sürekli ölçer. RTO süresi RTT'deki dalgalanmalara göre dinamik olarak güncellenir. RTO dolduğunda paket kayıp kabul edilir ve `cwnd` 1 MSS değerine düşürülür.
5. **Selective Acknowledgment (SACK - RFC 2018):** Klasik kümülatif ACK yapısı yerine, alıcının hangi spesifik blokları aldığını göndericiye iletmesini sağlar. Böylece sadece kaybolan segmentler yeniden iletilir.
6. **TCP Keepalive:** Sessiz kalan bağlantılarda karşı tarafın çöküp çökmediğini tespit etmek için belirli aralıklarla atılan denetim paketleridir.

---

## 5. Mimari ve Bileşenler

Linux kernel ağ yığınında bir TCP soketinin oluşturulması ve işletim sistemi seviyesindeki kuyruk mimarisi aşağıdaki şekilde modellenmiştir:

    [ Gelen TCP SYN Paketi ]
               |
               v
    +-----------------------------------+
    |            SYN QUEUE              |  <-- Half-Open Bağlantılar (SYN_RCVD)
    |  (SYN-ACK atıldı, ACK bekleniyor)  |  (Sınır: net.ipv4.tcp_max_syn_backlog)
    +-----------------------------------+
               |
          ACK Geldi
               |
               v
    +-----------------------------------+
    |           ACCEPT QUEUE            |  <-- Tamamlanmış Bağlantılar (ESTABLISHED)
    | (Uygulama accept() çağırana kadar) |  (Sınır: min(somaxconn, backlog))
    +-----------------------------------+
               |
        accept() syscall
               v
    [ Kullanıcı Alanı Uygulaması (NGINX/Go) ]

### 5.1. Linux TCP Backlog Kuyrukları

1. **SYN Queue (Half-Open Queue):** Üç yönlü el sıkışmanın ilk aşamasındaki (`SYN_RCVD`) bağlantıların tutulduğu kernel kuyruğudur. Sınırı `net.ipv4.tcp_max_syn_backlog` parametresi ile belirlenir.
2. **Accept Queue (Completely Established Queue):** El sıkışması tamamlanmış (`ESTABLISHED`), ancak kullanıcı alanındaki (user space) uygulamanın henüz `accept()` sistem çağrısı ile devralmadığı soketlerin beklediği kuyruktur. Sınırı `min(net.core.somaxconn, application_backlog)` formülüyle belirlenir.

### 5.2. Connection Tracking (conntrack) ve NAT

Netfilter mimarisinin temel bileşeni olan `conntrack`, sistemden geçen tüm IP paketlerini durumlandırarak takip eder (`NEW`, `ESTABLISHED`, `RELATED`, `INVALID`). 

Stateful (durumlu) güvenlik duvarları ve NAT (Ağ Adresi Dönüştürme) mekanizmaları kararlarını bu tabloya göre verir. NAT cihazı, iç ağdaki özel IP/Port bilgilerini dış ağdaki genel IP/Port bilgileriyle eşleştiren dinamik tabloyu `conntrack` üzerinde tutar.

---

## 6. Adım Adım Gerçek Teknik Akış

Bir istemcinin web sunucusundan veri talep ettiği senaryodaki adım adım teknik akış:

    İstemci (192.168.1.50)                              Sunucu (10.0.0.10:443)
       |                                                         |
       | --- 1. [SYN] Seq=1000, MSS=1460, WinScale=7 ----------> | (Accept/SYN Queue)
       | <-- 2. [SYN, ACK] Seq=5000, Ack=1001, MSS=1420 -------- |
       | --- 3. [ACK] Seq=1001, Ack=5001 ----------------------> | (Accept Queue -> accept())
       |                                                         |
       | --- 4. [PSH, ACK] Seq=1001, Ack=5001, Data (500B) ----> | (TLS/HTTP Request)
       | <-- 5. [ACK] Seq=5001, Ack=1501, Win=64240 ------------ |
       |                                                         |
       | <-- 6. [DATA] Seq=5001, Ack=1501, Data (1420B) -------- | (Response Part 1)
       | <-- 7. [DATA] Seq=6421, Ack=1501, Data (1420B) -------- | (Response Part 2 - KAYIP!)
       |                                                         |
       | --- 8. [ACK] Seq=1501, Ack=6421, SACK=[7841-9260] ----> | (Duplicate ACK / SACK)
       | <-- 9. [DATA] Seq=6421 (Fast Retransmit) -------------- | (Kayıp Paket Tamamlandı)

1. **Bağlantı Kurulumu:** İstemci `192.168.1.50:51234` soketinden `10.0.0.10:443` soketine `SYN` atar. MSS ve Window Scale parametreleri karşılıklı anlaşılır (Negotiation).
2. **Kuyruk Geçişi:** Sunucu `SYN-ACK` üretir ve kaydı `SYN Queue`'ya yazar. İstemciden `ACK` geldiğinde kayıt `Accept Queue`'ya aktarılır.
3. **İşleme:** Web sunucu yazılımı `accept()` çağrısı ile soketi kuyruktan alır ve sürece özel bir dosya tanımlayıcısı (File Descriptor - FD) atanır.
4. **Veri Transferi ve Kayıp Kurtarma:** İstemci isteği yollar. Sunucu veriyi MSS boyutlarına bölerek fırlatır. Yolda kaybolan paket (Adım 7) için istemci SACK bilgisi içeren Duplicate ACK döner (Adım 8). Sunucu RTO süresini beklemeden Fast Retransmit ile kayıp paketi yeniden iletir (Adım 9).

---

## 7. Gerçek Dünya Kullanım Senaryoları

### Senaryo 1: Yüksek Trafikli Web API Gateway (E-Ticaret Platformu)
* **Problem:** Anlık kampanya dönemlerinde API Gateway arkasındaki mikroservislere bağlantı açarken `Cannot assign requested address` ve `connection refused` hataları alınması.
* **Kök Neden:** Aşırı kısa süreli HTTP bağlantıları nedeniyle istemci tarafında Ephemeral Portlerin tükenmesi ve arka plandaki servislerde `Accept Queue` sınırının (`somaxconn`) aşılması.
* **Çözüm:** API Gateway üzerinde HTTP Keep-Alive / Connection Pooling yapısına geçilmesi, kernel seviyesinde `net.ipv4.ip_local_port_range` genişletilmesi ve `somaxconn` değerinin büyütülmesi.

### Senaryo 2: Canlı Video Yayın Platformu (IPTV / Video Streaming)
* **Problem:** Yüksek çözünürlüklü canlı yayınlarda kullanıcı tarafında takılmalar (buffering) ve sunucu tarafında yüksek CPU yükü oluşması.
* **Kök Neden:** TCP'nin retransmission ve akış denetimi gecikmelerinin canlı yayın akışını bozması.
* **Çözüm:** Taşıma katmanında TCP yerine UDP tabanlı protokollere (RTP/RTSP veya QUIC/HTTP3) geçilmesi. Kaybolan karelerin yeniden istenmesi yerine doğrudan atlanarak düşük gecikmenin korunması.

---

## 8. Güvenlik Perspektifi

### 8.1. SYN Flood Saldırısı
* **Teknik Sorun:** Saldırganın sahte (spoofed) IP adresleriyle yoğun `SYN` paketi fırlatarak sunucunun `SYN Queue` belleğini doldurması.
* **Çalışma Mantığı:** Sahte IP adreslerinden `ACK` yanıtı dönmeyeceği için bağlantılar zaman aşımına uğrayana kadar `SYN Queue` dolu kalır.
* **Ön Koşullar:** Sunucunun doğrudan IP üzerinden erişilebilir olması ve varsayılan küçük kuyruk sınırları.
* **Olası Etki:** Hizmet Dışı Bırakma (DoS/DDoS — Availability kaybı). Meşru kullanıcılar sunucuya bağlanamaz.
* **Tespit:** `dmesg` üzerinde `Possible SYN flooding on port...` uyarıları veya `ss -ant state syn-recv` çıktısındaki anormal artışlar.
* **Savunma (SYN Cookies - RFC 4987):** `net.ipv4.tcp_syncookies = 1` ayarlandığında, kernel `SYN Queue` dolduğu an bellekte durum (state) tutmayı bırakır. İstemciye atılan `SYN-ACK` paketindeki `ISN` alanına kriptografik bir hash gizlenir. Gerçek istemci `ACK` döndüğünde bu sayı doğrulanırsa bağlantı doğrudan `Accept Queue`'ya alınır.

### 8.2. UDP Flood Saldırısı
* **Teknik Sorun:** UDP durum takibi yapmadığı için hedefin rastgele portlarına devasa hacımda UDP paketi basılması.
* **Olası Etki:** Sunucu ağ kartının (NIC) dolması ve ICMP "Port Unreachable" üretme çabası nedeniyle CPU kilitlenmesi.
* **Savunma:** Uygulama veya host firewall seviyesinde engellenemez. Ağ seviyesinde Rate Limiting, BGP Blackholing veya DDoS Scrubbing servisleri gerektirir.

### 8.3. Port Taraması ve Session Hijacking
* **TCP SYN Scan (Stealth Scan):** Saldırgan `SYN` atar, gelen `SYN-ACK` cevabından portun açık olduğunu anlar ancak `RST` atarak el sıkışmayı tamamlamaz. Uygulama loglarında iz bırakmadan keşif yapar.
* **TCP Connect Scan:** Tam el sıkışma yapılır, uygulama loglarına kayıt düşer.
* **TCP Session Hijacking:** Saldırganın araya girerek veya Sequence Number değerlerini tahmin ederek geçerli bir TCP oturumuna sahte veri enjekte etmesi. Modern Linux çekirdeklerindeki kriptografik ISN üretimi bu riski minimize eder.

---

## 9. Troubleshooting ve Operasyonel Yaklaşım

### 9.1. Kaynak Tükenmesi (Resource Exhaustion)

1. **Ephemeral Port Exhaustion:** İstemci tarafında kısa sürede binlerce bağlantı açılıp kapandığında soketlerin `TIME_WAIT` durumunda takılması.
   * *Tespit:* `ss -tan state time-wait | wc -l`
   * *Çözüm:* `net.ipv4.tcp_tw_reuse = 1` ve Connection Pooling.
2. **File Descriptor Exhaustion:** İşletim sisteminde her soket bir dosya tanımlayıcısıdır (FD).
   * *Süreç Sınırı:* `ulimit -n` aşıldığında uygulama loglarına `EMFILE (Too many open files)` düşer.
   * *Sistem Sınırı:* `/proc/sys/fs/file-max` aşıldığında kernel loglarına `ENFILE (Too many open files in system)` düşer.

### 9.2. `ss` Komutu ile Backlog Analizi

Dinleme modundaki (`LISTEN`) soketlerde `ss -lnt` komutunun çıktısı farklı anlamlar taşır:

    $ ss -lnt
    State      Recv-Q Send-Q Local Address:Port  Peer Address:Port
    LISTEN     129    128    0.0.0.0:80          0.0.0.0:*

* **`Send-Q`:** O port için tanımlanmış maksimum **Accept Queue kapasitesidir** (`backlog`).
* **`Recv-Q`:** O anda **Accept Queue içinde bekleyen**, uygulamanın henüz `accept()` etmediği bağlantı sayısıdır.
* **Kritik Bulgusal:** `Recv-Q > Send-Q` durumu var ise, uygulama kilitlenmiş veya CPU yetersizliği nedeniyle bağlantılara yetişemiyordur. Gelen yeni bağlantılar düşürülür (drop).

### 9.3. Paket ve Trafik Analiz Araçları (Wireshark & CLI)

* **`ss` (Socket Statistics):** Kernel soket bilgilerini doğrudan okur (`ss -tulpn`).
* **`lsof` (List Open Files):** Süreçlerin açtığı soket ve dosyaları eşleştirir (`lsof -i :80`).
* **`tcpdump`:** Komut satırından ağ trafiğini filtreleyerek paketleri yakalar (`tcpdump -i eth0 port 80 -n`).
* **Wireshark:** Yakalanan `.pcap` dosyalarının grafik arayüzde TCP akış birleştirmesi (`Follow TCP Stream`), RTT analizi ve retransmission incelemesinde kullanılır.

### 9.4. Sorun Giderme Komut Referansı

    # 1. Sistemdeki soket özetlerini ve durum dağılımını görme
    ss -s

    # 2. 80 portunu dinleyen süreci ve File Descriptor detayını bulma
    lsof -i :80

    # 3. Belirli bir arayüzde SYN paketlerini canlı yakalama (SYN Flood tespiti)
    tcpdump -i eth0 -n 'tcp[tcpflags] & (tcp-syn) != 0'

    # 4. Kernel seviyesinde düşen paket ve drop sayaçlarını inceleme
    nstat -az TcpExtListenDrops TcpExtListenOverflows

---

## 10. Production, SRE ve Platform Engineering Perspektifi

* **Observability (Gözlemlenebilirlik):** SRE ekipleri TCP sağlığını Prometheus ve `node_exporter` aracılığıyla şu anahtar metriklerle izler:
  * `node_netstat_Tcp_RetransSegs`: Ağdaki paket yeniden iletim oranı (Kayıp ve tıkanıklık göstergesi).
  * `node_netstat_TcpExt_ListenOverflows`: Accept queue dolduğu için reddedilen bağlantı sayısı.
  * `node_nf_conntrack_entries` / `node_nf_conntrack_entries_limit`: Connection tracking tablosunun doluluk oranı.
* **Kubernetes CNI ve Service Mesh:** Container ortamlarında Pod'lar arası iletişimde kullanılan CNI (örn: Cilium, Calico) altyapıları `conntrack` tablosunu yoğun şekilde kullanır. `nf_conntrack: table full` hatası tüm cluster iletişimini felç edebilir.
* **Tıkanıklık Algoritması Seçimi:** Modern Linux dağıtımları varsayılan olarak **CUBIC** algoritmasını kullanır. Yüksek bant genişliği ve gecikmenin kritik olduğu cloud native altyapılarda Google tarafından geliştirilen **BBR (Bottleneck Bandwidth and RTT)** algoritması tercih edilmektedir.

---

## 11. Sınırlamalar, Alternatifler ve Trade-off'lar

| Katman / Protokol | Avantajlar | Dezavantajlar | Kullanım Senaryosu / Trade-off |
| :--- | :--- | :--- | :--- |
| **TCP** | %100 Güvenilirlik, sıralı teslimat, akış/tıkanıklık denetimi. | Yüksek başlık yükü, Handshake gecikmesi (Latency), Head-of-Line Blocking. | Veri kaybının tolere edilemediği durumlar (Web, Veritabanı, SSH). |
| **UDP** | Minimum gecikme, başlık yükü çok az (8B), el sıkışma yok. | Güvenilirlik yok, sıralama yok, akış denetimi yok. | Canlı yayın, VoIP, DNS, Oyunlar. |
| **QUIC (HTTP/3)** | UDP üzerinde çalışır. TLS 1.3 entegredir. Head-of-Line blocking sorununu çözer. Connection Migration destekler. | Yüksek CPU kullanımı, eski firewall'lar tarafından engellenebilme riski. | Modern web, mobil uygulamalar, yüksek performanslı içerik dağıtımı (CDN). |

---

## 12. Doğrulanmış Parametreler ve Komut Referansı

Gerektiğinde `/etc/sysctl.conf` dosyasına eklenebilecek güvenli kernel optimizasyon parametreleri:

    # SYN Flood korumasını aktif et (Zorunlu)
    net.ipv4.tcp_syncookies = 1

    # Accept Queue sınırını artır (Varsayılan 128 genelde yetersizdir)
    net.core.somaxconn = 4096

    # Half-open bağlantı kuyruk sınırını yükselt
    net.ipv4.tcp_max_syn_backlog = 8192

    # Ephemeral port aralığını genişlet
    net.ipv4.ip_local_port_range = 1024 65535

    # TIME_WAIT soketlerinin güvenli durumlarda yeniden kullanımına izin ver
    net.ipv4.tcp_tw_reuse = 1

    # Conntrack tablosu kapasitesini artır
    net.netfilter.nf_conntrack_max = 262144

> ⚠️ **Kritik Uyarı:** `net.ipv4.tcp_tw_recycle` parametresi Linux 4.12 çekirdeği ile birlikte tamamen kaldırılmıştır. NAT arkasındaki istemcilerin paketlerinin düşmesine neden olduğu için **kesinlikle kullanılmamalıdır.**

---

## 13. Referanslar ve Kaynakça

* **RFC 793 / RFC 9293:** Transmission Control Protocol (TCP Specification)
* **RFC 768:** User Datagram Protocol (UDP Specification)
* **RFC 2018:** TCP Selective Acknowledgment Options (SACK)
* **RFC 1323 / RFC 7323:** TCP Extensions for High Performance (Window Scaling)
* **RFC 4987:** TCP SYN Flooding Attacks and Common Mitigations (SYN Cookies)
* **Linux Kernel Networking Documentation:** `kernel.org/doc/Documentation/networking/`
* **Linux Man Pages:** `ss(8)`, `lsof(8)`, `tcpdump(8)`, `sysctl(8)`
