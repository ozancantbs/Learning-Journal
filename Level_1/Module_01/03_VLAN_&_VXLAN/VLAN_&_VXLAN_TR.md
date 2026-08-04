# 03 — VLAN & VXLAN

Bu doküman, bilgisayar ağlarında trafiğin mantıksal olarak yalıtılması, ölçeklenmesi ve yönetilmesi amacıyla kullanılan Yerel Alan Ağı Sanallaştırması (VLAN) ile Genişletilebilir Sanal Yerel Alan Ağı (VXLAN) teknolojilerini ele almaktadır. Modül kapsamında, geleneksel Katman 2 (Layer 2) segmentasyon yaklaşımlarından modern bulut ve veri merkezi altyapılarında kullanılan Katman 3 (Layer 3) üst ağ (overlay) mimarilerine geçiş süreci, mimari bileşenler, güvenlik riskleri ve sorun giderme (troubleshooting) pratikleri incelenmektedir. Bu dokümanı tamamlayan okuyucu, modern ağ segmentasyonu mimarilerini tasarlama, doğrulama ve operasyonel hataları ayıklama yetkinliği kazanacaktır.

## 1. Genel Bakış

Ağ segmentasyonu (Network segmentation), büyük ve tek parça bir ağ altyapısının performans, güvenlik ve yönetilebilirlik gerekçeleriyle daha küçük, mantıksal veya fiziksel alt parçalara bölünmesi işlemidir. Geleneksel ağlarda tek bir büyük yayın alanı (Broadcast Domain), ağa bağlı tüm cihazların ürettiği yayın trafiğinin (örneğin ARP istekleri) tüm ağa yayılmasına neden olur. Bu durum hem bant genişliği tüketimini artırır hem de güvenlik açısından yanal hareket (lateral movement) riskini doğurur.

Mantıksal segmentasyon teknolojileri olan VLAN ve VXLAN, fiziksel altyapıyı değiştirmeden ağ içinde mantıksal sınırlar çekerek yayın alanlarını izole eder (Broadcast-domain isolation). Klasik ağ mimarilerinde Katman 2 seviyesinde yalıtım sağlayan VLAN teknolojisi kullanılırken; bulut bilişim, çoklu kiralama (multi-tenancy) ve devasa ölçekli veri merkezlerinin ortaya çıkışıyla birlikte Katman 3 altyapıları üzerinde sanal Katman 2 tünelleri oluşturan VXLAN teknolojisi standart hâle gelmiştir.

## 2. Neden Önemlidir?

Modern altyapı mimarilerinde ağ segmentasyonu ve tünelleme protokolleri şu disiplinler için kritik öneme sahiptir:

*   **Ağ Mühendisliği:** Fiziksel kablolama ve cihaz maliyetlerini düşürürken, esnek ve dinamik ağ topolojileri oluşturmayı sağlar. Veri merkezlerinde Spanning Tree Protocol (STP) kısıtlamalarından kurtularak tüm fiziksel hatların etkin kullanılmasını (ECMP) mümkün kılar.
*   **Siber Güvenlik ve Defansif Güvenlik:** Saldırı yüzeyini (attack surface) daraltır. Kritik sunucu altyapılarını, misafir ağlarını ve yönetim arayüzlerini birbirinden yalıtarak bir alt ağdaki ihlalin diğer ağlara sıçramasını engeller.
*   **Bulut ve Platform Mühendisliği (Cloud & Platform Engineering):** Kubernetes gibi konteyner orkestrasyon platformlarında ve genel bulut (Public Cloud) altyapılarında, farklı müşterilere veya izole pod ağlarına birbirleriyle çakışan IP adres alanları (overlapping IPs) atayabilme esnekliği sunar.

## 3. Temel Kavramlar ve Terminoloji

### Fiziksel ve Mantıksal Segmentasyon (Physical vs Logical Segmentation)
Fiziksel segmentasyon, ağları birbirinden ayırmak için her bir grup veya departman adına ayrı anahtarlar (switch), kablolar ve yönlendiriciler (router) konumlandırılmasıdır. Esnek değildir ve yüksek maliyetlidir. Mantıksal segmentasyon ise aynı fiziksel donanım üzerinde yazılımsal ve mantıksal etiketler kullanarak yalıtılmış ağlar oluşturma işlemidir.

### VLAN Tanımlayıcıları (VLAN IDs)
802.1Q standardında bir çerçevenin hangi sanal ağa ait olduğunu belirleyen 12 bitlik bir alandır. Matematiksel olarak $2^{12} = 4096$ adet benzersiz değer alabilir:
*   **0 ve 4095:** Sistem kullanımı için ayrılmıştır, portlara atanamaz.
*   **1:** Varsayılan (Default) VLAN'dır. Cihazlar ilk açıldığında tüm portlar bu VLAN'a dahildir. Silinemez veya adı değiştirilemez.
*   **2 - 1005:** Standart VLAN aralığıdır. Günlük ağ operasyonlarında kullanılır.
*   **1006 - 4094:** Genişletilmiş (Extended) VLAN aralığıdır. Genellikle servis sağlayıcılar ve büyük veri merkezleri tarafından tercih edilir; switch üzerinde VTP (VLAN Trunking Protocol) şeffaf (transparent) modda değilse yerel olarak saklanamaz.

