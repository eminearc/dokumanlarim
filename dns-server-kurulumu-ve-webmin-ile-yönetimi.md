# BIND9 DNS Sunucusu Kurulum Rehberi

## Giriş

Bu dokümanda BIND9 DNS sunucusunu nasıl kurduğum, yapılandırdığımı ve test ettiğimi adım adım anlattım. Kurulumu Ubuntu 24.04 sisteminde gerçekleştirdim. Arkadaşımın liman adlı domainini zone olarak kullandım.

---

## Adım 1: BIND9 Kurulumu

DNS sunucusu yazılımını yüklemek için aşağıdaki komutu çalıştırdım:
```bash
sudo apt install bind9
```

Bu komut BIND9 ve gerekli bağımlılıklarını sisteme kurdu.

---

## Adım 2: BIND9 Yapılandırması

### 2.1 Ana Ayarlar Dosyasını Düzenleme

DNS sunucusunun temel ayarlarını yapılandırmak için `/etc/bind/named.conf.options` dosyasını açtım:
```bash
sudo nano /etc/bind/named.conf.options
```

Dosyada aşağıdaki ayarları yaptım:
```conf
options {
    directory "/var/cache/bind";
    
    forwarders {
        0.0.0.0;
    };
    
    dnssec-validation auto;
    listen-on-v6 { any; };
    recursion yes;
    listen-on port 53 {192.168.1.10; 127.0.0.1;};
    allow-query {any;};
};
```

**Ne yaptığım:**
- `listen-on port 53`: DNS sunucusunun hangi IP adresinde ve portta dinleme yapacağını belirledim. Sunucunun IP adresi (192.168.1.10) ve localhost ile dinlemesi için ayarladım.
- `allow-query`: DNS sorgularını kimin yapabileceğini belirledim. `any` değeri ile herkesten sorgu kabul etmesini sağladım.
- `recursion yes`: Sunucunun özyinelemeli sorguları desteklemesini aktif hale getirdim.

---

## Adım 3: DNS Bölgesi Tanımlama

### 3.1 Yerel Bölge Dosyasını Oluşturma

DNS bölgesini tanımlamak için `/etc/bind/named.conf.local` dosyasını açtım:
```bash
sudo nano /etc/bind/named.conf.local
```

Aşağıdaki satırları ekledim:
```conf
zone "liman" {
    type master;
    file "/etc/bind/db.liman";
};
```

**Ne yaptığım:**
- "liman" adında yeni bir DNS bölgesi tanımladım.(zone)
- Bu sunucuyu bu bölge için ana (master) sunucu olarak ayarladım.
- Bölgenin kayıtlarının nerede tutulacağını belirtim.

---

## Adım 4: DNS Kayıtlarını Yapılandırma

### 4.1 Bölge Dosyasını Oluşturma

DNS kayıtlarını içerecek dosyayı oluşturdum:
```bash
sudo nano /etc/bind/db.liman
```

Aşağıdaki içeriği dosyaya yazdım:
```dns
;
; BIND zone file for liman
;
$TTL    604800
@       IN      SOA     localhost. root.localhost. (
                              1         ; Serial
                         604800         ; Refresh
                          86400         ; Retry
                        2419200         ; Expire
                         604800 )       ; Negative Cache TTL
;
; Name Server (Bu sunucunun kendisi)
@       IN      NS      localhost.
; A Records (IP Adresleri)
@       IN      A       192.168.1.5   ; <-- @ işareti 'liman' anlamına gelir
```

**Ne yaptığım:**
- `$TTL 604800`: Kayıtların geçerlilik süresini belirledim (saniye cinsinden, 7 gün).
- `SOA`: Start of Authority kaydını ekledim. Bölge hakkında temel bilgileri içeriyor.
- `NS`: Name Server kaydı ekledim. Bu bölgenin hangi sunucu tarafından yönetildiğini belirttim.
- `A`: Address kaydı ekledim. Etki alanı adını IP adresine eşledim.
- `@`: Bölgenin kökünü (liman) temsil edecek şekilde kullandım.

---

## Adım 5: DNS Sunucusunun Test Edilmesi

### 5.1 Yerel Sorgulama

DNS sunucusunun çalışıp çalışmadığını test ettim. `dig` komutunu kullandım:
```bash
dig @localhost liman
```

Aldığım çıktı:
```
; <<>> DiG 9.18.39-0ubuntu0.24.04.2-Ubuntu <<>> @localhost liman
; (1 server found)
;; global options: +cmd
;; Got answer:
;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 31546
;; flags: qr aa rd ra; QUERY: 1, ANSWER: 1, AUTHORITY: 0, ADDITIONAL: 1

;; QUESTION SECTION:
;liman.                IN      A

;; ANSWER SECTION:
liman.          604800  IN      A       192.168.1.5

;; Query time: 1 msec
;; SERVER: 127.0.0.1#53(localhost) (UDP)
;; WHEN: Wed Nov 19 10:54:18 UTC 2025
;; MSG SIZE  rcvd: 78
```

