# Attack Plan

## 1. Tujuan

Dokumen ini menjelaskan rencana pengujian keamanan yang akan dilakukan oleh
Red Team terhadap Target Node dalam lingkungan laboratorium kelompok.

Pengujian bertujuan untuk:

- Mengidentifikasi service yang dapat diakses
- Menguji efektivitas kontrol hardening
- Menghasilkan traffic dan log untuk dianalisis Blue Team
- Membandingkan kondisi sebelum dan setelah hardening
- Mendokumentasikan temuan serta rekomendasi perbaikan

Dokumen ini merupakan rencana awal. Pengujian hanya boleh dilakukan setelah
Target Node, scope, dan jadwal telah disetujui seluruh anggota kelompok.

## 2. Informasi Lingkungan Pengujian

| Komponen | Informasi |
|---|---|
| Attacker Node | RED-KALI |
| Sistem Operasi Attacker | Kali Linux |
| Target Node | SRV-IOT |
| Sistem Operasi Target | Ubuntu Server |
| Monitoring Node | SOC-SECURITY-ONION |
| Platform Monitoring | Security Onion |
| Jaringan | Tailscale VPN |
| Data Pengujian | Dummy atau sintetis |
| Status Implementasi | Planned |

Alamat IP aktual akan diperbarui setelah seluruh node bergabung ke jaringan
Tailscale yang sama.

## 3. Scope Pengujian

### In Scope

Pengujian hanya mencakup:

- Target Node milik kelompok
- Web application replika backend IoT
- REST API
- SSH pada Target Node
- Database dummy melalui aplikasi
- Service dan port yang telah disetujui
- Traffic yang melewati lingkungan laboratorium kelompok

### Out of Scope

Pengujian tidak mencakup:

- Perangkat atau server milik pihak lain
- Infrastruktur publik
- Jaringan kampus di luar lingkungan lab yang disetujui
- Akun dan data nyata
- Social engineering
- Malware
- Persistence
- Penghapusan atau perubahan data secara permanen
- Denial-of-service tanpa izin khusus
- Pengujian yang dapat mengganggu perangkat anggota

## 4. Target Service

Target Node direncanakan menjalankan service berikut:

| Port | Protokol | Service | Fungsi | Status |
|---:|---|---|---|---|
| 22 | TCP | SSH | Administrasi server | Planned |
| 80 | TCP | HTTP | Web application dan REST API | Planned |
| 443 | TCP | HTTPS | Web application terenkripsi | Planned |
| 3306 | TCP | MySQL/MariaDB | Database backend internal | Planned |

Port database direncanakan hanya tersedia secara lokal dan tidak diekspos
langsung kepada Attacker Node.

## 5. Skenario Pengujian

### AP-01: Network Reconnaissance

- **Kategori:** Reconnaissance
- **Status:** Planned
- **Target:** SRV-IOT
- **Tujuan:** Mengidentifikasi host, port, dan service yang dapat diakses dari
  Attacker Node.
- **Tool:** Nmap atau tool discovery lain yang disetujui.
- **Expected activity:**
  - Koneksi ke beberapa port pada Target Node
  - Identifikasi service yang tersedia
  - Traffic TCP dan ICMP pada lingkungan lab
- **Expected evidence:**
  - Waktu mulai dan selesai
  - IP Attacker Node
  - IP Target Node
  - Daftar port yang terdeteksi
  - Screenshot atau output hasil pengujian
  - Log atau packet capture dari Blue Team
- **Success criteria:**
  - Red Team dapat mengidentifikasi port yang tersedia
  - Blue Team dapat menemukan sebagian atau seluruh aktivitas pengujian
  - Hasil aktual dapat dibandingkan dengan IP plan dan hardening plan

### AP-02: Web and API Request Testing

- **Kategori:** Web/Application Testing
- **Status:** Planned
- **Target:** Web application dan REST API pada SRV-IOT
- **Tujuan:** Memeriksa respons aplikasi terhadap request normal dan input dummy
  yang tidak sesuai format.