### Erişim ve Taşıma Portları (Access Ports vs Trunk Ports)
*   **Access Port (Erişim Portu):** Yalnızca tek bir VLAN'a hizmet veren portlardır. Son kullanıcı cihazları (PC, yazıcı, sunucu arayüzleri) bu portlara bağlanır. Bu porttan çıkan çerçevelerde standart Ethernet yapısı korunur, herhangi bir VLAN etiketi bulunmaz.
*   **Trunk Port (Taşıma Portu):** Tek bir fiziksel hat üzerinden birden fazla VLAN trafiğini taşımak üzere yapılandırılan switch'ler arası veya switch-router arası otoban hatlarıdır. Geçen paketlerin karışmaması için etiketleme mekanizmasına ihtiyaç duyar.

### IEEE 802.1Q ve VLAN Etiket Yapısı (VLAN Tag Structure)
Trunk hatlarında standart etiketleme protokolü IEEE 802.1Q'dur. Orijinal Ethernet çerçevesinin Kaynak MAC (Source MAC) alanı ile Tip/Uzunluk (Type/Length) alanı arasına **4 baytlık (32 bit)** bir veri ekler:

```
+-------------------+---------------------+-----------------+---------+
| Dest/Src MAC (12B)|  802.1Q Tag (4B)    | Type/Length (2B)| Data... |
+-------------------+---------------------+-----------------+---------+
                        |
                        +--> TPID (16 bit): 0x8100
                        +--> TCI  (16 bit):
                              |--> PCP (3 bit)
                              |--> DEI (1 bit)
                              |--> VLAN ID (12 bit)
```

*   **TPID (Tag Protocol Identifier - 16 bit):** `0x8100` değerini alarak çerçevenin 802.1Q ile etiketlendiğini belirtir.
*   **TCI (Tag Control Information - 16 bit):** Üç alt alandan oluşur:
    *   **PCP (Priority Code Point - 3 bit):** Hizmet Kalitesi (QoS) ve CoS işlemlerinde çerçevenin öncelik sınıfını (0-7 arası) belirtir.
    *   **DEI (Drop Eligible Indicator - 1 bit):** Ağda yoğunluk yaşandığında paketin öncelikli olarak düşürülüp düşürülemeyeceğini belirler.
    *   **VLAN ID (VID - 12 bit):** Çerçevenin ait olduğu mantıksal ağı temsil eden kimlik numarasıdır.

### Etiketli ve Etiketsiz Çerçeveler (Tagged and Untagged Frames)
*   **Tagged Frame:** İçerisinde 4 baytlık 802.1Q başlığı taşıyan çerçevelerdir. Trunk hatlar üzerinde dolaşırlar.
*   **Untagged Frame:** Standart Ethernet yapısına sahip, üzerinde VLAN etiketi barındırmayan çerçevelerdir. Erişim portlarında ve son kullanıcı cihazlarında işlem görürler.

### Yerel VLAN (Native VLAN)
Bir trunk portuna gelen etiket taşımayan (untagged) çerçevelerin hangi VLAN'a dahil edileceğini belirleyen kural setidir. Varsayılan olarak tüm üreticilerde VLAN 1'dir. Hat genelinde kontrol trafiğinin (CDP, LLDP, STP, DTP) taşınmasında rol oynar. Karşılıklı trunk portlarında eşleşmek zorundadır.

### Ses VLAN Temelleri (Voice VLAN Fundamentals)
Aynı fiziksel erişim portu üzerinden hem IP Telefon (VoIP) ses trafiğini hem de bilgisayar veri trafiğini taşımak için kullanılan hibrit bir mekanizmadır. Telefon, switch'ten aldığı CDP/LLDP bilgisiyle ses paketlerini etiketli (tagged) olarak gönderirken, arkasına bağlı bilgisayarın veri paketlerini etiketsiz (untagged) olarak iletir. Bu sayede ses trafiğine öncelik (QoS) verilirken veri yalıtımı da sürdürülür.

### VXLAN Kavramları: Underlay, Overlay, VNI, VTEP
*   **Underlay Network (Alt Ağ):** Fiziksel anahtarlar, yönlendiriciler ve kablolardan oluşan altyapıdır. Tek görevi, Katman 3 yönlendirme protokolleri (OSPF, BGP, IS-IS) aracılığıyla IP paketlerini en kararlı ve hızlı şekilde iletmektir.
*   **Overlay Network (Üst Ağ):** Underlay ağın üzerinde oluşturulan, fiziksel topolojiden bağımsız sanal tünel ağıdır. VXLAN bu katmanda çalışarak Katman 2 çerçevelerini tüneller vasıtasıyla taşır.
*   **VNI (VXLAN Network Identifier):** VLAN ID'nin yerini alan **24 bitlik** tanımlayıcı alandır. Değeri $2^{24} = 16.777.216$ adet benzersiz sanal ağ oluşturulmasına imkan tanır.
*   **VTEP (VXLAN Tunnel Endpoint):** VXLAN tünellerinin başlangıç ve bitiş noktası olan bileşendir. Donanımsal bir switch veya hypervisor seviyesinde yazılımsal bir modül olabilir. Yerel ağdan gelen Katman 2 çerçevesini alır, VXLAN başlığı ekleyerek Katman 3 paketine sarar (Encapsulation) veya tünelden gelen paketi açarak (Decapsulation) yerel ağa bırakır.

## 4. Çalışma Mantığı

