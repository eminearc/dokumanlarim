<img width="937" height="729" alt="image" src="https://github.com/user-attachments/assets/4f87b74a-b63b-40ed-8dac-3889364162fb" />
## CDP Yapılandırması

### Ankara Router

#### Tüm Interface'lerde CDP Aktifleştirme
```cisco
R1-ankara(config)#interface GigabitEthernet2/0
R1-ankara(config-if)#cdp enable
R1-ankara(config-if)#exit

R1-ankara(config)#interface GigabitEthernet1/0
R1-ankara(config-if)#cdp enable
R1-ankara(config-if)#exit

R1-ankara(config)#interface FastEthernet0/0
R1-ankara(config-if)#cdp enable
R1-ankara(config-if)#exit
R1-ankara(config)#exit
```

#### Portları Up Duruma Getirme
```cisco
R1-ankara(config)#interface GigabitEthernet1/0
R1-ankara(config-if)#no shutdown
R1-ankara(config-if)#exit

R1-ankara(config)#interface GigabitEthernet2/0
R1-ankara(config-if)#no shutdown
R1-ankara(config-if)#exit

R1-ankara(config)#interface GigabitEthernet3/0
R1-ankara(config-if)#no shutdown
R1-ankara(config-if)#exit

R1-ankara(config)#exit
R1-ankara#write memory
```

#### CDP Durumu Kontrolü
```cisco
R1-ankara#show cdp interface
```
<img width="904" height="437" alt="image" src="https://github.com/user-attachments/assets/a3aa1f2e-b92c-41c5-8aaf-868076e8c672" />

```
R1-ankara#show cdp neighbors

```
<img width="893" height="147" alt="image" src="https://github.com/user-attachments/assets/6676b7b2-a705-41fa-892f-09131d2ed4b4" />
Ankara router başta hiçbir routerı gormez normaldir. Çünkü daha diğerlerine tanıtmadık bu routerı.


### Çankaya Router
```cisco
cankaya#enable
cankaya#configure terminal
cankaya(config)#cdp run

cankaya(config)#interface GigabitEthernet1/0
cankaya(config-if)#no shutdown
cankaya(config-if)#cdp enable
cankaya(config-if)#exit

cankaya(config)#interface FastEthernet0/0
cankaya(config-if)#no shutdown
cankaya(config-if)#cdp enable
cankaya(config-if)#exit

cankaya(config)#end
cankaya#write memory
```

CDP komşu kontrolü:
```cisco
cankaya#show cdp neighbors
```
<img width="907" height="226" alt="image" src="https://github.com/user-attachments/assets/2ca086c6-d39b-404c-ad72-2a728c817faf" />

Çıktıda Ankara router komşu olarak görünür.

---

### Aksaray Router
```cisco
aksaray#enable
aksaray#conf t
aksaray(config)#cdp run

aksaray(config)#interface f0/0
aksaray(config-if)#no shutdown
aksaray(config-if)#cdp enable
aksaray(config-if)#exit

aksaray(config)#end
aksaray#write memory
```

CDP komşu kontrolü:
```cisco
aksaray#show cdp neighbors
```

**Çıktı:**
```
Capability Codes: R - Router, T - Trans Bridge, B - Source Route Bridge
                  S - Switch, H - Host, I - IGMP, r - Repeater, P - Phone, 
                  D - Remote, C - CVTA, M - Two-port Mac Relay 

Device ID        Local Intrfce     Holdtme    Capability  Platform  Port ID
R1-ankara.example.local
                 Fas 0/0           131              R B   7206VXR   Gig 2/0
```

---

### Ankara Router - Final Durum
```cisco
R1-ankara#show cdp neighbors
```
Bu komut ile tüm bağlı routerlar (Çankaya, Aksaray) görülebilir.

<img width="904" height="135" alt="image" src="https://github.com/user-attachments/assets/95226975-de8a-41c4-8200-63d8417eaf5e" />

