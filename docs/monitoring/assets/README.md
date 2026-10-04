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
