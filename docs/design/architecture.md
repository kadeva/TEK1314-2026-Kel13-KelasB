# System Architecture

## 1. Gambaran Umum

Proyek ini merancang lingkungan laboratorium keamanan siber untuk melakukan
pengujian, hardening, dan monitoring pada replika backend sistem IoT.

Arsitektur terdiri dari tiga node utama:

1. Attacker Node
2. Target Node
3. Monitoring Node

Setiap node direncanakan berada pada perangkat atau jaringan fisik yang berbeda
dan dihubungkan menggunakan Tailscale VPN sebagai encrypted overlay network.

## 2. Diagram Arsitektur

```
┌─────────────────────────┐
│ Attacker Node           │
│ Hostname: RED-KALI      │
│ OS: Kali Linux          │
│ Role: Red Team          │
└────────────┬────────────┘
             │
             │ Traffic pengujian
             │ melalui Tailscale VPN
             ▼
┌─────────────────────────┐
│ Target Node             │
│ Hostname: SRV-IOT       │
│ OS: Ubuntu Server       │
│                         │
│ - Web Application       │
│ - REST API              │
│ - Database Dummy        │
│ - SSH Server            │
└────────────┬────────────┘
             │
             │ Log dan traffic
             ▼
┌─────────────────────────┐
│ Monitoring Node         │
│ Hostname:               │
│ SOC-SECURITY-ONION      │
│ OS: Security Onion      │
│ Role: Blue Team         │
└─────────────────────────┘
```

## 3. Attacker Node

Attacker Node digunakan oleh Red Team untuk menghasilkan traffic pengujian yang terkontrol terhadap Target Node.

Rencana fungsi
Melakukan network reconnaissance
Mengidentifikasi port dan service yang tersedia
Mengirim request menuju web application dan REST API
Menguji pencatatan aktivitas autentikasi
Menghasilkan traffic yang dapat dianalisis oleh Blue Team

### Sistem operasi
Kali Linux

### Batasan
Pengujian hanya dilakukan terhadap Target Node milik kelompok.
Pengujian tidak dilakukan terhadap sistem publik atau pihak lain.
Seluruh aktivitas harus sesuai dengan attack plan.
Pengujian menggunakan data dummy.

## 4. Target Node

Target Node merupakan replika sederhana backend sistem IoT.

Target Node tidak menggunakan data IoT asli. Web application, API, dan database akan menggunakan data sintetis untuk mensimulasikan pengiriman dan penyimpanan data perangkat IoT.

### Sistem operasi
Ubuntu Server

### Komponen yang direncanakan
Web application atau dashboard
REST API
Database MySQL atau MariaDB
SSH untuk administrasi server
Web server Apache atau Nginx

### Service yang direncanakan
| Port | Protokol | Service       | Fungsi                       |
| ---: | -------- | ------------- | ---------------------------- |
|   22 | TCP      | SSH           | Administrasi server          |
|   80 | TCP      | HTTP          | Web application dan REST API |
|  443 | TCP      | HTTPS         | Web application terenkripsi  |
| 3306 | TCP      | MySQL/MariaDB | Database backend internal    |

Database direncanakan hanya menerima koneksi lokal dari aplikasi dan tidak diekspos langsung kepada Attacker Node.

## 5. Monitoring Node
Monitoring Node digunakan oleh Blue Team untuk mengumpulkan dan menganalisis informasi keamanan.

### Platform
Security Onion

### Rencana fungsi
Monitoring aktivitas jaringan
Mengumpulkan metadata traffic
Mencari aktivitas berdasarkan source dan destination IP
Mengidentifikasi protocol dan port
Menganalisis timestamp aktivitas
Mengumpulkan bukti berupa log, alert, atau packet capture

### Keterbatasan arsitektur

Security Onion direncanakan bergabung ke jaringan Tailscale yang sama. Namun, keberadaan Security Onion dalam jaringan tersebut tidak secara otomatis menjamin visibility terhadap traffic antara Attacker Node dan Target Node.

Metode monitoring masih berstatus TBD dan akan diuji melalui salah satu metode:

Routing traffic melalui Monitoring Node
Traffic mirroring
Packet capture pada Target Node
Import file PCAP ke Monitoring Node
Pengiriman log sistem dan aplikasi
Analisis log manual sebagai metode alternatif

## 6. Tailscale VPN

Tailscale direncanakan sebagai jaringan virtual yang menghubungkan node-node proyek dari perangkat dan lokasi fisik berbeda.

### Tujuan penggunaan
Menghubungkan Attacker Node dengan Target Node
Memberikan alamat privat untuk setiap node
Menghindari eksposur langsung service ke internet
Mempermudah pengujian lintas jaringan fisik

## 7. Alur Data

Alur utama pengujian adalah sebagai berikut:

Red Team menjalankan traffic pengujian dari Attacker Node.
Traffic dikirim melalui Tailscale VPN.
Target Node menerima traffic pada service yang diizinkan.
Target Node menghasilkan system log, authentication log, web access log, dan application log.
Blue Team mengumpulkan traffic atau log menggunakan metode monitoring yang berhasil diterapkan.
Hasil pengujian dicocokkan berdasarkan timestamp, source IP, destination IP, protocol, dan port.
Temuan didokumentasikan dalam repository kelompok.

## 8. Pembagian Tanggung Jawab

### Project Lead
Mengoordinasikan rancangan arsitektur
Memastikan integrasi antarnode
Mengelola repository dan dokumentasi
Memastikan scope pengujian dipatuhi

### Target Infrastructure
Menyiapkan Ubuntu Server
Menyiapkan web application dan REST API
Menyiapkan database dummy
Mendokumentasikan konfigurasi service

### Blue Team
Menyiapkan Security Onion
Merancang kontrol hardening
Mengumpulkan log dan bukti monitoring
Menganalisis aktivitas Red Team

### Red Team
Menyiapkan Kali Linux
Menyusun attack plan
Menghasilkan traffic pengujian terkontrol
Mencatat command, waktu, target, dan hasil pengujian

## 9. Status Implementasi
| Komponen                | Status      |
| ----------------------- | ----------- |
| Draft topologi          | Completed   |
| IP address plan         | In progress |
| Tailscale VPN           | Planned     |
| Attacker Node           | Planned     |
| Target Node             | Planned     |
| Web application dan API | Planned     |
| Database dummy          | Planned     |
| Monitoring Node         | Planned     |
| Monitoring visibility   | TBD         |
| Hardening               | Planned     |
| Attack simulation       | Not started |

## 10. Catatan Pengembangan
Arsitektur masih dapat berubah berdasarkan:

Hasil pengujian konektivitas Tailscale
Kemampuan Security Onion memperoleh visibility traffic
Ketersediaan resource perangkat
Arahan dosen dan asisten praktikum
Hasil implementasi Target Node
