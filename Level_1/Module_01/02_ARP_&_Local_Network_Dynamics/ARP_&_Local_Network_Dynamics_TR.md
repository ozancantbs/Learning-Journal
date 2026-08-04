# ARP & Yerel Ağ Dinamikleri (ARP & Local Network Dynamics)

Bu doküman, bilgisayar ağlarında Katman 2 (Veri Bağı Katmanı - Data Link Layer) ile Katman 3 (Ağ Katmanı - Network Layer) arasındaki geçişi sağlayan Adres Çözümleme Protokolü (Address Resolution Protocol - ARP) mimarisini, işletim sistemi çekirdeğindeki durum yönetimini, sanallaştırma/konteyner ortamlarındaki davranışlarını ve ilgili siber güvenlik riskleri ile savunma mekanizmalarını incelemektedir. Doküman, bir Sistem, Ağ veya Site Güvenilirliği Mühendisinin (SRE) yerel ağ dinamiklerini anlaması, izlemesi ve sorunlarını çözmesi için gereken temel ve ileri düzey yetkinlikleri kazandırmayı amaçlar.

## 1. Genel Bakış

Adres Çözümleme Protokolü (ARP), yerel bir ağ (LAN veya Subnet) içerisindeki mantıksal Katman 3 adreslerini (IPv4), fiziksel Katman 2 adreslerine (MAC Adresi) dönüştürmekle görevli olan ve Katman 2.5 olarak konumlandırılan bir ağ protokolüdür. 

Ağ üzerindeki ana bilgisayarlar (Host) ve yönlendiriciler (Router), uygulamalar seviyesinde birbiriyle konuşurken mantıksal IP adreslerini kullanırlar. Ancak, fiziksel ağ kartları (NIC) ve yerel ağ anahtarları (Switch), veriyi kabloya veya havaya fırlatırken hangi fiziksel porta yönlendireceklerini bilmek için MAC adreslerine ihtiyaç duyarlar. ARP, bu iki farklı katman arasındaki haritalandırmayı otomatik olarak gerçekleştirerek, IP paketlerinin doğru donanım hedefine ulaşmasını sağlar.

## 2. Neden Önemlidir?

*   **Ağ Mühendisliği:** Yerel bir ağ segmentinde veri iletiminin başlayabilmesi için ARP çözümlenmesinin başarıyla tamamlanması şarttır. ARP mekanizmasının doğru çalışmaması, tüm L3 routing mimarisinin tabanda felç olmasına yol açar.
*   **Sistem Yönetimi ve SRE:** Linux ve Windows işletim sistemleri, ağ performansını optimize etmek için ARP kayıtlarını çekirdek (Kernel) seviyesinde bir bellek alanında tutar. Paket kayıplarının, yüksek gecikme sürelerinin (Latency) ve "No route to host" gibi sistem hatalarının kök nedeni ekseriyetle bu tabloların durum geçişlerindeki aksaklıklardır.
*   **Siber Güvenlik (Defensive Security):** ARP protokolünün doğası gereği durumsuz (Stateless) ve doğrulamadan yoksun olması, yerel ağları Ortadaki Adam (Man-in-the-Middle) ve Hizmet Engelleme (Denial of Service) saldırılarına açık hale getirir. Altyapının güvenliğini sağlamak amacıyla ağ seviyesinde alınacak önlemlerin (DAI, DHCP Snooping) mimari sınırlarını bilmek kritik önem taşır.
*   **Platform Engineering & Cloud:** Konteyner (Container) ağları ve Kubernetes CNI (Container Network Interface) mimarileri, podlar arası iletişimi sağlamak için yazılımsal köprüler (Linux Bridges) ve gelişmiş ARP manipülasyonları (Proxy ARP, VXLAN Encapsulation) kullanır. Altyapı mühendislerinin bu sanal katmanları debug edebilmesi için ARP temellerine hakim olması gerekir.

## 3. Temel Kavramlar ve Terminoloji

*   **IPv4 Adresi:** Ağ katmanında cihazları mantıksal ve hiyerarşik olarak tanımlayan 32-bitlik adrestir.
*   **MAC Adresi (Media Access Control):** Veri bağı katmanında ağ kartına üretici tarafından gömülen, benzersiz 48-bitlik fiziksel donanım adresidir.
*   **Yayın (Broadcast):** Yerel ağdaki tüm cihazlara gönderilen mesaj türüdür. L2 seviyesindeki adresi `FF:FF:FF:FF:FF:FF` şeklindedir.
*   **Tekli Gönderim (Unicast):** Ağdaki yalnızca belirli bir tek hedefe yönelik yapılan veri iletimidir.
*   **ARP Önbelleği (ARP Cache / Neighbor Table):** İşletim sistemlerinin, ağda sürekli broadcast yapmamak için öğrendikleri `IP <-> MAC` eşleşmelerini geçici olarak sakladıkları RAM alanıdır.
*   **Varsayılan Geçit (Default Gateway):** Yerel ağın (Subnet) dışındaki uzak bir ağa (örneğin internete) gönderilecek tüm paketlerin teslim edilmesi gereken sınır yönlendirici (Router) arayüzüdür.

## 4. Çalışma Mantığı

