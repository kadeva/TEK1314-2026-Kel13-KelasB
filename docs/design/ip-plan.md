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