### VLAN'lar Arası Yönlendirme (Inter-VLAN Routing)
Farklı VLAN'lara üye cihazlar doğrudan Katman 2 seviyesinde haberleşemez. İletişim için bir Katman 3 cihazına (Router veya Katman 3 Switch) ihtiyaç duyulur.

#### Router-on-a-Stick (RoaS)
Fiziksel bir yönlendiricinin tek bir arayüzünün (interface) switch üzerindeki bir trunk portuna bağlanması yöntemidir. Yönlendirici üzerinde her bir VLAN için mantıksal alt arayüzler (subinterface) oluşturulur ve her alt arayüze ilgili VLAN'ın varsayılan ağ geçidi (default gateway) IP adresi atanır. Trafik, switch'ten yönlendiriciye gider, orada yönlendirilir ve aynı fiziksel hat üzerinden farklı bir VLAN etiketiyle switch'e geri döner. Bu durum "tek hat üzerinde darboğaz" (bottleneck) riski yaratır.

#### SVI (Switched Virtual Interfaces) ve Katman 3 Anahtarlama
Modern ağlarda Inter-VLAN yönlendirme, Katman 3 switch'ler içerisindeki mantıksal yönlendirme arayüzleri (SVI) kullanılarak gerçekleştirilir. Her VLAN için bir `interface Vlan <ID>` oluşturulur. Yönlendirme işlemi, cihazın donanımsal ASIC (Application-Specific Integrated Circuit) yongaları üzerinde hat hızında (wire-speed) yapıldığından RoaS modelindeki gecikme ve darboğaz sorunları yaşanmaz.

### VXLAN Trafik Yönlendirme ve Öğrenme Mekanizmaları
VXLAN veri düzleminde (data plane) "Mac-in-UDP" enkapsülasyonu kullanır. Bir VTEP'in, hedef MAC adresinin hangi uzak VTEP arkasında olduğunu öğrenmesi için üç farklı yöntem mevcuttur:

#### Flood-and-Learn (Taşır ve Öğren)
Geleneksel veri düzlemi öğrenme mekanizmasıdır. Bir VTEP, bilmediği bir hedef MAC adresine (Unicast/Broadcast/Multicast) sahip bir çerçeve aldığında, bu paketi underlay ağda önceden tanımlanmış bir IP Çoklu Gönderim (Multicast) grubuna gönderir. Gruptaki tüm VTEP'ler paketi alır, açar ve kaynak MAC adresinin hangi kaynak VTEP IP'sine ait olduğunu yerel MAC tablolarına kaydeder. Ölçeklenebilirliği düşüktür ve ağ üzerinde gereksiz trafik yükü oluşturur.

#### Head-end Replication (Giriş Çoğaltması / Ingress Replication)
Multicast trafiğinin underlay ağda desteklenmediği durumlarda kullanılır. VTEP, gelen tek bir yayılım paketini (broadcast/multicast) kendisi çoğaltarak, statik olarak listelenmiş veya dinamik olarak öğrenilmiş diğer tüm uzak VTEP'lerin IP adreslerine ayrı ayrı Tekli Gönderim (Unicast) paketleri halinde gönderir.

#### EVPN ve BGP EVPN Temelleri (Kontrol Düzlemi Kontrolü)
Modern veri merkezlerinde "Flood-and-Learn" yerine kontrol düzlemi (control plane) protokolü olarak **MP-BGP EVPN (Multiprotocol BGP Ethernet VPN)** kullanılır. BGP EVPN, VTEP'lerin arkasındaki cihazların MAC ve IP adreslerini veri düzlemindeki taşmalarla değil, BGP yönlendirme güncellemeleri (**MAC/IP Advertisement - Route Type 2**) aracılığıyla birbirlerine duyurmasını sağlar. Bu sayede bilinmeyen tekli gönderim (Unknown Unicast) trafiği için ağ genelinde taşma (flooding) yapılmasına gerek kalmaz, ağ trafiği optimize edilir.

## 5. Mimari ve Bileşenler

### Geleneksel Kampüs Ağı vs Spine-Leaf Mimarisi
Geleneksel üç katmanlı (Core, Distribution, Access) ağ yapılarında trafik ağırlıklı olarak istemciden sunucuya doğrudur (Kuzey-Güney trafiği). Bu mimaride döngüleri önlemek için Spanning Tree Protocol (STP) kullanılır ve yedekli hatların yarısı bloklanır.

Modern veri merkezlerinde ise trafik sunucuların kendi arasındadır (Doğu-Batı trafiği). Bu trafiği optimize etmek için iki katmanlı **Spine-Leaf Mimarisi** kullanılır. Spine-Leaf mimarisi, VXLAN'ın underlay temelini oluşturur.

```
       +-----------------+       +-----------------+
       |  Spine Switch 1 |       |  Spine Switch 2 |  <-- Underlay (Katman 3 / IP Fabric)
       +-----------------+       +-----------------+
          /     \                   /     \
         /       \                 /       \
        /         \               /         \
+---------------+ +---------------+ +---------------+
| Leaf 1 (VTEP) | | Leaf 2 (VTEP) | | Leaf 3 (VTEP) |  <-- VXLAN Sınırı (Edge)
+---------------+ +---------------+ +---------------+
    |       \         /       |         |       \
[Sunucu A]  [Sunucu B]    [Sunucu C] [Sunucu D]  [Sunucu E] <-- Overlay (Sanal Katman 2)
```

