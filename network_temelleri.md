# AĞ TEKNOLOJİLERİ VE SİSTEM YÖNETİMİ DOKÜMANI

## BÖLÜM 1: AĞ STANDARTLARI VE PROTOKOLLERİ

Bir ağdaki cihazlar veri göndermek ve almak için aynı prosedürleri kullanmalıdır. Bu prosedürlere **protokol** denir.

### Protokolleri Belirleyen Kurum ve Kuruluşlar

* **IEEE (Institute of Electrical and Electronics Engineers):**
    * En bilinen katkılarından biri LAN ve MAN için **IEEE 802** standardıdır.
    * Wi-Fi ve Ethernet gibi teknolojilerin temelini oluşturur.
* **IETF (Internet Engineering Task Force):**
    * TCP/IP protokolü gibi internetin temel taşlarını oluşturan teknolojiler üzerine çalışır.
    * **RFC (Request for Comments)** belgeleri aracılığıyla standartları yayınlar.
* **ISO (International Organization for Standardization):**
    * Uluslararası standartlar geliştiren ve yayınlayan bağımsız, sivil bir kuruluştur.
    * **ISO/IEC 27000** serisi, bilgi güvenliği yönetimi standartlarını içerir.
* **ITU (International Telecommunication Union):**
    * Birleşmiş Milletler'e bağlı bir kurumdur.
    * **ITU-T** sektörü telekomünikasyon için teknik standartlar oluştururken, **ITU-R** radyo frekanslarının kullanımı ile ilgili standartları yönetir.
* **ANSI (American National Standards Institute):**
    * Uluslararası standartlarla Amerikan standartlarının uyumlu olmasına yardımcı olur.
* **W3C (World Wide Web Consortium):**
    * World Wide Web için standartlar geliştiren bir konsorsiyumdur.
    * HTML, CSS ve XML gibi web teknolojilerinin standartlarını belirler.
    * Webin açık, erişilebilir ve evrensel olarak çalışmasını sağlamak için kılavuzlar yayınlar.

***

## BÖLÜM 2: AĞ TÜRLERİ

### LAN (Local Area Network)
* Küçük bir alanı kapsar, yüksek hızlı veri transferi sunar.
* **Gbps (Gigabit Per Second)** düzeyinde veri transfer hızı sağlar.
* Ağa bağlı cihazlar, ortak kaynakları paylaşabilir.
* Ağdaki verilerin korunması için güvenlik protokolleri veya ağ güvenlik cihazları kullanılabilir.

### WAN (Wide Area Network)
* Şehirler, ülkeler hatta kıtalar arası iletişimi mümkün kılarak, uzaktaki ağları birleştiren geniş kapsamlı ağlardır.
* Fiber optik kablolar, uydu bağlantıları, telefon hatları gibi çeşitli teknolojiler kullanılır.
* LAN'lara göre daha düşük veri transfer hızları sunar.
* Küresel ölçekte iletişim ve kaynak paylaşımı sağlar.

### MAN (Metropolitan Area Network)
* Şehir ölçeğindeki bir coğrafi alanda dağılmış olan bilgisayar ağlarını birbirine bağlar.
* Büyük dosyaların ve verilerin hızlı bir şekilde transfer edilmesine olanak tanır.
* Şehir yönetimleri, kamu güvenliği, sağlık hizmetleri ve eğitim kurumları tarafından tercih edilir.
* Kurulumu genellikle internet servis sağlayıcıları (ISS) veya büyük kuruluşların IT departmanları tarafından yapılır.

### INTERNET
* Farklı coğrafi konumlardaki kullanıcıların ve sistemlerin birbiriyle etkileşime geçmesini sağlayan küresel ağ altyapısıdır.

### INTRANET
* Bir organizasyon veya kuruluş içinde sınırlı kullanıcı grubuna özel olarak tasarlanmış özel bir ağdır.
* Genel erişime kapalıdır, yalnızca yetkili kullanıcılar erişebilir.

***

## BÖLÜM 3: AĞ CİHAZLARI

### NIC (Network Interface Card)
Bilgisayarın ağlara bağlanmasını sağlayan fiziksel donanım bileşenidir.
* Her NIC, dünyada benzersiz olan bir **MAC (Media Access Control)** adresine sahiptir.
* **Kablolu NIC:** Ethernet kablosu (RJ-45) veya fiber optik ile bağlanır. Kararlı ve güvenlidir.
* **Kablosuz NIC:** Wi-Fi teknolojisi ile kablosuz bağlantı sağlar.

### HUB
* Ağdaki cihazları birbirine bağlar.
* Gelen veriyi **bağlı olan tüm portlara** gönderir (Broadcast).
* Gereksiz trafik yaratır ve güvenlik açısından zayıftır (tüm cihazlar veriyi görebilir).

