# Ağ Yönlendirme ve Alt Ağ Yönetimi (Network Routing & Subnetting)

Bu doküman, bilgisayar ağlarının temel taşları olan IPv4 adres mimarisini, alt ağ oluşturma (subnetting) mekanizmalarını, paket yönlendirme (routing) algoritmalarını ve modern altyapılardaki (Linux, Kubernetes, Bulut Bilişim) yönlendirme pratiklerini ele almaktadır. Bu bölüm, ağ katmanındaki (Layer 3) veri akışını optimize etmek, güvenlik sınırları çizmek ve karmaşık sistem mimarilerini troubleshoot edebilmek için gereken derin teknik yetkinlikleri kazandırmayı amaçlar.

## 1. Genel Bakış

Ağ Yönlendirme ve Alt Ağ Yönetimi, internet mimarisinin ve kurumsal ağların ölçeklenebilir, performanslı ve güvenli bir şekilde çalışmasını sağlayan mekanizmalar bütünüdür. Alt ağ yönetimi (Subnetting), devasa bir mantıksal ağın daha küçük, yönetilebilir ve yalıtılmış fiziksel ya da mantıksal parçalara bölünmesi işlemidir. Bu işlem, ağ üzerindeki yayın (broadcast) trafiğini sınırlayarak bant genişliğini korur ve güvenlik duvarı kuralları için net sınırlar oluşturur.

Yönlendirme (Routing) ise, bir kaynaktan çıkan veri paketinin (IP Packet), hedef adrese ulaşması için ağ topolojileri boyunca hangi yolları (paths) takip etmesi gerektiğine karar veren süreçtir. OSI modelinin Ağ Katmanında (Layer 3) gerçekleşen bu süreç olmasaydı, milyarlarca cihazın birbirleriyle doğrudan veya dolaylı olarak iletişim kurması imkansız hale gelirdi. Modern sistem mimarilerinde yönlendirme, sadece donanımsal router cihazlarında değil; Linux çekirdeğinde (kernel space), konteyner ağ arayüzlerinde (CNI) ve bulut sağlayıcılarının sanal ağlarında (VPC) yazılımsal olarak da yoğun bir şekilde icra edilir.

## 2. Neden Önemlidir?

Bu konunun anlaşılması, sistem yönetimi ve modern altyapı mühendisliği disiplinlerinde kritik bir öneme sahiptir:

*   **Siber Güvenlik ve Savunma (Defensive Security):** Ağların alt ağlara düzgün bölünmemesi, saldırganların sızdığı bir sistemde yatayda kolayca hareket etmesine (lateral movement) neden olur. Doğru bir adres planlaması, mikro-segmentasyonun ve sıkı güvenlik duvarı politikalarının (ACL) zeminini hazırlar.
*   **DevOps ve Platform Mühendisliği:** Kubernetes cluster mimarilerinde Pod'ların ve Service'lerin birbirleriyle nasıl konuştuğunu anlamak, CNI (örn. Calico, Cilium) yapılandırmalarını yönetmek doğrudan yönlendirme tabloları ve alt ağ bilgisine dayanır.
*   **Bulut Mimarisi (Cloud Engineering):** AWS, Google Cloud veya Azure üzerinde kurgulanan VPC'ler, Route Table'lar, internet gateway'ler ve VPN bağlantıları tamamen CIDR blokları ve yönlendirme mantığıyla çalışır. Hatalı bir rota tasarımı, servis kesintilerine veya servislerin internete açık hale gelmesine yol açabilir.
*   **Site Reliability Engineering (SRE):** Yüksek erişilebilirlik, yük dengeleme (Anycast routing, ECMP) ve ağ kaynaklı gecikme (latency) problemlerinin kök neden analizi (troubleshooting) bu katmandaki derin bilgiye ihtiyaç duyar.

## 3. Temel Kavramlar ve Terminoloji

Ağ katmanındaki işlemleri ve protokolleri analiz edebilmek için aşağıdaki kavramların teknik karşılıklarının bilinmesi şarttır:

*   **IPv4 Adres Mimarisi (IPv4 Address Architecture):** 32 bit uzunluğunda, noktalarla ayrılmış 4 oktetten (8 bitlik bloklar) oluşan mantıksal adresleme sistemidir. Toplamda $2^{32}$ (yaklaşık 4.3 milyar) benzersiz adres barındırır.
*   **İkili ve Onlu Gösterim (Binary and Decimal Representation):** Bilgisayarlar IP adreslerini 32 bitlik ikili (binary) formatta işlerken (örn. `11000000.10101000.00000001.00000001`), insanlar kolay okuyabilmek için bunu noktalı onlu (dotted-decimal) formata dönüştürür (`192.168.1.1`).
*   **Ağ ve Host Bölümleri (Network and Host Portions):** Bir IP adresinin ilk kısmı paketin hangi ağa ait olduğunu (Network ID), kalan kısmı ise o ağ içindeki hangi spesifik cihaza (Host ID) ait olduğunu belirtir.
*   **Alt Ağ Maskesi (Subnet Mask):** 32 bitlik bir maskedir. IP adresinin neresinin ağ (1'lerden oluşur) ve neresinin host (0'lardan oluşur) bölümü olduğunu ayırt etmek için mantıksal AND (VE) işlemine tabi tutulur.
*   **CIDR Notasyonu ve Prefiks Uzunluğu (CIDR Notation & Prefix Length):** Sınıfsız Ağ Karası Yönlendirme (Classless Inter-Domain Routing). Alt ağ maskesindeki yan yana gelen "1" bitlerinin sayısının, IP adresinin sonuna eğik çizgi eklenerek yazılmasıdır (Örn. `255.255.255.0` yerine `/24`).
*   **Genel ve Özel Adresler (Public and Private Addresses):** Public adresler internet üzerinde yönlendirilebilen küresel olarak benzersiz adreslerdir. Private adresler ise (RFC 1918 ile belirlenen `10.0.0.0/8`, `172.16.0.0/12`, `192.168.0.0/16`) internete doğrudan çıkamaz, yerel ağlarda ücretsiz kullanılır ve NAT (Network Address Translation) ile internete açılır.

### Özel Amaçlı IPv4 Adresleri

| Adres Bloğu / Adres | Teknik Açıklama | Sistemdeki Rolü |
| :--- | :--- | :--- |
| `127.0.0.0/8` | Geri Döngü (Loopback) Adresleri | Cihazın kendi TCP/IP yığınını test etmesi ve yerel servislerin (localhost - `127.0.0.1`) ağ kartına inmeden haberleşmesi için ayrılmıştır. |
| `169.254.0.0/16` | Bağlantı-Yerel Adresi (Link-Local / APIPA) | DHCP sunucusundan adres alamayan cihazların işletim sistemi tarafından otomatik olarak atadığı, sadece aynı Layer 2 anahtarına bağlı cihazların konuşabildiği yönlendirilemeyen bloktur. |
| `0.0.0.0` | Belirtilmemiş Adres (Unspecified Address) | Henüz bir IP'ye sahip olmayan cihazların (örn. DHCP isteği atarken) veya sunucu yazılımlarının "tüm ağ arayüzlerini dinle" (bind) talimatı verirken kullandığı adrestir. Rota tablosunda ise varsayılan rotayı simgeler. |
| `255.255.255.255` | Sınırlı Yayın Adresi (Limited Broadcast) | Yerel ağdaki (Layer 2 broadcast domain içindeki) tüm cihazlara aynı anda veri göndermek için hedef IP olarak seçilen adrestir. Router'lar bu paketi diğer ağlara geçirmez (drop eder). |