*   **Spine Katmanı:** Ağın çekirdeğidir. Leaf switch'ler arasındaki paketleri yönlendirir. Spine switch'ler VXLAN tünel durumlarını veya MAC adreslerini tutmaz; yalnızca dış IP başlığına göre yönlendirme yapar.
*   **Leaf Katmanı:** Sunucuların, depolama ünitelerinin ve güvenlik duvarlarının bağlandığı katmandır. Her bir Leaf switch birer **VTEP** noktasıdır. Enkapsülasyon ve dekapsülasyon işlemleri burada gerçekleşir.
*   **ECMP (Equal-Cost Multi-Pathing):** Leaf'ler arası trafik, tüm aktif Spine switch'ler üzerinden yük dengeli olarak dağıtılır. STP'deki gibi hatların bloklanması söz konusu değildir.

## 6. Adım Adım Gerçek Teknik Akış

Bir VXLAN ağında, VNI 10010 içerisindeki Sunucu A'nın ($MAC_A$, $IP_A$), farklı bir kabindeki Sunucu B'ye ($MAC_B$, $IP_B$) paket gönderme süreci adım adım aşağıda açıklanmıştır:

```
[Sunucu A] -> (L2 Frame) -> [Leaf 1 (VTEP 1)] -> (VXLAN/UDP/IP) -> [Spine] -> [Leaf 2 (VTEP 2)] -> (L2 Frame) -> [Sunucu B]
```

1.  **Çerçeve Üretimi:** Sunucu A, hedef olarak Sunucu B'nin MAC adresini yerleştirerek standart bir Ethernet çerçevesi üretir ve bağlı olduğu **Leaf 1** switch'inin erişim (access) portuna gönderir.
2.  **VNI Eşleştirme ve Kontrol Düzlemi Sorgusu:** Çerçeve Leaf 1'e ulaştığında, portun bağlı olduğu VLAN tanımlaması okunur ve bu VLAN'ın eşleştiği **VNI 10010** tespit edilir. Leaf 1, BGP EVPN tablosunu kontrol ederek $MAC_B$ adresinin **Leaf 2 (VTEP 2)** arkasında konumlandığını doğrular.
3.  **VXLAN Enkapsülasyonu:** Leaf 1, orijinal Ethernet çerçevesini bozmadan alır ve sırasıyla şu başlıkları ekler:
    *   **VXLAN Başlığı:** 8 bayt uzunluğundadır ve VNI alanı `10010` olarak set edilir.
    *   **Dış UDP Başlığı:** Kaynak portu orijinal paketin hash değerine göre dinamik belirlenir (ECMP dengelenmesi için), hedef port ise standart IANA değeri olan **4789** olarak yazılır.
    *   **Dış IP Başlığı:** Kaynak IP adresi olarak *VTEP 1 Loopback IP*, hedef IP adresi olarak *VTEP 2 Loopback IP* yazılır.
    *   **Dış Ethernet Başlığı:** Paketin bir sonraki atlamadaki (next-hop) Spine switch'e ulaşabilmesi için ilgili fiziksel MAC adresleri eklenir.
4.  **Underlay Yönlendirme:** Paket fiziksel ağa (Spine katmanı) bırakılır. Spine switch'ler yalnızca Dış IP başlığındaki *VTEP 2 IP* adresine bakar ve standart IP yönlendirme protokollerine göre paketi Leaf 2'ye ulaştırır.
5.  **Dekapsülasyon:** Paket Leaf 2'ye (VTEP 2) ulaştığında, dış UDP portunun 4789 olduğu görülerek paket VXLAN işleme modülüne aktarılır. Dış başlıklar (Ethernet, IP, UDP, VXLAN) tamamen sökülür.
6.  **Teslimat:** Ortaya çıkan orijinal Katman 2 çerçevesi, VNI 10010 ile ilişkili olan yerel port üzerinden doğrudan Sunucu B'ye iletilir. Sunucu B paketi aldığında tünelleme işleminden habersizdir.

## 7. Gerçek Dünya Kullanım Senaryoları

### Senaryo 1: Kurumsal Şirket Kampüs Ağı Tasarımı
*   **Problem:** Tek bir yerel ağ üzerinde muhasebe, misafirler ve sistem yöneticilerinin cihazlarının aynı yayın alanında bulunması, hassas verilerin koklanması (sniffing) ve yetkisiz erişim riski oluşturmaktadır.
*   **Yaklaşım:** Ağ, VLAN yapılandırması ile segmentlere ayrılır:
    *   `VLAN 10 - User`: Standart personel bilgisayarları.
    *   `VLAN 20 - Server`: Şirket içi yerel sunucular.
    *   `VLAN 30 - Guest`: Şirkete gelen misafirler için yalnızca internet çıkışına izin verilen izole ağ.
    *   `VLAN 99 - Management`: Switch, router ve firewall cihazlarının yönetim arayüzleri.
*   **Fayda ve Karmaşıklık:** Ağ geçitleri Katman 3 switch üzerinde SVI arayüzlerinde sonlandırılır ve aralarına Erişim Kontrol Listeleri (ACL) yazılarak kullanıcıların yönetim ağına erişimi tamamen engellenir. Bu durum, statik ACL'lerin büyümesiyle birlikte operasyonel yönetim yükü getirir.

