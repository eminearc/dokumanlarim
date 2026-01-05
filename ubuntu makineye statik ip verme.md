# Ubuntu 24.04.LTS Makinede Statik ip yapılandırma
*  önce configürasyon dosyasına girelim 
sudo nano /etc/netplan/50-cloud-init.yaml

*  içeriği belirlediğimiz ip ve DNS ayarlarına göre güncelleyelim. Bağlı olduğumuz ağdaki DNS serverını bulmak için ``resolvectl status`` komutu kullanılabilir.
```
network:
  version: 2
  ethernets:
    ens18:
      dhcp4: no
      addresses:
        - 10.xxx.xxx.3/24
      routes:
        - to: 0.0.0.0/0
          via: 10.xxx.xxx.1
      nameservers:
        addresses: [10.xxx.0.5]
```

* son olarak değişiklikler uygulanır
  `` sudo netplan apply``