*   **Ağ Adresi (Network Address):** Bir alt ağın ilk adresidir. Host bitlerinin tamamı ikili sistemde `0` olan adrestir. Cihazlara atanamaz, ağın kendisini temsil eder.
*   **Yayın Adresi (Broadcast Address):** Bir alt ağın son adresidir. Host bitlerinin tamamı ikili sistemde `1` olan adrestir. O ağdaki tüm cihazlara veri göndermek için kullanılır, cihazlara atanamaz.
*   **Kullanılabilir Host Aralığı (Usable Host Range):** Ağ adresi ile Yayın adresi arasında kalan ve gerçek cihazlara (sunucu, router bacağı, PC) atanabilen adres kümesidir. Formülü: $2^{\text{host bitleri}} - 2$ formülü ile hesaplanır.
*   **VLSM (Variable Length Subnet Mask):** Değişken Uzunluklu Alt Ağ Maskesi. Bir ağ bloğunun ihtiyaç duyulan host sayılarına göre farklı boyutlarda (farklı prefiks uzunluklarında) alt ağlara bölünmesi tekniğidir. IP israfını engeller.
*   **Süper Ağ Oluşturma (Supernetting / Route Summarization):** Rota Özetleme. Birbirini takip eden birden fazla alt ağ rotasını, tek bir daha kısa prefiksli (küçük CIDR değerli) rota altında birleştirme işlemidir. Router cihazlarının hafıza ve işlemci yükünü azaltır.
*   **Varsayılan Ağ Geçidi (Default Gateway):** Yerel ağda bulunmayan bir hedef IP'ye gönderilecek paketlerin, ağdan çıkabilmesi için gönderildiği router arayüzünün IP adresidir.
*   **Otonom Sistem (Autonomous System - AS):** Tek bir idari merkez (ISP, büyük teknoloji şirketi veya üniversite) tarafından yönetilen ve ortak bir yönlendirme politikasına sahip IP ağları topluluğudur. Küresel internet AS'lerin BGP ile birbirine bağlanmasıyla oluşur.
*   **Yönetimsel Mesafe (Administrative Distance - AD):** Bir router'ın farklı yönlendirme kaynaklarından (statik rota, OSPF, BGP) aynı hedef ağa giden yollar öğrendiğinde, hangi kaynağa daha çok güveneceğini belirleyen öncelik değeridir. Değer düştükçe güvenilirlik artar (Örn: Doğrudan bağlı = 0, Statik = 1, OSPF = 110).
*   **Yönlendirme Metriği (Routing Metric):** Aynı yönlendirme protokolü içinde, aynı hedefe giden birden fazla alternatif yoldan hangisinin daha kaliteli (en kısa veya en hızlı) olduğunu belirleyen hesaplama değeridir. OSPF maliyet (cost - bant genişliği tabanlı) kullanırken, RIP atlama sayısını (hop count) kullanır.
*   **ECMP (Equal-Cost Multi-Path):** Eşit Maliyetli Çoklu Yol. Bir hedefe giden aynı metriğe ve AD değerine sahip birden fazla aktif rota varsa, trafiğin bu yollar arasında akış bazlı olarak dağıtılarak (yük dengeleme) iletilmesidir.

## 4. Çalışma Mantığı

Ağ katmanında verinin işlenmesi ve hedefe ulaştırılması sıkı kurallara bağlı deterministik algoritmalardan geçer.

### Alt Ağ Hesaplama Mantığı (Subnet Calculation)

Bir IP bloğunun alt ağlara bölünmesi, host bitlerinden ödünç alınarak (borrowing bits) ağ bitlerine eklenmesi mantığına dayanır.

1.  **Girdi:** Bir ana ağ bloğu (örn. `192.168.1.0/24`) ve ihtiyaç duyulan alt ağ veya host sayısı.
2.  **Mantıksal Ayrıştırma:** `/24` maskesinde 24 bit ağa, $32 - 24 = 8$ bit hosta aittir. Toplam host sayısı $2^8 = 256$'dır. Kullanılabilir host sayısı $256 - 2 = 254$'tür.
3.  **Bit Ödünç Alma:** Eğer bu ağı her biri en az 30 host barındıran alt ağlara bölmek istersek; formülümüz $2^h - 2 \ge 30$ şeklindedir. Buradan host bit sayısı ($h$) en az 5 olmalıdır.
4.  **Yeni Maske Belirleme:** Toplam 32 bit olduğuna göre, yeni ağ bit sayısı $32 - 5 = 27$ olur. Yani yeni maskemiz `/27` (`255.255.255.224`) olacaktır.
5.  **Alt Ağların Çıkarılması:** Ödünç alınan bit sayısı $27 - 24 = 3$ bittir. Oluşacak alt ağ sayısı $2^3 = 8$'dir. Her alt ağ $2^5 = 32$ blok boyutunda ilerler:
    *   Subnet 0: `192.168.1.0/27` (Kullanılabilir: .1 - .30, Broadcast: .31)
    *   Subnet 1: `192.168.1.32/27` (Kullanılabilir: .33 - .62, Broadcast: .63)
    *   ... bu şekilde 32'şer artarak devam eder.

### Rota Arama ve En Uzun Prefiks Eşleşmesi (Route Lookup & Longest Prefix Match - LPM)

Bir router veya işletim sistemi çekirdeği bir IP paketini aldığında, paketin hedef IP'sini kendi yönlendirme tablosundaki (routing table) kurallarla karşılaştırır. Karşılaştırma adımları şu şekildedir:

1.  **Hedef IP Tespiti:** Gelen IP paketinin başlığından (header) `Destination IP` alanı okunur (Örn: `192.168.1.45`).
2.  **Tablo Taraması:** Rota tablosundaki tüm satırlar teker teker incelenir. Hedef IP ile rotanın maskesi mantıksal AND işlemine sokulur. Elde edilen sonuç rota tablosundaki ağ adresi ile eşleşiyor mu bakılır.
3.  **LPM (Longest Prefix Match) Kuralı:** Eğer hedef IP birden fazla rota satırıyla eşleşiyorsa, maske uzunluğu (prefix length) en büyük olan yani en spesifik olan rota seçilir.
    *   Rota A: `192.168.1.0/24` via `Gateway X`
    *   Rota B: `192.168.1.32/27` via `Gateway Y`
    *   Varsayılan Rota: `0.0.0.0/0` via `Gateway Z`
    *   `192.168.1.45` adresi üçüne de uyar. Ancak Rota B `/27` ile en uzun prefikse sahip olduğu için paket `Gateway Y` üzerinden gönderilir.
