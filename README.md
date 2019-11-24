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
```
route add -net 192.168.0.128 netmask 255.255.255.128 gw 192.168.0.2 #A1
route add -net 10.151.73.24 netmask 255.255.255.248 gw 192.168.0.2 #DMZ
route add -net 192.168.0.8 netmask 255.255.255.252 gw 192.168.0.6 #A4
route add -net 192.168.1.0 netmask 255.255.255.0 gw 192.168.0.6 #A5
route add -net 192.168.0.16 netmask 255.255.255.248 gw 192.168.0.6 #A6
```

#### Blastoise
```
route add -net 0.0.0.0 netmask 0.0.0.0 gw 192.168.0.1
```

#### Venusaur
```
route add -net 0.0.0.0 netmask 0.0.0.0 gw 192.168.0.5
route add -net 192.168.0.16 netmask 255.255.255.248 gw 192.168.0.10 #A6
```

#### Arceus
```
route add -net 0.0.0.0 netmask 0.0.0.0 gw 192.168.0.9
#route add -net 0.0.0.0 netmask 0.0.0.0 gw 192.168.0.5
```

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

2. Oleh karena itu maka kalian diminta untuk mendrop semua akses SSH dari luar Topologi (UML) Kalian pada server yang memiliki ip DMZ (DHCP dan DNS SERVER) pada PIKACHU demi menjaga keamanan.
### Jawab:
Membuat script ``no2.sh`` pada UML PIKACHU, yang berisikan sebagai berikut:
``iptables -A FORWARD -p tcp --dport 22 -d 10.151.73.24/29 -i eth0 -j DROP``

3. Karena tim kalian maksimal terdiri dari 2 atau 3 orang saja, Satoshi meminta kalian untuk hanya membatasi DHCP dan DNS server hanya boleh menerima maksimal 2 atau 3(jumlah kelompok) koneksi ICMP secara bersamaan yang berasal dari mana saja menggunakan iptables pada masing masing server, selebihnya akan di DROP.
### Jawab:
Membuat script ``no3.sh`` pada UML ARTICUNO dan MEW, seperti di bawah ini:
``iptables -A INPUT -p icmp -m connlimit --connlimit-above 3 --connlimit-mask 0 -j DROP``

4. iptables -A INPUT -p icmp -m connlimit --connlimit-above 3 --connlimit-mask 0 -j DROP
### Jawab:
Membuat script ``no4.sh`` pada UML PIKACHU, dengan isi seperti ini:
``iptables -t nat -A PREROUTING -d 10.151.73.27 -s 10.151.36.0/24 -p tcp --dport 1234 -j DNAT --to-destination 192.168.0.19:1234 ``

5. Sedangkan ketika diakses dari subnet INFORMATIKA akan diarahkan pada MOLTRES dengan port 1234.
### Jawab:
Membuat script ``no5.sh`` pada UML PIKACHU, dengan isi seperti ini:
``iptables -t nat -A PREROUTING -d 10.151.73.27 -s 10.151.252.0/22 -p tcp --dport 1234 -j DNAT --to-destination 192.168.0.18:1234``

6. Akses dari subnet AJK hanya diperbolehkan pada pukul 08.00 - 17.00 pada hari Senin sampai Jumat
### Jawab:
Membuat file script ``no6.sh`` pada UML MEW:
```
iptables -A INPUT -s 10.151.36.0/24 -m time --timestart 08:00 --timestop 17:00 --weekdays Mon,Tue,Wed,Thu,Fri -j ACCEPT
iptables -A INPUT -s 10.151.36.0/24 -m time --timestart 17:01 --timestop 07:59 -j REJECT
```

7. Akses dari subnet INFORMATIKA hanya diperbolehkan pada pukul 17.00 hingga pukul 09.00 setiap harinya. Selain itu paket akan di REJECT
### Jawab:
Membuat file script ``no6.sh`` pada UML MEW:
```
iptables -A INPUT -s 10.151.252.0/22 -m time --timestart 09:01 --timestop 16:59 -j REJECT
```

9. Karena kita memiliki 2 buah WEB Server, Satoshi ingin PIKACHU disetting sehingga setiap request dari client yang mengakses DNS Server akan didistribusikan secara bergantian pada MEWTWO port 80 dan MOLTRES port 80.
### Jawab:
Belum kami kerjakan.

10. Karena banyak paket yang di drop oleh tim kalian, Satoshi ingin agar semua paket didrop oleh firewall (dalam topologi) tercatat dalam log pada setiap UML yang memiliki aturan drop.
### Jawab:
Belum kami kerjakan.


Sekian dari lapres kami, terima kasih dan mohon maaf apabila ada kesalahan.
