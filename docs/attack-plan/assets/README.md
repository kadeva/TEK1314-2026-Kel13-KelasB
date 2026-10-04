# Attack Plan Documentation

Folder ini berisi perencanaan, pelaksanaan, dan bukti pengujian keamanan
yang dilakukan oleh Red Team terhadap Target Node dalam lingkungan
laboratorium kelompok.

## Tujuan

Dokumentasi Attack Plan digunakan untuk:

- Menentukan scope pengujian keamanan
- Mendokumentasikan skenario pengujian Red Team
- Mencatat tools dan target yang digunakan
- Mengumpulkan bukti aktivitas Red Team
- Mencocokkan aktivitas dengan hasil monitoring Blue Team
- Mengevaluasi efektivitas hardening pada Target Node

## Lingkungan Pengujian

| Node | Hostname | Platform | Fungsi |
|---|---|---|---|
| Attacker Node | RED-KALI | Kali Linux | Menjalankan pengujian keamanan |
| Target Node | SRV-IOT | Ubuntu Server | Menjalankan web app, API, dan database dummy |
| Monitoring Node | SOC-SECURITY-ONION | Security Onion | Monitoring dan analisis aktivitas |

Seluruh node direncanakan terhubung melalui Tailscale VPN. IP aktual setiap
node akan dicatat pada:
`../design/ip-plan.md`

Skenario yang Direncanakan
AP-01: Network Reconnaissance

Pengujian untuk mengidentifikasi port dan service yang dapat diakses pada Target Node.

AP-02: Web and API Request Testing

Pengujian respons web application dan REST API terhadap request normal serta input dummy yang tidak sesuai format.

AP-03: Authentication Logging Test

Pengujian pencatatan login SSH berhasil dan gagal menggunakan akun dummy yang telah disiapkan.

AP-04: Database Exposure Verification

Verifikasi bahwa database tidak dapat diakses langsung dari Attacker Node dan hanya tersedia bagi aplikasi yang membutuhkannya.

Detail setiap skenario tersedia pada:
`attack-plan.md`

## Format Bukti

Setiap bukti menggunakan ID yang sesuai dengan skenario pengujian.

Contoh penamaan file:

`AP-01-red-output.txt
AP-01-blue-log.png
AP-02-http-response.txt
AP-02-web-access-log.png
AP-03-auth-test.png
AP-03-auth-log.txt
AP-04-port-verification.txt`

Bukti dapat berupa:
Screenshot terminal
Output command dalam format teks
Request dan response HTTP yang telah disanitasi
Authentication log
Web access log
Security Onion alert atau Hunt result
Packet capture
Catatan timestamp pengujian

## Status Pengujian
| ID    | Skenario                       | Status  |
| ----- | ------------------------------ | ------- |
| AP-01 | Network Reconnaissance         | Planned |
| AP-02 | Web and API Request Testing    | Planned |
| AP-03 | Authentication Logging Test    | Planned |
| AP-04 | Database Exposure Verification | Planned |

Status yang digunakan:

Planned: Belum dilaksanakan
**Ready**: Target dan monitoring sudah siap
**In Progress**: Sedang dilaksanakan
**Completed**: Pengujian telah selesai
**Verified**: Hasil Red Team telah dicocokkan dengan bukti Blue Team
**Cancelled**: Pengujian dibatalkan atau tidak lagi relevan

## Aturan Pengujian

Pengujian hanya dilakukan terhadap Target Node milik kelompok.
Pengujian harus sesuai dengan scope pada attack-plan.md.
Red Team harus berkoordinasi dengan Blue Team sebelum pengujian.
Setiap pengujian harus mencatat waktu mulai dan selesai.
Seluruh data dan akun pengujian harus menggunakan data dummy.
Pengujian dihentikan jika Target Node menjadi tidak stabil.
Sistem publik dan perangkat pihak lain tidak termasuk dalam scope.

## Keamanan Dokumentasi
Repository tidak boleh memuat:
Password
Private key
API token
Session cookie
Database credential
Data pribadi
Alamat atau informasi sensitif lainnya

Informasi sensitif pada screenshot dan output harus disensor sebelum diunggah.

## Dokumen Terkait

Rancangan arsitektur: `../design/architecture.md`

Perencanaan IP: `../design/ip-plan.md`

Rencana hardening: `../hardening/hardening-plan.md`

Rencana monitoring: `../monitoring/monitoring-plan.md`