4.  **Çıktı:** Paket ilgili ağ arayüzüne (interface) veya bir sonraki düğüme (next-hop) iletilmek üzere kuyruğa alınır. Eşleşen hiçbir rota yoksa paket çöpe atılır (drop) ve kaynağa `ICMP Destination Unreachable` mesajı gönderilir.

### IP Fragmantasyonu (IP Fragmentation) ve Path MTU Discovery (PMTUD)

Her ağ ortamının bir seferde taşıyabileceği maksimum Layer 3 paket boyutu vardır. Buna **MTU (Maximum Transmission Unit)** denir. Standart Ethernet için bu değer 1500 byte'tır.

*   **Fragmantasyon:** Eğer bir router, gelen paketin boyutundan daha küçük MTU'ya sahip bir arayüze (örn. 1400 byte'lık bir VPN tüneli) paketi yönlendirmek zorunda kalırsa ve IP başlığındaki `DF (Don't Fragment)` biti set edilmemişse, paketi parçalara (fragment) ayırır. Her parçaya yeni bir IP başlığı eklenir, `Fragment Offset` ve `MF (More Fragments)` bitleri yazılır. Parçalanan paketlerin birleştirilmesi (reassembly) hedef cihazda gerçekleştirilir. Fragmantasyon router işlemcisini yorar ve paket kaybı riskini artırır.
*   **Path MTU Discovery (PMTUD):** Modern sistemler fragmantasyondan kaçınmak için PMTUD mekanizmasını kullanır. Kaynak cihaz, göndereceği paketlerin IP başlığına `DF=1` yazar. Yol üstündeki bir router'ın MTU limitine takılındığında, router paketi drop eder ve kaynağa `ICMP Type 3, Code 4 (Fragmentation Needed and DF set)` mesajı gönderir. Bu ICMP mesajının içinde desteklediği sonraki MTU değerini de belirtir. Kaynak cihaz bu bilgiye göre paket boyutunu küçültür.

### Politika Tabanlı Yönlendirme (Policy-Based Routing - PBR)

Standart yönlendirme sadece hedef IP adresine bakarak karar verir. Politika Tabanlı Yönlendirme (`ip rule` mekanizması ile gerçekleştirilir) ise yönlendirme kararı verirken hedef IP dışında kriterleri de işleme katabilir:
*   Kaynak IP adresi (Source IP)
*   Protokol türü (TCP, UDP, ICMP)
*   Port numaraları (örn. HTTP trafiğini farklı, SSH trafiğini farklı hattan uçurmak)
*   Paket üzerindeki işaretler (skb->mark veya firewall mark - fwmark)

PBR sayesinde sistem yöneticisi, standart yönlendirme tablosuna bakılmadan önce paketi özel tanımlanmış alternatif yönlendirme tablolarına (routing tables) yönlendirebilir.

### TTL (Time to Live) ve ICMP Çalışma Temeli

*   **TTL (Time to Live - Yaşam Süresi):** IP başlığında 8 bitlik bir alandır (0-255 arası). Bir paket her router geçişinde (hop), o router tarafından TTL değeri 1 azaltılır. Eğer bir paket döngüye (routing loop) girerse, sonsuza kadar ağda dönmesini engellemek için TTL değeri `0`'a ulaştığı anda router paketi drop eder ve kaynağa `ICMP Time Exceeded (Type 11, Code 0)` paketi fırlatır. `traceroute` aracı tam olarak bu mekanizmayı kullanarak çalışır.

## 5. Mimari ve Bileşenler

Bir ağ yönlendirme altyapısı, mantıksal katmanlar ve dinamik karar mekanizmalarından oluşur.

### Kontrol Düzlemi (Control Plane) ve Veri Düzlemi (Data Plane)

*   **Control Plane (Kontrol Düzlemi):** Ağ topolojisini öğrenen, yönlendirme tablolarını oluşturan ve en iyi yolları hesaplayan zeka katmanıdır. Yönlendirme protokolleri (OSPF, BGP) burada çalışır. Linux dünyasında `frr` (Free Range Routing) veya `bird` gibi servisler control plane görevi görür.
*   **Data Plane / Forwarding Plane (Veri Düzlemi):** Gelen paketleri control plane tarafından oluşturulan tablolara (FIB - Forwarding Information Base) bakarak çok hızlı bir şekilde hedef arayüze yönlendiren katmandır. Linux kernel space içindeki yönlendirme alt sistemi data plane'dir.

### Rota Türleri

1.  **Doğrudan Bağlı Rotalar (Directly Connected Routes):** Cihazın kendi arayüzüne (interface) bir IP ve maske tanımlandığında otomatik oluşan rotadır. AD değeri `0`'dır. Herhangi bir ağ geçidine ihtiyaç duymaz.
2.  **Statik Rotalar (Static Routes):** Sistem yöneticisi tarafından elle girilen sabit rotalardır. Ağ topolojisi değiştiğinde otomatik güncellenmezler. Güvenilirdir (AD=1), CPU tüketimi azdır.
3.  **Varsayılan Rotalar (Default Routes):** Rota tablosunda daha spesifik bir kural bulunamayan tüm paketlerin gönderileceği "son çare" rotasıdır. `0.0.0.0/0` şeklinde gösterilir.
4.  **Yüzen Statik Rotalar (Floating Static Routes):** Dinamik bir yönlendirme protokolünün yedeklenmesi amacıyla yazılan statik rotadır. AD değeri bilinçli olarak dinamik protokolün AD değerinden yüksek tutulur (örn. OSPF için 110 ise, yüzen statik rota için 150 verilir). Ana hat koptuğunda ve OSPF rotası tablodan düştüğünde bu rota otomatik olarak devreye girer.

### Dinamik Yönlendirme Temelleri ve Protokol Sınıfları

Ağ topolojisindeki değişiklikleri otomatik algılayıp yönlendirme tablolarını kendi kendine güncelleyen sistemlerdir. Topolojinin değişip tüm router'ların güncel durumdan haberdar olması sürecine **Yakınsama (Convergence)** denir. Yakınsama süresinin kısa olması ağ kalitesi için kritiktir.

*   **Distance Vector (Uzaklık Vektörü):** Router'lar topolojinin tamamını bilmez, sadece komşularından aldıkları yönlendirme tablolarını kendi tablolarına eklerler (dedikodu tabanlı). En iyi yol hesaplamasında atlama sayısı (hop count) gibi basit değerlere bakarlar. Örn: RIP.
*   **Link-State (Bağlantı Durumu):** Her router tüm ağın topoloji haritasını (Link-State Database) çıkarır. Dijkstra algoritmasını kullanarak kendisini merkez alıp hedeflere giden en kısa yolları hesaplar. Yakınsama süreleri çok kısadır, hafıza tüketimleri yüksektir. Örn: OSPF (Open Shortest Path First).
*   **Path-Vector (Yol Vektörü):** Paketin hedefe ulaşırken geçeceği Otonom Sistemlerin (AS) listesini (AS-Path) tutar. Döngüleri engellemek ve politika tabanlı yönlendirme yapmak için tasarlanmıştır. Örn: BGP (Border Gateway Protocol).

