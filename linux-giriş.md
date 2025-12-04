## Linux'a Giriş: Felsefe ve Temeller

Linux, **güvenliği**, **esnekliği** ve **açık kaynak** yapısıyla bilinen güçlü bir işletim sistemi çekirdeğidir.

### Temel Prensipler
* **Özgürlük:** Kullanıcılara yazılımı kontrol etme, değiştirme ve yeniden dağıtma özgürlüğü tanır, bu da inovasyonu ve şeffaflığı teşvik eder.
* **İşbirliği:** Dünya genelindeki geliştirici topluluğu tarafından sürekli geliştirilir ve desteklenir.
* **Çoklu Görev ve Çoklu Kullanıcı:** Birden fazla kullanıcının aynı anda sistemi kullanmasına ve her kullanıcının aynı anda birden fazla program çalıştırmasına olanak tanır. Özellikle sunucu ortamlarında kritik bir özelliktir.

### Çekirdek (Kernel)
**Çekirdek**, işletim sisteminin kalbidir. Bilgisayarın **donanımını** (CPU, bellek, çevre birimleri) yönetir ve tüm yazılım uygulamalarının fiziksel donanımla güvenli bir şekilde etkileşim kurmasını sağlar.

***

## Linux Mimarisi

Linux, katmanlı bir yapıda organize edilmiştir:

1.  **Donanım Katmanı (Hardware):** Sistemin fiziksel bileşenleridir (CPU, RAM, depolama, çevre birimleri).
2.  **Çekirdek Katmanı (Kernel):** Yazılım ve donanım arasında aracıdır. Kaynakları yönetir ve programların birbirine müdahale etmesini önler.
3.  **Kabuk Katmanı (Shell):** Kullanıcının **çekirdek** ile iletişim kurmasını sağlar. Genellikle **Komut Satırı Arayüzü (CLI)** veya bir **Grafik Kullanıcı Arayüzü (GUI)** olabilir.
4.  **Sistem Yardımcı Programları (System Utilities):** Dosya yönetimi, ağ yapılandırması ve yazılım yükleyiciler gibi görevleri gerçekleştirmek için gereken araçları ve uygulamaları içerir.

***

## Linux Dağıtımları (Distributions)

Bir Linux dağıtımı, **Linux Çekirdeği** etrafında oluşturulmuş eksiksiz bir işletim sistemidir.

### Temel Bileşenler
* Linux Çekirdeği
* Sistem Kütüphaneleri
* Yazılım Uygulamaları
* Paket Yöneticisi (Örn: `apt`, `dnf`)
* Önyükleyici (Bootloader)
* Grafik Kullanıcı Arayüzü (GUI) (Örn: GNOME, KDE - isteğe bağlıdır ancak yaygındır)

### Popüler Dağıtımlar ve Kullanım Alanları
| Dağıtım | Odak Noktası | Kullanım Alanları |
| :--- | :--- | :--- |
| **Ubuntu** | Kullanım kolaylığı | Masaüstü, Sunucu, Bulut |
| **Fedora** | Güncel teknolojiler | Geliştiriciler, Sistem Yöneticileri |
| **CentOS** | Kararlılık, Kurumsal | Sunucular, Kurumsal Ortamlar |
| **Debian** | Kararlılık | İleri düzey kullanıcılar, Stabil ortamlar |
| **Arch Linux** | Özelleştirme, Sadeliğe | Deneyimli kullanıcılar, Geliştirme, Kişisel Sunucular |
| **Kali Linux** | Güvenlik | Güvenlik araştırmaları, Penetrasyon testi, Etik Hacking |

***

## Linux Kabuğu (Shell) ve Temel Komutlar

**Kabuk**, kullanıcının komutları yorumlayıp çekirdeğe ilettiği programdır. **Terminal uygulaması** ise kullanıcının kabukla etkileşim kurmasını sağlayan kapıdır.

### Yaygın Kabuk Türleri
* **BASH** (Bourne Again SHell): En yaygın kullanılan ve varsayılan kabuktur. Kapsamlı betikleme yeteneklerine sahiptir.
* **ZSH** (Z Shell): Otomatik tamamlama, özelleştirilebilir istemler ve eklenti desteğiyle popülerdir.
* **Fish** (Friendly Interactive SHell): Kullanıcı dostu, sözdizimi vurgulama ve otomatik öneriler sunar.

