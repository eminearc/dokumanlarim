## R1 Router Konfigürasyonu

### Temel Ayarlar
```cisco
hostname R1
ip domain-name example.local

interface FastEthernet0/0
 ip address 10.x.x.x 255.255.255.0
 no shutdown

interface GigabitEthernet2/0
 ip address 192.168.1.1 255.255.255.0
 no shutdown
```

### SSH ve Kullanıcı Ayarları
```cisco
crypto key generate rsa
! Modulus sorusuna: 2048

username admin privilege 15 secret 1234
ip ssh version 2

line vty 0 4
 transport input ssh
 login local
```

### SNMP Ayarları
```cisco
snmp-server community public RO
snmp-server community private RW
exit
write memory
```

### Bağlantı Testleri

Ping testi:
```bash
ping 10.x.x.x
```

SNMP testi:
```bash
snmpwalk -v2c -c public 10.x.x.x
```

SSH bağlantısı (eski algoritma desteği ile):
```bash
ssh \
  -oKexAlgorithms=+diffie-hellman-group14-sha1 \
  -oHostKeyAlgorithms=+ssh-rsa \
  -oCiphers=+aes128-cbc \
  admin@10.x.x.x
```

---

## Huawei Router Konfigürasyonu

### IP ve VLAN Ayarları
```bash
configure vlan default add ports 1 untagged
configure vlan default ipaddress 192.168.1.2 255.255.255.0
enable ipforwarding vlan default
enable port 1
```

### Statik Route
```bash
ip route static 0.0.0.0 0.0.0.0 192.168.1.1
```

### SSH Konfigürasyonu
```bash
enable ssh2
create account admin
# Şifre: 1234 (iki kez gir)
configure account admin access all
```

### SNMP Konfigürasyonu
```bash
configure snmp add community public view DefaultView
save configuration
```

## Juniper Router Konfigürasyonu

### İlk Adımlar
```bash
root@% cli
root> configure 
Entering configuration mode
```

### IP Konfigürasyonu
```bash
[edit]
root# set interfaces em0 unit 0 family inet address 10.x.x.x/24 
[edit]
root# set routing-options static route 0.0.0.0/0 next-hop 10.x.x.x 
[edit]
root# commit and-quit 
```

### Root Authentication Hatası

Eğer aşağıdaki hatayı alırsak:
```
[edit]
  'system'
    Missing mandatory statement: 'root-authentication'
error: commit failed: (missing statements)
```

Root şifresi ayarlayalım:
```bash
[edit]
root# set system root-authentication plain-text-password 
New password: Juniper0
Retype new password: Juniper0
[edit]
root# commit and-quit 
commit complete
Exiting configuration mode
```

### Interface Kontrolü
```bash
root> show interfaces terse 
```

Eğer çıktıda `ge-0/0/0` portu gözükmüyorsa:

**Çözüm:** GNS3'te Network ayarlarından Type kısmını **Intel Gigabit Ethernet (e1000)** olarak değiştir.

### Interface Yeniden Konfigürasyonu
```bash
configure
set interfaces em0 unit 0 family inet address 10.x.x.x/24
commit
quit 
```

Konfigürasyonu kontrol et:
```bash
show configuration interfaces em0
```

Ping testi:
```bash
ping 10.x.x.x
```

### SSH Yapılandırması
```bash
set system services ssh
set system login user emine class super-user
set system login user emine authentication plain-text-password
```

Şifre: `Junos0`
```bash
commit
```

Ana makineden SSH testi:
```bash
ssh emine@10.x.x.x
```

### SNMP Yapılandırması
```bash
configure
set snmp community EmineNet authorization read-only
commit
exit
```

Ana makineden SNMP testi:
```bash
snmpwalk -v2c -c EmineNet 10.x.x.x
```