#### IGP (Interior Gateway Protocol) ve EGP (Exterior Gateway Protocol)

*   **IGP:** Tek bir Otonom Sistem (AS) içinde yönlendirme yapmak için kullanılır. Hız ve iç topoloji detayları ön plandadır. (Örn: OSPF, EIGRP).
*   **EGP:** Farklı Otonom Sistemler arasında internet seviyesinde yönlendirme sağlamak için kullanılır. Güvenlik ve idari politikalar ön plandadır. İnternetin omurgasını oluşturan küresel protokol **BGP**'dir.

### Linux Çekirdeği Yönlendirme Mimarisi

Linux işletim sistemi gelişmiş bir yazılımsal router'dır. Çekirdek seviyesindeki bileşenler şunlardır:

```
        +---------------------------------------+
        |             Gelen Paket               |
        +---------------------------------------+
                            |
                            v
              +---------------------------+
              |   Policy Routing Rules    | <--- `ip rule` (Hangi tabloya bakılacak?)
              +---------------------------+
                            |
             +--------------+--------------+
             |                             |
             v                             v
     +---------------+             +---------------+
     |  Table: main  |             |  Table: custom| <--- `ip route` (LPM Algoritması)
     +---------------+             +---------------+
             |                             |
             +--------------+--------------+
                            |
                            v
        +---------------------------------------+
        |   Forwarding / Local Delivery (FIB)   |
        +---------------------------------------+
```

*   **Network Namespaces (Ağ İsim Alanları):** Linux çekirdeğinin ağ yığınını (interfaces, routing tables, firewall rules) tamamen yalıtmasına izin veren sanallaştırma özelliğidir. Her isim alanının (namespace) kendi bağımsız `main` yönlendirme tablosu vardır.
*   **Konteyner ve Kubernetes Yönlendirmesi:** Docker konteynerleri veya Kubernetes Pod'ları aslında birer network namespace içinde çalışır. Pod içinden çıkan trafik, `veth pair` (sanal ethernet çifti) aracılığıyla ana makinenin (node) namespace'ine aktarılır. Node üzerindeki yönlendirme tablosu veya CNI tarafından yönetilen kurallar (örn. BGP mesh veya VXLAN tünelleme) paketi hedef node'a veya harici ağa yönlendirir.
*   **Cloud Route Tables:** Bulut sağlayıcılarında (AWS VPC, Azure VNet), ağ kartlarının ötesinde, subnet seviyesinde çalışan soyutlanmış yazılımsal yönlendirme tabloları bulunur. Bu tablolar sanal makinelerin kendi içlerindeki yönlendirme tablolarından bağımsız olarak çalışır ve trafiğin internet geçidine (IGW) mi yoksa bir firewall appliance cihazına mı gideceğini kesin olarak belirler.

## 6. Adım Adım Gerçek Teknik Akış

Bir Linux sunucusu üzerinde, özel bir network namespace içindeki uygulamadan çıkan paketin, dış dünyadaki bir hedefe giderken geçtiği Layer 3 aşamaları ve yönlendirme kararları:

### Senaryo Bileşenleri
*   Namespace: `ns-prod`
*   Namespace İçindeki IP: `10.200.1.2/24` (Arayüz: `veth-child`)
*   Ana Makine (Host) Arayüz IP'si: `10.200.1.1/24` (Arayüz: `veth-host`)
*   Host Dış Dünya Arayüzü: `eth0` (`192.168.1.50/24`)
*   Hedef IP: `8.8.8.8` (İnternette bir DNS sunucusu)

### Akış Adımları

1.  **Paket Oluşturma ve Yerel Kontrol:** `ns-prod` içindeki uygulama `8.8.8.8` adresine bir paket gönderir. Namespace içindeki IP yığını, hedef IP'yi (`8.8.8.8`) kendi maskesi (`10.200.1.2/24`) ile AND işlemine sokar. Hedefin yerel ağda (`10.200.1.0/24`) olmadığını anlar.
2.  **Namespace Rota Tablosu Sorgusu:** Namespace içindeki yönlendirme tablosuna bakılır. En uzun prefiks eşleşmesi aranır. Eşleşen tek kural varsayılan rotadır: `default via 10.200.1.1 dev veth-child`.
3.  **L2 Çözümleme ve Transfer:** Paket `10.200.1.1` ağ geçidine gönderilmek üzere `veth-child` arayüzünden çıkar. `veth pair` mantığı gereği paket anında ana makine (host) tarafındaki `veth-host` arayüzünde belirir.
4.  **Host Çekirdeği İleri Taşıma Kararı (IP Forwarding):** Host çekirdeği paketi alır. Paketin hedef IP'si kendisine ait olmadığı için paketi yönlendirmesi gerektiğini anlar. Çekirdekte `net.ipv4.ip_forward` parametresinin `1` olup olmadığı kontrol edilir. `0` ise paket burada sessizce drop edilir. `1` ise devam eder.
5.  **Host Policy Rules (`ip rule`) Kontrolü:** Host üzerindeki kural veri tabanına bakılır. Varsayılan olarak `local`, `main` ve `default` tablolarına sırayla bakılması söylenir. Özel bir kural (PBR) yoksa `main` tablosu taranır.
6.  **Host `main` Yönlendirme Tablosu Sorgusu (LPM):**
    *   `10.200.1.0/24 dev veth-host proto kernel scope link src 10.200.1.1` -> Uymuyor.
    *   `192.168.1.0/24 dev eth0 proto kernel scope link src 192.168.1.50` -> Uymuyor.
    *   `default via 192.168.1.1 dev eth0` -> Eşleşiyor (LPM kazananı).
7.  **MTU ve Fragmantasyon Kontrolü:** `eth0` arayüzünün MTU değeri (1500) kontrol edilir. Paket boyutu bu değerden küçükse fragmantasyon yapılmaz. Paket başlığındaki TTL değeri 1 azaltılır.
8.  **Çıkış:** Paket, Layer 2 başlığı eklenerek (Hedef MAC olarak `192.168.1.1` router bacağının MAC adresi yazılır) `eth0` fiziksel arayüzünden dış ağa fırlatılır.

## 7. Gerçek Dünya Kullanım Senaryoları

### Senaryo A: Kurumsal Ağ İçin VLSM Tabanlı Adres Planlaması
*   **Problem:** Bir şirkete `172.16.10.0/23` (512 adres) IP bloğu verilmiştir. Şirkette 3 farklı departman vardır: Sistem (120 host), Muhasebe (50 host), AR-GE (200 host). Klasik sabit uzunluklu subnetting yapılırsa IP yetmemekte veya israf olmaktadır.
*   **Yaklaşım:** VLSM kullanılarak en büyük host ihtiyacından başlanarak planlama yapılır:
    1.  **AR-GE (200 host):** En yakın $2^h - 2 \ge 200$ değeri için $h=8$ bit gerekir ($256 - 2 = 254$). Maske `/24` olur. Atanan blok: `172.16.10.0/24`. (Kullanılabilir: .1 - .254).
    2.  **Sistem (120 host):** Kalan boş yer `172.16.11.0`. $2^h - 2 \ge 120$ için $h=7$ bit gerekir ($128 - 2 = 126$). Maske `/25` olur. Atanan blok: `172.16.11.0/25`. (Kullanılabilir: .129 - .254).
    3.  **Muhasebe (50 host):** Kalan boş yer `172.16.11.128`. $2^h - 2 \ge 50$ için $h=6$ bit gerekir ($64 - 2 = 62$). Maske `/26` olur. Atanan blok: `172.16.11.128/26`. (Kullanılabilir: .129 - .190).
