Nelerin Görülebileceğini Tanımladık (MIB View)
Önce, bu SNMP kullanıcısının router'da neleri görmeye izni olduğunu tanımlayan v3view adında bir "görünüm" oluşturduk. iso kelimesini kullanarak "her şeyi görme" izni verdik.
```
[Huawei] snmp-agent mib-view v3view include iso

```
[Huawei] snmp-agent group v3 v3group privacy read-view v3view
```
komut satırındayken şu komutu çalıştırarak önce grubu oluşturun:
```
[Huawei] snmp-agent group v3 v3group privacy read-view v3view
```
Kullanıcıyı gruba bağlayın. (Bu komut, v3user kullanıcısını v3group grubuna üye yapar.)
```
[Huawei] snmp-agent usm-user v3 v3user group v3group
```
Kullanıcı için "Authentication" (Kimlik Doğrulama) ayarını yapın. (Bu, v3user kullanıcısının kimlik doğrulama şifresini ve yöntemini belirler.)
```
snmp-agent usm-user v3 v3user authentication-mode sha
```
şifre gir:AuthSifre123!
```
[Huawei]snmp-agent usm-user v3 v3user privacy-mode aes128 
Please configure the privacy password (<8-64>)
Enter Password: PrivSifre456!
Confirm password: PrivSifre456!
```
bunları yaptıktan sonra host makinemde snmp walk çalıştı
```
snmpwalk -v 3 -l authPriv -u v3user -a SHA -A "AuthSifre123!" -x AES -X "PrivSifre456!" 10.67.67.100
```
