## 🇹🇷 Türkçe

### 📌 Mimari Genel Bakış
Bu laboratuvar, dağıtık sistemlerde veri bütünlüğünü korumak, sistem istismarını önlemek ve veri tabanı performansını optimize etmek amacıyla tasarlanmış, endüstri standardı backend dayanıklılık tekniklerinin pratik uygulamasını içerir:
1. **Güvenli REST API & Middleware Motoru:** Uygulama katmanında tehdit engelleme ve trafik akış kontrolü.
2. **İlişkisel İşlem (Transaction) & Eşzamanlılık (Concurrency) Kontrolü:** Yüksek eşzamanlılıklı veri tabanı operasyonları için izolasyon seviyeleri ve kilitleme mekanizmaları.
3. **Yüksek Trafikli Önbellekleme Mimarisi:** Yoğun trafik altında birincil ilişkisel veri tabanlarının yükünü hafifletmeye yönelik bellek katmanı (RAM) veri erişim stratejileri.

---

### 🏗️ Uygulanan Mühendislik Modülleri

#### 1. Güvenli REST API & Middleware Motoru (Node.js)
* **Amaç:** Hacimsel ve uygulama katmanı güvenlik tehditlerine karşı korunan kurumsal düzeyde bir giriş noktası oluşturmak.
* **Mimari ve İşleyiş:**
  * **Trafik Kontrolü (Rate Limiting):** Kötüye kullanımı, veri kazıma (scraping) girişimlerini ve HTTP flood vektörlerini sınırlandırmak için sabit pencereli istek kısıtlama algoritmalarının uygulanması (örneğin IP başına dakikada maksimum 5 istek).
  * **Middleware Müdahale Hattı:** İş mantığı (business logic) tetiklenmeden önce gelen HTTP trafiğini yakalayan, doğrulayan ve filtreleyen merkezi istek işleme mimarisi.
  * **HTTP Durum Standartlaştırması:** Sınır ihlallerinde istemciye yapılandırılmış `429 Too Many Requests` HTTP durum kodu ve standartlaştırılmış hata şeması dönülmesi.

#### 2. SQL Transaction & Lock Mimarisi (PostgreSQL)
* **Amaç:** Eşzamanlı okuma/yazma işlemlerinde ACID prensiplerini garanti altına almak ve veri tutarlılığını korumak.
* **Mimari ve İşleyiş:**
  * **İşlem İzolasyonu:** Çalışma durumunu yönetmek için açık `BEGIN`, `COMMIT` ve `ROLLBACK` blokları kullanılarak katı işlem sınırlarının oluşturulması.
  * **Eşzamanlılık & Kilitleme Kontrolü:** Sık erişilen satırlarda (örneğin stok düşme, bakiye transferi) **Yarış Koşullarını (Race Conditions)**, **Kirli Okumaları (Dirty Reads)** ve **Kayıp Güncellemeleri (Lost Updates)** önlemek amacıyla karamsar kilitleme (`FOR UPDATE`) mekanizmasının uygulanması.
  * **Rollback Dayanıklılığı:** Herhangi bir adımda hata oluştuğunda tüm işlemin geri alınmasını (atomic execution) sağlayarak sistem durumunun bozulmasını engelleyen yapı.

#### 3. Yüksek Trafikli Mimari & Önbellek Tasarımı (Redis + API)
* **Amaç:** Bellek katmanında önbellekleme yaparak veri tabanı okuma gecikmesini en aza indirmek ve yüksek trafik dalgalanmalarını karşılamak.
* **Mimari ve İşleyiş:**
  * **Cache-Aside (Lazy Loading) Deseni:** Gelen okuma isteklerinin önce önbellek katmanında karşılanması:
    * **Cache Hit (Önbellek İsabeti) ⚡:** Verinin birincil veri tabanına uğramadan, milisaniyenin altında bir hızla doğrudan RAM'den sunulması.
    * **Cache Miss (Önbellek Iskalaması) 🐢:** Verinin önce ilişkisel veri tabanından sorgulanması, ardından eşzamansız olarak önbelleğe yazılması ve istemciye iletilmesi.
  * **Geçersiz Kılma & TTL Yönetimi:** Eski veri riskini ortadan kaldırmak ve bellek şişmesini önlemek için katı Yaşam Süresi (TTL) politikalarının uygulanması (örneğin 10 saniyelik otomatik silinme).

---

### 🎓 Kazanılan Mühendislik Yetkinlikleri
* **Sistem Dayanıklılığı:** Ağ sınırlaması, middleware filtreleme ve DB düzeyinde eşzamanlılık kontrollerini kapsayan çok katmanlı savunma tekniklerinin kurgulanması.
* **Performans Optimizasyonu:** Değişken verilerin bellek içi (in-memory) önbellek katmanından sunularak birincil veri tabanı üzerindeki okuma baskısının azaltılması.
* **Veri Bütünlüğü:** Eşzamanlı çalışan veri akışlarında katı işlem kilitleri (transactional locking) kullanılarak veri bozulmalarının önüne geçilmesi.