*   **Elde Edilen Fayda:** Tek bir `/23` bloğundan tüm departmanların ihtiyacı israfsız karşılanmış ve ilerisi için `172.16.11.192/26` gibi boş bloklar rezerve edilmiştir.

### Senaryo B: Çoklu Bulut / On-Premise Bağlantısında Asimetrik Yönlenmenin Engellenmesi
*   **Problem:** Bir şirket ağı, AWS bulut ortamına hem hızlı bir kiralık hat (Direct Connect) hem de yedek bir IPsec VPN üzerinden bağlıdır. Şirket içi sunuculardan AWS'e giden paketler Direct Connect üzerinden giderken, dönüş paketleri VPN üzerinden gelmekte (Asymmetric Routing), bu durum aradaki stateful firewall cihazlarının paketleri drop etmesine sebep olmaktadır.
*   **Yaklaşım:** BGP yönlendirme protokolü kuralları manipüle edilir. AWS yönündeki BGP konfigürasyonunda, VPN hattından anons edilen şirket içi IP bloklarına `AS-Path Prepending` uygulanır. Yani VPN yolu yapay olarak uzatılır. Aynı zamanda on-prem router üzerinde Direct Connect rotasının metriği (veya yerel önceliği - Local Preference) daha iyi olacak şekilde ayarlanır.
*   **Elde Edilen Fayda:** Trafiğin hem gidiş hem dönüş yönünde Direct Connect hattını seçmesi garanti altına alınır, firewall drop problemleri çözülür.

## 8. Güvenlik Perspektifi

Ağ yönlendirme mimarileri altyapının damarları gibidir; bu katmanda meydana gelecek zafiyetler veya konfigürasyon hataları tüm sistemi kör edebilir.