### Senaryo 2: Çoklu Kiracılı (Multi-Tenant) Bulut Veri Merkezi ve Kubernetes CNI Yapılandırması
*   **Problem:** Bir veri merkezinde hizmet alan binlerce farklı müşterinin (kiracı) sanal makinelerinin veya Kubernetes pod ağlarının izole edilmesi gerekmektedir. Geleneksel 4096 VLAN sınırı yetersiz kalmaktadır ve müşteriler kendi iç ağlarında aynı IP bloklarını (örneğin `10.0.0.0/24`) kullanmak istemektedir.
*   **Yaklaşım:** Fiziksel altyapı Katman 3 Spine-Leaf mimarisine dönüştürülür ve üzerine VXLAN Overlay ağı kurulur. Kubernetes ortamında Calico veya Cilium gibi CNI (Container Network Interface) eklentileri VXLAN tünelleme modunda yapılandırılır.
*   **Fayda:** Her kiracıya veya izole pod ağına benzersiz bir VNI (örneğin Müşteri A için VNI 100001, Müşteri B için VNI 100002) atanır. 16 milyon ağ sınırı sayesinde ölçekleme problemi yaşanmaz. Farklı VNI'lar içindeki çakışan IP'ler başlık enkapsülasyonu sayesinde birbirine karışmaz.

## 8. Güvenlik Perspektifi

### VLAN Hopping (VLAN Atlama Saldırıları)
Saldırganın izni olmayan farklı bir VLAN'a ait trafiğe erişmesi veya o VLAN'a paket enjekte etmesi durumudur. İki temel yöntemle gerçekleştirilir:

#### 1. Switch Spoofing (Anahtar Taklidi)
*   **Teknik Sorun:** Portların varsayılan olarak dinamik trunk pazarlığına (DTP - Dynamic Trunking Protocol) açık olması.
*   **Çalışma Mantığı:** Saldırgan kendi cihazından DTP paketleri göndererek bağlı olduğu switch portunu kandırır ve portun "Trunk" moduna geçmesini sağlar. Trunk moduna geçen port üzerinden ağdaki tüm VLAN'lara ait etiketli paketleri kabul edebilir veya gönderebilir.
*   **Ön Koşullar:** Switch portunun `dynamic desirable` veya `dynamic auto` modunda bırakılmış olması.
*   **Savunma:** Kullanıcı portları kesinlikle statik erişim (`switchport mode access`) moduna alınmalı ve DTP protokolü kapatılmalıdır (`switchport nonegotiate`).

#### 2. Double Tagging (Çift Etiketleme)
*   **Teknik Sorun:** Switch'in yerel (Native) VLAN paketlerini taşırken etiketi sökme ve kontrol etmeme davranışı.
*   **Çalışma Mantığı:** Saldırgan, gönderdiği paketin üzerine üst üste iki adet VLAN etiketi ekler. Dıştaki etiket, mevcut portun dahil olduğu yerel (Native) VLAN ID'si ile aynıdır (örneğin VLAN 1). İçteki etiket ise hedef alınan kurbanın VLAN ID'sidir (örneğin VLAN 10). İlk switch paketi aldığında, dıştaki etiket Native VLAN'a ait olduğu için o etiketi söker ve paketi trunk hattına etiketsiz veya içteki etiketle iletir. Bir sonraki switch içteki etiketi (VLAN 10) görür ve paketi doğrudan kurbanın ağına yönlendirir. Bu saldırı tek yönlüdür (saldırgan dönen paketleri alamaz ancak paket enjekte edebilir).
*   **Ön Koşullar:** Saldırganın Native VLAN ile aynı VLAN'da bulunması ve trafiğin bir trunk hattından geçmek zorunda olması.
*   **Savunma:** Trunk hatlarında kullanılan Native VLAN ID'si, hiçbir kullanıcı arayüzüne atanmamış, boşta duran atıl bir VLAN (örneğin VLAN 999) olarak değiştirilmelidir.

### Native-VLAN ve Trunk Yanlış Yapılandırmaları (Misconfigurations)
*   **Risk:** Karşılıklı iki switch arasındaki trunk hattında Native VLAN tanımlarının farklı yapılması (Native VLAN Mismatch).
*   **Etki:** Bir switch'ten VLAN 5 üzerinden çıkan etiketsiz paket, karşı switch tarafından kendi Native VLAN'ı (örneğin VLAN 10) olarak kabul edilir. Bu durum veri sızıntılarına (VLAN Leakage), paket döngülerine ve Spanning Tree kararsızlıklarına yol açar.

### Özel VLAN Kavramları (Private VLAN - PVLAN)
Aynı VLAN içerisindeki cihazların Katman 2 seviyesinde birbirleriyle olan iletişimini daha alt düzeyde kısıtlamak için kullanılan mekanizmadır. Port tipleri üçe ayrılır:
*   **Promiscuous (Serbest Port):** Ağdaki tüm portlarla (Isolated ve Community) konuşabilir. Genellikle yönlendirici, firewall veya merkezi sunucuların bağlı olduğu porttur.
*   **Isolated (Yalıtılmış Port):** Yalnızca Promiscuous portlar ile konuşabilir. Aynı VLAN içerisindeki diğer Isolated portlar dahil hiç kimseyle haberleşemez. (Örnek: Otel odaları, DMZ sunucuları).
*   **Community (Topluluk Portu):** Kendi grubu içerisindeki diğer topluluk portlarıyla ve Promiscuous port ile konuşabilir. Farklı bir Community grubuyla veya Isolated portlarla konuşamaz.