`ANSWER SECTION` kısmında "liman" etki alanının 192.168.1.5 IP adresine başarıyla eşlendiğini gördüm.

---

## Adım 6: Diğer Makinelerden Erişim

### 6.1 Hosts Dosyası Üzerinden Yapılandırma

Diğer makinelerde DNS sunucusundan sorgu yapmaya başlamak için hosts dosyasını güncelledim. Sorguyu yapacak makinede:
```bash
sudo nano /etc/hosts
```

Aşağıdaki satırı ekledim:
```
192.168.1.5    liman
```

Dosyayı kaydettim.

### 6.2 DNS Resolver Ayarlarını Yapılandırma

DNS sunucusunun IP adresini tanıyabilmesi için `/etc/resolv.conf` dosyasını düzenledim:
```bash
sudo nano /etc/resolv.conf
```

Aşağıdaki satırı ekledim:
```
nameserver 192.168.1.10
```

Burada `192.168.1.10`, DNS sunucusunun çalıştığı makine IP adresidir.

Dosyayı kaydettim ve sistemi yeniden başlattım.

### 6.3 Diğer Makineden Sorgu Yapma

Sonra diğer makineden DNS sunucusuna sorgu yapabileceğini doğruladım:
```bash
dig @192.168.1.10 liman
```

Veya daha basit:
```bash
nslookup liman
```
### 6.3 Diğer makinede browserdan liman arayüzüne gitme
diğer makinede browsera ``https://liman`` yazdığımda arayüze erişebildiğimi gördüm.
<img width="1639" height="787" alt="image" src="https://github.com/user-attachments/assets/6bfe9efc-5998-4aa7-bcbc-a3d3ae2737ba" />

---

## Notlar ve İpuçları

- NetworkManager bazı durumlarda `/etc/resolv.conf` dosyasını otomatik olarak değiştirdiğini fark ettim. Bu durumda kalıcı ayar yapmak gerekebilir.

- DNS sunucusu sorunlar yaşadığı zaman, BIND9 log dosyalarını kontrol ettim:
```bash
sudo journalctl -u bind9 -f
```

- Daha fazla kayıt eklemek istediğimde, `db.liman` dosyasına A, CNAME, MX gibi kayıtları ekleyebileceğimi öğrendim.

---

## Sonuç

Bu rehberde BIND9 DNS sunucusunu kurduk, yapılandırdık ve test ettik. Sunucu artık yerel ağ içinde "liman" adlı etki alanı için sorguları cevaplamaya hazır.



webmin arayüzüne girdim:

1. Yeni "Bölgeler" (Zones) Eklemek
Nasıl Yapılır: Webmin ana sayfasında "Create master zone" linkine tıklayarak.
   <img width="1155" height="203" alt="image" src="https://github.com/user-attachments/assets/f1b88382-0cc9-42e7-baed-41a2ecb2535d" />

Senaryo: Yarın "Kubernetes" projesi için k8s.local adında yeni bir alan adı oluşturup, tüm küme (cluster) IP'lerini buraya girebilirsin.
<img width="1015" height="534" alt="image" src="https://github.com/user-attachments/assets/03e5c6fb-00b7-4541-9958-b22c5cd46339" />

2. Bölge İçine Ekleyebileceğin "Kayıtlar" (Records)
   Bir bölgenin içine girdiğinde (az önceki ekran görüntüsündeki liste), şu kayıt türlerini ekleyebilirsin:
   <img width="1342" height="785" alt="image" src="https://github.com/user-attachments/assets/f501e838-faa9-46c9-976a-412e84a0a2e8" />


A - Address (Adres Kaydı): En Temel Olan.
Ne yapar: İsim -> IPv4 adresi eşleşmesi.

Örnek: kamera.liman -> 192.168.1.50

Kullanım: Sunucular, yazıcılar, cihazlar için.

CNAME - Name Alias (Takma İsim): Tembeller İçin.

Ne yapar: Bir ismi başka bir isme yönlendirir (IP'ye değil).

Örnek: ftp.liman -> dosya.liman

Kullanım: Eğer dosya.liman'ın IP adresi değişirse, ftp için ayrıca ayar yapmana gerek kalmaz; o da otomatik güncellenmiş olur.

3. İleri Düzey Ayarlar (Global Server Options)
Webmin ana sayfasında "Forwarding and Transfers" veya "Access Control Lists" gibi butonlar göreceksin. Buradan şunları yapabilirsin:

Forwarding (Yönlendirme): Bizim elle named.conf.options dosyasına eklediğimiz 8.8.8.8 ayarı var ya? İşte onu buradan görsel olarak yönetebilirsin. "Benim bilmediğim adresleri kime sorayım?" ayarıdır.

ACLs (Erişim Kontrolü): "Benim DNS sunucumu kimler kullanabilir?" ayarıdır.

Örneğin: "Sadece 10.67.x.x ağındakiler bana soru sorabilsin, yabancılar (internet) soramasın" diyerek güvenliği artırabilirsin.


