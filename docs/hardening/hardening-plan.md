# Hardening Plan

## 1. Tujuan

Dokumen ini menjelaskan rencana hardening pada Target Node yang menjalankan
replika backend sistem IoT.

Hardening dilakukan untuk:

- Mengurangi attack surface
- Membatasi akses tidak sah
- Melindungi web application, API, dan database
- Meningkatkan kualitas logging
- Menyiapkan Target Node untuk pengujian Red Team

## 2. Informasi Target Node

| Komponen | Rencana |
|---|---|
| Hostname | SRV-IOT |
| Sistem Operasi | Ubuntu Server |
| Fungsi | Replika backend IoT |
| Web Server | Apache atau Nginx, TBD |
| Backend/API | TBD |
| Database | MySQL atau MariaDB, TBD |
| Jaringan | Tailscale VPN |
| Data | Dummy atau sintetis |

## 3. Status Hardening

Status yang digunakan dalam dokumen ini:

- **Planned:** Belum diterapkan
- **In Progress:** Sedang diterapkan atau diuji
- **Implemented:** Sudah diterapkan
- **Verified:** Sudah diuji dan memiliki bukti
- **Not Applicable:** Tidak relevan dengan implementasi final

## 4. Access and Authentication

### H-01: Menggunakan User Non-Root

- **Status:** Planned
- **Tujuan:** Mengurangi penggunaan akun dengan hak akses penuh.
- **Rencana implementasi:** Membuat user administrator non-root dan menggunakan
  `sudo` hanya ketika diperlukan.
- **Bukti before:** Daftar user dan metode administrasi sebelum hardening.
- **Bukti after:** Output identitas user dan keanggotaan grup `sudo`.

### H-02: Menonaktifkan Direct Root Login melalui SSH

- **Status:** Planned
- **Tujuan:** Mencegah login jarak jauh langsung menggunakan akun root.
- **Rencana implementasi:** Mengubah konfigurasi SSH agar root tidak dapat login
  secara langsung.
- **Bukti before:** Nilai konfigurasi root login sebelum perubahan.
- **Bukti after:** Nilai konfigurasi setelah perubahan dan hasil pengujian login.

### H-03: Menggunakan SSH Key Authentication

- **Status:** Planned
- **Tujuan:** Mengurangi ketergantungan pada autentikasi password.
- **Rencana implementasi:** Menambahkan public key administrator pada Target Node.
- **Bukti before:** Login masih menggunakan password.
- **Bukti after:** Login menggunakan SSH key berhasil.

### H-04: Menerapkan Kebijakan Password

- **Status:** Planned
- **Tujuan:** Mengurangi risiko penggunaan password yang lemah.
- **Rencana implementasi:** Menetapkan panjang minimum, kompleksitas, dan
  pembatasan percobaan login apabila didukung.
- **Bukti before:** Konfigurasi password awal.
- **Bukti after:** Konfigurasi password yang sudah diperbarui.

## 5. Service and Attack Surface

### H-05: Mengaktifkan Host Firewall

- **Status:** Planned
- **Tujuan:** Membatasi koneksi masuk hanya untuk service yang diperlukan.
- **Rencana implementasi:** Menggunakan UFW dengan kebijakan default deny incoming.
- **Port yang direncanakan:**
  - `22/TCP` untuk SSH
  - `80/TCP` untuk HTTP
  - `443/TCP` untuk HTTPS
- **Bukti before:** Status firewall sebelum diaktifkan.
- **Bukti after:** Status firewall dan daftar rule setelah hardening.

### H-06: Menutup Port yang Tidak Diperlukan

- **Status:** Planned
- **Tujuan:** Mengurangi jumlah service yang dapat diakses dari jaringan.
- **Rencana implementasi:** Memeriksa listening port dan menutup service yang tidak
  digunakan.
- **Bukti before:** Hasil pemeriksaan port sebelum hardening.
- **Bukti after:** Hasil pemeriksaan port setelah hardening.

### H-07: Menonaktifkan Service yang Tidak Digunakan

- **Status:** Planned
- **Tujuan:** Mengurangi attack surface dan penggunaan resource.
- **Rencana implementasi:** Mengidentifikasi service aktif, lalu menonaktifkan
  service yang tidak mendukung fungsi Target Node.
- **Bukti before:** Daftar service aktif.
- **Bukti after:** Daftar service setelah hardening.

### H-08: Melakukan Security Update

- **Status:** Planned
- **Tujuan:** Memperbaiki kerentanan yang sudah memiliki security patch.
- **Rencana implementasi:** Memperbarui repository dan package sistem.
- **Bukti before:** Daftar package yang dapat diperbarui.
- **Bukti after:** Riwayat update dan status package.

## 6. Data and Application Security

### H-09: Membatasi Akses Database

- **Status:** Planned
- **Tujuan:** Mencegah database diakses langsung oleh Attacker Node.
- **Rencana implementasi:** Mengikat database ke localhost atau interface internal.
- **Bukti before:** Alamat dan port database sebelum pembatasan.
- **Bukti after:** Database hanya menerima koneksi yang dibutuhkan aplikasi.