Bir yerel ağda, kaynak cihaz hedef cihazın IP adresini biliyor fakat veri çerçevesini (Frame) oluşturmak için gereken hedef MAC adresine sahip değilse, süreç sırasıyla şu adımlarla işletilir:

```
+------------------+                   +--------------------+
|  Kaynak Cihaz    |                   |    Hedef Cihaz     |
+------------------+                   +--------------------+
         |                                        |
         |---- 1. ARP Request (Broadcast) ------->| (Tüm ağ duyar, 
         |    "192.168.1.20 kimde? Bana söyle"    |  hedef eşleşen alır)
         |                                        |
         |<--- 2. ARP Reply (Unicast) ------------| (Sadece kaynağa cevap)
         |    "192.168.1.20 bende! MAC: BB:BB..." |
         |                                        |
   [Cache Güncellenir]                            |
   [Veri Paketi Gönderilir]                       |
```

### İlk Paket Gecikmesi (First-Packet Delay)
Bir uygulama (örneğin `ping` veya bir HTTP istemcisi), önbellekte karşılığı olmayan bir IP adresine ilk kez paket göndermek istediğinde, işletim sistemi çekirdeği asıl veri paketini hafızadaki geçici bir kuyruğa (Buffer) alır ve bekletir (`Hold` durumu). Ardından ağa bir ARP Request fırlatır. Karşı taraftan ARP Reply gelene, bu yanıt çekirdek tarafından işlenene ve asıl paket L2 etiketleriyle giydirilene kadar geçen sürede bir gecikme yaşanır. Ağ yoğunluğuna veya işletim sistemi kuyruk limitlerine bağlı olarak bu esnadaki ilk paket düşebilir (Drop). Sonraki paketler ise önbellekten okunduğu için mikrosaniyeler seviyesinde ve kayıpsız iletilir.

## 5. Mimari ve Bileşenler

### ARP Mesaj Yapısı (Message Structure)
ARP paketleri, bir IP paketinin içine kapsüllenmez; doğrudan Ethernet çerçevesinin (Ethernet Frame) veri (Payload) kısmına yerleştirilir. Standart bir Ethernet çerçevesinde `EtherType` alanı `0x0806` değerini aldığında, arkasından gelen verinin bir ARP mesajı olduğu anlaşılır.

| Alan Adı | Boyut (Bit/Bayt) | Teknik Açıklama |
| :--- | :--- | :--- |
| Hardware Type (HTYPE) | 16-bit | Kullanılan ağ teknolojisi (Örn: Ethernet için `1`) |
| Protocol Type (PTYPE) | 16-bit | Çözümlenen mantıksal protokol (Örn: IPv4 için `0x0800`) |
| Hardware Length (HLEN) | 8-bit | Fiziksel adres boyutu (Ethernet/MAC için `6` bayt) |
| Protocol Length (PLEN) | 8-bit | Mantıksal adres boyutu (IPv4 için `4` bayt) |
| Operation (OPER) | 16-bit | Mesajın türü (Request için `1`, Reply için `2`) |
| Sender Hardware Address (SHA) | 48-bit / 6 Bayt | Paketi gönderen cihazın MAC adresi |
| Sender Protocol Address (SPA) | 32-bit / 4 Bayt | Paketi gönderen cihazın IPv4 adresi |
| Target Hardware Address (THA) | 48-bit / 6 Bayt | Hedeflenen cihazın MAC adresi (Request aşamasında `00:00:00:00:00:00`) |
| Target Protocol Address (TPA) | 32-bit / 4 Bayt | Aranan/Hedeflenen cihazın IPv4 adresi |

### Linux Çekirdeği Komşuluk Tablosu ve Durum Makinesi
Linux kernel mimarisi, IPv4 ARP ve IPv6 Neighbor Discovery kayıtlarını soyut bir **Neighbor Table (Komşuluk Tablosu)** altında birleştirir. Tablodaki dinamik kayıtlar, ağın durumuna ve zaman aşımı (Expiration) mekanizmalarına göre bir durum makinesi (State Machine) üzerinden yönetilir:

*   **INCOMPLETE (INCMP):** Adres çözümleme süreci başlamıştır. Ağa bir ARP Request gönderilmiş ancak henüz geçerli bir ARP Reply alınmamıştır.
*   **REACHABLE:** Kayıt tamamen geçerli ve güvenilirdir. Yakın zamanda (Linux için varsayılan olarak ~30 saniye içinde) bu komşudan L2/L3 seviyesinde pozitif bir onay (TCP ACK veya doğrudan ARP yanıtı) alınmıştır.
*   **STALE:** `REACHABLE` süresi dolmuştur. Bilgi hala tablodadır ancak doğruluğu şüphelidir. Sistem durup dururken ağa paket fırlatmaz; ne zaman ki bu IP'ye yeni bir veri gönderilmek istenir, kayıt anında `DELAY` durumuna çekilir.
*   **DELAY:** `STALE` kayda veri gönderildikten sonraki ilk aşamadır. Çekirdek, üst katmanlardan (Örn: L4 TCP bağlantısından) paketin ulaştığına dair bir onay gelmesi için kısa bir süre (varsayılan 5 saniye) bekler. Onay gelirse doğrudan `REACHABLE` durumuna geri döner.
*   **PROBE:** `DELAY` süresince üst katmanlardan bir onay alınamadığında girilen durumdur. Linux çekirdeği, komşunun hala orada olup olmadığını anlamak için ağa doğrudan **tekli gönderim (Unicast) ARP Request** paketleri yollar.
*   **FAILED:** Yapılan tüm sondajlar (Probe) yanıtsız kaldığında kayıt bu duruma geçer. Uygulama katmanına "No route to host" hatası fırlatılır.