### Rota Gaspı (Route Hijacking)
*   **Teknik Sorun:** BGP protokolünün varsayılan olarak birbirine güvenen yapıda olması ve anons edilen IP bloklarının doğruluğunu doğrulamaması.
*   **Çalışma Mantığı:** Kötü niyetli veya hatalı yapılandırılmış bir Otonom Sistem (AS), kendisine ait olmayan bir IP bloğunu (örn. büyük bir bankanın public IP'lerini) internete daha spesifik bir prefiks ile (LPM kuralından faydalanmak için `/24` olarak) anons eder. Küresel internet trafiğinin bir kısmı banka yerine saldırganın router'ına akar.
*   **Olası Etki:** Veri hırsızlığı, Ortadaki Adam (MitM) saldırıları, servis dışı bırakma (DoS).
*   **Savunma:** BGP anonslarında **RPKI (Resource Public Key Infrastructure)** kullanımı zorunlu tutulmalıdır. RPKI, hangi AS'in hangi IP bloğunu anons etmeye yetkili olduğunu kriptografik olarak doğrular.

### Asimetrik Yönlendirme (Asymmetric Routing) ve State Filtrelemesi Riskleri
*   **Teknik Sorun:** Bir TCP bağlantısına ait istek paketinin (SYN) farklı bir router/firewall üzerinden, yanıt paketinin (SYN-ACK) ise tamamen farklı bir yoldan geçmesi durumudur.
*   **Çalışma Mantığı:** Yönlendirme tablolarındaki maliyet dengesizlikleri veya dinamik rota değişiklikleri buna sebep olur. Tek başına bir ağ hatası değildir; ancak paketin geçtiği ikinci yolda bulunan `Stateful Inspection` (Durum Denetimli) bir firewall, kendi tablosunda o bağlantıya ait bir `SYN` kaydı görmediği için gelen `SYN-ACK` veya `ACK` paketini geçersiz/sahte sayarak drop eder.
*   **Savunma:** Ağ yollarında simetriyi zorlamak için metrik senkronizasyonu yapılmalı ya da firewall cihazlarında asimetrik trafiğe izin veren özel bölgeler (zones) tanımlanmalıdır. Linux tarafında `rp_filter` (Reverse Path Filtering) mekanizması `strict` modda ise asimetrik paketleri drop eder; bu tür çok yollu topolojilerde `loose` moda (`2`) alınmalıdır.

### ICMP Güvenlik Riskleri
*   **Teknik Sorun:** ICMP paketlerinin ağ keşfi ve ağ kararlılığı için kritik olması, ancak saldırganlar tarafından keşif (reconnaissance) veya tünelleme amacıyla kötüye kullanılabilmesi.
*   **Risk Detayı:** `ICMP Echo Request (Ping)` ağ haritası çıkarmak için kullanılır. Daha da tehlikelisi, `ICMP Type 3 (Destination Unreachable)` paketleri manipüle edilerek sistemlerin bağlantıları koparılabilir. Ayrıca ICMP paketlerinin data alanına veri gizlenerek dışarı sızdırma (ICMP Tunneling) yapılabilir.
*   **Savunma:** Dış dünyaya karşı sınır firewall'larda `ICMP Type 3 (Destination Unreachable)` ve `Type 11 (Time Exceeded - PMTUD için gerekli)` dışındaki gereksiz tüm ICMP tipleri (özellikle inbound Echo Request) kısıtlanmalıdır.

## 9. Hardening ve Best Practices

Ağ yönlendirme katmanının operasyonel olarak güvenli kılınması için uygulanması gereken kontroller:

| Kontrol | Neden Önemli? | Uygulama Alanı | Sınırlama / Trade-off |
| :--- | :--- | :--- | :--- |
| **uRPF (Unicast Reverse Path Forwarding) Aktifleştirilmesi** | Kaynak IP adresi sahtekarlığını (IP Spoofing) engeller. Bir arayüzden gelen paketin kaynak IP'sine geri giden bir rota yoksa paket drop edilir. | Router sınır kapıları, Linux Sunucular (`sysctl net.ipv4.conf.all.rp_filter=1`) | Asimetrik yönlendirme yapılan meşru ağlarda haklı paketlerin de drop edilmesine yol açar. |
| **BGP Peer Kimlik Doğrulaması** | Yetkisiz cihazların yönlendirme protokolüne dahil olup sahte rotalar enjekte etmesini engeller. | Otonom sistem sınırları, veri merkezi omurga router'ları | Parola yönetimi ve rotasyon yükü getirir; şifreleme CPU'ya ek yük bindirebilir (MD5/TCP-AO). |
| **Kara Delik Rotası (Black-hole Routing) Tanımlama** | Null0 veya blackhole rotaları, DDoS saldırısı altında olan veya kullanılmayan iç IP bloklarına gelen trafiği CPU harcamadan düşürmeyi sağlar. | Sınır Router'lar, Linux Rota Tabloları (`ip route add blackhole 10.99.0.0/24`) | Yanlışlıkla canlı bir servisin IP bloğu kara deliğe atılırsa kalıcı servis kesintisine yol açar. |
| **Route Filtering (Rota Filtreleme)** | Dinamik protokollerde komşulardan alınacak veya komşulara verilecek rotaları sınırlandırır. (Örn: RFC 1918 adreslerinin internete anons edilmesini engellemek). | Tüm Dinamik Yönlendirme (OSPF, BGP) yapılandırmaları | Topolojiye yeni eklenen meşru ağların filtreye takılarak anons edilememesi riski (Operasyonel takip gerektirir). |

## 10. Avantajlar, Dezavantajlar ve Trade-off'lar

Ağ tasarımlarında seçilen yöntemlerin getirdiği ödünleşimler (trade-offs):

### Statik Yönlendirme vs Dinamik Yönlendirme (OSPF/BGP)
*   **Statik Yönlendirme:**
    *   *Avantajlar:* Sıfır CPU/RAM yükü, tam deterministik yapı, yapılandırma kolaylığı, yüksek güvenlik (dışarıdan manipüle edilemez).
    *   *Dezavantajlar:* Ölçeklenemez. 100 router'lı bir ağda bir hat koptuğunda yüzlerce satırı elle güncellemek gerekir. Yedeklilik (failover) mekanizmaları kısıtlıdır.
*   **Dinamik Yönlendirme:**
    *   *Avantajlar:* Kendi kendini iyileştiren ağ (Self-healing). Hat koptuğunda saniyeler içinde alternatif yolu bulur (Convergence). İnsan müdahalesine ihtiyacı azdır.
    *   *Dezavantajlar:* Protokollerin çalışması için CPU ve hafıza tüketimi. Protokol açıklarından veya yanlış anonslardan kaynaklı küresel kesinti riski.

### Klasik Subnetting vs VLSM
*   **Klasik Subnetting (Sabit Uzunluk):** Ağ yönetimini ve takibini kolaylaştırır (Her yer /24 ise akılda tutması kolaydır). Ancak IP adres alanını çok hızlı tüketir ve büyük israfa yol açar.
*   **VLSM:** Adres israfını neredeyse sıfıra indirir. Ancak ağ dokümantasyonunu karmaşıklaştırır, troubleshooting esnasında hangi IP'nin hangi maskeye ait olduğunun sürekli hesaplanmasını gerektirir.

## 11. Yaygın Yanlış Anlamalar

### Yanlış Anlama 1
*   *Yanlış Düşünce:* Alt ağ maskesi (Subnet Mask) cihazın dış ağlarla konuşmasını engellemek için kullanılan bir güvenlik filtresidir.
*   *Neden Yanlış:* Alt ağ maskesi bir filtre değil, bir adres ayrıştırma aracıdır. Cihazın, hedefin yerel ağda olup olmadığını anlamasını sağlar. Eğer yerel ağda değilse paket zaten varsayılan ağ geçidine (Gateway) gönderilir. Gateway üzerinde engelleyici bir kural (ACL) yoksa paket her yere gidebilir.
*   *Gerçek Sistemdeki Etki:* Bir sunucuya `/30` verip "bu sadece yanındaki cihazla konuşsun" demek tek başına yetmez; yönlendirme tablosu temizlenmeli ya da firewall kuralları yazılmalıdır.

### Yanlış Anlama 2
*   *Yanlış Düşünce:* `ping` komutu çalışmıyorsa veya drop oluyorsa o hedefe giden yönlendirme yolu kesinlikle kopuktur.
*   *Neden Yanlış:* Paket yönlendirme tablosuna göre hedefe başarıyla ulaşıyor olabilir. Ancak hedef cihazdaki veya yol üstündeki bir firewall `ICMP Echo Request` paketlerini drop ediyor olabilir. Veya gidiş yolu sağlamdır ama dönüş yolu (Asymmetric Routing) kopuktur.
*   *Doğru Teknik Yaklaşım:* Yönlendirme kontrolü için sadece ping'e güvenilmemeli, `traceroute` ile yol analizi yapılmalı ve Layer 4 seviyesinde (`nc`, `telnet`) port testleri gerçekleştirilmelidir.

## 12. Troubleshooting ve Validation

Ağ yönlendirme sorunlarını sistematik olarak çözmek için işletim sistemi araçlarının doğru yorumlanması gerekir.

### Temel Linux Ağ Komutları ve Analizi

#### 1. Rota Tablosunu İnceleme
```bash
ip route show
```
*   *Ne işe yarar?:* Çekirdeğin güncel aktif yönlendirme tablosunu döner.
*   *Çıktı Yorumlama:*
    ```text
    default via 192.168.1.1 dev eth0 proto dhcp src 192.168.1.50 metric 100 
    10.244.0.0/16 dev cni0 proto kernel scope link src 10.244.0.1 
    192.168.1.0/24 dev eth0 proto kernel scope link src 192.168.1.50 metric 100
    ```
    *   `10.244.0.0/16` ağından bir IP'ye paket gideceğinde doğrudan `cni0` sanal arayüzüne (Kubernetes köprüsü) gönderilecektir (`scope link` yolda router olmadığını, hedefin doğrudan bağlı olduğunu belirtir).
    *   İnternete gidecek paketler (`default`) `192.168.1.1` gateway cihazına paslanacaktır.

#### 2. Kural Politikalarını İnceleme
```bash
ip rule show
```
*   *Ne işe yarar?:* Politika tabanlı yönlendirmenin (PBR) kurallarını listeler.
*   *Çıktı Yorumlama:*
    ```text
    0:      from all lookup local
    32766:  from all lookup main
    32767:  from all lookup default
    ```
    *   Sistem öncelikle `local` tablosuna bakar (kendi IP'leri ve loopback). Bulamazsa `main` (standart `ip route` tablosu) tablosuna geçer. Buraya özel kaynak IP'li kurallar eklenerek trafik manipüle edilebilir.

#### 3. `tracepath` ve `traceroute` ile Yol Analizi
```bash
tracepath -n 8.8.8.8
```
*   *Ne işe yarar?:* Hedefe giderken geçilen tüm router'ları adım adım listeler ve yol üzerindeki **Path MTU** değerini tespit eder (`tracepath` yetki gerektirmez ve UDP portları kullanır).
*   *Şüpheli Durum:* Çıktıda sürekli `* * *` veya `no reply` görülmesi, o noktadaki router'ın ICMP/UDP paketlerine yanıt vermeyi güvenlik gerekçesiyle kapattığını veya paketin orada drop olduğunu gösterir.

#### 4. Canlı Paket Yakalama (Packet Capture)
```bash
sudo tcpdump -nni eth0 icmp or port 179
```
*   *Ne işe yarar?:* `eth0` kartı üzerinden geçen ICMP (ping/PMTUD) veya port 179 (BGP) trafiğini canlı izler. Paketlerin sunucudan çıkıp çıkmadığını veya geri dönüp dönmediğini doğrulamak için kesin kanıttır.

## 13. Uygulamalı Doğrulama (Hands-on Lab)

**Amaç:** Linux üzerinde tamamen yalıtılmış iki network namespace oluşturmak, bunları sanal bir kablo (`veth pair`) ile birbirine bağlamak, yönlendirme tablolarını elle yazarak aralarında Layer 3 haberleşmesini sağlamak ve IP forwarding mekanizmasını doğrulamak.

### Ortam ve Gereksinimler
*   Herhangi bir modern Linux dağıtımı (Ubuntu, Debian, RHEL)
*   Kök kullanıcı (root) yetkisi (`sudo`)

### Oluşturulacak Mimari
```text
  [ Namespace: ns-alpha ]               [ Namespace: ns-beta ]
  IP: 10.10.10.2/24                     IP: 10.20.20.2/24
  Interface: veth-alpha                 Interface: veth-beta
           |                                     |
           +---------------+     +---------------+
                           |     |
                     [ Host Namespace ]
                     IP: 10.10.10.1/24 (veth-h-alpha)
                     IP: 10.20.20.1/24 (veth-h-beta)
                     Routing Enabled: net.ipv4.ip_forward=1
```

### Adımlar ve Teknik Nedenleri

```bash
# 1. Network Namespaces oluşturulması
sudo ip netns add ns-alpha
sudo ip netns add ns-beta
# Teknik Neden: Ağ yığınlarını çekirdek seviyesinde birbirini görmeyecek şekilde izole ediyoruz.

# 2. Sanal kabloların (veth pairs) üretilmesi
sudo ip link add veth-alpha type veth peer name veth-h-alpha
sudo ip link add veth-beta type veth peer name veth-h-beta
# Teknik Neden: İki ucu olan sanal ağ kabloları oluşturuyoruz. Bir ucunu namespace'e, diğer ucunu host'a bağlayacağız.

# 3. Kablo uçlarının ilgili isim alanlarına taşınması
sudo ip link set veth-alpha netns ns-alpha
sudo ip link set veth-beta netns ns-beta

# 4. Host tarafındaki arayüzlerin ayağa kaldırılması ve IP atanması
sudo ip link set veth-h-alpha up
sudo ip addr add 10.10.10.1/24 dev veth-h-alpha

sudo ip link set veth-h-beta up
sudo ip addr add 10.20.20.1/24 dev veth-h-beta
# Teknik Neden: Host makinesini bu sanal ağlara birer bacak ile dahil ediyoruz ki router görevi görebilsin.

# 5. Namespace içindeki arayüzlerin yapılandırılması
sudo ip netns exec ns-alpha ip link set veth-alpha up
sudo ip netns exec ns-alpha ip addr add 10.10.10.2/24 dev veth-alpha
sudo ip netns exec ns-alpha ip link set lo up

sudo ip netns exec ns-beta ip link set veth-beta up
sudo ip netns exec ns-beta ip addr add 10.20.20.2/24 dev veth-beta
sudo ip netns exec ns-beta ip link set lo up

# 6. NAMESPACE İÇİ YÖNLENDİRME TABLOLARININ DOLDURULMASI (Kritik Adım)
sudo ip netns exec ns-alpha ip route add default via 10.10.10.1
sudo ip netns exec ns-beta ip route add default via 10.20.20.1
# Teknik Neden: ns-alpha kendi ağı dışındaki (10.20.20.0 gibi) paketleri kime atacağını bilmez.
# Ona varsayılan ağ geçidi (default gateway) olarak host'un IP'sini gösteriyoruz.

# 7. HOST ÜZERİNDE IP FORWARDING AKTİFLEŞTİRİLMESİ
sudo sysctl -w net.ipv4.ip_forward=1
# Teknik Neden: Linux çekirdeğine, kendisine ait olmayan ama yönlendirme tablosunda eşleşen paketleri çöpe atmayıp diğer arayüze geçirmesini (yönlendirmesini) emrediyoruz.
```

### Doğrulama (Validation)

`ns-alpha` içinden `ns-beta` içindeki IP'ye ping atarak Layer 3 yönlendirmenin çalıştığını test edin:

```bash
sudo ip netns exec ns-alpha ping -c 3 10.20.20.2
```

**Beklenen Sonuç:**
```text
PING 10.20.20.2 (10.20.20.2) 56(84) bytes of data.
64 bytes from 10.20.20.2: icmp_seq=1 ttl=63 time=0.045 ms
64 bytes from 10.20.20.2: icmp_seq=2 ttl=63 time=0.061 ms
64 bytes from 10.20.20.2: icmp_seq=3 ttl=63 time=0.058 ms

--- 10.20.20.2 ping statistics ---
3 packets transmitted, 3 received, 0% packet loss, time 2046ms
rtt min/avg/max/mdev = 0.045/0.054/0.061/0.007 ms
```
*Not: TTL değerinin `63` gelmesi paketin yolda bir adet Layer 3 router'dan (Host makinesinin çekirdeğinden) geçtiğinin ve değerinin 1 azaltıldığının kesin kanıtıdır (İşletim sistemi varsayılan başlangıç TTL'i 64'tür).*

### Sorun Çıkarsa Kontrol Edilecek Noktalar
*   `ping: connect: Network is unreachable`: Namespace içindeki default gateway rotası (`ip route add default...`) eklenmemiş demektir.
*   Paketler gidiyor ama yanıt dönmüyorsa: Host üzerinde `iptables` veya `nftables` kuralları `FORWARD` zincirindeki paketleri drop ediyor olabilir. Kontrol etmek ve geçici izin vermek için: `sudo iptables -P FORWARD ACCEPT`.

### Temizleme (Clean-up)

Oluşturulan sanal yapıların ana sisteme zarar vermemesi için silinmesi:
```bash
sudo ip netns del ns-alpha
sudo ip netns del ns-beta
```
*Not: Namespace'ler silindiğinde onlara bağlı olan `veth` sanal kabloları çekirdek tarafından otomatik olarak temizlenir. Ekstra bir silme komutuna gerek yoktur.*

## 14. Production Ortamı Perspektifi

Laboratuvar ortamında elle yazılan rotalar veya tekil namespace'ler, canlı yükler altında ölçeklenemez ve yönetilemez hale gelir. Canlı ortam gerçekleri şunlardır:

*   **Kubernetes Ağ Alt Yapısı (CNI):** Büyük ölçekli Kubernetes cluster'larında (örn. 500+ node), binlerce Pod'un birbiriyle konuşması için statik rotalar yazılmaz. **Calico** gibi CNI bileşenleri, her node üzerinde bir BGP daemon (`bird`) çalıştırır. Her node, kendi üzerinde koşan Pod'ların IP bloklarını diğer node'lara BGP üzerinden dinamik olarak anons eder. Böylece cluster içi yakınsama (convergence) milisaniyeler seviyesine iner.
*   **Anycast Routing Yapıları:** Production ortamında yüksek erişilebilirlik (HA) sağlamak için coğrafi olarak farklı yerlerdeki sunuculara aynı IP adresi verilir. BGP protokolü sayesinde, internet üzerindeki kullanıcılar o IP'ye istek attığında internet servis sağlayıcıları paketi LPM ve BGP AS-Path kurallarına göre kullanıcıya en yakın/en stabil durumdaki veri merkezine yönlendirir (CDN ve DNS servislerinin temel çalışma mantığı).
*   **Bulut Altyapısı Limitleri (Quotas):** AWS veya Google Cloud üzerinde VPC Route Table kullanırken satır limiti (Route Table başına genellikle varsayılan 50 veya 100 rota) bulunur. Kurumsal ağları buluta bağlarken tüm iç ağ IP'lerini teker teker yazmak yerine `Route Summarization` (Özetleme) yapılarak tüm iç ağ örn. `10.0.0.0/8` şeklinde tek satırda buluta öğretilmelidir. Aksi takdirde limitlere takılınır ve ağ genişleyemez.

## 15. Diğer Teknolojilerle İlişkisi

*   **Linux Çekirdeği (Kernel Space):** Yönlendirme performansı, çekirdeğin ağ yığınının (sk_buff yapıları) ne kadar hızlı işlendiğine bağlıdır. Yüksek throughput gerektiren production sistemlerinde geleneksel yönlendirme tabloları yerine **XDP (eXpress Data Path)** veya **eBPF** kullanılarak paketler daha kernel space'in üst katmanlarına çıkmadan, doğrudan ağ kartı sürücüsü seviyesinde yönlendirilebilir (Cilium CNI mimarisi).
*   **Infrastructure as Code (Terraform):** Bulut ortamlarında alt ağ tasarımı ve yönlendirme tabloları elle kurulmaz. Aşağıdaki gibi deklaratif kod blokları ile altyapı kod olarak yönetilir:
    ```hcl
    resource "aws_route_table" "prod_route" {
      vpc_id = aws_vpc.main.id
      route {
        cidr_block = "0.0.0.0/0"
        gateway_id = aws_internet_gateway.gw.id
      }
    }
    ```

## 16. Junior Seviyede Bilinmesi Gerekenler

Junior bir sistem, ağ veya platform mühendisinin mülakatlarda ve günlük operasyonlarda kesinlikle bilmesi gereken asgari seviye:

*   **Açıklayabilmeli:** `/24` ile `/23` maskesi arasındaki farkı, hangisinin daha fazla cihaz barındırabileceğini ve nedenini.
*   **Ayırt Edebilmeli:** Rota tablosundaki `Gateway` kavramı ile Layer 2 anahtar (Switch) arasındaki farkı. Paket yerel ağ dışına çıkmıyorsa ağ geçidine uğramayacağını bilmeli.
*   **Doğrulayabilmeli:** Bir Linux sunucuda internete çıkış sorunu olduğunda `ip route` komutunu çalıştırıp `default` rotanın varlığını ve doğruluğunu kontrol edebilmeli.
*   **Yanlış Anlamamalı:** Private IP adreslerinin (`192.168.x.x`, `10.x.x.x`) internet üzerinde doğrudan yönlendirilemeyeceğini, bu yüzden evdeki veya şirketteki bu IP'lerin internete çıkarken kesinlikle bir NAT işleminden geçmek zorunda olduğunu unutmamalı.

## 17. Kısa Özet

1.  **IPv4 Adresleme:** 32 bitlik mantıksal adresleme sistemidir; Ağ (Network) ve Cihaz (Host) bölümlerinden oluşur.
2.  **Alt Ağ Maskesi (Subnet Mask):** Bir IP adresinin hangi bitlerinin ağa, hangi bitlerinin hosta ait olduğunu mantıksal AND işlemiyle belirler.
3.  **CIDR Notasyonu:** Maskedeki 1 bitlerinin sayısını eğik çizgiyle (örn. `/24`) gösteren sınıfsız adresleme standartıdır.
4.  **Kullanılabilir IP Formülü:** Bir alt ağda ilk adres Ağ Adresi, son adres Yayın (Broadcast) adresidir. Cihazlara atanabilen IP sayısı $2^{\text{host bitleri}} - 2$ ile bulunur.
5.  **LPM (Longest Prefix Match):** Rota tablosunda bir hedef IP ile eşleşen birden fazla kural varsa maskesi en uzun (en spesifik) olan rota kazanır.
6.  **Varsayılan Rota (`0.0.0.0/0`):** Tabloda hedef IP ile eşleşen spesifik bir kural bulunamadığında paketin gönderildiği son çare rotasıdır.
7.  **Yönetimsel Mesafe (AD):** Farklı yönlendirme kaynaklarının güvenilirlik derecesidir. Değer düştükçe rotanın önceliği artırılır.
8.  **Dinamik Yönlendirme:** OSPF (Link-State) iç ağlarda hız ve topoloji haritası için kullanılırken; BGP (Path-Vector) internetin omurgasında otonom sistemler arası yönlendirmeyi sağlar.
9.  **IP Fragmantasyonu ve PMTUD:** Paket boyutu hat MTU'sundan büyükse parçalanır. Performans kaybını önlemek için PMTUD mekanizması ile en uygun MTU dinamik olarak keşfedilir.
10. **Linux Politika Tabanlı Yönlendirme:** `ip rule` ve `ip route` bileşenleri yardımıyla sadece hedefe göre değil, kaynak IP veya protokole göre de gelişmiş yönlendirme yapılabilir.
11. **Güvenlik Tehditleri:** BGP mimarisindeki doğrulanmamış anonslar Rota Gaspına (Route Hijacking) yol açar; korunmak için RPKI altyapısı şarttır.
12. **Troubleshooting Sıralaması:** Ağ yönlendirme hatalarında sırasıyla `ip route`, `ip rule`, `tracepath` kontrol edilmeli ve canlı trafik `tcpdump` ile izlenmelidir.

## 18. Kaynaklar

*   **RFC 791 — Internet Protocol (IP) Specification**
    *   *Doğrulanan Konu:* IPv4 başlık yapısı, fragmantasyon mekanizması ve TTL çalışma prensipleri.
*   **RFC 1519 & RFC 4632 — Classless Inter-Domain Routing (CIDR)**
    *   *Doğrulanan Konu:* Sınıfsız adresleme mimarisi ve yönlendirme tablolarında LPM (Longest Prefix Match) algoritmasının standardizasyonu.
*   **RFC 1918 — Address Allocation for Private Internets**
    *   *Doğrulanan Konu:* İnternet üzerinde yönlendirilmeyen yerel ağlara tahsis edilmiş özel (private) IP adres bloklarının tanımları.
*   **Linux Çekirdeği Ağ Dokümantasyonu (Kernel Networking Subsystem)**
    *   *Doğrulanan Konu:* `ip-sysctl.txt` içindeki `net.ipv4.ip_forward` ve `rp_filter` (Reverse Path Filtering) parametrelerinin çalışma modları.
*   **Cisco IOS Yönlendirme Teknolojileri Kılavuzu (Routing Technologies Guide)**
    *   *Doğrulanan Konu:* Administrative Distance (AD) varsayılan değerleri, OSPF ve BGP protokol mimarilerinin temel metrik hesaplamaları.
```
