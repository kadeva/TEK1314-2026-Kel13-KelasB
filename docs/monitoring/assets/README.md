# Monitoring Documentation

Folder ini berisi perencanaan, pelaksanaan, dan bukti monitoring aktivitas
keamanan pada Target Node selama pengujian Red Team.

## Tujuan

Dokumentasi monitoring digunakan untuk:

- Mencatat aktivitas jaringan dari Attacker Node menuju Target Node
- Mengidentifikasi source IP dan destination IP
- Mengidentifikasi protocol, port, service, dan timestamp
- Mengumpulkan log, alert, dan packet capture
- Mencocokkan aktivitas Red Team dengan bukti Blue Team
- Mendokumentasikan hasil analisis dan keterbatasan monitoring

## Lingkungan Monitoring

| Node | Hostname | Platform | Fungsi |
|---|---|---|---|
| Attacker Node | RED-KALI | Kali Linux | Menghasilkan traffic pengujian |
| Target Node | SRV-IOT | Ubuntu Server | Menjalankan web app, API, dan database dummy |
| Monitoring Node | SOC-SECURITY-ONION | Security Onion | Monitoring traffic dan analisis log |

Seluruh node direncanakan terhubung menggunakan Tailscale VPN.

Alamat IP aktual setiap node akan dicatat pada:

`../design/ip-plan.md`


## Format Bukti

Contoh penamaan file:

MON-01-icmp-traffic.png

MON-02-network-activity.pcap

MON-03-ssh-connection.txt

MON-04-web-access-log.txt

MON-05-authentication-log.txt

MON-06-database-verification.txt


Jika bukti berkaitan langsung dengan Attack Plan, nama file dapat menggunakan kedua ID:

AP-01-MON-02-network-activity.png

AP-02-MON-04-web-access-log.txt

AP-03-MON-05-authentication-log.txt

AP-04-MON-06-database-verification.txt

## Bukti monitoring dapat berupa:

Screenshot Security Onion

Alert atau Hunt result

Packet capture

Output tcpdump

Authentication log

Firewall log

Web access dan error log

Application log

Database log

Catatan timestamp pengujian

Informasi yang Harus Dicatat


## Setiap aktivitas monitoring harus mencatat:

Event ID

Attack Plan ID terkait

Tanggal dan waktu

Source IP

Destination IP

Protocol

Source dan destination port

Service yang diakses

Metode monitoring

Hasil deteksi

Nama file bukti

Ringkasan analisis