### H-10: Menggunakan Database User dengan Least Privilege

- **Status:** Planned
- **Tujuan:** Membatasi dampak jika akun aplikasi mengalami kompromi.
- **Rencana implementasi:** Membuat akun database khusus untuk aplikasi dengan
  privilege minimum.
- **Bukti before:** Hak akses database awal.
- **Bukti after:** Hak akses akun aplikasi setelah pembatasan.

### H-11: Melakukan Validasi Input pada Web Application dan API

- **Status:** Planned
- **Tujuan:** Menolak input yang tidak sesuai format dan mengurangi risiko
  manipulasi request.
- **Rencana implementasi:** Melakukan validasi tipe data, panjang input, field
  wajib, dan format request.
- **Bukti before:** Request tidak valid masih diterima atau belum ditangani.
- **Bukti after:** Request tidak valid ditolak dengan respons yang sesuai.

### H-12: Memisahkan Credential dari Source Code

- **Status:** Planned
- **Tujuan:** Mencegah username, password, token, atau database credential
  tersimpan langsung di repository.
- **Rencana implementasi:** Menyimpan credential pada environment variable atau
  file konfigurasi yang tidak dimasukkan ke Git.
- **Bukti before:** Metode penyimpanan konfigurasi awal.
- **Bukti after:** Credential dipindahkan dan file sensitif tercantum dalam
  `.gitignore`.

## 7. Logging and Monitoring

### H-13: Mengaktifkan dan Memeriksa Authentication Log

- **Status:** Planned
- **Tujuan:** Mencatat aktivitas login berhasil dan gagal.
- **Rencana implementasi:** Memastikan aktivitas SSH tercatat pada system log.
- **Bukti before:** Kondisi log sebelum traffic pengujian.
- **Bukti after:** Log percobaan login dengan timestamp dan source address.

### H-14: Mengaktifkan Web Access dan Error Log

- **Status:** Planned
- **Tujuan:** Mencatat request yang diterima oleh web application dan API.
- **Rencana implementasi:** Mengaktifkan access log dan error log pada web server.
- **Bukti before:** Kondisi konfigurasi logging awal.
- **Bukti after:** Log HTTP yang memuat timestamp, source address, method, path,
  dan status code.

### H-15: Menyiapkan Application Log

- **Status:** Planned
- **Tujuan:** Mencatat aktivitas penting pada web application dan API.
- **Rencana implementasi:** Mencatat keberhasilan dan kegagalan autentikasi,
  request tidak valid, serta error aplikasi.
- **Bukti before:** Logging aplikasi belum tersedia atau masih terbatas.
- **Bukti after:** Event aplikasi tercatat tanpa menyimpan password atau token.

## 8. Ringkasan Kontrol

| ID | Kontrol | Kategori | Status |
|---|---|---|---|
| H-01 | User non-root | Access and Authentication | Planned |
| H-02 | Disable SSH root login | Access and Authentication | Planned |
| H-03 | SSH key authentication | Access and Authentication | Planned |
| H-04 | Password policy | Access and Authentication | Planned |
| H-05 | Host firewall | Service and Attack Surface | Planned |
| H-06 | Close unnecessary ports | Service and Attack Surface | Planned |
| H-07 | Disable unused services | Service and Attack Surface | Planned |
| H-08 | Security update | Service and Attack Surface | Planned |
| H-09 | Restrict database access | Data and Application | Planned |
| H-10 | Least-privilege database user | Data and Application | Planned |
| H-11 | Application input validation | Data and Application | Planned |
| H-12 | Separate credentials from source code | Data and Application | Planned |
| H-13 | Authentication logging | Logging and Monitoring | Planned |
| H-14 | Web access and error logging | Logging and Monitoring | Planned |
| H-15 | Application logging | Logging and Monitoring | Planned |

## 9. Rencana Pengumpulan Bukti

Setiap kontrol yang diterapkan akan memiliki bukti berupa:

1. Screenshot atau output kondisi sebelum hardening.
2. Konfigurasi atau command yang digunakan.
3. Screenshot atau output kondisi setelah hardening.
4. Hasil pengujian untuk memastikan kontrol bekerja.
5. Penjelasan singkat mengenai perubahan dan dampaknya.

Bukti akan disimpan pada:
```
docs/hardening/assets/
```
## 10. Prioritas Implementasi

Kontrol yang diprioritaskan pada implementasi awal:

Security update
User non-root
Host firewall
Penutupan port yang tidak diperlukan
Pembatasan akses database
Authentication logging
Web access logging
Pemisahan credential dari source code

Kontrol lain akan diterapkan sesuai kebutuhan dan teknologi aplikasi yang digunakan.

## 11. Catatan

Dokumen ini masih berupa rencana awal. Daftar kontrol dapat berubah setelah:

Web application dan database final ditentukan
Target Node berhasil diimplementasikan
Topologi jaringan selesai diuji
Kelompok menerima arahan dari dosen atau asisten praktikum
Attack plan Red Team selesai disusun