### Dosya ve Dizin Yönetimi Komutları
| Komut | Açıklama | Örnek Kullanım |
| :--- | :--- | :--- |
| **pwd** | Mevcut çalışma dizinini (konumu) gösterir. | `pwd` |
| **ls** | Dizin içeriğini listeler. | `ls -l` (detaylı listeleme) / `ls -a` (gizli dosyalar dahil) |
| **touch** | Yeni bir dosya oluşturur. | `touch dosya.txt` |
| **mkdir** | Yeni bir klasör (dizin) oluşturur. | `mkdir belgeler` |
| **cp** | Dosya ve klasörleri kopyalar. | `cp kaynak hedef` / `cp -r klasör1 klasör2` (klasörler için) |
| **mv** | Dosya ve klasörleri taşır veya yeniden adlandırır. | `mv eski_ad yeni_ad` / `mv dosya dizin/` |
| **find** | Dosya sisteminde arama yapar. | `find / -name "notlar.txt"` / `find /home -type d` |
| **locate** | Dosya veritabanında daha hızlı arama yapar. | `locate notlar.txt` |
| **which** | Bir komutun tam yolunu gösterir. | `which python` |

### Metin ve Log Dosyası İşleme Komutları
| Komut | Açıklama | Kullanım Durumu |
| :--- | :--- | :--- |
| **head** | Dosyanın ilk 10 satırını (varsayılan) görüntüler. | `head -n 5 dosya.txt` (İlk 5 satır) |
| **tail** | Dosyanın son 10 satırını (varsayılan) görüntüler. | `tail -n 15 log.txt` (Son 15 satır) |
| **grep** | Dosyalarda metin arar ve eşleşen satırları gösterir. | `grep 'hata' log.txt` |
| **wc** | Dosyadaki satır, kelime ve karakter sayısını sayar. | `wc -l /etc/passwd` (Sadece satır sayısı) |
| **sed** | Metinleri filtrelemek ve dönüştürmek için kullanılır. | `sed 's/eski/yeni/' dosya.txt` (Değiştirme) |
| **awk** | Sütun bazlı verilerle çalışırken etkili, metin ve veri işleme aracıdır. | `awk '{print $1}' isimler.txt` (Her satırın ilk sütununu yazdırma) |
| **Log Dosyası** | `auth.log` dosyası, kullanıcı kimlik doğrulama olaylarını kaydeder (giriş/çıkış, `sudo` kullanımları, SSH oturumları). | - |

***

## Paket Yönetimi (Debian Tabanlı)

Paket yöneticisi (**APT**), sisteminize yazılım yüklemek, kaldırmak ve güncellemek için kullanılır.

* **Paket Listesi Güncelleme:**
    * `sudo apt update`: Yerel paket listesi veritabanını güncel sunuculardan çeker. Bu, en güncel yazılım sürümlerini bulmanızı sağlar.

* **Paket Kurma:**
    * `sudo apt install paket_adı`

* **Paket Kaldırma:**
    * `sudo apt remove paket_adı`: Paketi kaldırır, ancak ayar dosyalarını ve deb dosyalarını bırakır.
    * `sudo apt purge paket_adı`: Paketi **ve** tüm ayar dosyalarını tamamen kaldırır.

***

## Kullanıcı ve Grup Yönetimi

Linux'ta her kullanıcı, bir **Kullanıcı ID (UID)** ve **Grup ID (GID)** ile tanımlanır.

### Kullanıcı Türleri
* **Sistem Kullanıcıları:** Kurulum sırasında oluşturulur, sistem servislerini çalıştırmak için kullanılır.
* **Normal Kullanıcılar:** Yönetici tarafından oluşturulur ve sisteme erişim sağlar.

### Kullanıcı Komutları
* **Oluşturma:** `useradd -u 1002 -d /home/john -s /bin/bash john`
    * `-u`: UID'yi belirler.
    * `-d`: Ana dizini belirler.
    * `-s`: Varsayılan kabuğu belirler.
