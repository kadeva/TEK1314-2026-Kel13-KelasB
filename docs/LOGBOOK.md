# Project Logbook

## Informasi Proyek

- **Mata Kuliah:** TEK1314 Keamanan Siber
- **Jenis Proyek:** Project-Based Learning
- **Skenario:** Pengujian dan monitoring keamanan replika backend IoT
- **Repository:** Repository PBL kelompok
- **Status:** Design and Planning Phase

## Anggota dan Pembagian Peran

| Anggota | Peran | Tanggung Jawab Utama |
|---|---|---|
| Deval | Project Lead | Koordinasi, integrasi, repository, dokumentasi, dan final review |
| Izzy | Target Infrastructure | Target VM, web application, API, database, dan service |
| Bintang | Blue Team | Topologi, Security Onion, hardening, monitoring, dan analisis log |
| Syahrial | Red Team | Kali Linux, attack plan, traffic pengujian, dan dokumentasi pengujian |

## Status Akses Repository

| Anggota | Akses GitHub | Status |
|---|---|---|
| Deval | Write access | Active |
| Bintang | Write access | Active |
| Izzy | Belum tersedia | Pending |
| Syahrial | Belum tersedia | Pending |

Anggota yang belum memiliki akses GitHub dapat mengirimkan hasil pekerjaan
kepada Project Lead atau Blue Team. Kontribusi tetap dicatat menggunakan nama
anggota yang menghasilkan pekerjaan tersebut.

## Catatan Aktivitas

| Tanggal | Anggota | Aktivitas | Hasil atau Output | Status | Bukti |
|---|---|---|---|---|---|
| 27-09-2026 | Seluruh anggota | Diskusi pembagian peran proyek | Lead, Blue Team, dan Red Team ditentukan | Selesai | Diskusi kelompok |
| 27-09-2026 | Seluruh anggota | Diskusi awal kebutuhan Security Onion | Kebutuhan Monitoring Node dan keterbatasan resource diidentifikasi | Selesai | Diskusi kelompok |
| 27-09-2026 | Deval dan Izzy | Diskusi penempatan Security Onion | Security Onion direncanakan berjalan pada perangkat dengan resource memadai | Selesai | Diskusi kelompok |
| 27-09-2026 | Seluruh anggota | Menentukan pembagian node | Attacker, Target, dan Monitoring Node dibagi berdasarkan peran | Selesai | Diskusi kelompok |
| 27-09-2026 | Bintang dan Izzy | Diskusi rancangan jaringan | Tailscale VPN dipilih sebagai rancangan awal koneksi antarnode | Selesai | Diskusi kelompok |
| 04-10-2026 | Deval | Membuat struktur awal repository | Folder dokumentasi untuk design, hardening, attack plan, dan monitoring dibuat | Selesai | Commit GitHub |
| 04-10-2026 | Deval | Menyusun README utama | Deskripsi proyek, arsitektur, role, service, dan status proyek didokumentasikan | Selesai | `README.md` |
| 04-10-2026 | Deval | Membuat draft topologi jaringan | Diagram Attacker, Target, Monitoring, dan Tailscale VPN dibuat | Selesai | `docs/design/topology.png` |
| 04-10-2026 | Deval | Menyusun IP Address Plan | Alokasi node, service, dan IP Tailscale berstatus TBD didokumentasikan | Selesai | `docs/design/ip-plan.md` |
| 04-10-2026 | Deval | Menyusun arsitektur sistem | Fungsi node, alur data, dan rancangan konektivitas didokumentasikan | Selesai | `docs/design/architecture.md` |
| 04-10-2026 | Deval | Menyusun rencana hardening | Kontrol keamanan Target Node dan rencana bukti before-after disusun | Selesai | `docs/hardening/hardening-plan.md` |
| 04-10-2026 | Deval | Menyusun draft Attack Plan | Scope dan empat skenario awal pengujian Red Team dibuat | Selesai | `docs/attack-plan/attack-plan.md` |
| 04-10-2026 | Deval | Menyusun rencana monitoring | Plan A Security Onion dan Plan B packet capture/log disusun | Selesai | `docs/monitoring/monitoring-plan.md` |
| 04-10-2026 | Deval | Menambahkan dokumentasi folder | README untuk Attack Plan dan Monitoring dibuat | Selesai | Commit GitHub |
| 04-10-2026 | Seluruh anggota | Menyiapkan konsultasi PBL | Pertanyaan tentang topologi, Tailscale, dan visibility Security Onion dikumpulkan | Berjalan | Catatan kelompok |
| 05-10-2026 | Seluruh anggota | Mengikuti penjelasan PBL melalui Google Meet | Menunggu pelaksanaan | Direncanakan | Catatan konsultasi |

