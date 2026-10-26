  # Hardening Plan

## 1. Tujuan

Dokumen ini menjelaskan rencana dan progres hardening pada Target Node yang
menjalankan replika backend sistem IoT.

Hardening dilakukan untuk:

- Mengurangi attack surface
- Membatasi akses tidak sah
- Melindungi web application, API, dan database
- Meningkatkan kualitas logging
- Menyiapkan Target Node untuk pengujian Red Team

## 2. Informasi Target Node

| Komponen | Implementasi |
|---|---|
| Hostname | SRV-IOT |
| Sistem Operasi | Ubuntu Server |
| Fungsi | Replika backend IoT |
| IoT Platform | ThingsBoard |
| Web Port | 8080/TCP |
| Database | PostgreSQL |
| Remote Administration | SSH pada 2222/TCP |
| Jaringan | Tailscale VPN |
| Data | Dummy atau sintetis |

## 3. Status Hardening

Status yang digunakan dalam dokumen ini:

- **Planned:** Belum diterapkan
- **In Progress:** Sedang diterapkan atau diuji
- **Implemented:** Sudah diterapkan, tetapi belum seluruhnya diverifikasi
- **Verified:** Sudah diterapkan, diuji, dan memiliki bukti
- **Not Applicable:** Tidak relevan dengan implementasi final

---

## 4. Access and Authentication

### H-01: Menggunakan User Non-Root

- **Status:** Implemented
- **Tujuan:** Mengurangi penggunaan akun dengan hak akses penuh.
- **Implementasi:** Administrasi server dilakukan menggunakan akun non-root
  `kelompok13`.
- **Current evidence:** Penggunaan akun `kelompok13` pada terminal server.
- **Verifikasi lanjutan:** Menampilkan identitas user dan keanggotaan grup
  administrator menggunakan `id` dan `groups`.

### H-02: Menonaktifkan Direct Root Login melalui SSH

- **Status:** Implemented
- **Tujuan:** Mencegah login jarak jauh secara langsung menggunakan akun root.
- **Implementasi:** Mengubah konfigurasi efektif SSH dari
  `PermitRootLogin without-password` menjadi `PermitRootLogin no`.
- **Bukti before:** `PermitRootLogin without-password`.
- **Bukti after:** `PermitRootLogin no`.
- **Verifikasi lanjutan:** Melakukan pengujian login langsung menggunakan akun
  root untuk memastikan koneksi ditolak.

### H-03: Menggunakan SSH Key Authentication

- **Status:** Implemented
- **Tujuan:** Mengurangi risiko penggunaan password dan credential guessing.
- **Implementasi:**
  - `PubkeyAuthentication yes`
  - `PasswordAuthentication no`
- **Bukti current state:** Output konfigurasi efektif SSH dari `sshd -T`.
- **Catatan:** Konfigurasi public key authentication sudah aktif pada kondisi
  awal dan dipertahankan setelah hardening.
- **Verifikasi lanjutan:** Menguji login user non-root menggunakan public key
  yang sah.

### H-04: Menerapkan Kebijakan Password

- **Status:** Planned
- **Tujuan:** Mengurangi risiko penggunaan password yang lemah pada akun lokal.
- **Rencana implementasi:** Menetapkan panjang minimum, kompleksitas, dan
  pembatasan penggunaan ulang password apabila relevan.
- **Bukti before:** Konfigurasi kebijakan password awal.
- **Bukti after:** Konfigurasi kebijakan password setelah perubahan.

### H-04A: Membatasi Percobaan dan Koneksi SSH

- **Status:** Implemented
- **Tujuan:** Mengurangi risiko percobaan autentikasi berulang dan membatasi
  koneksi SSH yang belum terautentikasi.
- **Implementasi:**
  - Mengubah `MaxAuthTries` dari `6` menjadi `3`.
  - Mengubah `MaxStartups` dari `10:30:100` menjadi `5:30:20`.
- **Bukti before:**
  - `MaxAuthTries 6`
  - `MaxStartups 10:30:100`
- **Bukti after:**
  - `MaxAuthTries 3`
  - `MaxStartups 5:30:20`
- **Konfigurasi yang tidak berubah:**
  - `LoginGraceTime 120`
  - `PerSourceMaxStartups none`
