# Lapres_Modul5_JA02
Laporan Praktikum Soal Shift Jarkom oleh Kelompok A02.

A. Satoshi memberikan rancangan topologi jaringan usahanya sebagai berikut:
<img src="pictures/Jaringan Satoshi.png" width="800">

B. Dalam soal ini, kami menggunakan teknik VLSM untuk membuat topologi:


Berikut ini adalah file topologi.sh yang dibuat supaya UML dapat berjalan:
```
# Switch
uml_switch -unix switch0 > /dev/null < /dev/null &
uml_switch -unix switch1 > /dev/null < /dev/null &
uml_switch -unix switch2 > /dev/null < /dev/null &
uml_switch -unix switch3 > /dev/null < /dev/null &
uml_switch -unix switch4 > /dev/null < /dev/null &
uml_switch -unix switch5 > /dev/null < /dev/null &
uml_switch -unix switch6 > /dev/null < /dev/null &

# Router
xterm -T PIKACHU -e linux ubd0=PIKACHU,jarkom umid=PIKACHU eth0=tuntap,,,10.151.72.13 eth1=daemon,,,switch5 eth2=daemon,,,switch4 mem=64M &
xterm -T VENUSAUR -e linux ubd0=VENUSAUR,jarkom umid=VENUSAUR eth0=daemon,,,switch5 eth1=daemon,,,switch6 eth2=daemon,,,switch3 mem=64M &
xterm -T BLASTOISE -e linux ubd0=BLASTOISE,jarkom umid=BLASTOISE eth0=daemon,,,switch4 eth1=daemon,,,switch1 eth2=daemon,,,switch0 mem=64M &
xterm -T ARCEUS -e linux ubd0=ARCEUS,jarkom umid=ARCEUS eth0=daemon,,,switch6 eth1=daemon,,,switch2 mem=64M &

# DNS + Web Server
xterm -T ARTICUNO -e linux ubd0=ARTICUNO,jarkom umid=ARTICUNO eth0=daemon,,,switch0 mem=128M &
xterm -T MEW -e linux ubd0=MEW,jarkom umid=MEW eth0=daemon,,,switch0 mem=128M &
xterm -T MEWTWO -e linux ubd0=MEWTWO,jarkom umid=MEWTWO eth0=daemon,,,switch2 mem=128M &
xterm -T MOLTRES -e linux ubd0=MOLTRES,jarkom umid=MOLTRES eth0=daemon,,,switch2 mem=128M &

# Klien
xterm -T SNORLAX -e linux ubd0=SNORLAX,jarkom umid=SNORLAX eth0=daemon,,,switch1 mem=64M &
xterm -T PSYDUCK -e linux ubd0=PSYDUCK,jarkom umid=PSYDUCK eth0=daemon,,,switch3 mem=64M &
```
<img src="pictures/topologi.png" width="1000">

