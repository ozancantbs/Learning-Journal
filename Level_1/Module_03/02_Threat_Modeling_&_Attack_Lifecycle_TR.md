## 🇹🇷 Türkçe Dökümantasyon

### Genel Bakış
Bir sistemi etkili bir şekilde savunmak için güvenlik mühendislerinin saldırgan gibi düşünebilmesi gerekir. Tehdit modelleme çerçeveleri, güvenlik analizlerini yapılandırmaya, sızma noktalarını tespit etmeye ve saldırı döngüsünü kırarak zararı önlemeye yardımcı olur.

---

### 1. STRIDE Tehdit Modeli
Microsoft tarafından geliştirilen STRIDE, tehditleri ihlal ettikleri güvenlik ilkesine göre kategorize eder:

| Kategori | Tehdit Türü | İhlal Edilen İlke | Örnek Senaryo |
| :--- | :--- | :--- | :--- |
| **S** | **Spoofing** (Kimlik Taklidi) | Authentication | SSH anahtarını çalarak yetkili sistem yöneticisi gibi girmek. |
| **T** | **Tampering** (Veri Kurcalama) | Integrity | Sistem loglarını değiştirmek veya HTML kodlarını bozmak. |
| **R** | **Repudiation** (İnkar Etme) | Accountability | İzleri silerek "Bu işlemi ben yapmadım" demek. |
| **I** | **Info Disclosure** (Bilgi İfşası) | Confidentiality | Veritabanı şifrelerinin bulunduğu .env dosyasını dışarıya açık bırakmak. |
| **D** | **Denial of Service** (Servis Dışı Bırakma) | Availability | Sunucu kaynaklarını tüketerek servisi çökertmek. |
| **E** | **Elevation of Privilege** (Yetki Yükseltme) | Authorization | Hatalı SUID izinlerini kullanıp root yetkisine erişmek. |

---

### 2. Siber Saldırı Zinciri (Cyber Kill Chain)
Lockheed Martin tarafından geliştirilen bu 7 aşamalı model, gelişmiş bir siber saldırının adımlarını açıklar:

[1. Keşif] -> [2. Silahlandırma] -> [3. İletim] -> [4. Sızma] -> [5. Kurulum] -> [6. Komuta Kontrol] -> [7. Hedefe Ulaşma]

1. **Keşif (Reconnaissance):** Hedef hakkında bilgi toplama (nmap ile port ve servis taraması).
2. **Silahlandırma (Weaponization):** Tespit edilen zafiyete uygun zararlı yazılım hazırlama.
3. **İletim (Delivery):** Zararlıyı hedefe ulaştırma (E-posta, açık portlar).
4. **Sızma (Exploitation):** Sistemdeki açığı tetikleyerek içeri girme.
5. **Kurulum (Installation):** Sunucuda kalıcılık sağlama (Arka kapı / backdoor bırakma, kullanıcı ekleme).
6. **Komuta Kontrol (C2):** Dışarıdaki saldırgan sunucusuyla gizli iletişim kanalı kurma.
7. **Hedefe Ulaşma (Actions on Objectives):** Asıl amacı gerçekleştirme (Veri çalma, fidye yazılımı çalıştırma).

> **Savunma Stratejisi:** Saldırı zincirinin herhangi bir halkasını kırarsan saldırı tamamen başarısız olur!

---

### 3. MITRE ATT&CK Çerçevesi
Saldırganların gerçek dünyada kullandığı Taktik, Teknik ve Prosedürlerin (TTP) haritalandırıldığı dünya standardı siber güvenlik kütüphanesidir.
* **Taktik (Neden?):** Saldırganın o anki amacı (Örn: Privilege Escalation - Yetki Yükseltme).
* **Teknik (Nasıl?):** Amaca ulaşmak için kullanılan yöntem (Örn: SUID Bits - Hatalı izin kullanımı).
