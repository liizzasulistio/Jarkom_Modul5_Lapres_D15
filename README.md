# Jarkom_Modul5_Lapres_D15
- Ammar Alifian Fahdan (05111840000007)
- Lii'zza Aisyah Putri Sulistio (05111840000073)

---
## Keterangan

| UML  | Keterangan | IP Address | Memori |
| :-----: | :-------: | :-----: | :-----: |
| SURABAYA | IP Tuntap | 10.151.78.66 | 96M |
| MALANG | DNS Server | 10.151.79.130 | 128M |
| MOJOKERTO | DHCP Server | 10.151.79.131 | 128M |
| PROBOLINGGO | Web Server | sesuai perhitungan subnet | 128M |
| MADIUN | Web Server | sesuai perhitungan subnet | 128M |
| BATU | Client | sesuai perhitungan subnet | 96M |
| KEDIRI | Client | sesuai perhitungan subnet | 96M |
| SIDOARJO | Client | sesuai perhitungan subnet - dhcp | 96M |
| GRESIK | Client| sesuai perhitungan subnet - dhcp | 96M |

---
## Persiapan


1. Konfigurasi topologi.sh dan bye.sh sesuai dengan gambar topologi yang diberikan.
- Topologi
~~~
# Switch
uml_switch -unix switch0 > /dev/null < /dev/null &
uml_switch -unix switch1 > /dev/null < /dev/null &
uml_switch -unix switch2 > /dev/null < /dev/null &
uml_switch -unix switch3 > /dev/null < /dev/null &
uml_switch -unix switch4 > /dev/null < /dev/null &
uml_switch -unix switch5 > /dev/null < /dev/null &

# Router
xterm -T SURABAYA -e linux ubd0=SURABAYA,jarkom umid=SURABAYA eth0=tuntap,,,10.151.78.65 eth1=daemon,,,switch0 eth2=daemon,,,switch4 mem=96M &
xterm -T KEDIRI -e linux ubd0=KEDIRI,jarkom umid=KEDIRI eth0=daemon,,,switch0 eth1=daemon,,,switch1 eth2=daemon,,,switch3 mem=96M &
xterm -T BATU -e linux ubd0=BATU,jarkom umid=BATU eth0=daemon,,,switch4 eth1=daemon,,,switch2 eth2=daemon,,,switch5 mem=96M &

# Server
xterm -T MALANG -e linux ubd0=MALANG,jarkom umid=MALANG eth0=daemon,,,switch2 mem=128M &
xterm -T MOJOKERTO -e linux ubd0=MOJOKERTO,jarkom umid=MOJOKERTO eth0=daemon,,,switch2 mem=128M &
xterm -T MADIUN -e linux ubd0=MADIUN,jarkom umid=MADIUN eth0=daemon,,,switch1 mem=128M &
xterm -T PROBOLINGGO -e linux ubd0=PROBOLINGGO,jarkom umid=PROBOLINGGO eth0=daemon,,,switch1 mem=128M &

# Client
xterm -T SIDOARJO -e linux ubd0=SIDOARJO,jarkom umid=SIDOARJO eth0=daemon,,,switch5 mem=96M &
xterm -T GRESIK -e linux ubd0=GRESIK,jarkom umid=GRESIK eth0=daemon,,,switch3 mem=96M &
~~~

- Bye
~~~
uml_mconsole SURABAYA halt
uml_mconsole KEDIRI halt
uml_mconsole BATU halt
uml_mconsole MALANG halt
uml_mconsole MOJOKERTO halt
uml_mconsole MADIUN halt
uml_mconsole PROBOLINGGO halt
uml_mconsole SIDOARJO halt
uml_mconsole GRESIK halt
~~~


2. Membagi IP Address ke dalam beberapa subnet, agar lebih mudah maka metode yang digunakan adalah VLSM.

| Subnet  | Jumlah IP | Submask |
| :-----: | :-------: | :-----: |
| A01  | 201  | /23 |
| A02  | 2  | /30 |
| A03  | 2  | /30 |
| A04  | 211  | /23 |
| A05  | 3  | /29 |
| **Total**  | **419**  | **/22** |

3. Membuat pohon untuk membagi subnet.


4. Mendapatkan IP masing-masing subnet.

| Subnet  | IP Address | Netmask |
| :-----: | :-------: | :-----: |
| A01  | 192.168.1.1 - 192.168.1.200  | 255.255.255.0 |
| A02  | 192.168.0.0 | 255.255.255.252 |
| A03  | 192.168.0.4  | 255.255.255.252 |
| A04  | 192.168.2.2 - 192.168.2.11  | 255.255.255.0 |
| A05  | 192.168.0.8 | 255.255.255.248 |

5. Mengatur konfigurasi interface pada masing-masing UML, kemudian lakukan `service networking restart` pada Router dan Server.
- SURABAYA (*Router*)
~~~
auto eth0
iface eth0 inet static
address 10.151.78.66
netmask 255.255.255.252
gateway 10.151.78.65

auto eth1
iface eth1 inet static
address 192.168.0.5
netmask 255.255.255.252

auto eth2
iface eth2 inet static
address 192.168.6.1
netmask 255.255.255.252
~~~

