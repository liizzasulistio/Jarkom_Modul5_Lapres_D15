# Jarkom_Modul5_Lapres_D15

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

## Persiapan
1. Konfigurasi topologi.sh dan bye.sh sesuai dengan gambar topologi yang diberikan.
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
