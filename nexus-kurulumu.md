## Nexus Repository Manager Kurulumu

### Java OpenJDK 8 Kurulumu

Nexus Repository Manager, Java OpenJDK ve JRE v8 gerektirir.

#### Sistem Güncellemesi
```bash
sudo apt update
```

#### Java OpenJDK 8 Kurulumu
```bash
sudo apt install openjdk-8-jdk
```

Kurulumu onaylamak için `Y` girin ve `ENTER`'a basın.

#### Java Sürüm Kontrolü
```bash
java -version
```

---

### Sistem Ayarları

#### Nexus Kullanıcısı Oluşturma
```bash
sudo useradd -d /opt/nexus -s /bin/bash nexus
sudo passwd nexus
```

#### Ulimit Ayarları

Geçici olarak ulimit'i ayarlama:
```bash
ulimit -n 65536
```

Kalıcı ulimit yapılandırması:
```bash
sudo nano /etc/security/limits.d/nexus.conf
```

**İçerik:**
```
nexus - nofile 65536
```

---

### Nexus Repository Manager Kurulumu

#### Nexus Paketini İndirme
```bash
wget https://download.sonatype.com/nexus/3/nexus-3.41.1-01-unix.tar.gz
```

#### Paketin Ayıklanması
```bash
tar xzf nexus-3.41.1-01-unix.tar.gz
```

Bu işlem iki dizin oluşturur:
- `nexus-3.41.1-01` - Nexus paketi ana dizini
- `sonatype-work` - Nexus çalışma dizini

#### Dizinleri Taşıma
```bash
mv nexus-3.41.1-01 /opt/nexus
mv sonatype-work /opt/
```

#### Sahiplik Değiştirme
```bash
chown -R nexus:nexus /opt/nexus /opt/sonatype-work
```

---

### Nexus Yapılandırması

#### Kullanıcı Yapılandırması
```bash
sudo nano /opt/nexus/bin/nexus.rc
```

**İçerik:**
```
run_as_user="nexus"
```

#### Heap Bellek Ayarları
```bash
sudo nano /opt/nexus/bin/nexus.vmoptions
```

**Değiştirilecek parametreler:**
```
-Xms1024m
-Xmx1024m
-XX:MaxDirectMemorySize=1024m
```

#### Host Yapılandırması
```bash
sudo nano /opt/sonatype-work/nexus3/etc/nexus.properties
```

**İçerik:**
```
application-host=127.0.0.1
```

---

### Nexus'u SystemD Servisi Olarak Çalıştırma

#### Servis Dosyası Oluşturma
```bash
sudo nano /etc/systemd/system/nexus.service
```

**İçerik:**
```ini
[Unit]
Description=nexus service
After=network.target

[Service]
Type=forking
LimitNOFILE=65536
ExecStart=/opt/nexus/bin/nexus start
ExecStop=/opt/nexus/bin/nexus stop
User=nexus
Restart=on-abort

[Install]
WantedBy=multi-user.target
```

#### Servisi Başlatma
```bash
sudo systemctl daemon-reload
sudo systemctl start nexus.service
sudo systemctl enable nexus.service
```

#### Servis Durumu Kontrolü
```bash
sudo systemctl status nexus.service
```

Nexus artık `127.0.0.1:8081` adresinde çalışmaktadır.

---

### Nginx Reverse Proxy Yapılandırması

#### Nginx Kurulumu
```bash
sudo apt install nginx
```

#### Nginx Durumu Kontrolü
```bash
sudo systemctl is-enabled nginx
sudo systemctl status nginx
```

#### Nexus İçin Server Block Oluşturma
```bash
sudo nano /etc/nginx/sites-available/nexus
```

**İçerik:**
```nginx
upstream nexus3 {
  server 127.0.0.1:8081;
}

server {
    listen 80;
    server_name nexus.howtoforge.local;

    location / {
        proxy_pass http://nexus3/;
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection "upgrade";
        proxy_set_header Host $http_host;

        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forward-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forward-Proto http;
        proxy_set_header X-Nginx-Proxy true;

        proxy_redirect off;
    }
}
```

**Not:** `server_name` değerini kendi alan adınıza göre değiştirin.

#### Server Block'u Aktifleştirme
```bash
sudo ln -s /etc/nginx/sites-available/nexus /etc/nginx/sites-enabled/
sudo nginx -t
```

#### Nginx'i Yeniden Başlatma
```bash
sudo systemctl restart nginx
```

Nexus Repository Manager artık alan adınız üzerinden erişilebilir durumdadır.