### SWITCH (Anahtar)
* Ağdaki cihazları birbirine bağlar ancak Hub'dan daha akıllıdır.
* **MAC adresleri** tabanında çalışır; veriyi yalnızca hedef cihaza iletir.
* **Switch Türleri:**
    * **Yönetilmeyen Switch:** Tak-çalıştır, ayar gerektirmez, küçük ağlar içindir.
    * **Yönetilebilir Switch:** VLAN, QoS gibi gelişmiş ayarlar sunar, büyük ağlar içindir.
    * **Akıllı Switch:** Yönetilenlere benzer ancak daha basittir, KOBİ'ler içindir.

### MODEM
* Analog sinyalleri dijital verilere, dijital verileri analog sinyallere çevirir (**Mo**dulator-**Dem**odulator).
* **Türleri:** Dial-Up, DSL (ADSL/VDSL), Kablo, Fiber Optik, Kablosuz (3G/4G/5G).

### ACCESS POINT (Erişim Noktası)
* Kablosuz cihazların kablolu bir ağa bağlanabilmesini sağlar.
* Wi-Fi sinyalini alır ve ethernet üzerinden router veya switch'e aktarır.

### FIREWALL (Güvenlik Duvarı)
* Ağ içindeki ve dışındaki trafik arasında güvenlik sağlar.
* İzinsiz erişimi önler ve zararlı trafiği engeller.
* **Türleri:**
    * **Paket Filtreleme:** IP, port ve protokole göre filtreler.
    * **Durum Denetimli (Stateful):** Bağlantı durumunu takip eder.
    * **Uygulama Katmanı (Proxy):** İçeriği derinlemesine inceler.
    * **Next-Generation (NGFW):** IPS ve derin paket inceleme özelliklerine sahiptir.

***

## BÖLÜM 4: KABLOLAMA VE BAĞLANTI

### Koaksiyel Kablo
Eski bir teknolojidir, günümüzde daha çok TV/Uydu sistemlerinde görülür.
* **RG-6:** Kablo TV ve internet için kullanılır.
* **RG-58:** Eski "İnce ağ" Ethernet (10Base2).
* **Konnektörler:** BNC (Kamera/Ağ), F-Konnektör (TV/Uydu).

### Çift Bükümlü Kablo (Twisted Pair)
LAN kurulumunda en yaygın kullanılan kablodur.
* **UTP (Unshielded):** Korumasız, en yaygın türdür (Ev/Ofis).
* **STP (Shielded):** Korumalı, parazitin yoğun olduğu sanayi ortamlarında kullanılır.
* **Konnektörler:**
    * **RJ-11:** Telefon (4 pin).
    * **RJ-45:** Ethernet/Ağ (8 pin).
    * **DB-9:** Seri bağlantılar (9 pin).

### Bağlantı Standartları (TIA/EIA 568A & 568B)
RJ-45 için renk dizilim standartlarıdır. **568B** daha yaygın kullanılır.
* **Düz Bağlantı:** Farklı cihazlar arası (PC -> Switch).
* **Çapraz Bağlantı:** Aynı cihazlar arası (PC -> PC, Switch -> Switch). *Not: Modern cihazlarda Auto-MDIX sayesinde buna gerek kalmamıştır.*

### Fiber Optik Kablo
Işık sinyalleri ile veri iletir. Çok yüksek hız ve uzun mesafe sağlar. EMI'den etkilenmez.
* **Çok Modlu (MMF):** Kısa mesafe (Bina içi), daha ucuz.
* **Tek Modlu (SMF):** Uzun mesafe (Şehirlerarası), daha pahalı.
* **Konnektörler:** LC, SC, MTRJ.

***

## BÖLÜM 5: AĞ TOPOLOJİLERİ

1.  **Yıldız (Star):** Tüm cihazlar merkezi bir noktaya (Switch/Hub) bağlanır. En yaygın kullanımdır. Arıza tespiti kolaydır.
2.  **Halka (Ring):** Cihazlar halka şeklinde bağlanır. Veri tek yönde akar. Bir kopukluk tüm ağı etkiler.
3.  **Otobüs (Bus):** Tek bir ana hat üzerine tüm cihazlar bağlanır. Kablo koparsa ağ durur.
4.  **Ağaç (Tree):** Yıldız topolojilerin hiyerarşik birleşimidir.
5.  **Örgü (Mesh):**
    * **Tam Örgü:** Her cihaz diğerine bağlıdır. Çok güvenlidir ama pahalıdır.
    * **Kısmi Örgü:** Kritik cihazlar çoklu bağlanır.
6.  **Eşten Eşe (P2P):** Merkezi sunucu yoktur, her cihaz eşittir.
7.  **Hibrit:** Birden fazla topolojinin karışımıdır.

***

## BÖLÜM 6: İLETİŞİM MODLARI VE TÜRLERİ