### Spanning Tree İlişkisi
Geleneksel ağlarda her bir VLAN için arkada ayrı bir STP ağacı çalışır (Per-VLAN Spanning Tree - PVST+). Bu durum CPU kaynaklarını tüketebilir ancak VLAN bazlı yük dengelemeye izin verir. VXLAN mimarisinde ise underlay ağ tamamen Katman 3 yönlendirmeye dayandığı için hat bazlı döngü oluşma ihtimali yoktur; dolayısıyla veri merkezlerinde STP'ye olan ihtiyaç ortadan kalkar.

### VXLAN Güvenlik Zafiyetleri ve Sınırlamaları
VXLAN enkapsülasyonu varsayılan olarak herhangi bir şifreleme (encryption) veya kimlik doğrulama (authentication) mekanizması içermez. Paketler underlay ağ üzerinde açık metin (cleartext) olarak taşınır. Bu durum, fiziksel hatta araya girebilecek bir saldırganın tünel içerisindeki tüm veri trafiğini görebilmesi anlamına gelir. Savunma amacıyla VXLAN trafiğinin alt katmanda **IPsec** veya **MACsec** protokolleri ile şifrelenmesi gerekmektedir.

## 9. Hardening ve Best Practices

Ağ altyapısının güvenli ve performanslı çalışması için uygulanması gereken sıkılaştırma adımları aşağıda listelenmiştir:

| Kontrol | Neden Önemli? | Uygulama Alanı | Sınırlama / Trade-off |
| :--- | :--- | :--- | :--- |
| **DTP Kapatılması & Statik Port Tanımı** | Switch Spoofing ve yetkisiz trunk kurulumunu engeller. | Tüm kullanıcı erişim portları. | Dinamik cihaz yönetimini zorlaştırır, manuel yapılandırma gerektirir. |
| **Native VLAN Değişimi & Etiketlenmesi** | Double Tagging saldırılarını ve veri sızıntılarını önler. | Tüm Switch-to-Switch Trunk hatları. | Ağ genelinde tutarlı yapılandırılmazsa bağlantı kopmalarına neden olur. |
| **Kullanılmayan Portların Kapatılması** | Fiziksel olarak ağa sızma girişimlerini engeller. | Boşta duran tüm switch portları. | Yeni bir cihaz bağlanacağında operasyonel müdahale ve zaman maliyeti doğurur. |
| **Jumbo Frames (MTU 9000) Yapılandırması** | VXLAN tünel ek yükünden (50-54 Bayt) kaynaklı paket parçalanmasını (fragmentation) önler. | Tüm Underlay Ağ Cihazları ve Arayüzleri. | Tüm altyapı cihazlarının uçtan uca yüksek MTU boyutunu desteklemesi zorunluluğu. |

## 10. Avantajlar, Dezavantajlar ve Trade-off'lar

### Performans ve Ölçeklenebilirlik
*   **VLAN:** Donanımsal olarak çok düşük gecikmeyle çalışır ancak en fazla 4096 adet ağ oluşturulabilir. Büyük ölçekli bulut ortamlarında bu sınır bir tıkanma noktasıdır.
*   **VXLAN:** 16 milyon VNI desteğiyle devasa bir ölçek sunar. Ancak paketlerin üzerine eklenen 50 baytlık ek yük nedeniyle veri iletim verimliliğini hafifçe düşürebilir ve paketlerin donanımsal ASIC'ler yerine yazılımla işlendiği eski cihazlarda CPU yükü oluşturur.

### Operasyonel Karmaşıklık
*   **VLAN:** Yapılandırması ve izlenmesi kolaydır. Geleneksel sorun giderme araçları (Wireshark vb.) yerel etiketleri doğrudan okuyabilir.
*   **VXLAN:** Arkasında BGP EVPN gibi karmaşık kontrol düzlemi protokolleri barındırır. Overlay ve underlay ayrımı nedeniyle hata tespiti daha fazla uzmanlık gerektirir.

## 11. Yaygın Yanlış Anlamalar

### Yanlış Anlama 1
*   **Yanlış Düşünce:** "VLAN ataması yapmak ağ güvenliğini tam anlamıyla sağlar, ekstra bir firewall kuralına gerek yoktur."
*   **Neden Yanlış?** VLAN yalnızca Katman 2 seviyesinde bir yalıtımdır. Paketler bir router veya Katman 3 switch (SVI) üzerinden yönlendirildiği anda, aralarında bir ACL veya Stateful Firewall kuralı yoksa trafik serbestçe akacaktır.
*   **Doğru Teknik Yaklaşım:** VLAN segmentasyonu yanal hareketi kısıtlamak için ilk adımdır ancak segmentler arası geçişler mutlaka Katman 3/4/7 seviyesinde erişim kontrol politikaları ile denetlenmelidir.

### Yanlış Anlama 2
*   **Yanlış Düşünce:** "VXLAN kullanıldığında alt ağdaki (underlay) IP yönlendiricilerin performansının veya hat yedekliliğinin önemi kalmaz."
*   **Neden Yanlış?** VXLAN bir üst ağdır (overlay). Üst ağın kararlılığı ve throughput değeri, tamamen alt ağdaki fiziksel yönlendiricilerin hatasız çalışmasına ve ECMP hatlarının doğruluğuna bağlıdır.
*   **Gerçek Sistemdeki Etki:** Underlay ağda yaşanacak bir paket kaybı veya jitter, overlay üzerindeki tüm sanal tünellerde katlanarak hissedilir.

