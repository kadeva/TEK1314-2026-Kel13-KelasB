# Monitoring Plan

## 1. Tujuan
Dokumen ini menjelaskan rencana monitoring aktivitas keamanan pada Target Node
selama proses pengujian Red Team.

Monitoring bertujuan untuk:

- Mengamati traffic dari Attacker Node menuju Target Node
- Mencatat aktivitas berdasarkan timestamp
- Mengidentifikasi source IP dan destination IP
- Mengidentifikasi protocol, port, dan service yang diakses
- Mencocokkan aktivitas Red Team dengan log Blue Team
- Mengumpulkan bukti untuk proses analisis dan laporan

## 2. Informasi Lingkungan

| Komponen | Informasi |
|---|---|
| Attacker Node | RED-KALI |
| Platform Attacker | Kali Linux |
| Target Node | SRV-IOT |
| Platform Target | Ubuntu Server |
| Monitoring Node | SOC-SECURITY-ONION |
| Platform Monitoring | Security Onion |
| Network | Tailscale VPN |
| Status Implementasi | Planned |

Alamat IP aktual masing-masing node akan dicatat pada:

`../design/ip-plan.md`

## 3. Scope Monitoring
Monitoring direncanakan mencakup:

Traffic ICMP antara Attacker Node dan Target Node

Network reconnaissance dan port scanning

Koneksi SSH

Request HTTP dan HTTPS

Request menuju REST API

Percobaan autentikasi berhasil dan gagal

Aktivitas web application

Koneksi aplikasi menuju database

Error pada sistem dan aplikasi


Monitoring hanya dilakukan pada lingkungan laboratorium kelompok.

## 4. Plan A: Security Onion
Security Onion direncanakan sebagai platform utama untuk monitoring dan analisis aktivitas jaringan.

**Data yang ingin dikumpulkan**

Timestamp

Source IP

Destination IP

Source port

Destination port

Network protocol

Alert atau event

Metadata koneksi

Packet capture jika tersedia


## Aktivitas yang ingin diamati

| ID     | Aktivitas              | Bukti yang Diharapkan                    |
| ------ | ---------------------- | ---------------------------------------- |
| MON-01 | ICMP atau ping         | Source IP, destination IP, dan timestamp |
| MON-02 | Network reconnaissance | Koneksi ke beberapa port Target Node     |
| MON-03 | Akses SSH              | Koneksi menuju port 22/TCP               |
| MON-04 | Akses web dan API      | Request menuju port 80 atau 443          |
| MON-05 | Authentication test    | Login berhasil atau gagal                |
| MON-06 | Database exposure test | Percobaan akses menuju port database     |


## Kebutuhan Implementasi

Agar Security Onion dapat melakukan monitoring, traffic harus:

Melewati interface yang dapat dipantau Monitoring Node
Disalin menggunakan traffic mirroring
Dirutekan melalui sensor
Dikirim dalam bentuk log
Diimpor dalam bentuk file PCAP

Security Onion yang hanya bergabung ke jaringan Tailscale belum tentu dapat melihat traffic langsung antara Attacker Node dan Target Node.

Metode visibility final masih berstatus:

`TBD`

## 5. Plan B: Packet Capture dan Manual Logs
Jika Security Onion tidak memperoleh visibility terhadap traffic Tailscale, monitoring dilakukan menggunakan packet capture dan log pada Target Node.

### Packet Capture

Tool yang direncanakan:

tcpdump

Wireshark

File PCAP


### Data yang dikumpulkan:

Timestamp

Source IP

Destination IP

Protocol

Source port

Destination port

Packet summary

System Log


### Sumber log yang direncanakan:

SSH authentication log

System journal

Firewall log

Service log

Web dan Application Log


### Sumber log yang direncanakan:

Web server access log

Web server error log

REST API log

Application error log

Authentication event

Request validation event

Database Log


### Database log digunakan jika diperlukan untuk:

Memeriksa koneksi aplikasi ke database

Mendeteksi kegagalan autentikasi database

Memastikan database tidak diakses langsung oleh Attacker Node


Password, query sensitif, token, dan credential tidak boleh dimasukkan ke repository.

## 6. Hubungan dengan Attack Plan

| Attack Plan                   | Aktivitas Monitoring                         | Sumber Bukti                       |
| ----------------------------- | -------------------------------------------- | ---------------------------------- |
| AP-01: Network Reconnaissance | Mendeteksi akses ke beberapa port            | Security Onion, PCAP, firewall log |
| AP-02: Web and API Testing    | Mencatat HTTP request dan response           | Web access log, application log    |
| AP-03: Authentication Test    | Mencatat login berhasil dan gagal            | Authentication log, system journal |
| AP-04: Database Exposure      | Memastikan port database tidak dapat diakses | PCAP, firewall log, database log   |


## 7. Prosedur Monitoring
### Sebelum Pengujian

Pastikan waktu pada seluruh node telah disinkronkan.

Catat IP Attacker Node, Target Node, dan Monitoring Node.

Pastikan Target Node dapat diakses melalui Tailscale.

Pastikan logging sistem dan aplikasi aktif.

Aktifkan packet capture jika diperlukan.