* **Silme:** `sudo userdel john`

### Grup Komutları
* **Grup Oluşturma:** `sudo groupadd development`
* **Gruba Kullanıcı Ekleme:** `sudo usermod -aG development john`
    * `-aG`: Kullanıcıyı mevcut gruplarından çıkarmadan belirtilen gruba ekler.
* **Grup Silme:** `sudo groupdel development`
* **Grup Görüntüleme:** `/etc/group` dosyasında listelenir.

***

## Süreç Yönetimi (Process Management)

Bir **süreç**, belleğe yüklenmiş ve CPU'da yürütülmekte olan bir programdır.

### Süreç Türleri
* **Ön Plan (Foreground):** Klavyeden girdi alır ve terminali meşgul eder.
* **Arka Plan (Background):** Arka planda çalışır, terminali kitlemez. Komut sonuna `&` eklenerek başlatılır. (Örn: `ping 127.0.0.1 &`)

### Süreç Kontrol Komutları
| Komut | Açıklama |
| :--- | :--- |
| **jobs** | Mevcut terminalde arka planda çalışan işleri (job) listeler. |
| **fg %N** | Belirtilen iş numarasındaki (`%N`) arka plan sürecini ön plana getirir. |
| **ps** | Sistem genelinde çalışan süreçleri görüntüler. |
| **kill PID** | Belirtilen **Süreç ID'sine (PID)** sahip sürece sonlandırma sinyali gönderir. |
| **kill -9 PID** | Süreci zorla kapatmak için kesme sinyali gönderir. |
| **Durdurma** | Ön plan süreçleri için: **Ctrl+C** |

***

## Ağ Yönetimi (Debian Tabanlı)

**Ağ Arayüz Kartları (NIC)**, bilgisayarın ağa bağlanmasını sağlar.

### `ifconfig` Komutu
* **Tüm Arayüzleri Listeleme:** `ifconfig` (mevcut aktif arayüzler) / `ifconfig -a` (aktif/pasif tüm arayüzler).
    * `eth0`: Ethernet kartı arayüzü.
    * `lo`: Geri döngü (loopback) arayüzü (127.0.0.1 yerel ağı).
* **Arayüz Kontrolü:**
    * Aktif etme: `ifconfig eth0 up`
    * Pasif etme: `ifconfig eth0 down`
* **IP/Netmask Atama:**
    * IP: `ifconfig eth0 172.20.1.110`
    * Netmask: `ifconfig eth0 netmask 255.255.255.0`
* **Promiscuous Modu:** Ağa gelen, bizimle alakalı olmayan paketleri de CPU'ya göndererek trafik izlemeyi sağlar.
    * Açma: `ifconfig eth0 promisc`
    * Kapatma: `ifconfig eth0 -promisc`
* **MAC Adresi Değiştirme:** `ifconfig eth0 hw ether AA:BB:CC:DD:EE:FF`

### DNS Ayarları
DNS ayarları, `/etc/resolv.conf` dosyası düzenlenerek yapılır.

***

## SSH (Secure Shell)

**SSH**, güvenli ağ hizmetleri için bir protokoldür. Uzak makineye güvenli bağlantı kurmayı sağlar.

### SSH Kurulumu
1.  `sudo apt-get update`
2.  `sudo apt-get install openssh-server`
3.  `sudo systemctl start ssh`
4.  `sudo systemctl enable ssh`

### SSH Anahtar Çifti (Key Pair) Oluşturma
Paroladan daha güvenli bir yöntemdir.
1.  **Anahtar Oluşturma:** `ssh-keygen`
2.  **Anahtarı Kopyalama:** Oluşturulan anahtarı uzak sunucuya kopyalamak için: `ssh-copy-id user@ip_address`
    * Bu adımdan sonra parola girmeden bağlanılabilir.

### SSH Portunu Değiştirme
Güvenlik için varsayılan portu (22) değiştirmek yaygındır.
1.  `/etc/ssh/sshd_config` dosyasını düzenleyin.
2.  `Port 2222` gibi farklı bir port ayarlayın.
3.  Değişikliklerden sonra servisi yeniden başlatın: `sudo systemctl restart ssh`