- **Verifikasi lanjutan:** Menguji pembatasan autentikasi secara terkontrol pada
  tahap pengujian berikutnya.

### H-04B: Mengaktifkan Fail2Ban untuk SSH

- **Status:** Implemented
- **Tujuan:** Membatasi percobaan autentikasi SSH berulang dari sumber yang sama.
- **Implementasi:** Mengaktifkan service Fail2Ban dan jail `sshd`.
- **Bukti before:**
  - Service `fail2ban` berstatus `inactive (dead)`.
  - Service belum diaktifkan saat boot (`disabled`).
  - Perintah `fail2ban-client status sshd` gagal karena socket Fail2Ban
    tidak tersedia.
- **Bukti after:**
  - Service `fail2ban` berstatus `active (running)`.
  - Service telah diaktifkan saat boot (`enabled`).
  - Jail `sshd` aktif.
  - `maxretry` dikonfigurasi sebanyak `5`.
  - `bantime` dikonfigurasi selama `3600` detik.
- **Current state:**
  - `Currently failed: 0`
  - `Currently banned: 0`
  - `Total banned: 0`
- **Verifikasi lanjutan:** Melakukan percobaan login gagal secara terkontrol
  dari Attacker Node untuk memastikan alamat sumber diblokir setelah mencapai
  batas percobaan.

---

## 5. Service and Attack Surface

### H-05: Mengaktifkan Host Firewall

- **Status:** In Progress
- **Tujuan:** Membatasi koneksi masuk hanya untuk service yang diperlukan.
- **Implementasi saat ini:** UFW telah aktif dengan kebijakan:
  - `deny incoming`
  - `allow outgoing`
  - `deny routed`
- **Port yang saat ini diizinkan:**
  - `22/TCP`
  - `80/TCP`
  - `443/TCP`
  - `8080/TCP`
- **Bukti before:** Tidak terdokumentasi.
- **Bukti current state:** Output `sudo ufw status verbose`.
- **Catatan:** Rule masih menerima koneksi dari `Anywhere` untuk IPv4 dan IPv6.
- **Tindak lanjut:** Membatasi rule berdasarkan kebutuhan service dan jaringan
  Tailscale.

### H-06: Menutup Port yang Tidak Diperlukan

- **Status:** In Progress
- **Tujuan:** Mengurangi jumlah service yang dapat diakses dari jaringan.
- **Rencana implementasi:** Memeriksa listening port dan menutup port yang tidak
  mendukung fungsi Target Node.
- **Port yang direncanakan:**
  - `2222/TCP` untuk SSH
  - `8080/TCP` untuk ThingsBoard
  - Port database hanya untuk akses internal
- **Catatan:** Rule UFW masih mengizinkan port `22`, meskipun SSH terdeteksi
  menggunakan port `2222`.
- **Tindak lanjut:** Menyesuaikan rule UFW dengan konfigurasi SSH aktual.
- **Bukti before:** Output port dan rule firewall saat ini.
- **Bukti after:** Output port dan rule firewall setelah pembatasan.

### H-07: Menonaktifkan Service yang Tidak Digunakan

- **Status:** Planned
- **Tujuan:** Mengurangi attack surface dan penggunaan resource.
- **Rencana implementasi:** Mengidentifikasi service aktif, lalu menonaktifkan
  service yang tidak mendukung Target Node.
- **Bukti before:** Daftar service aktif.
- **Bukti after:** Daftar service setelah hardening.

### H-08: Melakukan Security Update

- **Status:** Planned
- **Tujuan:** Memperbaiki kerentanan yang sudah memiliki security patch.
- **Rencana implementasi:** Memperbarui repository dan package sistem.
- **Bukti before:** Daftar package yang dapat diperbarui.
- **Bukti after:** Riwayat update dan status package.

---

## 6. Data and Application Security

### H-09: Membatasi Akses Database

- **Status:** Planned
- **Tujuan:** Mencegah PostgreSQL diakses langsung oleh Attacker Node.
- **Rencana implementasi:** Membatasi PostgreSQL agar hanya menerima koneksi
  dari localhost atau service yang membutuhkan.