Catat waktu mulai monitoring.

Konfirmasikan skenario yang akan dijalankan Red Team.


### Saat Pengujian

Red Team menjalankan satu skenario pada satu waktu.

Blue Team mencatat waktu mulai aktivitas.

Blue Team mengamati log, traffic, atau alert.

Target Infrastructure memantau kondisi service.

Pengujian dihentikan jika Target Node tidak stabil.


### Setelah Pengujian

Hentikan packet capture.

Simpan log dan bukti yang relevan.

Cocokkan timestamp Red Team dengan log Blue Team.

Catat source IP, destination IP, protocol, dan port.

Sanitasi informasi sensitif.

Upload bukti yang sudah diperiksa ke repository.

Buat ringkasan hasil monitoring.


## 8. Informasi yang Harus Dicatat
Setiap monitoring event harus mencatat:

| Data             | Keterangan                                      |
| ---------------- | ----------------------------------------------- |
| Event ID         | ID aktivitas monitoring                         |
| Attack Plan ID   | Skenario Red Team terkait                       |
| Tanggal          | Tanggal pengujian                               |
| Waktu mulai      | Timestamp awal                                  |
| Waktu selesai    | Timestamp akhir                                 |
| Source IP        | Alamat Attacker Node                            |
| Destination IP   | Alamat Target Node                              |
| Protocol         | ICMP, TCP, UDP, HTTP, atau lainnya              |
| Destination port | Port service target                             |
| Detection source | Security Onion, PCAP, atau log                  |
| Result           | Detected, partially detected, atau not detected |
| Evidence         | Nama file bukti                                 |
| Notes            | Temuan atau kendala                             |

## 9. Template Hasil Monitoring
Gunakan format berikut untuk setiap aktivitas:

Event ID:

Attack Plan ID:


Tanggal:

Waktu mulai:

Waktu selesai:


Source IP:

Destination IP:

Protocol:

Destination port:


Monitoring method:

Detection source:


Expected activity:

Observed activity:

Detection result:


Evidence:

Analysis:

Recommendation:

Status:

## 10. Success Criteria
Monitoring dinyatakan berhasil jika:

Traffic pengujian dapat dikaitkan dengan skenario Red Team
Timestamp Red Team dan Blue Team dapat dicocokkan
Source IP dan destination IP dapat diidentifikasi
Protocol atau service target dapat diidentifikasi
Bukti log atau PCAP dapat disimpan
Blue Team dapat menjelaskan aktivitas yang diamati
Informasi sensitif tidak terlihat pada bukti

Security Onion tidak harus menghasilkan alert untuk setiap aktivitas. Traffic normal seperti ping atau request web dapat muncul sebagai metadata atau log, bukan selalu sebagai alert keamanan.


## 11. Rencana Bukti
Bukti monitoring akan disimpan pada:

`docs/monitoring/assets/`


Contoh nama file:

MON-01-icmp-traffic.png

MON-02-network-scan.png

MON-03-ssh-connection.txt

MON-04-web-access-log.txt

MON-05-authentication-log.txt

MON-06-database-verification.txt


Jika bukti berkaitan langsung dengan Attack Plan, nama file dapat menggunakan kedua ID:

AP-01-MON-02-network-activity.png

AP-02-MON-04-web-access-log.txt

AP-03-MON-05-authentication-log.txt


## 12. Status Monitoring
| ID     | Aktivitas                    | Metode Utama                   | Status  |
| ------ | ---------------------------- | ------------------------------ | ------- |
| MON-01 | ICMP monitoring              | Security Onion atau PCAP       | Planned |
| MON-02 | Reconnaissance monitoring    | Security Onion atau PCAP       | Planned |
| MON-03 | SSH monitoring               | Network dan authentication log | Planned |
| MON-04 | Web/API monitoring           | Web access dan application log | Planned |
| MON-05 | Authentication monitoring    | Authentication log             | Planned |
| MON-06 | Database exposure monitoring | PCAP dan database log          | Planned |


## 13. Batasan
Batasan yang telah diketahui:


Traffic Tailscale dapat berjalan langsung antara Attacker Node dan Target Node.

Monitoring Node belum tentu menerima salinan traffic tersebut.

Security Onion membutuhkan resource komputasi yang relatif besar.

Beberapa aktivitas normal mungkin tidak menghasilkan alert.

Perbedaan waktu pada node dapat menyulitkan korelasi log.


## 14. Mitigasi Keterbatasan
Jika Security Onion tidak dapat melihat traffic, kelompok akan:


Menjalankan packet capture pada Target Node.


Mengumpulkan system dan application log.

Mengimpor PCAP untuk analisis jika memungkinkan.


## 15. Catatan
Dokumen ini masih berupa rencana awal dan dapat berubah berdasarkan:

Metode monitoring yang berhasil diuji

Implementasi final Tailscale

Konfigurasi Target Node

Kemampuan Security Onion

Hasil konsultasi dengan dosen atau asisten praktikum
Mencocokkan bukti berdasarkan timestamp.
Mendokumentasikan keterbatasan secara terbuka.
Meminta arahan dosen atau asisten praktikum.