### İletişim Modları (Akış Yönü)
1.  **Simpleks:** Tek yönlü (Örn: Klavye -> PC, Radyo yayını).
2.  **Half Duplex:** İki yönlü ama sırayla (Örn: Telsiz).
3.  **Full Duplex:** İki yönlü ve aynı anda (Örn: Telefon, Ethernet).

### İletişim Türleri (Hedef Sayısı)
1.  **Unicast:** Bire Bir (1 -> 1).
2.  **Multicast:** Bire Çok/Grup (1 -> Belirli Grup).
3.  **Broadcast:** Bire Herkes (1 -> Tüm Ağ).

***

## BÖLÜM 7: ADRESLEME (MAC ve IP)

### I. Fiziksel Adresleme: MAC Adresi
* NIC kartının değişmez kimliğidir.
* 48 bit (6 bayt) uzunluğundadır, Hexadecimal gösterilir (Örn: `00:21:70:6F:06:F2`).
* İlk 24 bit üreticiyi temsil eder.

### II. Mantıksal Adresleme: IP Adresi (IPv4)
* Ağdaki cihazı tanımlayan, değiştirilebilir mantıksal adrestir.
* 32 bitten oluşur (4 oktet). Örn: `192.168.1.10`.
* **Bileşenler:** IP Adresi, Alt Ağ Maskesi (Subnet Mask), Varsayılan Ağ Geçidi (Gateway).

#### IPv4 Adres Sınıfları
| Sınıf | Ağ Bitleri | Host Bitleri | Adres Aralığı |
| :--- | :--- | :--- | :--- |
| **A** | 8 | 24 | 1.0.0.0 - 126.255.255.255 |
| **B** | 16 | 16 | 128.0.0.0 - 191.255.255.255 |
| **C** | 24 | 8 | 192.0.0.0 - 223.255.255.255 |

#### IP Adres Türleri
* **Genel (Public) IP:** İnternette kullanılan, ISS tarafından verilen benzersiz adresler.
* **Özel (Private) IP:** Yerel ağlarda (LAN) kullanılır, internete çıkamaz (NAT gerekir).
    * Aralıklar: `10.0.0.0/8`, `172.16.0.0/12`, `192.168.0.0/16`.
* **Geri Döngü (Loopback):** `127.0.0.1` (Localhost test adresi).

#### Alt Ağlar (Subnetting) ve CIDR
* **CIDR:** `/24` gibi gösterimlerdir. `/24` demek, 24 bitin ağa, kalan 8 bitin hosta ait olduğunu söyler.
* **Host Sayısı Formülü:** $2^n - 2$ (n = host biti sayısı).

***

## BÖLÜM 8: AĞ MODELLERİ (OSI ve TCP/IP)

### Genel Bakış
* **OSI Modeli (7 Katman):** Teorik referans modelidir. Sorun giderme ve eğitimde kullanılır.
* **TCP/IP Modeli (4 Katman):** İnternetin çalıştığı pratik modeldir.

### 1. OSI Modeli (Açık Sistemler Arası Bağlantı)
ISO tarafından geliştirilen, iletişimi 7 katmana bölen standarttır.

#### A. Üst Katmanlar (Yazılım)
* **7. Uygulama (Application):** Kullanıcı arayüzü (HTTP, FTP, SMTP).
* **6. Sunum (Presentation):** Veri formatlama, şifreleme, sıkıştırma (JPEG, ASCII).
* **5. Oturum (Session):** Oturum başlatma ve yönetme.

#### B. Alt Katmanlar (Veri Taşıma)
* **4. Taşıma (Transport):** Uçtan uca güvenli iletim (TCP/UDP). Veriyi **Segment**lere böler.
* **3. Ağ (Network):** Rotalama ve mantıksal adresleme (IP, Router). Veri birimi **Paket**tir.
* **2. Veri Bağlantı (Data Link):** Fiziksel adresleme (MAC, Switch). Veri birimi **Çerçeve (Frame)**dir.
* **1. Fiziksel (Physical):** Bitlerin kablo/sinyal ile taşınması. (Kablolar, Hub).

---

### 2. TCP/IP Modeli
İnternetin temel çerçevesidir. 4 Katmandan oluşur.

#### Katmanlar ve Protokoller
1.  **Uygulama Katmanı:** OSI'nin üst 3 katmanını kapsar. (HTTP, DNS, SSH, FTP).
2.  **Taşıma Katmanı:** Veri akış kontrolü.
    * **TCP:** Güvenilir, bağlantılı (3-way handshake), yavaş.
    * **UDP:** Güvensiz, bağlantısız, hızlı (Video/Ses akışı).
3.  **İnternet Katmanı:** En uygun yolun bulunması (IP, ICMP, ARP).
4.  **Ağ Erişim Katmanı:** Fiziksel donanım ve sürücüler (Ethernet).

***