## 12. Troubleshooting ve Validation

### Sistematik Sorun Çözme Adımları
1.  **Fiziksel Durum Kontrolü:** Portun sinyal durumu ve donanımsal hatalar (`CRC errors`, `input errors`) incelenir.
2.  **Katman 2 Tablo Doğrulaması:** Switch üzerinde cihazın MAC adresinin doğru portta ve doğru VLAN'da öğrenilip öğrenilmediği kontrol edilir.
3.  **Trunk Durum Doğrulaması:** Karşılıklı portların trunk modunda olup olmadığı ve ilgili VLAN'a izin verilip verilmediği doğrulanır.
4.  **Tünel Peer Doğrulaması (VXLAN için):** VTEP'ler arasındaki underlay bağlantısı ve UDP 4789 portunun erişilebilirliği test edilir.

### Temel Doğrulama Komutları (Cisco IOS-XE / NX-OS Tabanlı)

#### `show vlan brief`
*   **Ne işe yarar:** Switch üzerinde tanımlı olan VLAN'ları ve bu VLAN'lara atanmış aktif erişim portlarını listeler.
*   **Beklenen Sağlıklı Çıktı:** Hedef portun, atanması planlanan VLAN ID'sinin karşısında listelenmesi gerekir. Port listede yoksa trunk olarak yapılandırılmış veya kapatılmış olabilir.

#### `show interfaces trunk`
*   **Ne işe yarar:** Aktif trunk hatlarını, yerel (Native) VLAN durumunu ve hattan geçmesine izin verilen (`VLANs Allowed`) VLAN listesini gösterir.
*   **Şüpheli Durum:** Karşılıklı iki switch üzerinde Native VLAN değerlerinin farklı görünmesi veya hedef VLAN ID'sinin "Allowed and Active" listesinde yer almaması.

#### `show mac address-table interface <interface-id>`
*   **Ne işe yarar:** Belirtilen fiziksel porttan switch'in hangi MAC adreslerini öğrendiğini gösterir.
*   **Kullanım Amacı:** Son kullanıcı cihazı ağa paket gönderdiği halde bu tabloda MAC adresi oluşmuyorsa, cihazın ağ kartı veya kablo bağlantısı arızalı olabilir.

#### `show nve peers` (NX-OS / VXLAN)
*   **Ne işe yarar:** VTEP arayüzünün uzak VTEP'lerle kurduğu tünel ilişkilerini gösterir.
*   **Beklenen Sağlıklı Çıktı:** Uzak VTEP IP adresinin karşısında durumun `Up` olarak görünmesi gerekir.

### Uçtan Uca MTU Doğrulama Testi
VXLAN ağlarında paketlerin parçalanmasını önlemek için underlay ağın MTU boyutu test edilmelidir. Aşağıdaki Linux komutu, paketlerin bölünmesine izin vermeden (DF bit set edilerek) karşı VTEP'e 1550 baytlık paket gönderir:

```bash
ping -M do -s 1550 <Hedef_VTEP_IP>
```
*   **Çıktı Analizi:** Eğer komut `Frag needed and DF set` hatası veriyorsa, tünel hattı üzerindeki en az bir switch veya yönlendirici standart 1500 MTU sınırında kalmıştır ve VXLAN trafiğini yavaşlatacaktır. Altyapı MTU boyutu artırılmalıdır.

## 13. Uygulamalı Doğrulama

Bu konu için ayrı bir laboratuvar zorunlu değildir. Kavram, ilgili milestone projesi içinde uygulanacaktır.

## 14. Production Ortamı Perspektifi

*   **Failure Modes (Hata Senaryoları):** BGP EVPN kullanılan bir yapıda, bir Leaf switch üzerindeki BGP oturumu koptuğunda veri düzlemi trafiği hemen kesilmeyebilir ancak yeni taşınan sanal makinelerin MAC adresleri ağa duyurulamayacağı için "kara delik" (blackhole) durumları oluşur.
*   **High Availability (Yüksek Erişilebilirlik):** Sunucular veri merkezlerinde yedeklilik amacıyla iki farklı Leaf switch'e aynı anda bağlanır (vPC - Virtual Port Channel veya EVPN Multi-Homing). Bu durumda iki Leaf switch tek bir mantıksal VTEP gibi davranarak (`Anycast VTEP`) sunucuya kesintisiz hizmet sunar.
*   **Monitoring ve Veri Görselleştirme:** VXLAN trafiğinin analizi için geleneksel SNMP sayaçları yetersiz kalır. Tünel içi trafiğin izlenebilmesi için **OpenTelemetry** standartlarına uygun ağ telemetri verileri üretilmeli, akış verileri (NetFlow/sFlow) toplanmalı ve **Grafana** entegrasyonları ile tünel performansları anlık izlenmelidir.

## 15. Diğer Teknolojilerle İlişkisi

