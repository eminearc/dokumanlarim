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
