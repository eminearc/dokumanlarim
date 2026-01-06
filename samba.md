# Samba Kurulumu
* I. adım
```
sudo su
apt update
apt install samba -y
```
*  II. adım

```sudo /etc/samba/smb.conf```
bu içeriği yapıştır
```
[SharedFolder]
path = /etc/samba/paylasim
browseable = yes
writable = yes
guest ok = no
read only = no
```
```
mkdir -p /etc/samba/paylasim
chmod -R 775 /etc/samba/paylasim
```
```
Systemctl restart smbd
systemctl enable smbd
```

```
sudo smbpasswd -a ubuntu
```

aynı ağdaki başka bir makineden bağlantı deneyi yapalım:

```
smbclient -L //10.67.67.133 -U ubuntu
```

```
Password for [WORKGROUP\ubuntu]:

	Sharename       Type      Comment
	---------       ----      -------
	print$          Disk      Printer Drivers
	SharedFolder    Disk      
	IPC$            IPC       IPC Service (ubuntu server (Samba, Ubuntu))
```