*   **Linux:** Çekirdek (Kernel 3.7.0 ve sonrası), yerel olarak VXLAN arayüzleri oluşturulmasını destekler. Linux sunucular kendi aralarında herhangi bir fiziksel switch'e ihtiyaç duymadan yazılımsal tüneller kurabilir.
*   **Kubernetes:** Pod'ların düğümler (nodes) arası iletişimini sağlayan birçok CNI (Flannel, Calico, Cilium), Katman 3 ağlar üzerinde cluster içi iletişimi sağlamak için varsayılan olarak VXLAN tünellemesi kurar.
*   **Infrastructure as Code (IaC):** Devasa Spine-Leaf ağlarında yüzlerce switch üzerindeki VLAN, VNI ve BGP EVPN tanımlamalarının hatasız yapılabilmesi için **Terraform** ve **Ansible** altyapıları kullanılarak konfigürasyon şablonları (Templates) üzerinden otomasyon sağlanır.

## 16. Junior Seviyede Bilinmesi Gerekenler

*   VLAN'ın Katman 2 yalıtımı sağladığını, VXLAN'ın ise Katman 3 ağlar üzerinde Katman 2 tünelleri (Overlay) oluşturduğunu açıklayabilmek.
*   Bir erişim (Access) portu ile taşıma (Trunk) portu arasındaki temel farkı ve etiketleme (Tagging) işleminin neden trunk hatlarda zorunlu olduğunu bilmek.
*   Geleneksel VLAN dünyasında maksimum 4096 ağ sınırı olduğunu, VXLAN dünyasında ise bu sınırın 16 milyona çıktığını ezbere bilmek.
*   Farklı VLAN'lardaki cihazların birbirleriyle konuşabilmesi için araya mutlaka bir Katman 3 cihazının (Router veya SVI arayüzüne sahip L3 Switch) girmesi gerektiğini ayırt edebilmek.
*   İki switch arasındaki ağ koptuğunda ilk olarak yerel (Native) VLAN uyumsuzluğu veya `allowed vlan` listesindeki eksiklikleri kontrol etmeyi akıl edebilmek.

## 17. Kısa Özet

*   Ağ segmentasyonu, yayın alanlarını (Broadcast Domain) bölerek ağ performansını ve güvenliğini artırır.
*   Fiziksel segmentasyon donanım tabanlı ayrım yaparken, mantıksal segmentasyon (VLAN/VXLAN) yazılımsal ve etiket tabanlı ayrım sağlar.
*   IEEE 802.1Q standardı, Ethernet çerçevelerine 4 baytlık (32 bit) bir etiket ekleyerek trunk hatlarında trafiği ayrıştırır.
*   VLAN ID alanı 12 bit uzunluğundadır ve ağ sınırını matematiksel olarak 4096 ile kısıtlar.
*   Erişim (Access) portları etiketsiz çerçeveleri işlerken, Taşıma (Trunk) portları etiketli çerçeveleri taşır.
*   Native VLAN, trunk hatlardan geçen etiketsiz paketlerin hangi ağa dahil olacağını belirler ve hat genelinde eşleşmelidir.
*   Farklı VLAN'lar arası iletişim (Inter-VLAN routing), Router-on-a-Stick veya donanım hızında çalışan SVI arayüzleri ile gerçekleştirilir.
*   VLAN Hopping (Switch Spoofing ve Double Tagging) saldırıları, yanlış yapılandırılmış DTP ve Native VLAN ayarlarını suiistimal eder.
*   VXLAN, 24 bitlik VNI alanı sayesinde yaklaşık 16.7 milyon benzersiz sanal ağ oluşturulmasına izin vererek VLAN ölçek sınırını çözer.
*   VXLAN mimarisi, fiziksel L3 yönlendirme altyapısını (Underlay) ve onun üzerinde çalışan mantıksal tünel ağını (Overlay) birbirinden ayırır.
*   VTEP bileşenleri, Katman 2 çerçevelerini UDP 4789 portunu kullanarak Katman 3 paketlerine sarar (enkapsülasyon) ve açar.
*   Modern veri merkezlerinde tünel adreslerinin dinamik öğrenilmesi için "Flood-and-Learn" yerine BGP EVPN kontrol düzlemi kullanılır.
*   VXLAN paket başına 50-54 baytlık bir ek yük (overhead) getirdiği için, production altyapısında uçtan uca Jumbo Frames (MTU 9000) yapılandırılması zorunludur.

## 18. Kaynaklar

*   [RFC 7348 — Virtual eXtensible Local Area Network (VXLAN)](https://datatracker.ietf.org/doc/html/rfc7348)  
    *Doğrulanan konu:* VXLAN protokol tanımı, paket yapısı, UDP 4789 standart port ataması ve veri düzlemi işleyişi.
*   [IEEE 802.1Q Standard — Local and Metropolitan Area Networks: Bridges and Bridged Networks](https://ieeexplore.ieee.org/document/8684664)  
    *Doğrulanan konu:* 4 baytlık VLAN etiket yapısı, TPID, TCI alanları ve 12 bitlik VLAN ID sınırları.
*   [RFC 7432 — BGP MPLS-Based Ethernet VPN (EVPN)](https://datatracker.ietf.org/doc/html/rfc7432)  
    *Doğrulanan konu:* BGP EVPN kontrol düzlemi mimarisi, Route tipleri ve MAC/IP adresi dağıtım kuralları.
*   [Cisco Nexus 9000 Series NX-OS VXLAN Configuration Guide](https://www.cisco.com/c/en/us/support/switches/nexus-9000-series-switches/products-installation-and-configuration-guides-list.html)  
    *Doğrulanan konu:* NVE arayüz tanımlamaları, VNI haritalama süreçleri ve donanımsal VTEP çalışma pratikleri.
