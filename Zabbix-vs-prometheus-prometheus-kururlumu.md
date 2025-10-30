## Zabbix vs Prometheus Karşılaştırması

### Temel Farklar

| Özellik | Zabbix | Prometheus |
|---------|--------|------------|
| Veritabanı Tipi | Relational (MySQL, PostgreSQL vb.) | Time Series DB (zaman serisi odaklı) |
| Veri Çekme | Agent/Trap/Push tabanlı | Pull tabanlı (kendisi veri çeker) |
| Alert Sistemi | Web arayüzden tanımlanır | Config dosyalarıyla tanımlanır (rule-based) |
| Veri Modeli | Tablo yapısı, ilişkisel | Metric + Label yapısı, zaman serisi |
| Kullanım Alanı | Geleneksel IT altyapıları | Cloud-native, container ve mikroservis ortamları |
| Grafik ve UI | Dahili dashboard | Genellikle Grafana ile entegre |

---

### Prometheus'un Avantajları

#### 1. Daha Kolay Keşif ve Yapılandırma
- **Zabbix:** Her bir bağımlı öğeyi tek tek tanımlamanız gerekir.
- **Prometheus:** Endpoint'i tanımlamanız yeterlidir. Prometheus, o adresteki tüm metrikleri otomatik olarak çeker, etiketlerini okur ve etiketleri temel alarak yeni zaman serileri oluşturur. Ek bir metrik eklemek için sadece uygulamanın metrik vermesi yeterlidir.

#### 2. Daha Dinamik Etiketleme
- **Zabbix:** Etiketler genellikle manuel olarak veya şablonlarla tanımlanır.
- **Prometheus:** Etiketler (labels) doğrudan metrikle birlikte gelir. Örneğin: `mppt_values{city="Adana", ...}` gibi veri kaynağı kendi bağlamını sağlar ve Prometheus bu etiketleri otomatik olarak alır.

#### 3. Merkezi Kontrol
Veri toplama sıklığı (scrape interval) gibi ayarlar, merkezi olarak Prometheus sunucusundan yönetilir. Zabbix'teki gibi her bir ajanı tek tek yapılandırmanıza gerek kalmaz.

---

### Mimari Farklılıklar

#### Veritabanı
**Zabbix:**
- Metrikleri geleneksel ilişkisel veritabanına (MySQL, PostgreSQL vb.) yazar
- Genel amaçlı veritabanları, milyonlarca zaman damgalı metrik verisi için özel olarak optimize edilmemiştir
- Eski verilere erişim veya karmaşık sorgular zamanla yavaşlayabilir

**Prometheus:**
- Özel bir zaman serisi veritabanına yazar
- Veriyi zaman serisi (`metric_name{labels}`) olarak saklamak için sıfırdan tasarlanmıştır
- Binlerce metrik noktasını saniyeler içinde toplu olarak sorgulama son derece verimlidir

#### Odak Noktası
**Zabbix:**
- Kapsamlı bir izleme platformudur (İsviçre Çakısı gibi)
- Ağ keşfi, envanter yönetimi, web izleme, kullanıcı izinleri gibi birçok işlevi tek platformda birleştirir

**Prometheus:**
- Zaman serisi izleme sistemidir (Unix Felsefesi - tek işi çok iyi yapar)
- Metrik toplar ve sorgular
- Panolar için Grafana, gelişmiş uyarılar için Alertmanager gibi araçlarla entegre çalışır

---

### Detaylı Karşılaştırma

#### 1. Mimari ve Veri Toplama Yaklaşımı

**Veri Toplama Modeli:**
- **Zabbix:** İtme (Push) modeli - Ajanlar veya cihazlar veriyi merkezi sunucuya gönderir
- **Prometheus:** Çekme (Pull) modeli - Merkezi sunucu hedeflerdeki metrikleri belirli aralıklarla kendisi çeker

**Ajan/Exporter Yaklaşımı:**
- **Zabbix:** Hostlara Zabbix Ajanı kurulur, ağ cihazları için SNMP kullanılır
- **Prometheus:** Her kaynak için özel "Exporter" servisleri kullanılır (Node Exporter, SNMP Exporter vb.)

#### 2. Veritabanı ve Veri Modeli

**Veritabanı Türü:**
- **Zabbix:** Geleneksel ilişkisel veritabanı (PostgreSQL, MySQL)
- **Prometheus:** Özel tasarlanmış yüksek performanslı zaman serisi veritabanı