### Dinamik ve Statik Kayıt Ayrımı
*   **Dinamik Kayıtlar (Dynamic Entries):** Yukarıdaki durum makinesine tabi olan, ağ trafiğiyle otomatik öğrenilen ve zaman aşımı süresi bittiğinde tablodan silinen geçici kayıtlardır.
*   **Statik Kayıtlar (Static Entries):** Ağ yöneticisi tarafından el ile girilen kayıtlardır. **Zaman aşımı süreleri yoktur**, durum makinelerinden etkilenmezler ve el ile silinene veya sistem yeniden başlatılana kadar tabloda kalıcı olarak çakılı kalırlar.

## 6. Adım Adım Gerçek Teknik Akış

Bir istemcinin ağ üzerindeki iki farklı senaryoda paketleri nasıl sarmaladığını ve yönlendirdiğini inceleyelim.

### Senaryo A: Aynı Alt Ağda (Subnet) İletişim Akışı (Direct Routing)
*   **Kaynak (Cihaz A):** IP: `192.168.1.10/24`, MAC: `AA:AA:AA:AA:AA:AA`
*   **Hedef (Cihaz B):** IP: `192.168.1.20/24`, MAC: `BB:BB:BB:BB:BB:BB`

1.  **Alt Ağ Kontrolü:** Cihaz A, kendi IP'si ile hedef IP'yi alt ağ maskesi (`/24` -> `255.255.255.0`) ile mantıksal AND işlemine sokar. İkisinin de `192.168.1.0` ağında olduğunu anlar. İletişim doğrudan (Direct) olacaktır.
2.  **Önbellek Taraması:** Cihaz A, kendi yerel komşuluk tablosunda `192.168.1.20` IP'sine ait bir MAC adresi arar. Bulamazsa durum `INCOMPLETE` olur.
3.  **ARP Request Yayını:** Cihaz A, bir Ethernet çerçevesi hazırlar.
    *   *L2 Başlığı:* Kaynak MAC: `AA:AA:...`, Hedef MAC: `FF:FF:FF:FF:FF:FF` (Broadcast)
    *   *ARP Payload:* OPER: `1`, SPA: `192.168.1.10`, SHA: `AA:AA:...`, TPA: `192.168.1.20`, THA: `00:00:...`
4.  **Switch İşlemi:** Ağ anahtarı (Switch), gelen paket bir broadcast olduğu için paketin geldiği port hariç tüm portlarına bu çerçeveyi klonlayarak gönderir.
5.  **Hedef Analizi:** Ağdaki tüm cihazlar L2 katmanında bu paketi alır ve işlemciye iletir. ARP katmanında `TPA` (Target Protocol Address) alanına bakarlar. Cihaz B, kendi IP'sinin istendiğini görür. Diğer cihazlar paketi çöpe atar (Drop).
6.  **ARP Reply Yanıtı:** Cihaz B, kendi önbelleğine Cihaz A'nın `IP-MAC` bilgisini ekler/günceller. Ardından sadece Cihaz A'ya gidecek bir yanıt çerçevesi hazırlar.
    *   *L2 Başlığı:* Kaynak MAC: `BB:BB:...`, Hedef MAC: `AA:AA:...` (Unicast)
    *   *ARP Payload:* OPER: `2`, SPA: `192.168.1.20`, SHA: `BB:BB:...`, TPA: `192.168.1.10`, THA: `AA:AA:...`
7.  **Tamamlanma:** Cihaz A paketi alır, durum makinesini `REACHABLE` yapar ve asıl veri çerçevesini (Örn: HTTP, TCP) Kaynak MAC `AA:AA:...` ve Hedef MAC `BB:BB:...` olacak şekilde ağa bırakır.

### Senaryo B: Farklı Alt Ağda (Uzak Ağ) İletişim Akışı (Indirect Routing)
*   **Kaynak (Cihaz A):** IP: `192.168.1.10/24`, MAC: `AA:AA:AA:AA:AA:AA`
*   **Varsayılan Geçit (Gateway/Router):** LAN IP: `192.168.1.1`, LAN MAC: `GG:GG:GG:GG:GG:GG`
*   **Uzak Hedef (Dış Sunucu):** IP: `8.8.8.8` (MAC adresi yerel ağdan bilinemez)

