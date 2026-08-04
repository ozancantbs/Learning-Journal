# 05. Güvenlik Duvarı Hizmetleri ve Ağ Savunması (Firewalling & Network Defense)

## 🇹🇷 Türkçe Dokümantasyon

### Genel Bakış
Uç nokta ve çevre ağı seviyesindeki ağ savunması; gelen, giden ve yönlendirilen trafiğin önceden tanımlanmış güvenlik politikalarına göre filtrelenmesine dayanır. Linux, ağ iletişimleri üzerinde hassas kontrol sağlamak amacıyla paket filtreleme, Ağ Adresi Çevirisi (NAT) ve paket değiştirme (packet mangling) işlemlerini yürütmek için çekirdek alanındaki (kernel-space) **Netfilter** altyapısını kullanır.

---

### 1. Filtreleme Yaklaşımları ve Bağlantı Takibi

Trafik denetimi iki temel mimari modele dayanır:

* **Durumsuz (Stateless) Filtreleme:** Her bir paketi önceki paketlerin bağlamından bağımsız olarak tek başına inceler. Kurallar statik başlık alanlarıyla (Kaynak IP, Hedef IP, Kaynak Port, Hedef Port, Protokol) eşleşir. Yüksek performanslıdır ancak IP sahtekarlığına (spoofing) ve durum dışı paket atlatmalarına karşı savunmasızdır.
* **Durumlu (Stateful) Filtreleme:** Linux çekirdeğinin **Conntrack (Connection Tracking - Bağlantı Takibi)** modülünü kullanarak aktif ağ bağlantılarının durumunu takip eder. Paketleri, onaylanmış bir oturum içindeki bağlamlarına göre değerlendirir.

#### Conntrack Durum Sınıflandırmaları:
* **NEW:** Yeni bir bağlantı başlatan paketler (örneğin gelen ilk TCP SYN paketi).
* **ESTABLISHED:** Daha önce onaylanmış, çift yönlü bir bağlantıya ait olan paketler.
* **RELATED:** Mevcut bir oturumla ilişkili ikincil bir bağlantı başlatan paketler (örneğin FTP veri kanalları veya ICMP hata mesajları).
* **INVALID:** Bilinen hiçbir bağlantı durumuyla eşleşmeyen veya bozulmuş başlıklara sahip olan paketler (güvenli politikalarda varsayılan olarak düşürülür/DROP).

---

### 2. Linux Güvenlik Duvarı Ekosistemi: Netfilter, iptables & nftables

#### Netfilter Mimarisi
Paket yakalama, inceleme ve değiştirme işlemleri için çekirdek seviyesinde kancalar (`PREROUTING`, `INPUT`, `FORWARD`, `OUTPUT`, `POSTROUTING`) sağlayan Linux çekirdek alt sistemidir.

#### `iptables` Çerçevesi
Belirli tablolar ve zincirler üzerinde çalışan geleneksel kullanıcı alanı (userspace) aracıdır:
* **Tablolar:** `filter` (varsayılan paket filtreleme), `nat` (adres çevirisi), `mangle` (paket başlığı değiştirme), `raw` (bağlantı takibini devre dışı bırakma).
* **Varsayılan Zincirler:** `INPUT` (sunucunun kendisine gelen), `OUTPUT` (sunucudan çıkan), `FORWARD` (sunucu üzerinden yönlendirilen).

#### `nftables` Çerçevesi
`iptables` yerini alan modern altyapıdır. Tek bir çerçeve altında birleştirilmiş çift yığın (IPv4/IPv6) sözdizimi, daha hızlı kural arama ağaçları, daha düşük bellek kullanımı ve dinamik paket içeriği eşleme özellikleri sunar.

---

### 3. NAT, Oran Sınırlama (Rate Limiting) ve Ağ Segmentasyonu

* **Ağ Adresi Çevirisi (NAT - Network Address Translation):**
  * **SNAT (Source NAT / Masquerade):** Dışarı giden paketlerin kaynak IP adresini değiştirerek özel iç ağların genel (public) ağlara erişmesini sağlar.
  * **DNAT (Destination NAT / Port Forwarding):** Gelen paketlerin hedef IP/port bilgisini değiştirerek dış trafiği iç ağdaki servislere yönlendirir.
* **Oran Sınırlama (Rate Limiting):** Kaba kuvvet denemelerini ve DoS saldırılarını hafifletmek için bağlantı oranlarını veya paket frekansını kısıtlama işlemidir.
* **Ağ Segmentasyonu:** Katı güvenlik duvarı politikaları uygulamak ve yanal hareketleri (lateral movement) kısıtlamak amacıyla ağı izole edilmiş bölgelere (VLAN'lar, Alt Ağlar, DMZ) bölme işlemidir.

---

### 🛠️ Savunma Uygulamaları ve İnceleme Komutları

* **Yerleşmiş Bağlantılara İzin Veren Durumlu Kural Ekleme (iptables):**  
  `sudo iptables -A INPUT -m conntrack --ctstate ESTABLISHED,RELATED -j ACCEPT`

* **Kaba Kuvvet Saldırılarını Önlemek İçin SSH Bağlantılarını Oran Sınırlama (iptables):**  
  `sudo iptables -A INPUT -p tcp --dport 22 -m state --state NEW -m recent --set`  
  `sudo iptables -A INPUT -p tcp --dport 22 -m state --state NEW -m recent --update --seconds 60 --hitcount 4 -j DROP`

* **Aktif Conntrack Bağlantı Takip Tablosunu Listeleme:**  
  `sudo conntrack -L`

* **Aktif `nftables` Kural Kümesini Görüntüleme:**  
  `sudo nft list ruleset`

---

### 📊 Filtreleme ve Savunma Özet Matrisi

| Savunma Vektörü | Mekanik Amacı | Kritik Parametre / Sözdizimi | Operasyonel Kazanç |
| :--- | :--- | :--- | :--- |
| **Durumlu Takip** | Oturum bağlamını doğrular | `-m conntrack --ctstate` | Durum dışı taramaları ve sahte paketleri engeller |
| **Oran Sınırlama** | Bağlantı hacmini kısıtlar | `-m recent` veya `limit rate` | TCP bağlantı tükenmesini / DoS taşkınlarını önler |
| **Hedef NAT (DNAT)** | DMZ'ye Port Yönlendirme | `PREROUTING -j DNAT` | İç ağ altyapısını dış dünyadan gizler |
| **Segmentasyon** | Yanal hareketi sınırlandırır | VLAN'lar arası engelleme kuralları | İhlalleri izole bölgeler içinde hapseder |