- **Bukti before:** Listening address dan port PostgreSQL sebelum pembatasan.
- **Bukti after:** PostgreSQL hanya menerima koneksi internal yang diperlukan.

### H-10: Menggunakan Database User dengan Least Privilege

- **Status:** Planned
- **Tujuan:** Membatasi dampak jika credential aplikasi mengalami kompromi.
- **Rencana implementasi:** Membuat akun PostgreSQL khusus ThingsBoard atau
  aplikasi dengan privilege minimum.
- **Bukti before:** Hak akses database awal.
- **Bukti after:** Hak akses akun aplikasi setelah pembatasan.

### H-11: Melakukan Validasi Input pada Web Application dan API

- **Status:** Planned
- **Tujuan:** Menolak input yang tidak sesuai format dan mengurangi risiko
  manipulasi request.
- **Rencana implementasi:** Melakukan validasi tipe data, panjang input, field
  wajib, dan format request pada endpoint API.
- **Bukti before:** Request tidak valid masih diterima atau belum ditangani.
- **Bukti after:** Request tidak valid ditolak dengan response yang sesuai.

### H-12: Memisahkan Credential dari Source Code

- **Status:** Planned
- **Tujuan:** Mencegah username, password, token, atau database credential
  tersimpan langsung di repository.
- **Rencana implementasi:** Menyimpan credential pada environment variable atau
  file konfigurasi yang tidak dimasukkan ke Git.
- **Bukti before:** Metode penyimpanan konfigurasi awal.
- **Bukti after:** Credential dipindahkan dan file sensitif dicantumkan dalam
  `.gitignore`.

---

## 7. Logging and Monitoring

### H-13: Mengaktifkan dan Memeriksa Authentication Log

- **Status:** Planned
- **Tujuan:** Mencatat aktivitas login berhasil dan gagal.
- **Rencana implementasi:** Memastikan aktivitas SSH tercatat pada system log.
- **Bukti before:** Kondisi log sebelum traffic pengujian.
- **Bukti after:** Log percobaan login dengan timestamp dan source address.

### H-14: Mengaktifkan Web Access dan Error Log

- **Status:** Planned
- **Tujuan:** Mencatat request yang diterima oleh ThingsBoard dan service web.
- **Rencana implementasi:** Memastikan access log dan error log tersedia.
- **Bukti before:** Kondisi konfigurasi logging awal.
- **Bukti after:** Log HTTP yang memuat timestamp, source address, method, path,
  dan status code.

### H-15: Menyiapkan Application Log

- **Status:** Planned
- **Tujuan:** Mencatat aktivitas penting pada backend IoT dan ThingsBoard.
- **Rencana implementasi:** Mencatat autentikasi, request tidak valid, serta
  error aplikasi tanpa menyimpan password atau token.
- **Bukti before:** Logging aplikasi belum tersedia atau masih terbatas.
- **Bukti after:** Event aplikasi tercatat tanpa informasi sensitif.

---

## 8. Ringkasan Kontrol

| ID | Kontrol | Kategori | Status |
|---|---|---|---|
| H-01 | User non-root | Access and Authentication | Implemented |
| H-02 | Disable SSH root login | Access and Authentication | Implemented |
| H-03 | SSH key authentication | Access and Authentication | Implemented |
| H-04 | Password policy | Access and Authentication | Planned |
| H-04A | Restrict SSH authentication attempts | Access and Authentication | Implemented |
| H-05 | Host firewall | Service and Attack Surface | In Progress |
| H-06 | Close unnecessary ports | Service and Attack Surface | In Progress |
| H-07 | Disable unused services | Service and Attack Surface | Planned |
| H-08 | Security update | Service and Attack Surface | Planned |
| H-09 | Restrict PostgreSQL access | Data and Application | Planned |
| H-10 | Least-privilege PostgreSQL user | Data and Application | Planned |
| H-11 | Application input validation | Data and Application | Planned |
| H-12 | Separate credentials from source code | Data and Application | Planned |
| H-13 | Authentication logging | Logging and Monitoring | Planned |
| H-14 | Web access and error logging | Logging and Monitoring | Planned |
| H-15 | Application logging | Logging and Monitoring | Planned |

---

## 9. Bukti Hardening

Bukti disimpan pada:

```
docs/hardening/assets/
```