```
[Cihaz A]                                [Router (Gateway)]                    [Uzak Sunucu]
(192.168.1.10)                            (192.168.1.1)                        (8.8.8.8)
     |                                          |                                    |
     |-- 1. Hedef dış ağda mı? EVET             |                                    |
     |-- 2. Gateway MAC'i var mı? HAYIR         |                                    |
     |-- 3. ARP Req ("192.168.1.1 kimde?") ---->|                                    |
     |<- 4. ARP Rep ("Bende! MAC: GG:GG...") ---|                                    |
     |                                          |                                    |
     |== 5. Kapsülleme (Encapsulation) ========|                                    |
     |   Src IP: 192.168.1.10, Dst IP: 8.8.8.8  |                                    |
     |   Src MAC: AA:AA, Dst MAC: GG:GG         |                                    |
     |                                          |                                    |
     |-- 6. Çerçeveyi Router'a gönder --------->|-- 7. L2'yi sök, L3'e bak --------->|
                                                |   Yeni L2 ile hedefe yönlendir     |
```

1.  **Alt Ağ Kontrolü:** Cihaz A, `8.8.8.8` IP'sinin kendi yerel alt ağı olan `192.168.1.0/24` içerisinde olmadığını doğrular.
2.  **Gateway Yönelimi:** Altın Kural gereği, **yerel segment dışındaki bir IP adresi için asla doğrudan o IP adresine yönelik bir ARP araması yapılmaz.** Paket, yerel ağdan çıkabilmek için Varsayılan Geçit'e (Default Gateway) teslim edilmelidir.
3.  **Gateway ARP Çözümlemesi:** Cihaz A, önbelleğinde `192.168.1.1` (Gateway) IP'sinin MAC adresini arar. Eğer yoksa, `8.8.8.8` için değil, `192.168.1.1` için bir ARP Request süreci başlatır ve Router'ın MAC adresini (`GG:GG:...`) öğrenir.
4.  **Paket Kapsülleme (Encapsulation):** Cihaz A, ağ kartından çıkaracağı nihai veri çerçevesini şu katman eşleşmeleriyle oluşturur:
    *   **Kaynak IP (L3):** `192.168.1.10` (Nihai kaynak)
    *   **Hedef IP (L3):** `8.8.8.8` (Nihai mantıksal hedef)
    *   **Kaynak MAC (L2):** `AA:AA:AA:AA:AA:AA` (Paketi kabloya fırlatan cihaz)
    *   **Hedef MAC (L2):** `GG:GG:GG:GG:GG:GG` (**Router'ın arayüzü!**)
5.  **Router İşlemi:** Paket ağ anahtarları üzerinden geçerek Router'a ulaşır. Router, L2 başlığındaki Hedef MAC adresinin kendisine ait olduğunu görür ve Ethernet çerçevesini söküp atar (De-encapsulation). İçinden çıkan L3 IP paketindeki Hedef IP alanını (`8.8.8.8`) okur ve kendi yönlendirme (Routing) tablosuna göre paketi bir sonraki durağa (Next Hop) doğru yeniden sarmallayarak gönderir.

## 7. Gerçek Dünya Kullanım Senaryoları

### Gönüllü/Gereksiz ARP (Gratuitous ARP - GARP)
Ağdaki bir cihazın, hiç kimse ona soru sormamışken (bir istek gelmeden) kendi inisiyatifiyle ağa yayınladığı özel bir ARP mesajıdır. Bu mesajda paketin içindeki kaynak IP (`SPA`) ile hedef IP (`TPA`) alanları tamamen birbirinin aynısıdır (Cihazın kendi IP'si). İki temel kritik görev için kullanılır:

1.  **Çakışan Adres Tespiti (Duplicate Address Detection - DAD):** Bir sunucu veya ağ cihazı ağ kartını aktif ettiğinde (IP adresi atandığında), hemen ağa bir GARP paketi fırlatır. Eğer ağda aynı IP'yi kullanan başka bir cihaz varsa, o cihaz hemen bir ARP Reply ile itiraz eder. Böylece işletim sistemi IP çakışması olduğunu anlar, log üretir ve ağ kartını pasife çekerek ağ bütünlüğünü korur.
2.  **Yüksek Kullanılabilirlik ve Otomatik Geçiş (HA Failover / VIP):** İki adet yük dengeleyici (Load Balancer) veya Güvenlik Duvarının (Firewall) tek bir Sanal IP (Virtual IP - VIP) paylaştığı Keepalived veya VRRP altyapılarını düşünelim. Aktif (Master) cihaz çöktüğünde, Pasif (Backup) cihaz saniyeler içinde VIP'yi üzerine alır. Ancak ağdaki switchlerin ve diğer sunucuların önbelleğinde VIP hala eski cihazın MAC adresiyle eşleşmektedir. Yeni aktif olan cihaz, devreye girer girmez ağa yoğun bir şekilde **Gratuitous ARP** yayını yapar. Bu yayını alan switchler MAC adres tablolarını, sunucular ise ARP önbelleklerini anında günceller. Trafik kesintisiz bir şekilde yeni cihaza akar.

### Vekil ARP (Proxy ARP)
Bir yönlendiricinin (Router), kendisinde bulunmayan ve arkasındaki başka bir ağda yer alan bir IP adresi için gelen ARP isteklerine, **kendi fiziksel MAC adresini vererek** "onun adına" cevap vermesi mekanizmasıdır. 

*   **Kullanım Amacı:** Alt ağ maskesi (Subnet Mask) hatalı yapılandırıldığı için kendilerini aynı fiziksel ağda sanan eski tip legacy cihazların, aradaki router üzerinden şeffaf bir şekilde haberleşebilmesini sağlar.
*   **Sektör Gerçeği ve Trade-off:** Modern ağ mimarilerinde Proxy ARP, ağ karmaşıklığı yaratması, troubleshooting süreçlerini zorlaştırması ve ağın saldırı yüzeyini (Attack Surface) genişletmesi sebebiyle güvenlik politikaları gereği varsayılan olarak kapatılır (`net.ipv4.conf.all.proxy_arp = 0`).

## 8. Güvenlik Perspektifi

### ARP Spoofing (Sahtekarlık) ve ARP Poisoning (Zehirleme)
*   **Teknik Sorun:** ARP protokolünün durumsuz (Stateless) olmasıdır. İşletim sistemleri, ağdan gelen bir ARP Reply paketinin daha önce kendileri tarafından istenip istenmediğini doğrulamazlar; gelen her cevabı doğru kabul edip önbellek tablolarını güncellerler.
*   **Çalışma Mantığı:** Saldırgan, kurban cihaza sürekli olarak sahte ARP Reply paketleri göndererek *"Varsayılan Geçit'in (Router) IP'si bende, yeni MAC adresi benim ağ kartımdır"* der. Aynı anda Router'a da kurbanın IP'sinin kendisinde olduğunu söyler. Her iki cihazın da önbelleği yanlış bilgilerle manipüle edilir.
*   **Ön Koşullar:** Saldırganın hedef kurbanlar ile aynı fiziksel veya mantıksal Katman 2 ağ segmentinde (Aynı VLAN/Subnet) bulunması gerekir.
*   **Olası Etki:** 
    *   *Ortadaki Adam (Man-in-the-Middle - MitM):* Kurbanın dış dünyaya veya sunuculara gönderdiği tüm şifresiz trafik (HTTP, DNS, FTP) saldırganın ağ kartı üzerinden geçer. Gizlilik (Confidentiality) ve Bütünlük (Integrity) tamamen ihlal edilir.
    *   *Hizmet Engelleme (Denial of Service - DoS):* Saldırgan ağdaki tüm cihazlara Router'ın MAC adresinin `00:11:22:33:44:55` (ağda var olmayan hayali bir adres) olduğunu söyler. Tüm ağın dış dünya ile bağlantısı anında kopar. Erişilebilirlik (Availability) sıfırlanır.

```
+-----------------------------------------------------------------------------------+
| Risk / Saldırı Adı | Tespit Göstergeleri            | Savunma Kontrolü            |
| :----------------- | :-----------------              | :-----------------          |
| **ARP Poisoning**  | Tek IP'ye ait birden fazla MAC  | Dynamic ARP Inspection(DAI) |
|                    | Değişimi, Talepsiz ARP Reply'lar| DHCP Snooping Bağlantısı     |
+-----------------------------------------------------------------------------------+
```

## 9. Hardening ve Best Practices

### Dinamik ARP Denetimi (Dynamic ARP Inspection - DAI) & DHCP Snooping İlişkisi
Kurumsal ağ anahtarlarında (Switch) ARP zehirlemesini donanım seviyesinde engellemek için DAI teknolojisi kullanılır. DAI'nin çalışabilmesi için ağda öncelikle **DHCP Snooping** mekanizmasının kurulmuş olması zorunludur.

1.  **DHCP Snooping Aşaması:** Switch, kendi üzerinden geçen DHCP trafiğini pasif olarak dinler. Hangi porttan hangi MAC adresine sahip cihazın hangi IP adresini kiraladığını anlık olarak kaydeder ve dinamik bir güvenli veritabanı oluşturur (`DHCP Snooping Binding Database`).
2.  **DAI Denetim Aşaması:** Switch üzerinde DAI aktif edildiğinde, switch portlarından gelen tüm ARP paketleri donanım seviyesinde (ASIC) yakalanır. Paketin içindeki `Sender IP` ve `Sender MAC` bilgileri, DHCP Snooping tablosuyla karşılaştırılır. Eğer eşleşme doğrulanmazsa (yani porttaki cihaz başkasına ait bir IP için iddiada bulunuyorsa), switch o ARP paketini **anında drop eder** ve arayüzü gerekirse kapatır.
3.  **Güven Sınırları (Switch Trust Boundaries):** DAI yapılandırmasında portlar ikiye ayrılır:
    *   *Untrusted (Güvenilmez):* Standart kullanıcı bilgisayarları, sunucular ve podların bağlı olduğu tüm portlar. DAI denetimine tabidir.
    *   *Trusted (Güvenli):* Şirket router'ları, core switchler veya meşru statik IP kullanan yetkili sunucu portları. Bu portlar DAI denetiminden muaf tutulur; çünkü bu cihazların kendi üzerlerinden GARP veya Proxy ARP üretmesi normal bir operasyonel davranıştır.

### Statik ARP Kayıtlarının Sınırları
Küçük, izole ve kritik altyapılarda sunuculara geçitlerin MAC adresleri statik (`ip neigh add ... nud permanent`) olarak girilebilir. Ancak bu yöntem **ölçeklenemez (not scalable)**. Yüzlerce sanal makinenin veya konteynerin olduğu, cihazların sürekli silinip yeniden yaratıldığı, donanım arızalarında ağ kartlarının (NIC) değiştiği modern production ortamlarında statik ARP yönetimi sürdürülemez bir operasyonel yük ve insan hatası riski doğurur.

## 10. Avantajlar, Dezavantajlar ve Trade-off'lar

*   **Performans ve Latency:** ARP, yerel ağda tak-çalıştır (Plug-and-Play) bir kolaylık sağlar; statik IP-MAC haritalandırma ihtiyacını ortadan kaldırır. Ancak ilk paket gecikmesi (First-packet delay) ve ağ büyüdükçe artan broadcast trafiği birer performans maliyetidir.
*   **Broadcast Storm (Yayın Fırtınası) Riski:** Bir L2 ağ segmentinde cihaz sayısı binleri bulduğunda, ARP Request paketlerinin oluşturduğu kümülatif broadcast trafiği ağdaki tüm cihazların CPU'larını gereksiz yere meşgul eder. Bu nedenle production mimarilerinde büyük L2 broadcast domain'leri alt ağlara (Subnet/VLAN) bölünür.

### Modern Alternatif: IPv6 Neighbor Discovery Protocol (NDP)
IPv6 mimarisinde ARP tamamen ortadan kaldırılmıştır. Yerine ICMPv6 mesajlarını kullanan **Neighbor Discovery Protocol (NDP)** getirilmiştir.

```
+------------------------------------------------------------------------------------+
| Özellik             | IPv4 ARP                        | IPv6 NDP                   |
| :------------------ | :------------------------------ | :------------------------- |
| **Kullanılan Katman**| Katman 2.5                      | Katman 3 (ICMPv6 tabanlı)  |
| **Adres Arama**     | **Broadcast** (Herkes dinler)   | **Multicast** (Grup dinler)|
| **Güvenlik Mekanizması**| Yok (Doğal olarak zafiyetli)  | **SEND (Secure ND)** krip- |
|                     |                                 | tografik imzalama desteği  |
+------------------------------------------------------------------------------------+
```
NDP, broadcast yerine **Solicited-Node Multicast** adreslemesini kullanır. Bir IP adresi çözümlenmek istendiğinde, mesaj yerel ağdaki tüm cihazlara değil, sadece o IP adresinin son bitleriyle eşleşen çok küçük bir multicast grubuna iletilir. Ağ kartları donanımsal olarak kendilerini ilgilendirmeyen bu multicast trafiğini işlemciye iletmeden çöpe atar; böylece ağ performansı optimize edilir.

## 11. Yaygın Yanlış Anlamalar

*   **Yanlış Düşünce:** *"İnternet üzerindeki bir web sitesine erişirken, sunucum o web sitesinin IP'si için ARP isteği gönderir."*
    *   **Doğru Teknik Yaklaşım:** ARP yalnızca yerel L2 segmentinde çalışır. Router'lar broadcast paketlerini geçirmez. Dış dünyadaki bir IP'ye giderken sadece yerel ağdaki Varsayılan Geçit'in (Default Gateway) MAC adresi çözümlenir.
*   **Yanlış Düşünce:** *"Ağda statik IP adresi tanımlı olan bir sunucu asla ARP zehirlemesinden (ARP Poisoning) etkilenmez."*
    *   **Doğru Teknik Yaklaşım:** Bir sunucunun IP'sinin statik olması, onun ARP önbelleğinin dinamik çalışmasını engellemez. Sunucu, ağdan gelen sahte ARP Reply mesajlarını hala kabul etmeye ve önbelleğindeki Router MAC adresini değiştirmeye devam eder. Etkilenmemesi için IP'sinin değil, önbelleğindeki ARP kaydının statik (`permanent`) yapılması veya switch seviyesinde DAI kullanılması gerekir.

## 12. Troubleshooting ve Validation

Bir sistemde ağ bağlantı sorunları yaşandığında, Katman 2/3 seviyesindeki arızaları tespit etmek için şu adımlar izlenir:

### Linux Üzerinde Komşuluk İzleme (`ip neigh`)
Eski tip `arp` komutları modern Linux dağıtımlarında kullanımdan kaldırılmıştır (Deprecated). Bunun yerine `iproute2` paketinden gelen `ip neigh` alt komutları kullanılır.

*   **Soru:** Şu an çekirdeğin önbelleğindeki komşu durumları nedir?
    ```bash
    ip neigh show
    ```
    *Çıktı Analizi:*
    ```text
    192.168.1.1 dev eth0 lladdr 00:1c:2f:3a:4b:5c REACHABLE
    192.168.1.20 dev eth0 lladdr 00:1c:2f:3a:4b:99 STALE
    192.168.1.200 dev eth0  FAILED
    ```
    *   `192.168.1.1` (Gateway) ile iletişim sağlıklı ve aktiftir (`REACHABLE`).
    *   `192.168.1.20` ile yakın zamanda konuşulmamıştır, ilk veride doğrulanacaktır (`STALE`).
    *   `192.168.1.200` adresine gönderilen ARP istekleri yanıtsız kalmıştır (`FAILED`). Hedef cihaz kapalı, IP adresi ağda yok veya aradaki bir güvenlik duvarı ARP'yi engelliyor.

*   **Soru:** Şüpheli veya hatalı bir ARP kaydını el ile nasıl temizlerim?
    ```bash
    sudo ip neigh del 192.168.1.20 dev eth0
    ```
    *Açıklama:* Bu komut önbellekteki kaydı siler. Sistem bu IP ile tekrar konuşmak istediğinde ağa temiz bir ARP Request fırlatmak zorunda kalır. Operasyonel riski yoktur.

### `tcpdump` ile Canlı Paket Analizi
Ağda bir IP çakışması veya sahtekarlık şüphesi olduğunda, ağ kartı üzerinden akan ham paketler izlenmelidir.

*   **Soru:** Ağa gelen ve giden ARP paketlerini canlı olarak nasıl filtrelerim?
    ```bash
    sudo tcpdump -pni eth0 arp
    ```
    *   `-p`: Non-promiscuous mod. Ağ kartını sadece kendisini ve broadcast trafiğini ilgilendiren paketleri dinleyecek şekilde bırakarak işlemci yükünü azaltır.
    *   `-n`: İsim çözümlemesini kapatır; IP ve MAC adreslerini ham haliyle basarak zaman kaybını önler.
    *   `-i eth0`: Sadece `eth0` arayüzünü dinler.
    *   `arp`: Sadece ARP protokolüne ait paketleri yakalar.

*   **Sağlıklı Durum Çıktısı:**
    ```text
    10:00:01.100 ARP, Request who-has 192.168.1.50 tell 192.168.1.10, length 28
    10:00:01.102 ARP, Reply 192.168.1.50 is-at 00:1c:2f:aa:bb:cc, length 28
    ```
    *Yorum:* Bir istek yapılmış ve makul bir milisaniye içinde ilgili cihazdan tek bir yanıt alınmıştır.

*   **Şüpheli / Saldırı Durumu Çıktısı:**
    ```text
    10:05:00.200 ARP, Reply 192.168.1.1 is-at 00:1c:2f:99:99:99, length 28
    10:05:01.200 ARP, Reply 192.168.1.1 is-at 00:1c:2f:99:99:99, length 28
    ```
    *Yorum:* Ortada hiçbir `who-has` (Request) isteği yokken, ağa sürekli olarak tek taraflı `Reply` paketleri akmaktadır (Unsolicited ARP Reply). Bu durum net bir **ARP Poisoning** saldırısının veya ciddi bir network cihaz arızasının göstergesidir.

## 13. Uygulamalı Doğrulama

Bu konu için ayrı bir laboratuvar zorunlu değildir. Kavram, ilgili milestone projesi içinde uygulanacaktır.

## 14. Production Ortamı Perspektifi

### Sanallaştırma ve Konteyner Ağlarında ARP Dinamikleri
Modern veri merkezlerinde ve bulut altyapılarında ARP, fiziksel kabloların dışına çıkıp tamamen yazılımsal katmanlarda (Software-Defined Networking - SDN) koşturulur.

*   **Linux Bridges (Konteyner İçi L2 İletişim):** Bir Linux sunucuda Docker çalıştırıldığında çekirdekte `docker0` adında sanal bir switch (Bridge) oluşturulur. Sunucu üzerindeki konteynerler, `veth` (Virtual Ethernet) kablo çiftleri ile bu köprüye bağlanır. Aynı host üzerindeki iki konteyner birbiriyle konuşurken süreç tamamen Linux çekirdeğinin içindeki yazılımsal komşuluk tablosunda döner; fiziksel ağ kartına (NIC) hiçbir broadcast paket sızmaz.
*   **Kubernetes CNI Dünyası:** Podların farklı fiziksel sunucularda (Node) çalıştığı Kubernetes kümelerinde iletişim, seçilen CNI eklentisine göre değişir:
    *   *Overlay Ağlar (Örn: Flannel - VXLAN Modu):* Podlar kendilerini aynı L2 ağında sanıp ARP Request gönderseler de, CNI katmanı bu broadcast paketinin fiziksel ağa yayılmasına izin vermez. Node üzerindeki tünel uç noktası (VTEP), ARP isteğini yakalar, hedef podun hangi uzak Node üzerinde olduğunu kendi merkezi veritabanından (`etcd`) okur ve ARP paketini bir UDP paketinin içine sarmalayarak (**L2 over L3 Encapsulation**) doğrudan karşı Node'a postalar.
    *   *Routing Modları (Örn: Calico - BGP Modu):* Calico, yerel ağlardaki broadcast kirliliğini tamamen bitirmek için **Proxy ARP** mimarisinden yararlanır. Pod, dışarıya gitmek için bir ARP isteği yaptığında, bağlı olduğu Node'un sanal ağ kartı ucu bu isteğe Proxy ARP ile kendi MAC adresini vererek döner. Pod, tüm paketlerini doğrudan Node çekirdeğine (L3) fırlatır; Node ise paketi BGP rotalarına göre hedefe yönlendirir. Bu sayede cluster içinde geleneksel anlamda devasa L2 broadcast domain'leri oluşmaz.

## 15. Diğer Teknolojilerle İlişkisi

*   **Kubernetes NetworkPolicy:** NetworkPolicy katmanı Katman 3 ve Katman 4 seviyesinde (IP ve Port tabanlı) filtreleme yapar. ARP seviyesindeki (Katman 2) sahtekarlıkları veya tünelleme manipülasyonlarını engellemez; bu koruma CNI'ın alt yapısına veya host güvenlik politikalarına aittir.
*   **Linux Güvenlik Duvarları (iptables / nftables):** Standart `iptables` kuralları IP paketlerini inceler, ağ kartından geçen ham ARP paketlerine müdahale edemez. Linux çekirdeğinde ARP trafiğini filtrelemek için özel olarak tasarlanmış `arptables` veya modern `nftables` içindeki `arp` aileleri kullanılmalıdır.

## 16. Junior Seviyede Bilinmesi Gerekenler

*   ARP protokolünün mantıksal IP adreslerini fiziksel MAC adreslerine dönüştürdüğünü açıklayabilmek.
*   ARP Request mesajlarının tüm yerel ağa **Broadcast** olarak yayıldığını, ARP Reply mesajının ise sadece isteği yapan cihaza **Unicast** olarak döndüğünü bilmek.
*   Uzak bir ağdaki (Örn: internet) bir IP'ye giderken doğrudan o IP için ARP aranmayacağını, paketin Varsayılan Geçit'in (Default Gateway) MAC adresiyle sarmalanacağını ayırt edebilmek.
*   Linux terminalinde `ip neigh show` komutunu çalıştırıp, çıktıda görülen `REACHABLE` ile `FAILED` durumlarının ne anlama geldiğini yorumlayabilmek.
*   ARP'nin durumsuz (Stateless) yapısından ötürü ağda herkesin herkese yalan söyleyebileceğini (ARP Spoofing) ve bunun switch seviyesinde DAI / DHCP Snooping ile engellendiğini temel düzeyde kavramış olmak.

## 17. Kısa Özet

1.  **Görevi:** Mantıksal L3 IPv4 adreslerini, fiziksel L2 MAC adreslerine haritalandırır.
2.  **Konumu:** Doğrudan Ethernet çerçevesi içinde taşınır; Katman 2.5 protokolüdür.
3.  **İstek (Request):** Hedef MAC bilinmediği için yerel ağa `FF:FF:FF:FF:FF:FF` adresiyle broadcast yapılır.
4.  **Yanıt (Reply):** İlgili IP'ye sahip cihaz, kendi MAC adresini içeren unicast bir paketle yanıt döner.
5.  **Önbellek (Cache):** Sürekli broadcast yapmayı önlemek için veriler Linux'ta Neighbor Table içinde saklanır.
6.  **Durum Geçişleri:** Çekirdekteki dinamik kayıtlar `INCOMPLETE`, `REACHABLE`, `STALE`, `DELAY`, `PROBE`, `FAILED` durumları arasında döner.
7.  **Zaman Aşımı:** `STALE` durumundaki kayda yeni bir trafik gelmedikçe ağa yeni bir ARP paketi atılmaz.
8.  **Uzak Ağ Akışı:** Dış dünyadaki hedeflerin IP'si değişmez ancak L2 çerçevesi her router geçişinde sökülüp yeniden üretilir (Hedef MAC her zaman bir sonraki duraktır).
9.  **Gönüllü ARP (GARP):** IP çakışmalarını tespit etmek (DAD) ve HA failover senaryolarında önbellekleri anında güncellemek için cihazın kendi kendine bastığı broadcast'tir.
10. **Zafiyet:** Durumsuz (Stateless) yapısı sebebiyle kimlik doğrulaması yapamaz; ARP Spoofing ve MitM saldırılarına zemin hazırlar.
11. **Donanımsal Savunma:** Kurumsal ağlarda switchler, DHCP Snooping veritabanına dayanarak sahte ARP'leri **Dynamic ARP Inspection (DAI)** ile engeller.
12. **Gelecek (IPv6):** IPv6 dünyasında ARP ve broadcast tamamen kaldırılmış; yerini multicast tabanlı **NDP (Neighbor Discovery Protocol)** almıştır.

## 18. Kaynaklar

*   **RFC 826:** *An Ethernet Address Resolution Protocol -- or -- Converting Network Protocol Addresses to 48.bit Ethernet Address for Transmission on Ethernet Hardware*
    *   *Doğrulanan Konu:* Orijinal ARP mesaj yapısı, request/reply akış standartları ve operasyon kodları.
*   **RFC 4861:** *Neighbor Discovery for IP version 6 (IPv6)*
    *   *Doğrulanan Konu:* IPv6 NDP mimarisi, Solicited-Node Multicast mekanizması ve ARP ile olan mimari farklar.
*   **Linux Kernel Networking Documentation (IP Sysctl):** *`/proc/sys/net/ipv4/neigh/*` parameters*
    *   *Doğrulanan Konu:* Linux çekirdeğindeki komşuluk tablosu durum geçiş süreleri (`gc_stale_time`, `delay_first_probe_time`) ve Proxy ARP varsayılan davranışları.
*   **Cisco Catalyst Navigational Guides:** *Configuring Dynamic ARP Inspection and DHCP Snooping*
    *   *Doğrulanan Konu:* Donanım seviyesinde L2 güvenlik sınırları, güvenli/güvenilmez (Trusted/Untrusted) port teorisi ve DAI-DHCP Snooping binding tablosu ilişkisi.