## IP Konfigürasyonları

### Ankara Router Yapılandırması

#### Management Interface (G3/0)
```cisco
R1-ankara#conf t
R1-ankara(config)#interface g3/0
R1-ankara(config-if)#no ip address
R1-ankara(config-if)#ip address 10.x.x.x 255.255.255.0 
R1-ankara(config-if)#no shutdown 
R1-ankara(config-if)#end
R1-ankara#write memory
```

#### Ankara-Çankaya Bağlantısı (F0/0)
```cisco
R1-ankara#conf t
R1-ankara(config)#interface f0/0
R1-ankara(config-if)#ip address 192.168.12.1 255.255.255.0
R1-ankara(config-if)#no shutdown
R1-ankara(config-if)#end
R1-ankara#write memory
```

#### Ankara-Yenimahalle Bağlantısı (G1/0)
```cisco
R1-ankara#conf t
R1-ankara(config)#interface g1/0
R1-ankara(config-if)#ip address 192.168.13.1 255.255.255.0
R1-ankara(config-if)#no shutdown
R1-ankara(config-if)#end
R1-ankara#write memory
```

#### Ankara-Aksaray Bağlantısı (G2/0)
```cisco
R1-ankara#conf t
R1-ankara(config)#interface g2/0
R1-ankara(config-if)#ip address 192.168.14.1 255.255.255.0
R1-ankara(config-if)#no shutdown
R1-ankara(config-if)#end
R1-ankara#write memory
```

#### OSPF Yapılandırması
```cisco
R1-ankara(config)#router ospf 1
R1-ankara(config-router)#router-id 1.1.1.1
R1-ankara(config-router)#network 10.x.x.0 0.0.0.255 area 0
R1-ankara(config-router)#network 192.168.12.0 0.0.0.255 area 0
R1-ankara(config-router)#network 192.168.13.0 0.0.0.255 area 0
R1-ankara(config-router)#network 192.168.14.0 0.0.0.255 area 0
R1-ankara(config-router)#ip route 0.0.0.0 0.0.0.0 10.x.x.y
R1-ankara(config)#end
R1-ankara#write memory
```

---

### Çankaya Router Yapılandırması
```cisco
cankaya#conf t
cankaya(config)#interface FastEthernet0/0
cankaya(config-if)#ip address 192.168.12.2 255.255.255.0
cankaya(config-if)#no shutdown
cankaya(config-if)#exit

cankaya(config)#interface GigabitEthernet1/0
cankaya(config-if)#ip address 192.168.16.1 255.255.255.0
cankaya(config-if)#no shutdown
cankaya(config-if)#exit

cankaya(config)#router ospf 1
cankaya(config-router)#router-id 2.2.2.2
cankaya(config-router)#network 192.168.12.0 0.0.0.255 area 0
cankaya(config-router)#network 192.168.16.0 0.0.0.255 area 0
cankaya(config-router)#ip route 0.0.0.0 0.0.0.0 192.168.12.1
cankaya(config)#end
```

**OSPF Komşuluk Mesajı:**
```
*Aug  6 06:05:14.575: %OSPF-5-ADJCHG: Process 1, Nbr 1.1.1.1 on FastEthernet0/0 from LOADING to FULL, Loading Done
```

---

### Yenimahalle Router Yapılandırması
```cisco
yenimahalle#conf t
yenimahalle(config)#interface FastEthernet0/0
yenimahalle(config-if)#ip address 192.168.13.2 255.255.255.0  
yenimahalle(config-if)#no shutdown 
yenimahalle(config-if)#exit

yenimahalle(config)#interface GigabitEthernet1/0
yenimahalle(config-if)#ip address 192.168.15.1 255.255.255.0
yenimahalle(config-if)#no shutdown 
yenimahalle(config-if)#exit

yenimahalle(config)#router ospf 1
yenimahalle(config-router)#router-id 3.3.3.3
yenimahalle(config-router)#network 192.168.13.0 0.0.0.255 area 0
```