## Keputusan Proyek

### D-01: Pemilihan Target Node

- **Tanggal:** 27-09-2026
- **Keputusan:** Target Node menggunakan Ubuntu Server.
- **Alasan:** Ringan, mendukung web application, REST API, database, SSH, dan
  konfigurasi hardening.
- **Status:** Disetujui sementara.

### D-02: Pemilihan Skenario

- **Tanggal:** 04-10-2026
- **Keputusan:** Proyek menggunakan replika backend IoT.
- **Komponen:** Web application, REST API, database dummy, dan SSH.
- **Status:** Disetujui sementara.

### D-03: Pemilihan Jaringan

- **Tanggal:** 04-10-2026
- **Keputusan:** Tailscale VPN digunakan sebagai rancangan awal encrypted
  overlay network.
- **Catatan:** Alamat IP aktual dan konektivitas belum diuji.
- **Status:** Planned.

### D-04: Metode Monitoring

- **Tanggal:** 04-10-2026
- **Keputusan:** Security Onion menjadi metode monitoring utama.
- **Alternatif:** Packet capture, system log, web log, dan application log.
- **Catatan:** Visibility traffic Tailscale oleh Security Onion masih perlu
  diuji dan dikonfirmasi.
- **Status:** TBD.

## Tugas Aktif

| ID | Penanggung Jawab | Tugas | Output | Deadline | Status |
|---|---|---|---|---|---|
| T-01 | Izzy | Menentukan stack Target Node | OS, web server, backend, database, dan service | TBD | Assigned |
| T-02 | Izzy | Menyiapkan Target VM | Screenshot instalasi dan spesifikasi VM | TBD | Assigned |
| T-03 | Bintang | Review topologi | Topologi final dan catatan koneksi | TBD | Assigned |
| T-04 | Bintang | Menguji rencana monitoring | Hasil riset atau pengujian visibility Security Onion | TBD | Assigned |
| T-05 | Syahrial | Review Attack Plan | Tiga skenario utama dan tools yang digunakan | TBD | Assigned |
| T-06 | Syahrial | Menyiapkan Attacker Node | Bukti Kali Linux siap digunakan | TBD | Assigned |
| T-07 | Deval | Mengelola repository | Struktur dan dokumentasi terintegrasi | Berjalan | In Progress |
| T-08 | Deval | Menyusun checkpoint | `progress-summary.md` setelah progres teknis tersedia | TBD | Planned |

## Kendala

| ID | Tanggal | Kendala | Dampak | Rencana Tindak Lanjut | Status |
|---|---|---|---|---|---|
| K-01 | 27-09-2026 | Security Onion membutuhkan resource besar | Tidak semua laptop dapat menjalankannya | Gunakan perangkat yang lebih memadai dan siapkan Plan B | Open |
| K-02 | 04-10-2026 | Traffic Tailscale belum tentu terlihat oleh Security Onion | Monitoring jaringan dapat gagal | Uji routing, packet capture, atau manual log | Open |
| K-03 | 04-10-2026 | Izzy dan Syahrial belum memiliki akses repository | Tidak dapat commit secara langsung | Tambahkan sebagai collaborator atau kirim output kepada pengelola repo | Open |
| K-04 | 04-10-2026 | IP Tailscale belum tersedia | IP plan belum final | Perbarui setelah seluruh node bergabung ke tailnet | Open |

## Rencana Berikutnya

1. Mengikuti penjelasan PBL melalui Google Meet.
2. Mengonfirmasi topologi dan metode monitoring.
3. Menambahkan Izzy dan Syahrial sebagai collaborator GitHub.
4. Menentukan stack final Target Node.
5. Memperbarui IP plan setelah implementasi Tailscale.
6. Menguji konektivitas Attacker Node menuju Target Node.
7. Menguji visibility traffic pada Monitoring Node.
8. Mengumpulkan bukti hardening dan monitoring.
9. Membuat `docs/checkpoint/progress-summary.md`.

## Aturan Pembaruan Logbook

- Logbook diperbarui setiap terdapat aktivitas, keputusan, atau kendala penting.
- Setiap entri harus menyebutkan tanggal dan penanggung jawab.
- Status tugas harus diperbarui secara objektif.
- Kontribusi tetap dicatat atas nama pembuatnya meskipun diunggah anggota lain.
- Kolom bukti diisi dengan path file, commit, screenshot, atau catatan diskusi.
- Password, token, private key, dan data sensitif tidak dicantumkan.
- Logbook tidak digunakan untuk memberikan penilaian pribadi terhadap anggota.