C. bash ``topologi.sh`` pada PuTTY
D. Login ke semua uml yang ada, lalu
E. Buka ``sysctl.conf`` pada setiap router dengan mengetik ``nano /etc/sysctl.conf``, lalu hilangkan tanda pagar (#) bagian net.ipv4.ip_forward=1 .
F. kemudian, jalankan perintah ``sysctl -p`` pada setiap router.
G. Setting IP dari setiap UML dengan dengan perintah ``nano /etc/network/interfaces``.
#### Pikachu
```auto eth0
iface eth0 inet static
address 10.151.72.14
netmask 255.255.255.252
gateway 10.151.72.13

auto eth1
iface eth1 inet static
address 192.168.0.5
netmask 255.255.255.252

auto eth2
iface eth2 inet static
address 192.168.0.1
netmask 255.255.255.252
```

#### Blastoise
```
auto eth0
iface eth0 inet static
address 192.168.0.2
netmask 255.255.255.252
gateway 192.168.0.1

auto eth1
iface eth1 inet static
address 192.168.0.129
netmask 255.255.255.128

auto eth2
iface eth2 inet static
address 10.151.73.25
netmask 255.255.255.248
```

#### Snorlax
```auto eth0
iface eth0 inet static
address 192.168.0.130
netmask 255.255.255.128
gateway 192.168.0.129
```

#### Articuno
```
auto eth0
iface eth0 inet static
address 10.151.73.26
netmask 255.255.255.248
gateway 10.151.73.25
```

#### Mew
```
auto eth0
iface eth0 inet static
address 10.151.73.27
netmask 255.255.255.248
gateway 10.151.73.25
```

#### Venusaur
```
auto eth0
iface eth0 inet static
address 192.168.0.6
netmask 255.255.255.252
gateway 192.168.0.5

auto eth1
iface eth1 inet static
address 192.168.0.9
netmask 255.255.255.252

auto eth2
iface eth2 inet static
address 192.168.1.0
netmask 255.255.255.0
```

#### Psyduck
```
auto eth0
iface eth0 inet static
address 192.168.1.2
netmask 255.255.255.0
gateway 192.168.1.1
```

#### Arceus
```
auto eth0
iface eth0 inet static
address 192.168.0.10
netmask 255.255.255.252
gateway 192.168.0.9

auto eth1
iface eth1 inet static
address 192.168.0.17
netmask 255.255.255.248
```

#### Moltres
```
auto eth0
iface eth0 inet static
address 192.168.0.18
netmask 255.255.255.248
gateway 192.168.0.17
```

#### Mewtwo
```
auto eth0
iface eth0 inet static
address 192.168.0.19
netmask 255.255.255.248
gateway 192.168.0.17
```

H. Ketikkan ``service networking restart`` setelah mengetik config network seperti di atas.
I. Routing semua router yang ada, dengan cara membuat file bernama ``routing.sh`` di setiap router. Berikut adalah isi dari file-nya:
#### Pikachu 
route add -net 192.168.0.128 netmask 255.255.255.128 gw 192.168.0.2 #A1
route add -net 10.151.73.24 netmask 255.255.255.248 gw 192.168.0.2 #DMZ
route add -net 192.168.0.8 netmask 255.255.255.252 gw 192.168.0.6 #A4
route add -net 192.168.1.0 netmask 255.255.255.0 gw 192.168.0.6 #A5
route add -net 192.168.0.16 netmask 255.255.255.248 gw 192.168.0.6 #A6

#### Blastoise
route add -net 0.0.0.0 netmask 0.0.0.0 gw 192.168.0.1

#### Venusaur
route add -net 0.0.0.0 netmask 0.0.0.0 gw 192.168.0.5
route add -net 192.168.0.16 netmask 255.255.255.248 gw 192.168.0.10 #A6

#### Arceus
route add -net 0.0.0.0 netmask 0.0.0.0 gw 192.168.0.9
#route add -net 0.0.0.0 netmask 0.0.0.0 gw 192.168.0.5

J. Begitu selesai, jangan lupa bash ``routing.sh`` pada setiap router yang ada.
K. Export proxy pada setiap UML seperti berikut ini:
```export http_proxy="http://ITS-564462-56d06:82f78@proxy.its.ac.id:8080"
export https_proxy="http://ITS-564462-56d06:82f78@proxy.its.ac.id:8080"
export ftp_proxy="http://ITS-564462-56d06:82f78@proxy.its.ac.id:8080"
```

## Soal
1. Agar topologi yang kalian buat dapat mengakses keluar, kalian diminta untuk mengkonfigurasi PIKACHU menggunakan iptables, namun Satoshi melarang kalian menggunakan MASQUERADE karena terlalu mudah.Karena keberadaan jaringan tersebut sudah mulai diketahui dari oleh jaringan luar, Satoshi pun merasa panik, karena merasa jaringannya masih belum aman.
#### Jawab:
Membuat script ``no1.sh`` pada UML PIKACHU, yang berisikan sebagai berikut:
``iptables -t nat -A POSTROUTING -s 192.168.0.0/16 -o eth0 -j SNAT --to-source 10.151.72.14``

2. 
