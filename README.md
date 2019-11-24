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

C. bash ``topologi.sh`` pada PuTTY
D. Login ke semua uml yang ada, lalu
E. Buka ``sysctl.conf`` pada setiap router dengan mengetik ``nano /etc/sysctl.conf``, lalu hilangkan tanda pagar (#) bagian net.ipv4.ip_forward=1 .
F. kemudian, jalankan perintah ``sysctl -p`` pada setiap router.
G. Setting IP dari setiap UML dengan dengan perintah ``nano /etc/network/interfaces``.
