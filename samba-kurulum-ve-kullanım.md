# SAMBA KURULUM
```
apt update && sudo apt upgrade -y
apt install samba -y
samba --version
```

1. Paylaşılacak klasörü oluştur

```
sudo mkdir -p /srv/samba/paylasim
sudo chown nobody:nogroup /srv/samba/paylasim
sudo chmod 0777 /srv/samba/paylasim
```
2. Samba yapılandırmasına paylaşımı ekle
```
sudo nano /etc/samba/smb.conf
```
Dosyanın en altına şunu ekle:
```
[Paylasim]
   path = /srv/samba/paylasim
   browseable = yes
   read only = no
   guest ok = yes
```
3. Samba servisini yeniden başlat
```
sudo systemctl restart smbd
```
4. SMB kullanıcı tanımlamak için
```
sudo smbpasswd -a ubuntu
```


