# Jarkom-Modul-5-ITB08-2022

## Anggota:
| Nama                      | NRP        |
|---------------------------|------------|
| Salsabila Briliana A. S.  | 5027201003 |
| Muhammad Rifqi Fernanda   | 5027201050 |
| Gilang Bayu Gumantara     | 5027201062 | 

## Topologi 
---
![topologi5](https://user-images.githubusercontent.com/90242686/206859449-2ee7910e-77ad-4512-b8a7-491e474745f0.jpg)

## Subnetting
---
![subnetting5](https://user-images.githubusercontent.com/90242686/206859674-6bceb11d-99cf-4713-96f4-b335cd208e8f.jpg)

## Perhitungan Subnet

![perhitungansubnet](https://user-images.githubusercontent.com/90242686/206859716-c8662dc6-f043-4500-9b16-8e32e4610c99.jpg)

## Setting GNS3
---
**STRIX (sebagai Router / DHCP Relay)**
```
auto eth0
iface eth0 inet dhcp

auto eth1
iface eth1 inet static
        address 192.218.0.1
        netmask 255.255.255.252

auto eth2
iface eth2 inet static
         address 192.218.0.5
         netmask 255.255.255.252
```

**WESTALIS (sebagai Router / DHCP Relay)**
```
auto eth0
iface eth0 inet static
        address 192.181.0.2
        netmask 255.255.255.252
        gateway 192.218.0.1

auto eth1
iface eth1 inet static
        address 192.218.0.17
        netmask 255.255.255.248

auto eth2
iface eth2 inet static
         address 192.218.0.129
         netmask 255.255.255.128

auto eth3
iface eth3 inet static
         address 192.218.4.1
         netmask 255.255.252.0
```

**OSTANIA (sebagai Router / DHCP Relay)**
```
auto eth0
iface eth0 inet static
          address 192.218.0.6
          netmask 255.255.255.252
          gateway 192.218.0.5

auto eth1
iface eth1 inet static
          address 192.218.0.25
          netmask 255.255.255.248

auto eth2
iface eth2 inet static
          address 192.218.2.1
          netmask 255.255.254.0

auto eth3
iface eth3 inet static
          address 192.218.1.1
          netmask 255.255.255.0
```

**EDEN (sebagai DNS Server)**
```
auto eth0
iface eth0 inet static
       address 192.218.0.18
       netmask 255.255.255.248
       gateway 192.218.0.17
```

**WISE (sebagai DHCP Server)**
```
auto eth0
iface eth0 inet static
       address 192.218.0.19
       netmask 255.255.255.248
       gateway 192.218.0.17
```
 
**GARDEN (sebagai Web Server)**
```
auto eth0
iface eth0 inet static
       address 192.218.0.26
       netmask 255.255.255.248
       gateway 192.218.0.25
```

**SSS (sebagai Web Server)**
```
auto eth0
iface eth0 inet static
       address 192.218.0.27
       netmask 255.255.255.248
       gateway 192.218.0.25
``` 

**DESMOND (sebagai Client)**
```
auto eth0
iface eth0 inet dhcp
```

**FORGER (sebagai Client)**
```
auto eth0
iface eth0 inet dhcp
```

**BLACKBALL (sebagai Client)**
```
auto eth0
iface eth0 inet dhcp
```

**BRIAR (sebagai Client)**
```
auto eth0
iface eth0 inet dhcp
```

### Routing
```
route add -net 192.218.0.16 netmask 255.255.255.248 gw 192.218.0.2
route add -net 192.218.0.128 netmask 255.255.255.128 gw 192.218.0.2
route add -net 192.218.4.0 netmask 255.255.252.0 gw 192.218.0.2
route add -net 192.218.0.24 netmask 255.255.255.248 gw 192.218.0.6
route add -net 192.218.2.0 netmask 255.255.254.0 gw 192.218.0.6
route add -net 192.218.1.0 netmask 255.255.255.0 gw 192.218.0.6
```

### Setting DHCP Relay
Pada strix, ostania, westalis
- Install aplikasi isc-dhcp-relay
``` 
  apt-get install isc-dhcp-relay -y
```
- Edit file `/etc/default/isc-dhcp-relay`
- Restart isc-dhcp-relay
```
  service isc-dhcp-relay restart
``` 
  
### Setting DHCP Server
Pada Wise
- Install aplikasi isc-dhcp-server.
```
apt-get install isc-dhcp-server -y
```
- Edit file `/etc/default/isc-dhcp-server`
- Edit file `/etc/dhcp/dhcpd.conf` untuk menambahkan subnet sebagai berikut:
```
subnet 192.218.0.16 netmask 255.255.255.248 {
}
subnet 192.218.0.128 netmask 255.255.255.128 {
    range 192.218.0.130 192.218.0.254;
    option routers 192.218.0.129;
    option broadcast-address 192.218.0.255;
    option domain-name-servers 192.218.0.18;
    default-lease-time 600;
    max-lease-time 7200;
}
subnet 192.218.4.0 netmask 255.255.252.0 {
    range 192.218.4.2 192.218.4.254;
    option routers 192.218.4.1;
    option broadcast-address 192.218.4.255;
    option domain-name-servers 192.218.0.18;
    default-lease-time 600;
    max-lease-time 7200;
}
subnet 192.218.2.0 netmask 255.255.254.0 {
    range 192.218.2.2 192.218.2.254;
    option routers 192.218.2.1;
    option broadcast-address 192.218.2.255;
    option domain-name-servers 192.218.0.18;
    default-lease-time 600;
    max-lease-time 7200;
}
subnet 192.218.1.0 netmask 255.255.255.0 {
    range 192.218.1.2 192.218.1.254;
    option routers 192.218.1.1;
    option broadcast-address 192.218.1.255;
    option domain-name-servers 192.218.0.18;
    default-lease-time 600;
    max-lease-time 7200;
}
```
- Restart isc-dhcp-server.
```
service isc-dhcp-server restart
```

### Setting DNS Server
Pada Eden
- Install aplikasi bind9.
```
apt-get install bind9 -y
```
- Edit file `/etc/bind/named.conf.options`
- Restart bind9.
```
service bind9 restart
```

### Soal 1
Agar topologi yang kalian buat dapat mengakses keluar, kalian diminta untuk
mengkonfigurasi Strix menggunakan iptables, tetapi Loid tidak ingin menggunakan
MASQUERADE.

### Jawaban
*STRIX*
```
iptables -t nat -A POSTROUTING -s 192.218.0.0/16 -o eth0 -j SNAT --to-source (ip eth0)
```
- ip eth0 didapatkan dengan menjalankan command ip a pada Strix.