---

### Aksaray Router Yapılandırması
```cisco
aksaray#conf t
aksaray(config)#interface FastEthernet0/0
aksaray(config-if)#ip address 192.168.14.2 255.255.255.0
aksaray(config-if)#no shutdown 
aksaray(config-if)#exit

aksaray(config)#router ospf 1
aksaray(config-router)#router-id 4.4.4.4
aksaray(config-router)#network 192.168.14.0 0.0.0.255 area 0
aksaray(config-router)#end
aksaray#write memory
```

**OSPF Komşuluk Mesajı:**
```
*Aug  6 07:51:12.239: %OSPF-5-ADJCHG: Process 1, Nbr 1.1.1.1 on FastEthernet0/0 from LOADING to FULL, Loading Done
```

#### Aksaray Router Testleri

OSPF komşuluk kontrolü:
```cisco
aksaray#show ip ospf neighbor

Neighbor ID     Pri   State           Dead Time   Address         Interface
1.1.1.1           1   FULL/DR         00:00:31    192.168.14.1    FastEthernet0/0
```

OSPF route kontrolü:
```cisco
aksaray#show ip route ospf

Gateway of last resort is not set

      10.0.0.0/24 is subnetted, 1 subnets
O        10.x.x.0 [110/2] via 192.168.14.1, 00:01:02, FastEthernet0/0
O     192.168.12.0/24 [110/2] via 192.168.14.1, 00:01:02, FastEthernet0/0
O     192.168.13.0/24 [110/2] via 192.168.14.1, 00:01:02, FastEthernet0/0
O     192.168.15.0/24 [110/3] via 192.168.14.1, 00:01:02, FastEthernet0/0
O     192.168.16.0/24 [110/3] via 192.168.14.1, 00:01:02, FastEthernet0/0
```



## Bonus: Routerlara SNMP Yapılandırma ve Zabbix'e Ekleme

### Çankaya Router SNMP Yapılandırması
```cisco
cankaya#conf t
cankaya(config)#snmp-server community cankaya-ro RO
cankaya(config)#snmp-server community cankaya-rw RW
cankaya(config)#exit
cankaya#write memory
```

**Kontrol:**
```cisco
cankaya#show running-config | section snmp-server
```

---

### Yenimahalle Router SNMP Yapılandırması
```cisco
yenimahalle#conf t
yenimahalle(config)#snmp-server community yenimahalle-ro RO
yenimahalle(config)#snmp-server community yenimahalle-rw RW
yenimahalle(config)#exit
yenimahalle#write memory
```

**Kontrol:**
```cisco
yenimahalle#show running-config | section snmp-server
```

---

### Aksaray Router SNMP Yapılandırması
```cisco
aksaray#conf t
aksaray(config)#snmp-server community aksaray-ro RO
aksaray(config)#snmp-server community aksaray-rw RW
aksaray(config)#exit
aksaray#write memory
```

**Kontrol:**
```cisco
aksaray#show running-config | section snmp-server
```
<img width="908" height="672" alt="image" src="https://github.com/user-attachments/assets/efa1e28d-c812-46d7-93f4-2804bb8b470d" />

DEMO:
Hostumuzdan uç bilgisyararlara ping atma deneylerimiz başarılı oldu. Bu da demek oluyor ki ospf ve ip yapılandırmalarımız başarılı olmuş.

<img width="844" height="544" alt="image" src="https://github.com/user-attachments/assets/a1ffae98-58ca-4c1a-a1df-640e5ed8de60" />

Ankara routerımızdan show cdp neighbors dediğimizde tüm komşuları görebiliyoruz. Bu da demek oluyor ki cdp ayarlarımız da düzgün çalışıyor.

<img width="914" height="136" alt="image" src="https://github.com/user-attachments/assets/a9244936-2310-4aac-acea-49916b787acf" />


Teşekkürler.