**Metrik Yapısı:**
- **Zabbix:** Hiyerarşik "Öğe (Item)" ve "Bağımlı Öğeler (Dependent Items)"
- **Prometheus:** Her metrik "isim" ve "etiketler (labels)" ile tanımlanır

#### 3. Kapsam ve Temel Özellikler

**Odak Alanı:**
- **Zabbix:** Çok yönlü - izleme, raporlama, ağ keşfi, envanter yönetimi, web senaryoları
- **Prometheus:** Sadece metrik toplama, depolama ve sorgulama

**Yerleşik Özellikler:**
- **Zabbix:** Yerleşik panolar, raporlama, kullanıcı yönetimi
- **Prometheus:** Temel web arayüzü, gelişmiş panolar için Grafana gerekir

#### 4. Ölçeklenebilirlik ve Dağıtık Yapı

**Dağıtık Mimari:**
- **Zabbix:** Zabbix Proxy mimarisi
- **Prometheus:** Exporter'lar ve federasyon (federation)

**Performans:**
- **Zabbix:** Yoğun veri trafiği performans darboğazlarına yol açabilir
- **Prometheus:** Milyonlarca metriği düşük kaynak tüketimiyle çok hızlı işler

---

### Performans Karşılaştırması

#### 1. Hız
**Prometheus Daha Hızlı:**
- Zaman serisi verileri için özel tasarlanmış veritabanı
- PromQL sorgu dili milyonlarca veri noktasını saniyeler içinde işleyebilir
- Aktif çekme (pull) mekanizması veri alımını verimli hale getirir

**Zabbix:**
- Genel amaçlı ilişkisel veritabanı kullanır
- Büyük veri kümelerinde sorgular yavaşlayabilir

#### 2. Esneklik
**Prometheus Çok Daha Esnek:**
- Etiket (label) tabanlı veri modeli
- Yeni özellikler otomatik olarak etiket halinde gelir
- PromQL ile güçlü birleştirme, gruplama ve matematiksel işlemler

**Zabbix:**
- Item ve Dependent Item yapısı daha katı
- Yeni metrik için genellikle yapılandırma değişikliği gerekir

#### 3. Anlaşılırlık ve Kolaylık
**Zabbix - Başlangıçta Daha Kolay:**
- Tek pakette her şey mevcut
- Web arayüzü üzerinden kolay yapılandırma
- Kodlama bilgisi gerektirmez

**Prometheus - Mimari Karmaşık, Felsefe Basit:**
- Ek araçlara ihtiyaç duyar (Grafana, Alertmanager)
- Daha fazla öğrenme eğrisi
- "Veriyi kendim çekerim" ve "etiketlerle her şeyi yaparım" felsefesi anlaşıldığında kullanımı basitleşir

#### 4. Görselleştirme
**Zabbix:**
- Yerleşik grafikler, raporlar, ağ haritaları
- Tek arayüzden kapsamlı görselleştirme

**Prometheus + Grafana:**
- Prometheus'un kendi arayüzü temel sorgulama için yeterli
- Grafana ile entegre edildiğinde rakipsiz
- Esnek, özelleştirilebilir panolar
- Farklı veri kaynaklarını bir araya getirme yeteneği

---

## Prometheus Kurulumu

### Sistem Güncellemesi
```bash
sudo apt update && sudo apt upgrade -y
```

### Kullanıcı ve Klasör Oluşturma
```bash
sudo useradd --no-create-home --shell /bin/false prometheus
sudo mkdir /etc/prometheus
sudo mkdir /var/lib/prometheus
sudo chown prometheus:prometheus /etc/prometheus /var/lib/prometheus
```

### Prometheus İndirme ve Kurulum
```bash
wget https://github.com/prometheus/prometheus/releases/download/v2.50.1/prometheus-2.50.1.linux-amd64.tar.gz
tar xvf prometheus-2.50.1.linux-amd64.tar.gz
```

### Dosyaları Doğru Konuma Taşıma
```bash
sudo mv prometheus-2.50.1.linux-amd64/prometheus /usr/local/bin/
sudo mv prometheus-2.50.1.linux-amd64/promtool /usr/local/bin/
sudo mv prometheus-2.50.1.linux-amd64/consoles /etc/prometheus
sudo mv prometheus-2.50.1.linux-amd64/console_libraries /etc/prometheus
```

