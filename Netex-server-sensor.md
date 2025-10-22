## NETEX SERVER KURMA ADIMLARI
not: zabbixin LTS sürümünün kurulu olduğu varsayılmıştır. Zabbix kurulumu için: [zabbix kurulumu](https://www.zabbix.com/download?zabbix=7.0&os_distribution=ubuntu&os_version=24.04&components=server_frontend_agent&db=pgsql&ws=apache)

**_Postgresql kurulumu:_**

```
sudo apt update
sudo apt install postgresql postgresql-contrib

```

veritabanı sunucusuna bağlanma:

```
sudo -u postgres psql
```

Kullanıcı ve veritabanımızı oluşturalım 

```
CREATE USER netex WITH PASSWORD '1';
CREATE DATABASE netex WITH OWNER netex;

```

Daha sonrasında `\q` ile çıkış yapabiliriz


**_NETEX server kurulumu:_**

kurulumu size verilen deb dosyası ile yapabilirsiniz.

```
sudo apt install ./netex-sensor-455-x64.deb -y
```
Sonra düzenleme yapabilmek için `sudo su `ile root kullanıcısına geçiyoruz, oradan 
`cd /opt/netex` 
komutu ile netex klasörüne geçip `env.example` içindeki içeriği `.env` içerisine kopyalıyoruz.
 

```
cp env.example .env
```



`nano .env` ile .env dosyasını açıp içini düzenliyoruz:

- 
`storage configuration` kısmında farklı isimlerde DB ve USER oluşturulmadıysa değişiklik yapnmaya gerek yoktur. 

- Zabbix ayarlarını yaparken zabbix kurarken kullandığınız kullanıcı adı ve passwordu giriyoruz. URL kısmındaki ip adresi kısmına da zabbixin kurulu olduğu serverın ipsini giriyoruz.

- JWT KEY ‘i bulmak için  Liman'ın  kurulu olduğu makineye gidip 

```
 sudo su
cd /liman
cd server
cat .env 

```
diyoruz ve çıkan .env dosyasının içeriğinde en altta JWT_SECRET’ı buluyoruz. Sonrasında .env dosyasında JWT kısmına  yapıştırıyoruz.

- 
.env dosyasının aşağıdaki kısımlarını yukarıdaki bilgilere göre düzenliyoruz.

```
# Storage Configuration @required
DB_HOST=127.0.0.1
DB_NAME=netex
DB_PASS=netex
DB_PORT=5432
DB_USER=netex

ZABBIX_USERNAME="<zabbix_username>"
ZABBIX_PASSWORD="<zabbix_password>"
ZABBIX_URL="<http://zabbix_ip_adresi/zabbix/api_jsonrpc.php>"
ZABBIX_SYNC="ON"

# Liman JWT Secret for http admin service usage @not-required
JWT_SECRET= <yukarıda nasıl bulunacağı belirtildi.> 

```


Daha sonrasında bu iki komutla restart edip netex server'daki tüm   servisleri kontrol edelim:

```

sudo systemctl restart netex@*
sudo systemctl status netex@*

```

![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/n3mbpgroo5eahyiitpge.png)


![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/2bpzl7mhgm6qtjlj0pa1.png)


![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/on5pdoqw26v7wmtpy2u8.png)


![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/anme5pztrw1upqumshn5.png)



## NETEX SENSÖR KURMA ADIMLARI

netex kurulu makinede şunları yapalım:

```
sudo su
cd /opt/netex/clients
ls 

```
burada sensör için gerekli kurulum dosyaları sıralanır. Ubuntu cihaza deb dosyası şu şekilde kurulur:

`apt install ./netex-sensor-455-x64.deb -y `


`opt/netex-sensor/.env` dosyasının içeriğini düzenleyelim.

```
SERVER_URL="https://<netex_server_ip>:7782"
SENSOR_IP="<netex_server_ip>"
DNS_SERVER_URL="<DNS_server_ip>:53"
DEBUG_MODE="OFF"
PORT_MIRRORING_INTERFACE="ens18"

SOURCE_IP_KEY=8
DEST_IP_KEY=12
SOURCE_MAC_KEY=56
DEST_MAC_KEY=57

```

Not: Lisans olmadığında netex-sensör çalışmayacaktır.

** NETEX’İ LİMAN ARAYÜZÜNE EKLEME**

1. Limanı kurduğumuz ip adresindeki Liman arayüzüne gidelim.

2. Öncelikle sunucular ekranına gelelim ve sunucu ekle diyelim.

3. Bu kısımda portu kendimiz ekliyoruz bu portu bulmak için terminalde netex'i restart ettikten sonra `journalctl -u netex@* -f `çalıştırdığımda şöyle bir çıktı alıyoruz ve orada port numarası yazıyor. Onu resimdeki belirtilen alana giriyoruz.


![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/sw34tjt7lpimqx86chof.png)
bu durımda bu port 7781


![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/zpl60ase0aabf0nsv84u.png)



4. Anahatar Türü kısmında anahtarsız giriş seçelim.

5. Netex kurulu sunucumuzu ekleyelim ve sol panelden sunucumuza tıklayalım . eklentiler kısmına basalım. buradan da eklenti ekle diyelim.

6. Ekle diyerek ağ keşif seçelim ve böylelikle eklentiyi eklemiş olduk.

7. Arayüzde sol panelden eklentiye tıkladığımızda lisanslanmamış olsa da anasayfa açılır:


![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/14iumjznvwf2bax0d0nl.png)
