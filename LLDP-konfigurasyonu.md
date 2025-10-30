## LLDP Konfigürasyonu

Her VM'e LLDP agent kurulumu ve aktivasyonu:
```bash
sudo apt update
sudo apt install lldpd
sudo systemctl enable lldpd --now
```

Servis durumunu kontrol etmek için:
```bash
sudo systemctl status lldpd
```
<img width="904" height="567" alt="image" src="https://github.com/user-attachments/assets/51b22ca7-009d-488e-a229-4de50820b644" />

şimdi de komşuları detaylı görelim:
```
lldpcli show neighbors
```
<img width="719" height="821" alt="image" src="https://github.com/user-attachments/assets/f2be4cda-943c-4124-840a-226e6da68b77" />