### Dosya İzinleri
```bash
sudo chown prometheus:prometheus /usr/local/bin/prometheus
sudo chown prometheus:prometheus /usr/local/bin/promtool
sudo chown -R prometheus:prometheus /etc/prometheus/consoles
sudo chown -R prometheus:prometheus /etc/prometheus/console_libraries
```

### Yapılandırma Dosyası Oluşturma
```bash
sudo nano /etc/prometheus/prometheus.yml
```

**İçerik:**
```yaml
global:
  scrape_interval: 15s
  evaluation_interval: 15s

scrape_configs:
  - job_name: 'prometheus'
    static_configs:
      - targets: ['localhost:9090']
```

### Systemd Servis Dosyası Oluşturma
```bash
sudo nano /etc/systemd/system/prometheus.service
```

**İçerik:**
```ini
[Unit]
Description=Prometheus
Wants=network-online.target
After=network-online.target

[Service]
User=prometheus
Group=prometheus
Type=simple
ExecStart=/usr/local/bin/prometheus \
    --config.file=/etc/prometheus/prometheus.yml \
    --storage.tsdb.path=/var/lib/prometheus/ \
    --web.console.templates=/etc/prometheus/consoles \
    --web.console.libraries=/etc/prometheus/console_libraries

[Install]
WantedBy=multi-user.target
```

### Servisi Başlatma
```bash
sudo systemctl daemon-reload
sudo systemctl enable prometheus
sudo systemctl start prometheus
```

**Arayüze Erişim:** `http://localhost:9090`

---

## Dış Kaynaklardan Metrik Çekme

### Yapılandırma Dosyasını Düzenleme
```bash
sudo nano /etc/prometheus/prometheus.yml
```

### Yeni Scrape Job Ekleme
```yaml
scrape_configs:
  - job_name: 'prometheus'
    static_configs:
      - targets: ['localhost:9090']

  - job_name: 'arkadasimin_uygulamasi'
    static_configs:
      - targets: ['<IP_ADRESI>:<PORT>']
```

### Servisi Yeniden Başlatma
```bash
sudo systemctl restart prometheus
```

---

## Alert (Alarm) Oluşturma

### Kural Dosyası Oluşturma
```bash
sudo nano /etc/prometheus/rules.yml
```

**İçerik:**
```yaml
groups:
- name: mppt_alerts
  rules:
  - alert: YuksekPanelGucu
    expr: mppt_values{sensor="panel gucu"} > 1000
    for: 1m
    labels:
      severity: warning
      city: Adana
    annotations:
      summary: "Adana'daki panel gücü çok yüksek"
      description: "Adana'nın Ceyhan ilçesindeki Inonu Mahallesi'nde panel gücü {{ $value }} değerine ulaştı ve 1000'in üzerine çıktı."
```

### Prometheus Config'e Rule File Ekleme
```bash
sudo nano /etc/prometheus/prometheus.yml
```

**Eklenecek kısım:**
```yaml
rule_files:
  - "/etc/prometheus/rules.yml"
```

### Prometheus'u Yeniden Başlatma
```bash
sudo systemctl restart prometheus
sudo journalctl -u prometheus
```

**Alert Kontrolü:** `http://localhost:9090/alerts`

---

## Proje Özeti

### Yapılan İşlemler

1. **Prometheus Server Kurulumu**
   - Bilgisayara Prometheus server kuruldu

2. **Metrik Toplama**
   - Yunus'un hazırladığı metric formatındaki veriler Prometheus'a çekildi
   - Prometheus config dosyasına yeni bir job eklendi
   - Endpoint'in IP adresi ve port bilgileri yapılandırıldı
   - Arayüzde veriler isimlere göre filtrelendi ve grafik halinde izlendi

3. **Alert Oluşturma**
   - `YuksekPanelGucu` adında 1000 eşik değerini geçince uyaran bir alarm oluşturuldu
   - Rule YAML dosyası oluşturuldu
   - Prometheus config dosyasında rule dosyasının yeri tanımlandı
   - Arayüzde alarmın aktif olduğu gözlemlendi

4. **Araştırma**
   - Zabbix ve Prometheus farkları
   - Birbirlerine göre avantaj ve dezavantajları
   - Hız, esneklik, anlaşılırlık ve görselleştirme açısından karşılaştırma