- **Tool:** Web browser, curl, atau HTTP client yang disetujui.
- **Expected activity:**
  - Request HTTP atau HTTPS ke web application
  - Request ke endpoint API dummy
  - Input kosong, format tidak valid, atau field yang tidak lengkap
- **Expected evidence:**
  - URL atau endpoint yang diuji
  - HTTP method
  - Waktu pengujian
  - Status code dan respons aplikasi
  - Web access log
  - Application log
- **Success criteria:**
  - Input valid diproses sesuai fungsi aplikasi
  - Input tidak valid ditolak dengan respons yang sesuai
  - Request tercatat dalam access log atau application log
  - Informasi sensitif tidak ditampilkan pada pesan error

### AP-03: Authentication Logging Test

- **Kategori:** Authentication and Access Control
- **Status:** Planned
- **Target:** Service SSH pada SRV-IOT
- **Tujuan:** Memastikan percobaan autentikasi berhasil dan gagal dicatat oleh
  sistem.
- **Tool:** SSH client.
- **Expected activity:**
  - Percobaan login menggunakan akun dummy yang telah disiapkan
  - Percobaan autentikasi gagal dalam jumlah terbatas
  - Login sah menggunakan metode yang telah disetujui
- **Expected evidence:**
  - Timestamp pengujian
  - Source address
  - Username dummy
  - Hasil login berhasil atau gagal
  - Authentication log dari Target Node
- **Success criteria:**
  - Percobaan autentikasi tercatat
  - Log dapat dibedakan antara login berhasil dan gagal
  - Direct root login ditolak setelah hardening
  - Tidak ada password, private key, atau credential dalam repository

### AP-04: Database Exposure Verification

- **Kategori:** Service and Data Exposure
- **Status:** Planned
- **Target:** Database pada SRV-IOT
- **Tujuan:** Memastikan database tidak dapat diakses langsung dari Attacker Node.
- **Tool:** Network scanner atau database client yang disetujui.
- **Expected activity:**
  - Pemeriksaan apakah port database terlihat dari Attacker Node
  - Verifikasi bahwa aplikasi tetap dapat mengakses database secara lokal
- **Expected evidence:**
  - Hasil pemeriksaan port dari Attacker Node
  - Listening address database pada Target Node
  - Log koneksi aplikasi ke database
- **Success criteria:**
  - Port database tidak dapat diakses langsung dari Attacker Node
  - Database hanya listen pada localhost atau interface internal
  - Web application tetap berfungsi setelah pembatasan diterapkan

## 6. Prioritas Skenario

Tiga skenario utama yang diprioritaskan adalah:

1. **AP-01:** Network Reconnaissance
2. **AP-02:** Web and API Request Testing
3. **AP-03:** Authentication Logging Test

Skenario AP-04 digunakan sebagai pengujian tambahan jika database dan aplikasi
sudah berhasil diimplementasikan.

## 7. Prosedur Koordinasi Red Team dan Blue Team

Sebelum pengujian:

1. Red Team menginformasikan skenario yang akan dijalankan.
2. Target Infrastructure memastikan Target Node siap.
3. Blue Team memastikan logging atau packet capture berjalan.
4. Project Lead mencatat waktu mulai pengujian.
5. Seluruh anggota menyepakati durasi dan batas pengujian.

Saat pengujian:

1. Red Team menjalankan satu skenario pada satu waktu.
2. Red Team mencatat command atau aktivitas yang dilakukan.
3. Blue Team memantau log, alert, atau packet capture.
4. Target Infrastructure memantau stabilitas aplikasi.
5. Pengujian dihentikan jika Target Node tidak stabil.

Setelah pengujian:

1. Red Team mencatat hasil aktual.
2. Blue Team mengumpulkan bukti monitoring.
3. Timestamp Red Team dan Blue Team dicocokkan.
4. Kelompok mendokumentasikan temuan.
5. Target Node dikembalikan ke kondisi stabil jika diperlukan.

## 8. Informasi yang Dicatat Red Team

