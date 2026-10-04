# PBL Keamanan Siber TEK1314

Repository ini berisi dokumentasi proyek Project-Based Learning mata kuliah
Keamanan Siber TEK1314.

Proyek ini merancang lingkungan laboratorium untuk melakukan pengujian,
hardening, dan monitoring keamanan pada replika backend sistem IoT.

## Skenario Proyek

Kelompok merancang tiga node utama yang terhubung melalui jaringan virtual
Tailscale:

1. **Attacker Node**
   - Sistem operasi: Kali Linux
   - Fungsi: menghasilkan traffic pengujian keamanan terhadap Target Node

2. **Target Node**
   - Sistem operasi: Ubuntu Server
   - Fungsi: menjalankan replika backend IoT yang terdiri dari web application,
     API, dan database dummy

3. **Monitoring Node**
   - Sistem operasi: Security Onion
   - Fungsi: monitoring traffic, pengumpulan log, dan analisis aktivitas keamanan

Seluruh pengujian direncanakan hanya dilakukan pada lingkungan laboratorium
kelompok dengan menggunakan data sintetis atau dummy.


## Rancangan Topologi

Ketiga node direncanakan terhubung melalui encrypted overlay network
menggunakan Tailscale VPN.

Alur pengujian utama:

    Kali Linux / Attacker
              |
              | Traffic pengujian
              v
        Tailscale VPN
              |
              v
    Ubuntu Server / Target
    Web App + API + Database


Seluruh pengujian direncanakan hanya dilakukan pada lingkungan laboratorium kelompok dengan menggunakan data sintetis atau dummy.
Rancangan Topologi

Ketiga node direncanakan terhubung melalui encrypted overlay network menggunakan Tailscale VPN.

Security Onion direncanakan sebagai Monitoring Node. Metode agar Security Onion memperoleh visibility terhadap traffic Attacker menuju Target masih dalam tahap pengujian dan akan dikonfirmasi pada fase implementasi.

Diagram lengkap tersedia pada:

`docs/design/topology.png`


## Rencana IP Address

Alamat IP Tailscale akan diperoleh secara otomatis setelah setiap node bergabung ke jaringan Tailscale yang sama.

| Node       | Hostname           | Sistem Operasi | Tailscale IP | Fungsi                      |
| ---------- | ------------------ | -------------- | ------------ | --------------------------- |
| Attacker   | RED-KALI           | Kali Linux     | TBD          | Security testing            |
| Target     | SRV-IOT            | Ubuntu Server  | TBD          | Web app, API, dan database  |
| Monitoring | SOC-SECURITY-ONION | Security Onion | TBD          | Monitoring dan analisis log |

**Detail IP Address Plan tersedia pada:
docs/design/ip-plan.md**

## Service yang Direncanakan

Target Node direncanakan menjalankan service berikut:

| Port | Protokol | Service       | Fungsi                       |
| ---: | -------- | ------------- | ---------------------------- |
|   22 | TCP      | SSH           | Administrasi server          |
|   80 | TCP      | HTTP          | Web application dan REST API |
|  443 | TCP      | HTTPS         | Web application terenkripsi  |
| 3306 | TCP      | MySQL/MariaDB | Database backend internal    |

Port database direncanakan hanya dapat diakses secara lokal atau dari service aplikasi yang membutuhkan.

## Pembagian Peran

**Project Lead: Deval**
Mengoordinasikan pekerjaan kelompok
Mengintegrasikan hasil kerja setiap anggota
Mengelola repository dan dokumentasi
Memastikan scope dan timeline proyek terpenuhi

**Target Infrastructure: Izzy**
Menyiapkan Target Node
Menyiapkan web application dan API
Menyiapkan database berisi data dummy
Mendokumentasikan konfigurasi service Target Node

**Blue Team: Bintang**
Menyiapkan Monitoring Node
Merancang hardening dan monitoring
Mengumpulkan log serta bukti monitoring
Menganalisis aktivitas yang dihasilkan Red Team

**Red Team: Syahrial**
Menyiapkan Attacker Node
Menyusun attack plan
Menghasilkan traffic pengujian terkontrol
Mendokumentasikan command, target, waktu, dan hasil pengujian