- KEDIRI (*Router*)
~~~
auto eth0
iface eth0 inet static
address 192.168.0.6
netmask 255.255.255.252
gateway 192.168.0.5

auto eth1
iface eth1 inet static
address 192.168.0.9
netmask 255.255.255.248

auto eth2
iface eth2 inet static
address 192.168.2.1
netmask 255.255.255.0
~~~

- BATU (*Router*)
~~~
auto eth0
iface eth0 inet static
address 192.168.6.2
netmask 255.255.255.252
gateway 192.168.6.1

auto eth1
iface eth1 inet static
address 10.151.79.129
netmask 255.255.255.248

auto eth2
iface eth2 inet static
address 192.168.1.1
netmask 255.255.255.0
~~~

- MALANG (*DNS Server*)
~~~
auto eth0
iface eth0 inet static
address 10.151.79.130
netmask 255.255.255.248
gateway 10.151.79.129
~~~

- MOJOKERTO (*DHCP Server*)
~~~
auto eth0
iface eth0 inet static
address 10.151.79.131
netmask 255.255.255.248
gateway 10.151.79.129
~~~

- MADIUN (*Web Server*)
~~~
auto eth0
iface eth0 inet static
address 192.168.0.10
netmask 255.255.255.248
gateway 192.168.0.9
~~~

- PROBOLINGGO (*Web Server*)
~~~
auto eth0
iface eth0 inet static
address 192.168.0.11
netmask 255.255.255.248
gateway 192.168.0.9
~~~

- SIDOARJO (*Client*)
~~~
auto eth0
iface eth0 inet dhcp
~~~

- GRESIK (*Client*)
~~~
auto eth0
iface eth0 inet dhcp
~~~

6. Melakukan routing pada router SURABAYA, KEDIRI, dan BATU. Semua konfigurasi untuk routing disimpan dalam file `route.sh` agar ketika ingin mengakses UML kembali dapat dilakukan dengan pemanggilan `bash route.sh`, jika ingin melihat list routing yang ada dapat dilakukan dengan cara `route -n`.
- SURABAYA
~~~
route add -net 10.151.79.128 netmask 255.255.255.248 gw 192.168.6.2
route add -net 192.168.1.0 netmask 255.255.255.0 gw 192.168.6.2
route add -net 192.168.2.0 netmask 255.255.255.0 gw 192.168.0.6
route add -net 192.168.0.8 netmask 255.255.255.248 gw 192.168.0.6
~~~

- KEDIRI
~~~
route add -net 0.0.0.0 netmask 0.0.0.0 gw 192.168.0.5
~~~

- BATU
~~~
route add -net 0.0.0.0 netmask 0.0.0.0 gw 192.168.6.1
~~~

7. Melakukan `ping` pada masing-masing UML untuk mengetes apakah setiap UML sudah terkoneksi.


8. Mengatur DHCP Server dan DHCP Relay yang akan digunakan untuk memberikan IP pada Client.
- Pada MOJOKERTO lakukan `install isc-dhcp-server`, kemudian atur konfigurasinya sebagai berikut:
- Pada BATU dan KEDIRI lakukan `install isc-dhcp-relay`, kemudian atur konfigurasinya sebagai berikut:
- Lakukan `service isc-dhcp-server restart` pada MOJOKERTO dan `service isc-dhcp-relay restart` pada BATU dan KEDIRI.
- Lakukan `service networking restart` pada SIDOARJO dan GRESIK, kemudian cek IP Address dengan menggunakan `ifconfig`.
9. Mengatur DNS Server.
10. Mengatur Web Server.
---
## Proses Pengerjaan
1. Konfigurasi SURABAYA agar topologi dapat mengakses ke luar dengan menggunakan iptables namun dilarang untuk menggunakan MASQUERADE.
~~~
iptables -t nat -A POSTROUTING -s 192.168.0.0/16 -o eth0 -j SNAT --to-source 10.151.78.66
~~~

2. Drop semua akses SSH yang berasal dari luar topologi pada server yang memiliki IP DMZ, iptables diatur di SURABAYA.
~~~

~~~

3. DHCP dan DNS Server hanya boleh menerima 3 koneksi ICMP secara bersamaan yang berasal dari mana saja dan selebihnya di DROP, iptables diatur di masing-masing server.
~~~

~~~

4. Akses ke MALANG dari SIDOARJO hanya diperbolehkan pada pukul 07.00 - 17.00 pada hari Senin sampai Jumat, selain itu paket akan di `REJECT`.
~~~

~~~

5. Akses ke MALANG dari GRESIK hanya diperbolehkan pada pukul 17.00 hingga pukul 07.00 setiap harinya, selain itu paket akan di `REJECT`.
~~~

~~~

6. Setting Router SURABAYA agar setiap *request* dari *client* yang mengakses DNS Server akan didistribusikan secara bergantian pada PROBOLINGGO port 80 dan MADIUN port 80.
~~~

~~~

7. Semua paket yang di-drop oleh *firewall* (dalam topologi) tercatat dalam log pada setiap UML yang memiliki aturan drop.
~~~

~~~ 