Setiap pengujian harus mencatat:

| Data | Keterangan |
|---|---|
| Test ID | ID skenario, seperti AP-01 |
| Tanggal | Tanggal pelaksanaan |
| Waktu mulai | Timestamp awal |
| Waktu selesai | Timestamp akhir |
| Source IP | IP Attacker Node |
| Destination IP | IP Target Node |
| Target service | Service atau endpoint yang diuji |
| Tool | Tool yang digunakan |
| Aktivitas | Ringkasan pengujian |
| Hasil | Successful, unsuccessful, atau partially successful |
| Evidence | Nama file bukti |
| Catatan | Kendala dan observasi |

## 9. Template Hasil Pengujian

Gunakan format berikut setelah setiap skenario dilaksanakan:

```text
Test ID:
Tanggal:
Waktu mulai:
Waktu selesai:

Attacker IP:
Target IP:
Target service:
Tool:

Tujuan:
Aktivitas:
Expected result:
Actual result:

Blue Team visibility:
Evidence Red Team:
Evidence Blue Team:

Temuan:
Rekomendasi:
Status:
```

## 10. Rencana Bukti

Bukti Red Team dapat berupa:

Screenshot hasil pengujian
Output terminal dalam format teks
Catatan timestamp
Request dan response HTTP yang sudah disanitasi
Daftar port yang ditemukan

Bukti Blue Team dapat berupa:

Security Onion alert
Security Onion Hunt result
Packet capture
Web access log
Authentication log
Application log

Bukti direncanakan disimpan pada:
`docs/attack-plan/assets/`

Format nama file:
`AP-01-red-output.txt
AP-01-blue-log.png
AP-02-http-response.txt
AP-02-web-access-log.png
AP-03-auth-test.png
AP-03-auth-log.txt`

**Semua bukti harus diperiksa sebelum diunggah. Password, token, cookie, private key, database credential, dan informasi sensitif harus disensor atau dihapus.**

## 11. Hubungan dengan Hardening Plan

Pengujian akan digunakan untuk memverifikasi kontrol hardening berikut:
| Attack Plan | Kontrol yang Diverifikasi                                          |
| ----------- | ------------------------------------------------------------------ |
| AP-01       | Firewall, port exposure, dan unused services                       |
| AP-02       | Input validation, error handling, dan application logging          |
| AP-03       | SSH root login, authentication control, dan authentication logging |
| AP-04       | Database binding dan database access restriction                   |

Pengujian dapat dilakukan dalam dua tahap:

**Before hardening**, untuk mencatat kondisi awal.
**After hardening**, untuk memverifikasi perubahan keamanan.

Jika pengujian sebelum hardening berisiko terhadap stabilitas sistem, kelompok dapat menggunakan konfigurasi awal yang terdokumentasi sebagai baseline tanpa menjalankan seluruh pengujian.

## 12. Stop Conditions

Pengujian harus langsung dihentikan jika:

Target Node tidak merespons
Web application atau database berhenti bekerja
Penggunaan CPU, RAM, atau storage menjadi tidak stabil
Pengujian mengenai alamat di luar scope
Data berubah atau terhapus tanpa disengaja
Blue Team tidak dapat membedakan traffic pengujian
Project Lead atau pemilik Target Node meminta penghentian

## 13. Status Attack Plan

| ID    | Skenario                       | Status  |
| ----- | ------------------------------ | ------- |
| AP-01 | Network Reconnaissance         | Planned |
| AP-02 | Web and API Request Testing    | Planned |
| AP-03 | Authentication Logging Test    | Planned |
| AP-04 | Database Exposure Verification | Planned |

## 14. Catatan

Attack plan dapat berubah berdasarkan:

Implementasi final web application dan API
Port yang benar-benar tersedia
Kontrol hardening yang telah diterapkan
Kemampuan Security Onion memperoleh visibility
Arahan dosen atau asisten praktikum
Hasil konsultasi kelompok

Seluruh pengujian hanya dilakukan dalam lingkungan laboratorium yang telah disetujui.
