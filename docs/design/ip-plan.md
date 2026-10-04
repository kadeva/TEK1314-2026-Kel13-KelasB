# IP Address Plan

## 1. Ringkasan Jaringan

Topologi proyek menggunakan Tailscale VPN sebagai encrypted overlay network
untuk menghubungkan seluruh node yang berada pada perangkat atau jaringan
fisik berbeda.

Setiap node akan memperoleh alamat IP Tailscale setelah bergabung ke tailnet
yang sama.

- **Jenis jaringan:** Tailscale VPN
- **Model koneksi:** Encrypted overlay network
- **Status implementasi:** Planned
- **IP aktual:** To Be Determined (TBD)

## 2. Alokasi Node

| No. | Node | Hostname | Sistem Operasi | Tailscale IP | Fungsi |
|---:|---|---|---|---|---|
| 1 | Attacker Node | RED-KALI | Kali Linux | TBD | Menghasilkan traffic pengujian keamanan |
| 2 | Target Node | SRV-IOT | Ubuntu Server | TBD | Menjalankan web app, API, dan database dummy |
| 3 | Monitoring Node | SOC-SECURITY-ONION | Security Onion | TBD | Monitoring traffic dan analisis log |

## 3. Rencana Service Target

Target Node direncanakan menjalankan service berikut:

| Port | Protokol | Service | Fungsi | Rencana Akses |
|---:|---|---|---|---|
| 22 | TCP | SSH | Administrasi Target Node | Dibatasi melalui VPN |
| 80 | TCP | HTTP | Web application dan REST API | Dapat diakses Attacker Node |
| 443 | TCP | HTTPS | Web application terenkripsi | Dapat diakses Attacker Node |
| 3306 | TCP | MySQL/MariaDB | Database aplikasi | Localhost/internal only |

Port database tidak direncanakan untuk diekspos langsung kepada Attacker Node.

## 4. Alur Komunikasi

Alur komunikasi utama yang direncanakan:
```
RED-KALI
Attacker Node
     |
     | Traffic pengujian
     v
Tailscale VPN
     |
     v
SRV-IOT
Target Node
```
Monitoring Node akan bergabung ke jaringan Tailscale yang sama. Namun, metode agar Security Onion memperoleh visibility terhadap traffic antara Attacker Node dan Target Node masih perlu diuji pada tahap implementasi.
```
SOC-SECURITY-ONION
Monitoring Node
     |
     | Monitoring method
```

5. Rencana Pengujian Konektivitas

Setelah seluruh node terhubung ke Tailscale, kelompok akan melakukan:

Verifikasi IP Tailscale setiap node.
Pengujian ping dari Attacker Node menuju Target Node.
Pengujian akses HTTP atau HTTPS menuju Target Node.
Pengujian akses SSH yang telah diizinkan.
Pengujian visibility traffic pada Monitoring Node.
Dokumentasi source IP, destination IP, protocol, port, dan timestamp.

6. Catatan Monitoring

Bergabungnya Security Onion ke jaringan Tailscale tidak secara otomatis menjamin bahwa Security Onion dapat melihat traffic antara Attacker Node dan Target Node.

Metode monitoring yang akan diuji meliputi:

Routing traffic melalui Monitoring Node
Traffic mirroring
Packet capture pada Target Node
Import file PCAP
Analisis log sistem dan aplikasi

Metode final akan ditentukan berdasarkan hasil pengujian dan arahan dosen atau asisten praktikum.

7. Data yang Akan Diperbarui

Setelah implementasi Tailscale selesai, data berikut akan diperbarui:

IP Tailscale RED-KALI
IP Tailscale SRV-IOT
IP Tailscale SOC-SECURITY-ONION
Status konektivitas setiap node
Metode monitoring yang berhasil digunakan
Port final yang dapat diakses

