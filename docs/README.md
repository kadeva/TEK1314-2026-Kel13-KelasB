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


PBL Keamanan Siber TEK1314

Repository ini berisi dokumentasi proyek Project-Based Learning mata kuliah Keamanan Siber TEK1314.

Proyek ini merancang lingkungan laboratorium untuk melakukan pengujian, hardening, dan monitoring keamanan pada replika backend sistem IoT.
Skenario Proyek

Kelompok merancang tiga node utama yang terhubung melalui jaringan virtual Tailscale:

    Attacker Node
        Sistem operasi: Kali Linux
        Fungsi: menghasilkan traffic pengujian keamanan terhadap Target Node

    Target Node
        Sistem operasi: Ubuntu Server
        Fungsi: menjalankan replika backend IoT yang terdiri dari web application, API, dan database dummy

    Monitoring Node
        Sistem operasi: Security Onion
        Fungsi: monitoring traffic, pengumpulan log, dan analisis aktivitas keamanan

Seluruh pengujian direncanakan hanya dilakukan pada lingkungan laboratorium kelompok dengan menggunakan data sintetis atau dummy.
Rancangan Topologi

Ketiga node direncanakan terhubung melalui encrypted overlay network menggunakan Tailscale VPN.

Security Onion direncanakan sebagai Monitoring Node. Metode agar Security Onion memperoleh visibility terhadap traffic Attacker menuju Target masih dalam tahap pengujian dan akan dikonfirmasi pada fase implementasi.

Diagram lengkap tersedia pada:
docs/design/topology.png
